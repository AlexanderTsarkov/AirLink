# Product Documentation

This directory contains accepted product documentation and controlled product WIP. This file is a document index, not a product specification.

## Documentation Status

- Canonical documents describe accepted product truth.
- WIP documents are non-canonical drafts under owner review and are not implementation requirements.
- Supporting indexes and guidance describe documentation status and process without defining the product.

## Recommended Reading Order

1. `CurrentState.md` for the accepted state of the restarted project.
2. `wip/README.md` for the rules governing product WIP.
3. `wip/product-vision-reconstruction.md` for the Product Vision currently under review.
4. `wip/product-direction.md` for the long-term direction of AirLink as a service, including its product pillars, lifecycle, domains, and evolution principles.
5. `wip/flight-mode-model.md` for the operational lifecycle while the pilot intends to fly.
6. `wip/flight-model.md` for the boundaries and special points of one airborne episode.
7. `wip/navigation-model.md` for the limited navigation state and directional semantics of the initial scenario.
8. `vision/README.md` for the status of the future canonical Product Vision.

## Canonical Documents

- `CurrentState.md` — records accepted project state that should remain valid outside the active iteration.

## Work in Progress

- `wip/product-vision-reconstruction.md` — reconstructs the initial Product Vision while separating accepted direction from legacy concepts, proposed synthesis, hypotheses, deferred areas, and open questions.
- `wip/product-direction.md` — records the owner-approved long-term direction of AirLink as a service while keeping future concepts separate from MVP and release requirements.
- `wip/flight-mode-model.md` — defines the operational lifecycle of explicit Flight Mode, including ground waiting, repeated Flights, completion, and exit behavior.
- `wip/flight-model.md` — defines one airborne Flight, its boundaries, special points, and relationship to summary and replay.
- `wip/navigation-model.md` — defines only the navigation state and directional concepts required by the initial local-flight scenario.

## Supporting Indexes and Guidance

- `wip/README.md` — defines the status, handling, and promotion expectations for product WIP.
- `vision/README.md` — records that no canonical Product Vision has yet been accepted and identifies the intended promotion area.

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
