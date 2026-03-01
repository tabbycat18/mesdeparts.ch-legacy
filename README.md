# MesDeparts Legacy Client

This repository contains the legacy client only.
The real-time backend + new client are not part of this repo.

Static, dependency-free front-end for mesdeparts.ch. UI files live in `web-ui/` and are served as-is (no build step), with ES modules and versioned filenames to keep long-lived caches safe to bust.

Legacy note:
- The active RT backend + new client live in the proprietary `mesdeparts.ch-rt` repository.
- This repo is the legacy/archive flow and does not own the active RT backend or worker.

## Features
- Stop search with suggestions and favorites (stored locally; no account).
- Two bus views: by line (balanced by destination) or chronological; trains are always chronological.
- Filters for platform/line/train service plus “My favorites” mode.
- Direct mode (public API) for one-off checks and lightweight usage.
- Self-hosted SBB clock + digital clock; auto-refresh every 10–20 s (~3 h horizon).
- Multilingual (FR/DE/IT/EN), deep links via `?stationName=...&stationId=...`, installable PWA shell (API stays online).

## Entry points
- `web-ui/index.html`: single-board experience with language switcher, favorites, filters, and the SBB clock iframe (`clock/`).
- `web-ui/dual-board.html`: two boards side by side for kiosks/embeds, with separate station pickers and view/filter controls.
- `web-ui/manifest.webmanifest` + `web-ui/service-worker.js`: PWA shell; caches static assets, leaves API requests online-only.

## Architecture (versioned files)
- `main.v*.js`: boot; reads URL/localStorage defaults (`stationName`/`stationId`, language), wires event handlers, and starts refresh + countdown loops.
- `state.v*.js`: shared config/constants (refresh cadence, horizons, view modes, thresholds) and mutable `appState`.
- `logic.v*.js`: transport.opendata.ch client (or Cloudflare proxy when board mode is on), station resolve/nearby search, journey details fallback, delay/remark computation, grouping/sorting, and network detection for line colors.
- `ui.v*.js`: DOM rendering of the board, clocks, station search with suggestions/nearby, filters (line/platform/train service), favorites popovers, view toggle, auto-fit watcher, and embed state publication.
- `i18n.v*.js`: minimal translations (FR/DE/IT/EN) + language persistence.
- `favourites.v*.js`: localStorage (`md_favorites_v1`) helpers; no backend.
- `infoBTN.v*.js`: info/help/realtime/credits overlay shown from the “i” button.
- `style.v*.css`: board + dual layout styles, network color tokens, popovers.
- `clock/`: self-hosted SBB clock assets (Apache 2.0); cached by the service worker for offline/instant loads.

## Data sources (legacy)
- Primary data source is the public API `https://transport.opendata.ch/v1`:
  - `/stationboard` for departures
  - `/locations` for stop search + geolocation
  - `/connections` and `/journey` for journey details fallback
- Board mode uses a proxy base (`BOARD_API_BASE`) that comes from `window.__MD_API_BASE__`
  in `web-ui/index.html` and `web-ui/dual-board.html`.
- In this repo, `web-ui/index.html` and `web-ui/dual-board.html` now **do not** set
  `window.__MD_API_BASE__` by default, so legacy UI uses `transport.opendata.ch/v1` directly.
  If you want a legacy cache/proxy, set `window.__MD_API_BASE__` to your own legacy worker URL.
- Rate limit note: `transport.opendata.ch` inherits the upstream `search.ch` per-IP quotas.
  Using direct mode means **each user’s IP** gets its own daily quota. Using a shared proxy
  collapses all users onto **one IP** and makes the quota easier to hit.

## Why direct-only (legacy)
The legacy UI now defaults to **direct mode only** for clarity and fairness:
- Direct mode uses each user’s IP quota instead of a shared proxy IP.
- A shared proxy collapses all users onto one quota and is easier to exhaust.
- This repo is legacy/archival; avoiding a shared proxy reduces operational risk.

If you still want a legacy proxy, set `window.__MD_API_BASE__` to your own worker URL and
re-enable the board mode UI (see “Behavior/UX notes”).

## Data & refresh flow
- Default station is `Lausanne, motte` (id `8592082`); query params or stored values override it. Deep links use `?stationName=...&stationId=...`.
- Default bus view is **chronological** (“Display by min”).
- API base selection:
  - Direct mode always uses `https://transport.opendata.ch/v1`.
  - Board mode uses `BOARD_API_BASE` from `window.__MD_API_BASE__` (see “Data sources” above).
- `refreshDepartures` calls `/stationboard` (limit tuned to UI), rebuilds grouped rows (3 h horizon, train/bus split, line/platform filters, favorites-only mode, train service filters) and renders. Countdown column updates every 5 s from cached data.
- Stale board guard: if the stationboard looks “empty because everything is already in the past”, the UI triggers a cache-bypassing refetch at most once per minute per station to recover from sticky caches and will fall back to a direct (non-proxy) fetch if the board stays empty for ~1 minute.
- Station search uses `/locations` (with debounced caching) and an optional geolocation helper via `/locations?x/y`. Journey details overlay falls back to `/journey` or `/connections` when `passList` is missing.
- Embeds: pages add a `dual-embed` class when framed; `publishEmbedState` exposes current board state to the parent.

## Running locally
- Static server only; no bundler needed:
  ```sh
  cd web-ui
  python3 -m http.server 8000
  ```
  Then open http://localhost:8000.
- Tests (Node built-in): `npm test` from `web-ui/` (checks key helpers in `logic.*.js`). `package.json` has no deps.

## Versioning & deploy notes
- JS/CSS filenames carry a version tag (`*.vYYYY-MM-DD-N.*`). When you bump assets, update references in `index.html`, `dual-board.html`, and the `CORE_ASSETS`/`LAZY_ASSETS` lists inside `service-worker.js`, plus the visible version tags in the HTML headers.
- `service-worker.js` derives its cache name from the asset list; keep the list in sync with the actual files so cache busting remains automatic.
- The UI is fully static: host this folder on any static host (Netlify/Vercel/S3/nginx/Apache). A `.htaccess` file is not used here; rely on versioned filenames for cache control.

## Edge cache (Cloudflare Worker) — optional
- What it does: proxy in front of `transport.opendata.ch` with short TTLs to reduce upstream calls when many users watch the same stop.
- Worker files in this repo: `cloudflare-worker/worker.js`, `cloudflare-worker/wrangler.toml`.
- Point the UI: set `window.__MD_API_BASE__` to **your legacy worker URL** (not the RT backend).
  Otherwise it calls the public API directly.
- Board mode uses the proxy; direct mode calls the public API and auto-reverts to board mode after ~2 minutes unless the user keeps it on.

## Behavior/UX notes
- Filters/view changes are client-side only.
- Legacy UI currently **hides the board mode toggle** in the HTML (commented out). The logic
  remains in JS; re-enable by uncommenting the `board-mode-toggle*` elements in
  `web-ui/index.html` and `web-ui/dual-board.html`.
- Legacy UI now **forces direct mode** on load (ignores stored or URL `mode=` parameters).
- Language, favorites, board mode preference, and last station are stored in `localStorage`; nothing leaves the browser.
- The service worker pre-caches the shell and serves navigations cache-first with background revalidation; API calls always hit the network.
