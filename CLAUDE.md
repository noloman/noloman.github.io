# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static portfolio website hosted on GitHub Pages and Netlify. It uses Vue.js 2 for
templating, served as a single-page site with no build step.

## Architecture

- **index.html**: Main entry point. Contains all HTML structure, the full stylesheet, an inline
  SVG icon sprite, and the Vue.js application logic.
- **config.js**: Portfolio content (name, skills, backendWorks, works, contacts) exposed via the
  `window.PorfolioConfig` global. Edit this to change content, not `index.html`.
- **No build process**: Files are served directly; deploys to GitHub Pages and Netlify from repo root.

## Content model

`config.js` drives every section:

- `skills[]` — each needs an `area` of `"backend"` or `"mobile"`. The Stack section groups by this
  and renders backend first; anything with an unrecognised `area` is dropped from the page.
- `backendWorks[]` — the "Backend" group in Work. A `null` `link` renders as private: a `Private`
  tag and no outbound link. `type` is shown as a short stack tag, so keep it
  to a few words.
- `works[]` — the "Products" group in Work. Add `image` (+ `imageAlt`) to get a bento card with a phone
  screenshot; optional `summary` is a one-line blurb used on the card instead of `description`. `type` is optional; without it the platform is inferred from
  the link (Play Store, App Store, otherwise Web). Set `ownBackend: true` on anything served by
  the private Spring Boot service; it gets a "Runs on my backend" tag.

## Design system

Simple, light, editorial. Positioning is still backend-first: the Work section lists backend
before products, and the Stack section lists backend before mobile. Keep that order when adding
content.

- Single light theme (no dark mode, no toggle). All colors are custom properties on `:root`: warm
  off-white `--bg`, charcoal text, `--muted` for secondary text, `--line` hairlines.
- No accent color. The only color is the pastel tags (`.tag.private` yellow, `.tag.own` blue).
- Type: **Bricolage Grotesque** (700, tight tracking) for headings, **Geist** for body, **Geist Mono**
  for labels and tags, all from Google Fonts. Swap the heading face via `--display`.
- Layout is a single 960px column. Backend work and the stack are hairline-divided rows (`.row`).
  Products with an `image` render as a bento grid of cards (`.card`, pattern 4/2/2/4 columns of 6);
  products without one fall back to rows. The hero has a faint dot-grid backdrop.
- Icons are an inline `<svg><symbol>` sprite at the top of `<body>`, referenced via `<use>`. There
  is no icon font.
- Scroll-reveal is the `.reveal` class plus one `IntersectionObserver` in `mounted()`; stagger with
  `style="--i: n"`.
- The app landing pages (`anywhereroles/`, `kuokka/`, `stressi/`, `wod-tracker/`) use the same system. Each
  keeps ONE brand accent (blue, orange, indigo, flame) for small markers only; dark appears only inside
  product mock-ups. `wod-tracker/` self-hosts its fonts (`wod-tracker/fonts/`) instead of using the
  Google Fonts link. Their footers carry "Made by Manuel Lorenzo Parejo" and a "← All work" link.
- Quality floor: responsive to 320px, visible `:focus-visible` rings, and
  `prefers-reduced-motion` respected.

## Development

To preview locally, serve the directory (`python3 -m http.server`) and open it — opening
`index.html` over `file://` works too. To modify portfolio content, edit `config.js`.

## Linting

This project uses Trunk for linting:

```bash
trunk check        # Run all linters
trunk fmt          # Format files with prettier
```

Enabled linters: prettier, checkov, renovate, taplo, trufflehog, git-diff-check

## Dependencies

All dependencies are loaded via CDN (no package.json):

- Vue.js 2.x (jsDelivr)
- Google Fonts: Instrument Serif, Geist, Geist Mono

## Security

Run `snyk_code_scan` tool for any new code in Snyk-supported languages. Fix issues and rescan until clean.
