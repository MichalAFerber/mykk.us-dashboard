# Changelog

All notable changes to the MyKK Dashboard.

## March 16, 2026

### Features
- Weather radar fullscreen with current conditions and Rain/Clouds/Temp/Wind layer toggles
- Notepad modes: Plain Text, Rich Text, and Markdown with per-mode toolbars and live preview
- Rich Text export as .rtf file
- Stock quotes widget (Yahoo Finance default, optional Marketstack API)
- RSS feed widget
- Ambient sounds mixer (rain, fireplace, ocean, forest, and more)
- Daily Focus and To-Do List widgets
- Loading spinner with user's background during initial load
- Custom favicon URL setting
- Theme Toggle button in toolbar settings
- Light/dark theme available to free users
- Pro badges on settings TOC
- Docs link in settings replacing inline FAQ
- Dashboard pages system
- Greeting and Quotes widgets

### Bug Fixes
- Fixed extension communication (CSP fix: CustomEvent instead of inline script)
- Fixed intermittent Pro widget detection with polling fallback
- Fixed widget positions not persisting for Pro widgets
- Fixed widget drag positions not saving on first drag
- Fixed stock widget API URL
- Fixed light theme visibility for settings toggles
- Fixed large gaps between integration toggles in settings
- Stocks preserve user's CSV watchlist order

### Layout
- Default widget size: 6 columns (two per row on desktop)
- Dense grid packing fills gaps
- Tablet view: single-column matching mobile
- Fullscreen widgets respect light/dark theme

## March 15, 2026

### Features
- Stock quotes, RSS feeds, ambient sounds, and dashboard pages
- Extension status detection via data attribute

### Bug Fixes
- Fixed extension communication CSP issue

## March 14, 2026

### Features
- Custom MyKK dashboard logo and favicon
- Transparent PNG icons (16, 48, 128px)

## March 13, 2026

### Features
- Repository renamed to mykk.us-dashboard
- Google Sign-In authentication for Pro subscriptions
- Chrome extension integration

## March 9, 2026

### Features
- Overhauled settings UI with gated integrations
- Updated widget size defaults

## March 7-8, 2026

### Features
- Explicit grid placement with free-form drag-and-drop
- Removed spacer widget system
- Polished widget layout and search UX

### Bug Fixes
- Fixed search dropdown, calendar rows, popup z-index

## February 12-13, 2026

### Features
- Accessibility: ARIA labels, roles, live regions, focus management, screen reader support
- Touch-friendly UI with larger tap targets and swipe gestures
- Donetick chore widget with extension gating
- Section drag-and-drop rewritten (GitHub pinned-repos style)
- Full documentation, MIT license, expanded FAQ

### Bug Fixes
- Fixed grid layout overflow
- Fixed Firebase auth persistence
- Fixed cloud sync button visibility

## v1.0.0 — Initial Release

### Features
- Single HTML file dashboard (~400 KB)
- 12-column CSS grid with resizable widgets
- Multi-engine search (7 engines)
- Drag-and-drop bookmarks with auto-detected favicons
- Mini calendar, clock & date
- Notepad, paint canvas, webcam/selfie
- iFrame widget system
- Weather with OpenWeatherMap (current, forecast, radar, animations)
- Google Sign-In cloud sync via Firebase
- Gradient themes, custom backgrounds
- Mobile-friendly responsive layout
- Export/import settings
- Works 100% offline
