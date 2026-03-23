# Architecture

This page gives a high-level public architecture view of the legacy `mesdeparts.ch-legacy` repository.

It describes the legacy public codebase only, not the full current MesDeparts live system.

## Overview

At a high level, this repository packages an older browser-based MesDeparts client, an optional worker/proxy surface, and a small set of public documentation pages.

The repository should be read as a legacy public architecture snapshot: useful for understanding the older public codebase and for limited self-hosting, but not as a full picture of the current live service.

## Main Parts Of The Repo

### `web-ui/`

`web-ui/` contains the legacy static frontend.

At a public level, this includes:

- browser entry pages such as `index.html` and `dual-board.html`
- static frontend assets such as JavaScript, CSS, icons, manifest, and service worker files
- lightweight test files for parts of the frontend code

### `cloudflare-worker/`

`cloudflare-worker/` contains an optional public worker/proxy code surface.

At a high level, it is a separate legacy component that can sit between the browser client and an upstream public transport data path in a self-hosted setup.

### `docs/`

`docs/` contains the public documentation layer for the legacy repository, including self-hosting, limitations, and architecture pages.

## High-Level Data Flow

The legacy architecture is conceptually simple:

1. A browser loads the legacy frontend from `web-ui/`.
2. The frontend requests public transport data through its public-facing data path.
3. In a simple setup, the frontend talks directly to the upstream public data source.
4. In a self-hosted legacy setup, an optional worker/proxy layer may sit in front of that upstream source.
5. The browser UI renders the resulting departure information for the user.

This description is intentionally high-level. It explains the public shape of the legacy repository without documenting internal heuristics, hidden behavior, or private runtime details.

## What This Repo Represents

This repository represents the public legacy code surface for MesDeparts:

- an older static web client
- an optional worker/proxy layer
- a small public documentation layer

It is not the authoritative public home for the current live project. For that role, use the main `mesdeparts.ch` repository.

## What This Repo Does Not Contain

This repository does not contain:

- the current proprietary realtime/backend system
- the current production operations or deployment setup
- private infrastructure, secrets, or internal tooling
- full parity with the current live MesDeparts service

## Relationship To The Current Project

The current public-facing project home is the main `mesdeparts.ch` repository.

This legacy repository remains useful for readers who want the older public codebase, a legacy self-hosting surface, or a public historical reference, but it should not be treated as the complete current project.

## Where To Go Next

- [Repo README](../README.md)
- [Self-hosting](self-hosting.md)
- [Limitations](limitations.md)
- [Security policy](../SECURITY.md)
- Current public project repo: https://github.com/tabbycat18/mesdeparts.ch
