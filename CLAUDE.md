# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

MyKK Dashboard is a single-file, zero-build, vanilla JavaScript dashboard application — a customizable browser start page. The entire app lives in `index.html` (~6,900 lines of HTML, CSS, and JavaScript). There is no package.json, no bundler, and no framework. External libraries are loaded from CDNs.

## Development

**Local dev server** (any static HTTP server works):
```bash
python3 -m http.server 8000
# or
npx serve .
```

**Build for Cloudflare Pages:**
```bash
bash build.sh
```
This copies `index.html` and `_headers` to `dist/`, optionally injecting Firebase config from environment variables (`FIREBASE_API_KEY`, `FIREBASE_AUTH_DOMAIN`, `FIREBASE_PROJECT_ID`).

There are no tests, linters, or build tools beyond `build.sh`.

## Architecture

### Single-File Structure (`index.html`)

All code is in one file, organized by feature in this order:
1. **CSS** — Variables, glass morphism theme, flexbox layout with grid-snapped sizing, responsive breakpoints (900px tablet, 500px mobile)
2. **HTML** — `<main class="dashboard">` flex container with widget sections
3. **JavaScript** — State management, Firebase/auth, settings UI, widget renderers, resize/drag systems, event handlers, utilities

### Layout System

- Flexbox wrapping layout (`display: flex; flex-wrap: wrap`) on `.dashboard`
- Widget widths map to a 12-column grid: `calc((cols/12)*100% - 16px)`, heights snap to 80px row increments
- Sizing stored as `settings.widgetSizes[widgetId] = { cols, rows }` — same data model as before
- Widgets are resizable by dragging right/bottom/corner edges (snaps to nearest col/row)
- Widgets are reorderable via drag handles (6-dot grip icon, GitHub pinned-repos style)
- Structural wrappers (`.top-row`, `.left-column`, etc.) use `display: contents`
- Auto-height widgets: date, search, actions, time, shortcuts (favorites)
- Minimum widget size: 100px × 100px
- Breakpoints: full layout (desktop) → 100% width (≤900px) → stacked (≤500px)

### State Management

- Global `settings` object holds all user preferences
- `defaultSettings` defines the schema and defaults
- Persisted to **localStorage** (primary) with optional **Firebase Firestore** cloud sync
- Settings changes trigger re-renders of affected widgets

### Key Widgets

Date/Clock, Search Bar (7 engines), Weather (OpenWeatherMap + Leaflet radar map), Mini Calendar, Favorites/Bookmarks (drag-and-drop reorder), Donetick Chores (extension-gated), iFrame Widgets, Toolbar (fullscreen, webcam, paint canvas, notepad)

### Firebase Integration (Optional)

- Google Sign-In popup flow → Firestore per-user documents
- Config injected at build time or entered in settings
- Bi-directional sync with manual push/pull and auto-sync

### External Dependencies (all CDN-loaded)

- Firebase SDK 10.12.0, Leaflet 1.9.4, jsPDF 2.5.1, Inter font (Google Fonts)
- APIs: OpenWeatherMap, Google Favicons, CARTO/OSM map tiles

## Conventions

- **CSS**: Variables (`--glass-bg`, `--text-primary`, etc.), glass morphism with `backdrop-filter`, kebab-case classes
- **JS**: camelCase functions, direct DOM manipulation with `innerHTML` (use `escapeHtml()` for user content), no async patterns except Firebase operations
- **HTML IDs**: kebab-case with widget suffix (e.g., `date-widget`, `weather-widget`)
- **Accessibility**: ARIA labels, roles, live regions, focus management in modals — maintain these patterns when adding UI
- **Drag-and-drop**: GitHub pinned-repos style section reordering with touch support

## Deployment

Primary target is **Cloudflare Pages** (`build.sh` as build command, `dist` as output directory). Also works on GitHub Pages or any static host.
