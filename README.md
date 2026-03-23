# MesDeparts Legacy

Legacy public codebase for the older MesDeparts web client.

## Legacy Status

This repository preserves the legacy MesDeparts client and related public code. It is not the main current public surface for the project, and it is not the full current production system behind the live service.

## What This Repository Contains

- A static legacy web UI under `web-ui/`
- An optional Cloudflare Worker code surface under `cloudflare-worker/`
- Public repository files such as the security policy and trademark notice

This repo is the historical and self-hostable public code surface for the legacy version of MesDeparts.

## What This Repository Does Not Contain

- The current proprietary realtime/backend system
- The current production operational setup
- Private deployment or operations logic
- Secrets, private configuration, or debug surfaces

## Who This Repo Is For

- Self-hosters who want to inspect or run the legacy public client
- Maintainers making limited public-facing changes
- Archivists or technical readers who want the older public codebase

## Current Status

This is a legacy repository. It should be read as historical or maintenance-scope code, not as the canonical representation of the current live MesDeparts service.

Support expectations should stay limited: this repo remains useful as a public reference and legacy code surface, but it should not be treated as the current product stack.

## Quick Start

- To inspect or run the legacy UI locally, serve `web-ui/` as static files and open `index.html` or `dual-board.html`.
- The worker code in `cloudflare-worker/` is separate and optional.
- If you want the current public project overview instead of the legacy codebase, use the main `mesdeparts.ch` repository linked below.

## Repo Layout

- `web-ui/` - legacy static frontend
- `cloudflare-worker/` - optional worker/proxy code for the legacy surface
- `SECURITY.md` - private security reporting instructions
- `TRADEMARK.md` - trademark and branding notice

## Where To Go Next

- [Docs index](docs/README.md)
- [Self-hosting](docs/self-hosting.md)
- [Limitations](docs/limitations.md)
- [Architecture](docs/architecture.md)
- [Security policy](SECURITY.md)
- [Trademark notice](TRADEMARK.md)
- Current public project repo: https://github.com/tabbycat18/mesdeparts.ch
