# CLAUDE.md — AI Assistant Guide for actions.taio.app

## Project Overview

This is the **Taio Actions directory** — a static documentation and distribution site for sharing automations/workflows for the [Taio](https://taio.app) markdown editor (macOS/iOS/iPadOS). Built with [Docsify](https://docsify.js.org/) and hosted on GitHub Pages.

- **Live site:** https://actions.taio.app
- **Hosting:** GitHub Pages from the `/docs` directory
- **Domain:** Configured via `docs/CNAME`

## Repository Structure

```
/
├── README.md                  # Repo-level README
├── CLAUDE.md                  # This file
└── docs/                      # GitHub Pages root (all site content lives here)
    ├── index.html             # Docsify entry point & configuration
    ├── CNAME                  # Custom domain config
    ├── .nojekyll              # Disables Jekyll processing
    ├── README.md              # English landing page
    ├── _navbar.md             # Top navbar (language selector)
    ├── _sidebar.md            # Sidebar navigation
    ├── builtin.md             # Built-in actions docs
    ├── convert.md             # Conversion actions docs
    ├── format.md              # Formatting actions docs
    ├── preview.md             # Preview actions docs
    ├── search.md              # Search actions docs
    ├── service.md             # Service integration docs
    ├── utility.md             # Utility actions docs
    ├── _asset/                # Static assets (favicon)
    ├── _js/
    │   ├── index.js           # Docsify config + custom action import logic
    │   └── qrious.js          # QR code generation library
    ├── _style/
    │   ├── light.css          # Base light theme
    │   ├── custom.css         # Custom component styles
    │   └── dark.css           # Dark mode overrides (prefers-color-scheme)
    ├── actions/               # English action JSON definitions
    │   ├── builtin/
    │   ├── convert/
    │   ├── service/
    │   └── utility/
    └── cn/                    # Chinese localization (mirrors English structure)
        ├── README.md
        ├── _navbar.md
        ├── _sidebar.md
        ├── *.md               # Chinese docs for each category
        └── actions/           # Chinese action JSON definitions
```

## Technology Stack

- **Static site generator:** Docsify (client-side markdown rendering, no build step)
- **CDN:** fastly.jsdelivr.net for Docsify and plugins
- **Plugins:** docsify-edit-on-github, docsify-pagination, built-in search
- **Fonts:** Google Fonts (Roboto Mono, Source Sans Pro)
- **No package.json, no build process, no CI/CD pipeline**

## Development Workflow

### Running Locally

Since this is a purely static Docsify site, serve from the `docs/` directory:

```bash
# With Python
cd docs && python3 -m http.server 3000

# With Node (if docsify-cli is installed)
docsify serve docs
```

### Deploying

Push to `master` — GitHub Pages automatically serves from `/docs`.

### No Build or Test Steps

There is no build system, linter, or test suite. Changes to files in `docs/` are deployed as-is.

## Content Conventions

### Adding a New Action

1. **Create the action JSON file** in the appropriate category folder under `docs/actions/<category>/` (and `docs/cn/actions/<category>/` for Chinese)
2. **Add a documentation entry** in the corresponding `docs/<category>.md` (and `docs/cn/<category>.md`) markdown file
3. **Update the sidebar** in `docs/_sidebar.md` (and `docs/cn/_sidebar.md`) if adding a new category

### Action JSON Format

Action files are Taio workflow definitions in JSON. They are imported into Taio via the custom `taio://actions?action=import` URL scheme. The site's JavaScript (`_js/index.js`) auto-detects links to `.json` files and renders "Get Actions" import buttons.

### Bilingual Content

The site supports English (root) and Chinese (`/cn/`). Both share the same structure. Some actions are locale-specific (e.g., `pangu.json` is Chinese-only).

### Linking to Action Files

In documentation markdown files, link to action JSON files using relative paths. The custom JS in `_js/index.js` intercepts these links and provides:
- A "Get Actions" button
- QR code for mobile import (on desktop)
- Direct `taio://` URL scheme opening (on Apple devices)

External action files hosted on GitHub are also supported.

## Key Files to Know

| File | Purpose |
|------|---------|
| `docs/index.html` | Docsify config, CDN imports, theme loading |
| `docs/_js/index.js` | Core logic: device detection, action import buttons, QR codes, search config |
| `docs/_style/custom.css` | Custom button styles, typography, responsive layout |
| `docs/_style/dark.css` | Dark mode via `prefers-color-scheme` media query |
| `docs/_sidebar.md` | Navigation structure for English site |
| `docs/cn/_sidebar.md` | Navigation structure for Chinese site |

## Style & Design Notes

- **Theme color:** `#007AFF` (Apple blue)
- **Dark mode:** Automatic via CSS `prefers-color-scheme: dark`
- **Mobile-first:** Touch-friendly buttons, responsive layout
- **No emojis in code/docs** unless already present in existing content

## Git Conventions

- **Main branch:** `master`
- **Commit style:** Short imperative descriptions (e.g., "Add actions for removing extra spaces", "Fix search bar text color for dark mode")
- **No conventional commits prefix** — plain English descriptions
