# Product Documentation

This directory contains accepted product documentation and controlled product WIP. This file is a document index, not a product specification.

## Documentation Status

- Canonical documents describe accepted product truth.
- WIP documents are non-canonical drafts under owner review and are not implementation requirements.
- Supporting indexes and guidance describe documentation status and process without defining the product.

## Recommended Reading Order

1. `CurrentState.md` for the accepted state of the restarted project.
2. `vision/ProductVision.md` for AirLink's accepted enduring product purpose, mission, two interdependent product pillars, initial focus, and product principles.
3. `wip/README.md` for the rules governing product WIP.
4. `wip/mvp-0.1-scope.md` for the owner-reviewed WIP boundary of MVP 0.1 and its first meaningful product slice.
5. `wip/product-direction.md` for the long-term direction of AirLink as a service, including its broader product domains, lifecycle, and evolution principles.
6. `wip/product-governance.md` for the non-canonical WIP alignment procedure invoked by the active product-significance routing rule in `AGENTS.md`; the WIP does not independently establish repository policy.
7. `wip/flight-mode-model.md` for the operational lifecycle while the pilot intends to fly.
8. `wip/flight-model.md` for the boundaries and special points of one airborne episode.
9. `wip/navigation-model.md` for the limited navigation state and directional semantics of the initial scenario.
10. `vision/README.md` for the canonical Product Vision index and change-control note.

## Canonical Documents

- `CurrentState.md` — records accepted project and product state that should remain valid outside the active iteration.
- `vision/ProductVision.md` — defines AirLink's accepted enduring purpose, mission, two interdependent product pillars, initial product focus, product principles, and Vision boundaries.

## Work in Progress

- `wip/mvp-0.1-scope.md` — defines the owner-reviewed product-level boundary of MVP 0.1, its first meaningful end-to-end product slice, explicit non-scope, deferred decisions, and the mandatory Flight Simulation Framework.
- `wip/product-direction.md` — records the owner-approved long-term direction of AirLink as a service while keeping future concepts separate from MVP and release requirements.
- `wip/product-governance.md` — describes the alignment procedure referenced by the active routing rule in `AGENTS.md`; it remains WIP and non-canonical and does not independently control routing, trigger activation, or product authority. Because active policy invokes the current procedure, merged changes to its referenced checks or outcomes can affect required agent behavior and therefore require explicit owner approval and normal repository review.
- `wip/flight-mode-model.md` — defines the operational lifecycle of explicit Flight Mode, including ground waiting, repeated Flights, completion, and exit behavior.
- `wip/flight-model.md` — defines one airborne Flight, its boundaries, special points, and relationship to summary and replay.
- `wip/navigation-model.md` — defines only the navigation state and directional concepts required by the initial local-flight scenario.

## Supporting Indexes and Guidance

- `wip/README.md` — defines the status, handling, and promotion expectations for product WIP.
- `vision/README.md` — indexes the accepted canonical Product Vision and records its change-control expectation.

## Promotion Rule

A WIP document becomes canonical only after:

1. owner review;
2. explicit acceptance of its relevant content;
3. resolution or explicit retention of material open questions;
4. movement or rewriting into the appropriate canonical location;
5. update of this index and, when applicable, `CurrentState.md`.

Legacy material is never promoted by copying alone. It must be evaluated and rewritten for the restarted AirLink project.

## Index Maintenance Rule

Any change that creates, moves, renames, or removes a product document must update `docs/product/README.md` in the same change.
