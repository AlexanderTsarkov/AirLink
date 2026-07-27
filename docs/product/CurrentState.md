# AirLink Current State

## Purpose

This file records accepted project and product state that should remain valid outside the active iteration. It does not replace `ITERATION.md`, GitHub issues, release scopes, or detailed canonical specifications.

It is not a documentation progress log. Review status, promotion readiness, and temporary WIP relationships belong in the relevant iteration, issue, pull request, or documentation index.

## Current Product Definition

AirLink is being restarted as a new product project for pilots. The current foundation work defines the product and its first meaningful slice before implementation begins.

AirLink is intended to support real flying activity through two interdependent forms of value:

- Flight Support;
- Pilot Ecosystem.

Development begins with Flight Support for one paramotor pilot performing a solo local Flight. The initial product direction is a coherent preparation–Flight–completion experience that provides understandable information, reduces avoidable cognitive burden, and supports better-informed pilot decisions without replacing pilot judgment or responsibility.

## Accepted Facts

- The new GitHub repository is the canonical workspace for accepted AirLink documentation and future code.
- Existing Google Drive documents, older ParaFlight materials, and the local historical project folder are legacy information sources.
- Legacy sources may contain valuable product and domain knowledge, but they are not current specifications by default.
- The restarted project uses a documentation-first, bounded-iteration workflow.
- Product WIP is separated from canonical documentation.
- `_working/` is a local disposable workspace and is not committed.

## Accepted Decisions

- AirLink will be reconstructed selectively rather than migrated wholesale.
- The initial target user and validation scenario are one paramotor pilot performing a solo local Flight.
- The first meaningful product slice spans preparation, explicit Pre-Flight, Flight Mode, one or more Flights, completion, local retention, and later saved-Flight review.
- Flight Mode and Flight are distinct concepts; one Flight Mode period may contain multiple independent Flights.
- A minimum integrated Flight Simulation Framework is mandatory for developing and validating MVP 0.1, but it is not a third product pillar or direct end-user value.
- No application architecture, implementation technology, provider, algorithm, or storage design has been accepted yet.
- Every pull request starts as Draft.
- The project owner and ChatGPT perform the initial review.
- A PR is marked Ready for review only after explicit owner agreement; that transition triggers integrated GitHub Codex review.

## Implemented Capabilities

None. Product implementation has not started.

## Active Constraints

- Do not begin mobile, web, server, or infrastructure implementation during the current foundation iteration unless a separately approved iteration or issue authorizes it.
- Do not treat previous technology choices or feature lists as accepted requirements.
- Use only the minimum legacy material needed for the current product question.
- Do not treat WIP product artifacts as canonical specifications or implementation authority.

## Open Product Questions

Detailed product and domain decisions remain deferred to the bounded work that requires them, including parameter contracts, exceptional Flight boundaries, Active Navigation behavior, replay data requirements, architecture, providers, algorithms, schemas, and implementation technology.

## Last Updated

2026-07-27
