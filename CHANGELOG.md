# Changelog

All notable changes to the MyKK Dashboard.

## September 2, 2026

### Fixed
- **A lapsed subscription no longer reads as the customer's mistake.** When the Worker answers `403` for a lapsed subscriber, the ICS widget said *"Unable to load calendar feeds. Check the ICS URLs in Settings"* — blaming their configuration for our billing state and sending them to repair feeds that were never broken. Entitlement failures and load failures are now distinguished at the source and each says what is actually true; the lapsed message points at the subscription. The RSS and stocks widgets never carried the wrong blame, but could not explain a lapse either, and now can.
- **Cached calendar events are no longer served as though they were live.** The stale-cache path applied no age limit and gave no signal, so a calendar that stopped syncing weeks ago rendered exactly like a working one. Cached events are still shown — they are better than an empty widget — but now above a note saying they are saved rather than current, and saying so differently when the cause is a lapsed subscription.

### Security
- **A slow extension is no longer read as no extension.** The dashboard decided whether to use the extension's authenticated proxy by checking `window.mykkExtension`, which is populated from a handshake both repos describe as immediate and which is not: `content.js` awaits a round trip to an MV3 service worker — one Chrome terminates when idle, and which may revalidate the subscription over the network — before it writes the attribute. On a cold worker that outran the 2-second reveal poll, so `revealDashboard()` ran with the handshake unresolved and every widget took the pre-extension path: RSS feed URLs to `api.rss2json.com`, calendar URLs with their secret tokens to `corsproxy.io` and `allorigins.win`, and stock symbols plus the customer's own Marketstack key to the legacy endpoint — for someone who had the extension installed and was paying for the proxy that exists to prevent precisely that. The data path now waits for a real answer before choosing, with a long deadline on browsers that have seen the extension before and a short one on browsers that have not. The reveal keeps its own 2-second budget, because that is about paint.
- **Private calendar (ICS) URLs no longer leak when the extension answers with an error — which the entry below claimed, and the code did not do.** `fetchIcsText` routed through the extension first, but a `catch` let *any* extension failure fall through to the same `corsproxy.io` and `allorigins.win` calls the July 18 change was written to retire. That was invisible while the extension always answered. Since the Worker cutover it does not: `/api/data/ics` returns 403 for a lapsed subscriber, and every one of them took the fallback and handed a feed URL — often carrying a secret token, revocable only by regenerating the feed — to two third parties. A proxy-capable extension's answer is now final, success or failure, and the public proxies serve the no-extension path only. This is the shape the RSS and stocks widgets already used; ICS was the one written the other way round.

## July 18, 2026

### Security
- **Pro data now flows through the extension's authenticated proxy.** Stocks, RSS feeds, and calendar (ICS) feeds are fetched via the MyKK extension (v1.0.6+), which calls the authenticated `api.mykk.us/api/data/*` endpoints with the signed session token — the token never touches this page. This makes Pro data a real server-side boundary rather than a client-side toggle. Older/absent extensions fall back to the previous direct paths so nothing breaks during rollout.
- **Private calendar (ICS) URLs no longer leak to third parties.** ICS feeds are fetched server-side by the extension/Worker instead of via `corsproxy.io` / `allorigins.win`, so feed URLs that embed secret tokens stay private. RSS no longer depends on the third-party `rss2json` service when the extension is present.
- **Added a Content-Security-Policy** (`_headers`): `script-src` is restricted to the known CDNs (blocking injected external scripts), plus `object-src 'none'`, `base-uri 'self'`, and `frame-ancestors 'self'`. `connect-src`/`frame-src` remain permissive to preserve user-configured integrations (Donetick, custom iframes/feeds).

## July 16, 2026

### Features
- Calendar Feeds (ICS Sync) — subscribe to multiple ICS/iCal feeds (Google, Outlook, Apple, Nextcloud, `webcal://`); merged, color-coded Upcoming Events widget with recurring and all-day event support
- PWA / Offline Mode — installable web app manifest and service worker for instant loads and full offline support
- Keyboard shortcut: `/` focuses the search bar
- Dashboard pages can be reordered by dragging their tabs
- Custom CSS injection — apply your own stylesheet live from Settings → Appearance
- Customizable keyboard shortcuts + arrow-key navigation between bookmarks (Settings → Behavior & Shortcuts)
- Screensaver — idle full-screen drifting clock for kiosk/always-on displays
- Option to always show widget controls instead of only on hover
- High Contrast Mode — dedicated accessibility theme (black surfaces, white text/borders, strong focus outline)
- Docs: how to add custom bookmark icons

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
