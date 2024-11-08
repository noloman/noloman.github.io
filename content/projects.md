+++
title = "Projects"
slug = "projects"
+++

# Projects

Welcome to my projects page! Here you will find a collection of my work, including personal projects, collaborations, and contributions to open-source projects.

## Project 1: Remoti: web, iOS and Android

**Description:** Remote work is becoming increasingly popular in today’s digital age. With the advent of advanced technology and the internet, more and more people are looking for opportunities to work from home or from any location of their choice. This growing trend is fueled by the desire for a better work-life balance, the ability to avoid long commutes, and the flexibility to work in a comfortable environment.

Introducing Remoti, the ultimate solution for finding remote work opportunities directly from your iPhone. With Remoti, you can search for remote jobs in various categories and apply for them with ease. Whether you are looking for opportunities in software development, QA, product management, legal, design, or any other field, Remoti has got you covered. The app offers a wide range of job categories to cater to different professional backgrounds and skill sets.

One of the key features of Remoti is its robust filtering system. You can filter job listings based on several criteria to find the perfect match for your needs. For instance, you can filter by company name to find jobs from specific employers you are interested in. You can also filter by job category to narrow down your search to your field of expertise. Additionally, you can use tags associated with the job to find positions that match your specific skills and interests. Furthermore, you can filter by contract type, whether you are looking for freelance, full-time, or part-time positions. This flexibility allows you to tailor your job search according to your preferences and availability. Moreover, you can even filter by location requirements to find remote jobs that are open to candidates from specific regions or countries.

Remoti also offers a convenient feature that allows you to save interesting job listings to your phone. This way, you can easily access and review them at any time, ensuring you don’t miss out on any promising opportunities. You can bookmark jobs that catch your eye and come back to them later when you are ready to apply.

One of the standout features of Remoti is that you can enjoy all these functionalities without the need to register or log in. There is no account creation required, making the process seamless and hassle-free. You can start using Remoti immediately, without any barriers to entry.

Moreover, Remoti is designed to be user-friendly and accessible to everyone. You don’t need to be an engineer or a developer to navigate the app. It caters to all types of professionals looking for flexible job opportunities. Whether you are a seasoned professional or just starting your career, Remoti provides a platform to find the remote work that suits your needs.

In conclusion, Remoti is the go-to app for anyone seeking remote work opportunities. With its extensive job categories, powerful filtering options, and user-friendly interface, it simplifies the process of finding and applying for remote jobs. Start your journey towards a flexible and fulfilling work life with Remoti today!

**Technologies Used:** The web app is completely written using pure Go without any framework such as GORM, Fiber, Echo, etc., and it uses Go `html/template` for the frontend. The Go binary file is generated using a custom Dockerfile, and for data storage, it is using a PostgreSQL database. Using Docker Compose, I spin up the containers that talk to each other and it is deployed in a Digital Ocean droplet.

About the iOS app, it's written entirely in Swift (of course) and SwiftUI and it relies heavily on Core Data.
The Android app is the least mature of the three projects, and it just has jobs from two remote job boards, unlike the web and iOS app, that fetch content from 5 different job boards.

- **Link:** [Live Demo](https://remotiapp.com)
- **Link:** [App Store link](https://apps.apple.com/us/app/remoti-remote-work/id1567902235)
- **Link:** [Play Store link](https://play.google.com/store/apps/details?id=me.manulorenzo.remoti&pcampaignid=web_share)

---

Feel free to explore the projects and reach out if you have any questions or feedback!
