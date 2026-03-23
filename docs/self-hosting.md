# Self-Hosting

This page describes the safe public self-hosting surface of the legacy `mesdeparts.ch-legacy` repository.

It is intended for readers who want to run the legacy public client themselves. It does not describe the full current live system.

## What Can Be Self-Hosted

This repository exposes two public legacy components:

- `web-ui/` - a static legacy web client
- `cloudflare-worker/` - an optional worker/proxy code surface

The static web UI is the main self-hosting target. The worker layer is optional and should be treated as an additional legacy component, not a requirement.

## Typical Hosting Shape

The usual legacy hosting shape is simple:

1. Publish the contents of `web-ui/` on a static host.
2. Serve the UI as a browser-based client through `index.html` or `dual-board.html`.
3. Optionally add the worker layer if you want your own legacy proxy path in front of the public upstream data source.

For many readers, the static UI alone is the right starting point.

By default, the legacy client uses the public `transport.opendata.ch/v1` API path. The optional worker is only relevant if you want your own legacy-compatible proxy layer.

## What This Repo Does Not Provide

This repository does not provide:

- the current proprietary realtime/backend system
- the current live production setup
- private infrastructure or operations
- private secrets or configuration from the live service

Self-hosting this repository should be understood as running the legacy public codebase only.

## Basic Setup Outline

### Static UI

The simplest setup is to serve `web-ui/` as static files.

- Main entry page: `web-ui/index.html`
- Alternative dual-board page: `web-ui/dual-board.html`
- Static assets, manifest, and service worker are included in the same folder
- The included client is a static site, so no application build step is required just to serve it

For local inspection, a basic static server is enough. One simple example is:

```sh
cd web-ui
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

### Optional Worker

If you want to experiment with the optional worker layer, treat it as a separate legacy deployment surface.

- Worker source lives in `cloudflare-worker/`
- It is not required to run the static UI
- It should be configured and deployed only for your own self-hosted legacy environment

The checked-in worker files should be read as public legacy code, not as a supported copy of the current live deployment setup.

## Configuration Surface

Only a small public configuration surface should be assumed here.

### Static Hosting

- Host the contents of `web-ui/` together so the HTML, JavaScript, CSS, manifest, icons, and service worker remain aligned
- The simplest deployment shape is to publish that folder as a static site
- Keep the site layout simple and root-style if possible, because the checked-in service worker registers from `/service-worker.js`
- The repository includes example cache-header hints in `web-ui/.htaccess`, but exact hosting policy remains your choice in your own environment

### Entry Pages

- `index.html` is the main single-board entry
- `dual-board.html` is the alternate dual-board entry

### Public Client API Base Override

The frontend code includes a public client-side API base override through `window.__MD_API_BASE__`, which can be set before the application scripts load.

This is the public customization point for readers who want the legacy UI to call their own legacy-compatible endpoint instead of using its default public-facing data path.

If you do not need that customization, start with the default static setup first.

### Lightweight Verification

The `web-ui/` folder includes a minimal Node-based test command:

```sh
cd web-ui
npm test
```

This is optional, but it is a reasonable first check before publishing your own legacy copy.

## Limitations And Expectations

This self-hosting surface is legacy and limited by design.

- It is not the same as the current live MesDeparts service
- It is best suited for legacy use, experimentation, archival purposes, and limited self-hosting
- Public documentation here stays intentionally high-level and does not act as a private operations handbook

For the broader legacy boundary and support expectations, see [limitations.md](limitations.md).

## Where To Go Next

- [Repo README](../README.md)
- [Limitations](limitations.md)
- [Architecture](architecture.md)
- [Security policy](../SECURITY.md)
- Current public project repo: https://github.com/tabbycat18/mesdeparts.ch
