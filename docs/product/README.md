# Product Documentation

This directory contains accepted product documentation, owner-controlled product policy, and controlled product WIP. This file is a document index, not a product specification.

## Documentation Status

- Canonical documents describe accepted product truth.
- Product-policy artifacts provide long-term direction and conditional decision discipline without creating release scope, detailed behavior, or implementation authority.
- WIP documents are non-canonical drafts under owner review and are not implementation requirements.
- Supporting indexes and guidance describe documentation status and process without defining the product.

## Context Routing

### Baseline Product Context

1. `CurrentState.md` for accepted durable project and product state.
2. `vision/ProductVision.md` for AirLink's accepted enduring product purpose, mission, two interdependent product pillars, initial focus, and product principles.
3. The relevant issue, specification, or other task artifact for bounded scope and accepted detail.

### Conditional Product-Policy Context

When the product-significance routing gate in `AGENTS.md` triggers, or product significance is discovered:

1. consult `policy/ProductGovernance.md` for the conditional alignment procedure;
2. consult `policy/ProductDirection.md` as relevant long-term alignment context;
3. load only the other canonical, domain, issue, and decision artifacts needed for the affected concern.

Routine work does not load Product Governance or Product Direction by default. See `policy/README.md` for their roles and authority boundaries.

### Task-Relevant WIP

Consult `wip/README.md` and only the WIP documents that the task explicitly concerns or that the triggered alignment process identifies as directly relevant:

- `wip/mvp-0.1-scope.md` for the owner-reviewed WIP boundary of MVP 0.1 and its first meaningful product slice;
- `wip/mvp-0.1-engineering-map.md` for the concern-level MVP 0.1 responsibility map, including the live/replay source boundary and C10 replay/source-delivery concern;
- `wip/scenario-generator.md` for the minimum separate Scenario Generator responsibility boundary and its handoff to AirLink replay;
- `wip/flight-mode-model.md` for the operational lifecycle while the pilot intends to fly;
- `wip/flight-model.md` for the boundaries and special points of one airborne episode;
- `wip/navigation-model.md` for the limited navigation state and directional semantics of the initial scenario.

## Canonical Documents

- `CurrentState.md` — records accepted project and product state that should remain valid outside the active iteration.
- `vision/ProductVision.md` — defines AirLink's accepted enduring purpose, mission, two interdependent product pillars, initial product focus, product principles, and Vision boundaries.

## Product Policy

- `policy/README.md` — defines the role, conditional loading, authority boundaries, and change control of product-policy artifacts.
- `policy/ProductDirection.md` — is an accepted product-policy artifact describing AirLink's broader product domains, lifecycle relationships, and evolution principles without creating release or implementation requirements.
- `policy/ProductGovernance.md` — is an accepted conditional product-policy artifact containing the alignment procedure invoked through the routing gate in `AGENTS.md`; it does not approve product decisions or load itself for routine work.

## Work in Progress

- `wip/mvp-0.1-scope.md` — defines the owner-reviewed product-level boundary of MVP 0.1, its first meaningful end-to-end product slice, explicit non-scope, deferred decisions, and the mandatory Flight Simulation Framework.
- `wip/mvp-0.1-engineering-map.md` — is the owner-reviewed, non-canonical AL-0002 concern and responsibility map, including the common live/replay input boundary and C10 replay/source-delivery responsibility.
- `wip/mvp-0.1-first-slice-selection.md` — is the owner-selected, non-canonical AL-0002 planning record comparing first-slice candidates and selecting the Map Flight Core with early estimated wind, now exercised from a materialized replay stream; it is not implementation authority and is an input to paused issue #37.
- `wip/scenario-generator.md` — defines the minimum non-canonical Scenario Generator subproduct boundary, frozen source-stream output, and planning-level handoff to AirLink replay; issue #47 may expand this same document.
- `wip/flight-mode-model.md` — defines the operational lifecycle of explicit Flight Mode, including ground waiting, repeated Flights, completion, and exit behavior.
- `wip/flight-model.md` — defines one airborne Flight, its boundaries, special points, and relationship to summary and replay.
- `wip/navigation-model.md` — defines only the navigation state and directional concepts required by the initial local-flight scenario.

## Supporting Indexes and Guidance

- `wip/README.md` — defines the status, handling, and promotion expectations for product WIP.
- `vision/README.md` — indexes the accepted canonical Product Vision and records its change-control expectation.

## Promotion Rule

For ordinary WIP, a document becomes canonical only after:

1. owner review;
2. explicit acceptance of its relevant content;
3. resolution or explicit retention of material open questions;
4. movement or rewriting into the appropriate canonical location;
5. update of this index and, when applicable, `CurrentState.md`.

Legacy material is never promoted by copying alone. It must be evaluated and rewritten for the restarted AirLink project.

Accepted product-policy artifacts are living policy and do not follow this ordinary WIP-to-canonical promotion lifecycle.

## Index Maintenance Rule

Any change that creates, moves, renames, or removes a product document must update `docs/product/README.md` in the same change.
