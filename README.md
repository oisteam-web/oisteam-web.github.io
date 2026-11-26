# OIS Centralized Hub — v1.0

Split build of the hub. Generated from the single-file `ois-centralized-hub.html`.

## Layout

Keep the folder structure intact when hosting — all asset paths are relative:

```
ois_centralized_hub/
├── index.html
├── css/   (6 files)
└── js/    (7 files)
```

> **Trailing slash matters.** Linking to `.../index.html` or to the folder with a
> trailing slash (`.../`) both work. Linking to the folder without a trailing
> slash or filename will break the asset paths, because the browser treats the
> last segment as a file and looks for `css/` one level too high.

To rename `index.html`, just rename the file — nothing references it by name.

## Structure

### CSS — loaded in `<head>`, in this order

| File | Contents |
|---|---|
| `css/01-tokens.css` | Design tokens: `:root`, light + dark themes, type/radius/motion scales |
| `css/02-app.css` | Reset, layout, shell, sidebar, topbar, cards, forms, unified button tiers |
| `css/03-boot.css` | Boot / splash screen |
| `css/04-banner-generator.css` | Banner Generator host styles (was `<style id="bf-styles">`) |
| `css/05-tips-tricks.css` | Tips & Tricks host styles (was `<style id="tnt-styles">`) |
| `css/06-tips-overrides.css` | Tips & Tricks font/theme overrides |

### JS — plain `<script src>`, no modules

| File | Contents |
|---|---|
| `js/01-boot.js` | Boot sequence animation (loaded right after the boot markup) |
| `js/02-tips-pages.js` | `TNT_PAGES` data + `window.toggleVisibility` |
| `js/03-backfill-tool.js` | `mountBackfillTool()` |
| `js/04-mockup-generator.js` | `mountMockupTool()` |
| `js/05-legacy-banner.js` | Legacy banner template functions |
| `js/06-wcag-banner.js` | WCAG banner template functions |
| `js/07-app.js` | Icon set, tool registry, router, all `TOOL_MODULES`, start-up |

## ⚠️ Load order is significant

**Do not reorder the `<link>` or `<script>` tags in `index.html`, and do not add
`type="module"`.**

- **CSS** — several rules rely on *source order* rather than specificity
  (notably the unified button tiers in `02-app.css`). Reordering the stylesheets
  will change which rules win.
- **JS** — these files share globals (`TNT_PAGES`, `mountMockupTool`,
  `defaultBannerCon`, …) and are **not** ES modules. They rely on classic
  scripts executing top-to-bottom in document order. Adding `type="module"`
  would scope every global and defer execution, breaking `07-app.js`.

The numeric filename prefixes exist to make the required order obvious.

## External dependencies

Loaded from CDN in `index.html` — the page needs network access for these:

- Google Fonts — Plus Jakarta Sans, Geist Mono, Instrument Serif
- SortableJS 1.15.2 (drag-to-reorder on the home cards)

## Editing

This folder is now the thing you deploy. The original single-file
`ois-centralized-hub.html` in the parent directory is a snapshot from before the
split — if you edit it, changes will **not** appear here, and vice versa. Pick
one as canonical (recommended: this folder) to avoid drift.
