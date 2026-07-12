# AirLink Product Vision Reconstruction

## Status

This document is **WIP**, **non-canonical**, and **not an implementation requirement**. It is subject to owner review and later promotion. See the [product WIP policy](README.md) for the governing rules.

## Purpose

This document reconstructs the initial Product Vision for the restarted AirLink project. It records the current accepted direction, proposes concise formulations where wording is not yet accepted, and classifies retained legacy concepts, hypotheses, deferred areas, and open questions.

It is deliberately a product-level document. Detailed domain models, algorithms, data structures, architecture, and screen design belong outside this WIP.

## Sources and Source Priority

This reconstruction uses only:

1. the current owner-approved product decisions for this task;
2. canonical repository context: [`ITERATION.md`](../../../ITERATION.md) and [`CurrentState.md`](../CurrentState.md);
3. legacy document *AirLink: Product Vision*;
4. only the mission, problem, principles, and audience portions of legacy document *AirLink Vision and Specs*.

Current owner-approved decisions take precedence. Canonical repository documents define current project state and constraints. Legacy statements are historical source claims: they are retained only when consistent with current direction or explicitly classified for later review.

## Current Accepted Direction

AirLink is being reconstructed selectively for an initial, concrete pilot scenario. Its first value is reliable, semantically correct preflight weather information together with reliable presentation of important information during flight.

The product should reduce the pilot's need to combine information mentally across unrelated sources and interfaces. It does not replace pilot judgment, official aviation information, or regulatory responsibility.

Flight recording is secondary as immediate user value, but a replayable `Flight` and historically faithful data are important to the product and its development. A flight emulator is an important engineering foundation for testing flight behavior, data processing, and algorithms without requiring a real flight.

## Initial Target User

The initial target user is a paramotor pilot preparing for and conducting an actual flight. The initial framing must not broaden this primary audience to all aviation users.

Applicability to other pilots may be explored later, but it is not part of the accepted initial audience.

## Initial Operating Scenario

The first scenario is a solo local paramotor flight in visual meteorological conditions (VMC):

1. prepare for the flight at the pilot's current location;
2. take off;
3. fly freely;
4. return to the takeoff area.

Wind strength and gusts are critical preflight weather factors. Complex routes, long-distance navigation, fleet operations, schools, and commercial workflows are outside this initial scenario.

The Takeoff Point is the primary special navigation point. It is based on actual takeoff rather than the location where Flight Mode was entered.

## Product Problem — Proposed Synthesis

**Proposed formulation for owner review:** A paramotor pilot preparing for and performing a local flight must interpret weather and flight information that can be fragmented across tools, presented with inconsistent semantics, or require additional mental reconciliation at a time when clarity matters.

The legacy sources describe a broader fragmentation problem across preparation, flight, logging, sharing, and community activity. The current formulation retains the fragmentation theme but narrows it to the accepted initial pilot, weather, and in-flight information scope.

## Intended Value

The accepted initial value has two connected parts:

- reliable and semantically correct preflight weather information, initially for the pilot's current location;
- reliable presentation of important flight information during the flight.

AirLink should make the meaning and provenance of important quantities explicit. It must not conflate Ground Speed with Airspeed, Track with Heading, True North with Magnetic North, altitude ASL with altitude AGL, measured with derived or user-provided values, or actual with estimated values.

All pilot-facing navigation directions are intended to use True North. The exact parameter definitions and sources belong in later WIP models.

## Product Mission — Proposed Formulation

**Proposed formulation for owner review:** Help a paramotor pilot prepare for and conduct a local VMC flight with reliable weather and flight information whose meaning is clear.

This is a constrained synthesis, not accepted final wording. It intentionally excludes the legacy ambition to serve every participant and commercial activity across the wider free-flight ecosystem.

## Initial Product Boundaries

- The first client is Android, while the product should remain conceptually compatible with future Android and iOS clients.
- Preflight weather initially concerns the pilot's current location; no provider, forecast model, refresh interval, or warning threshold is selected here.
- Paramotor operation is considered only in VMC.
- Complex route navigation is deferred.
- Available fuel and average consumption may become user inputs, but fuel modeling and remaining-fuel logging are outside the first Flight Log scope.
- Architecture, frameworks, services, storage, APIs, and implementation technologies are not selected.

Normal application use and Flight Mode are distinct. Flight Mode is entered and exited explicitly, indicates intention to fly, and enables takeoff and landing detection. It is not a persisted domain object, and one entry may contain multiple Flights. Detailed lifecycle behavior belongs in a later Flight Mode model.

A `Flight` is one continuous airborne episode between takeoff and landing. There is no separate Flight Session entity. Its detailed data structure belongs in a later Flight model.

## Product Principles

- Prefer semantic correctness over visual convenience.
- Clearly distinguish measured, derived, estimated, and user-entered information.
- Do not present an ambiguous value without naming its meaning.
- Use True North as the reference for pilot-facing navigation directions.
- Define the domain model before designing the screen that represents it.
- Keep the pilot centered in the main preflight and flight presentation while map and compass graphics rotate according to the selected orientation; do not treat this as a screen-layout decision.
- Preserve historical values used during a Flight rather than silently replacing them during replay with results from future algorithm versions.
- Keep ordinary product logging distinct from diagnostic logging.
- Reconstruct and accept legacy concepts selectively rather than importing the old project wholesale.
- Prefer small, testable, reviewable increments and avoid premature architecture or feature expansion.

The final in-flight choice between Track-up and estimated Heading-up remains deferred for experimentation and pilot feedback.

## Initial Product Perspective

The initial perspective follows the pilot's immediate flight context:

- **Before flight:** present reliable current-location weather, with particular attention to wind strength and gusts.
- **During flight:** present important flight information with explicit semantics and a pilot-centered orientation model.
- **After flight:** retain enough physical-flight information for historically faithful replay, without recording the complete internal application state.

This perspective is not a feature catalog or a definition of screen layout. Detailed Flight Mode, Flight, navigation, logging, and parameter behavior requires separate owner-reviewed WIP.

## Longer-Term Direction

The legacy vision imagined AirLink as broad infrastructure supporting preparation, flight, analysis, community participation, and commercial activity for many roles. The restarted project does not accept that breadth as a current requirement.

The following remain bounded longer-term directions or hypotheses:

- the product may support other free-flight pilots after proving value for the initial paramotor scenario;
- future Android and iOS clients should be conceptually possible;
- preparation, in-flight support, and post-flight analysis may eventually form a coherent product journey;
- replayable Flights and emulation may support future product improvement and testing.

No marketplace, social platform, school-management system, or universal aviation ecosystem is currently accepted.

## Concepts Retained from Legacy

The following legacy concepts are retained because they are consistent with current accepted direction:

- pilots face fragmentation across tools and information sources;
- the product can support preparation, the flight itself, and later analysis, while the initial value remains concentrated before and during flight;
- an individual pilot benefits from accurate, understandable information;
- simulation can support development and testing without requiring a real flight;
- the product may create a coherent flight history, provided recorded historical meaning is preserved.

Legacy safety language is retained only as an aspiration to support better-informed pilot decisions. AirLink is not claimed to guarantee safety or replace judgment, official sources, or regulatory responsibility.

## Deferred or Not Yet Accepted Concepts

The following are not part of the accepted initial Product Vision:

- a broad audience of instructors, schools, tandem customers, tourists, manufacturers, families, and observers;
- social publishing, messaging, events, reviews, galleries, and community infrastructure;
- marketplaces, vouchers, equipment sales, and other commercial systems;
- school, instructor, fleet, CRM, or customer-flow management;
- detailed checklists, alerts, SOS behavior, landing-zone assistance, or automatic recommendations;
- complex routes, route sharing, and detailed navigation behavior;
- detailed weather, wind, fuel, equipment, or flight-log specifications;
- a claim that AirLink should become a universal platform or digital standard for free flight.

These concepts are deferred, rejected from the current scope, or require later product discovery. Their presence in legacy material does not make them current requirements.

## Assumptions and Hypotheses

- **Hypothesis:** Bringing weather and in-flight information into one coherent experience will reduce avoidable mental reconciliation for the pilot.
- **Hypothesis:** Explicit parameter semantics will improve trust and reduce misinterpretation.
- **Hypothesis:** A narrow paramotor scenario can establish product value before considering other pilot groups.
- **Hypothesis:** Replay and emulation will make flight behavior and algorithms more testable and reviewable.
- **Assumption:** The initial product can provide useful decision support without attempting to cover every phase, role, or service described by the legacy vision.

These statements require validation and must not be treated as accepted requirements.

## Open Product Questions

- Is the proposed problem formulation precise enough for the first product slice?
- What final mission wording should be promoted into canonical Product Vision?
- What observable pilot outcome should demonstrate that the initial product provides value?
- What is the minimum preflight weather set beyond the accepted emphasis on wind strength and gusts?
- What is the minimum in-flight information set for the initial scenario?
- Which wording accurately describes safety-related value without overstating the product's authority or capability?
- Should broader applicability to other free-flight pilots remain a long-term direction, or be removed until the first scenario is validated?
- Which, if any, broader legacy concepts deserve later product discovery?

## Non-goals of This WIP

This WIP does not define architecture, implementation technology, APIs, databases, mobile/web/server decomposition, algorithms, detailed domain models, recording intervals, warning thresholds, routes, UI layouts, a feature catalog, or an implementation plan.

It does not create canonical Product Vision or approve any retained legacy concept merely by mentioning it.

## Expected Promotion Path

After owner and ChatGPT review, accepted material may be rewritten or moved into the canonical `docs/product/vision/` area. Promotion requires explicit owner acceptance, deliberate handling of open questions, and an update to the product documentation index.
