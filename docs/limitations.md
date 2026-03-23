# Limitations

This page explains the practical boundaries of the legacy `mesdeparts.ch-legacy` repository.

The goal is not to diminish the usefulness of the repo. The goal is to make its role clear so public readers know what it does well, what it does not try to be, and what level of maintenance to expect.

## Legacy Status

`mesdeparts.ch-legacy` is an older public codebase for mesdeparts.ch. It preserves a legacy public client and related public code surfaces, but it is not the full current live mesdeparts.ch system.

This means the repository should be read as a legacy public surface, not as the canonical representation of the current live service.

## What To Expect From This Repo

This repository is useful for readers who want:

- the legacy public web client in `web-ui/`
- the optional public worker/proxy code surface in `cloudflare-worker/`
- a self-hostable legacy public surface
- lightweight public documentation for the legacy codebase

## What Not To Expect

This repository does not aim to provide:

- the current proprietary realtime/backend system
- current production behavior parity with the live service
- the current live production setup or operational workflows
- full feature parity with the current mesdeparts.ch service
- private support tooling, debug surfaces, or internal workflows

## Practical Limitations

Some limitations follow directly from the role of this repository:

- The legacy client depends on a public upstream transport data path.
- The legacy architecture differs from the current live product.
- Behavior may differ from what users see on the current live mesdeparts.ch service.
- Public documentation in this repository stays intentionally high-level and bounded.

These limits do not make the repo unusable. They mean it is best treated as legacy public code, self-hostable legacy surface, and public reference material.

## Maintenance And Support Expectations

This repository should be understood as limited-scope and legacy-oriented.

- Maintenance may happen, but it should be understood as limited or best-effort rather than as a promise of ongoing parity with the live service.
- Public readers can use the repo for reference, self-hosting, and legacy maintenance work.
- The repo should not be read as a commitment to reproduce the current mesdeparts.ch production stack in public.

## Non-Goals

This repository is not trying to become:

- the full current mesdeparts.ch production stack
- a public mirror of private operations or infrastructure
- a commitment to live-service parity
- a complete publication of internal tooling or private backend behavior

## Where To Go Next

- [Repo README](../README.md)
- [Self-hosting](self-hosting.md)
- [Architecture](architecture.md)
- [Security policy](../SECURITY.md)
- Current public project repo: https://github.com/tabbycat18/mesdeparts.ch
