# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static portfolio website hosted on GitHub Pages and Netlify. It uses Vue.js 2 with Vuetify 2 for the UI framework, served as a single-page application with no build step.

## Architecture

- **index.html**: Main entry point containing all HTML structure and inline Vue.js application logic
- **config.js**: Configuration file containing portfolio content (name, skills, works, contacts) via `window.PorfolioConfig` global object
- **No build process**: Files are served directly; deploys to GitHub Pages and Netlify from repo root

## Theme System

The site has dark/light theme toggle functionality. Theme preference is persisted in localStorage under the `theme` key. When modifying styles, ensure both themes are supported via Vuetify's theme system.

## Content Notes

The `works` array in `config.js` contains placeholder lorem ipsum descriptions that should be replaced with real project descriptions.

## Development

To preview locally, open `index.html` in a browser. No server or build commands are required.

To modify portfolio content, edit `config.js`.

## Linting

This project uses Trunk for linting:

```bash
trunk check        # Run all linters
trunk fmt          # Format files with prettier
```

Enabled linters: prettier, checkov, renovate, taplo, trufflehog, git-diff-check

## Dependencies

All dependencies are loaded via CDN (no package.json):
- Vue.js 2.x
- Vuetify 2.x
- Material Design Icons
- AOS (Animate On Scroll)

## Security

Run `snyk_code_scan` tool for any new code in Snyk-supported languages. Fix issues and rescan until clean.
