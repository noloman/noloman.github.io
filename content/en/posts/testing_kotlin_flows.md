---
author: Manuel Lorenzo
title: Testing Kotlin flows
date: 2024-11-09
categories:
  - Kotlin
description: Testing Kotlin flows can be kind of tricky
draft: false
---

After reading [this](https://proandroiddev.com/loading-initial-data-in-launchedeffect-vs-viewmodel-f1747c20ce62) article, I decided to write a post about testing Kotlin flows.
Testing Kotlin flows can be kind of tricky, but it is not impossible. In this post, I will show you how to test Kotlin flows using the `runBlockingTest` function from the `kotlinx-coroutines-test` library.

I started modifying a trivial view model I have:

```
@Open
class CategoriesViewModel(
    getCategoriesUseCase: GetCategoriesUseCase,
    dispatcher: CoroutineDispatcher = Dispatchers.IO
) : ViewModel() {
    val state = getCategoriesUseCase().stateIn(
        scope = viewModelScope,
        started = SharingStarted.WhileSubscribed(5000),
        initialValue = emptyList()
    )
}
```
The problem I was having here is that the view model would never capture the flow emissions, and would also end up getting `emptyList()` as the initial value. This was happening because the `SharingStarted.WhileSubscribed` was not working as expected. I needed to test this, but I didn't know how to do it.

After several iterations, I ended up wit this passing test:

```
@OptIn(ExperimentalCoroutinesApi::class)
    @Test
    fun `state emits initial categories and updates after flow emission`() = runTest {
        // Create a list to collect emissions
        val emissions = mutableListOf<List<CategoryDomainModel>>()

        // Launch a coroutine to collect emissions from the ViewModel state
        val job = launch {
            viewmodel.state.collect { categories ->
                emissions.add(categories) // Capture emissions
            }
        }

        // Wait for the initial emission (emptyList)
        advanceUntilIdle() // This ensures any pending coroutines complete
        assertEquals(listOf(emptyList()), emissions)

        // Simulate emission from the flow and let it propagate
        getCategoriesUseCase.invoke().collect {
            emissions.add(it) // Capture the categories after flow emits
        }

        // Wait for the flow to emit the categories
        advanceUntilIdle()

        // Cancel the job after the collection is done
        job.cancel()

        // Assert that the emission contains the expected data
        assertEquals(listOf(emptyList(), categoryDomainModels), emissions)
    }
```

1. We collect emissions from the `state` flow and assert that the emission contains the expected data.
2. We also simulate an emission from the flow and let it propagate.
3. Finally, we assert that the emission contains the expected data.

I don't think this is strictly a unit test, as we are testing the interaction between the view model and the use case. But it is a good way to test the flow emissions.

Then, I started investigating about a better way to test this, and I run into the  #test channel in the Kotlin Slack. More specifically I was referred to check [this](https://developer.android.com/jetpack/androidx/releases/lifecycle#2.8.0):

```
class MyViewModel(
  // Make Dispatchers.Main the default, rather than Dispatchers.Main.immediate
  viewModelScope: CoroutineScope = Dispatchers.Main + SupervisorJob()
) : ViewModel(viewModelScope) {
  // Use viewModelScope as before, without any code changes
}

// Allows overriding the viewModelScope in a test
fun Test() = runTest {
  val viewModel = MyViewModel(backgroundScope)
}
```

This is a new feature in the `2.8.0` version of the `lifecycle` library. It allows you to override the `viewModelScope` in a test. This is a great feature, as it allows you to test the view model in isolation.
This exactly snippet didn't work by the way, so I changed it to:

```
@Open
class CategoriesViewModel(
    getCategoriesUseCase: GetCategoriesUseCase,
    viewModelScope: CoroutineScope = CoroutineScope(Dispatchers.Main + SupervisorJob()),
) : ViewModel(viewModelScope) {
    val state = getCategoriesUseCase().stateIn(
        scope = viewModelScope,
        started = SharingStarted.WhileSubscribed(5000),
        initialValue = emptyList()
    )
}
```
(Notice that `Dispatchers.Main + SupervisorJob()` return a `CoroutineContext` and not a `CoroutineScope` so I had to wrap the addition in a `CoroutineScope` constructor.)

After the change, I could test the emissions in an easier way:

```
@Test
    fun `Given the use case emits categories_When state is collected_Then it should emit the categories`() =
        runTest {
            everySuspend { getCategoriesUseCase.invoke() } returns flowOf(
                fakeCategoryDomainModelList
            )
            val viewModel = CategoriesViewModel(getCategoriesUseCase, backgroundScope)

            viewModel.state.test {
                skipItems(1)
                asserter.assertEquals(
                    "Expect a list of categories", fakeCategoryDomainModelList, awaitItem()
                )
                cancelAndIgnoreRemainingEvents()
            }
        }
```
