# AirLink

AirLink is a documentation-first product project for pilots.

The current project phase and work boundaries are defined in `ITERATION.md`.

## Current Working Model

- GitHub is the canonical workspace for accepted documentation and future code.
- Existing Google Drive, ParaFlight, and local historical materials are legacy sources, not current specifications.
- `ITERATION.md` defines the active work cycle and its boundaries.
- `docs/product/CurrentState.md` records accepted state outside the active iteration.
- Product drafts remain under `docs/product/wip/` until explicitly promoted.
- `_working/` is a local disposable workspace and is not committed.

## Documentation

- [Documentation map](docs/README.md)
- [Product documentation](docs/product/README.md)
- [Current state](docs/product/CurrentState.md)
- [Active iteration](ITERATION.md)
- [AI agent policy](AGENTS.md)
- [Local execution overlay](CLAUDE.md)

## Review Workflow

Every pull request starts as Draft. The project owner and ChatGPT perform the initial review. A PR is marked Ready for review only after explicit owner agreement; that transition triggers the integrated GitHub Codex review.
