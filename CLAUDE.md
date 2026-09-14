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
- `backendWorks[]` — the "Services" section. A `null` `link` renders as private: hollow status
  marker, a `Private` tag, and no outbound link. `type` is shown as a short stack tag, so keep it
  to a few words.
- `works[]` — the "Products" section. `type` is optional; without it the platform is inferred from
  the link (Play Store, App Store, otherwise Web). Set `ownBackend: true` on anything served by
  the Spring Boot service shown in the hero diagram.

## Design system

Positioning is backend-first: the site leads with backend work and frames mobile as background.
Keep that hierarchy when adding sections.

- Single committed dark theme. There is no light mode and no theme toggle; all colors come from
  the custom properties on `:root`.
- Sodium-amber (`--amber`) is the only accent — spend it on the hero diagram, eyebrows, status
  markers and hover states. `--steel` is structural, not an accent.
- Type: **Archivo** (display and body, variable width axis) and **IBM Plex Mono** (all labels,
  tags, nav and data), both from Google Fonts.
- The hero topology diagram is hand-authored inline SVG on a 400x356 viewBox. Pulse paths carry
  `pathLength="100"` so one dot traverses each path in the same time regardless of its length.
- Icons are an inline `<svg><symbol>` sprite at the top of `<body>`, referenced via `<use>`. There
  is no icon font — do not reintroduce one for a handful of glyphs.
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
- Google Fonts: Archivo, IBM Plex Mono

## Security

Run `snyk_code_scan` tool for any new code in Snyk-supported languages. Fix issues and rescan until clean.
