# Scenario Generator Boundary

## Status and Authority

Related decisions: [issue #45 — Runtime Replay vs Scenario Generation](https://github.com/AlexanderTsarkov/AirLink/issues/45).

This document is a **minimum WIP subproduct-boundary artifact** created under issue [#46](https://github.com/AlexanderTsarkov/AirLink/issues/46). It is non-canonical, is not implementation authority, and does not select Scenario Generator architecture, technology, storage, deployment, user interface, or delivery plan.

Issue #47 may expand this same document. A separate competing Scenario Generator boundary document must not be created.

## Purpose

The Scenario Generator makes the mandatory MVP 0.1 simulation capability practical without assigning deterministic simulated-Flight calculation to AirLink runtime.

It enables an authored scenario or another approved generation input to be evaluated, verified, and exported as a frozen source-equivalent stream before AirLink consumes it. AirLink then replays that materialized stream through its normal C4/C5-facing boundaries.

## Sources

This boundary uses the owner decision record in issue #45, execution scope in issue #46, the corrected [MVP 0.1 Engineering Map](mvp-0.1-engineering-map.md), the [first-slice selection](mvp-0.1-first-slice-selection.md), and Generator-specific material from paused Draft PR #44 as source material only. No legacy material was used.

## Responsibility Boundary

The Scenario Generator owns, at planning level:

- high-level scenario authoring;
- scenario phase definition and evaluation;
- truth state, trajectory, motion, and environment;
- the physical model needed to materialize a scenario;
- deterministic generation of source-equivalent position, Ground Speed, Track, pressure, orientation, weather, and other approved source observations;
- baseline source cadences;
- source-error and variation profiles;
- baseline availability, invalidity, and interruption-event materialization;
- deterministic generation order;
- scenario visualization and truth verification;
- validation of generation formulas and reference sequences;
- export of a frozen source-equivalent stream with identity, version, compatibility, and integrity evidence.

These responsibilities establish ownership only. Their detailed semantics and realization remain for issue #47 and later authorized work.

## Explicit Non-Responsibilities

The Scenario Generator does not own:

- AirLink Flight Mode or Flight lifecycle;
- AirLink takeoff or landing detection;
- AirLink calculation of altitude, vertical speed, estimated wind, distance, orientation, or Flight aggregates;
- AirLink map, Flight Screen, recording, Summary, or saved-Flight behavior;
- replay-source activation, replay-session state, playback controls, delivery scheduling, batching, delay, redelivery, collision handling, or runtime delivery diagnostics;
- C4/C5 normalization, validity, freshness, availability interpretation, or downstream degradation behavior;
- pilot-facing runtime use of Generator phases, truth, formulas, or expected answers.

The Generator is a product-enabling subproduct boundary, not a third AirLink product pillar and not an alternative AirLink runtime path.

## Conceptual Output

The Generator outputs a frozen, ordered collection of source-equivalent observations and availability or invalidity events. Conceptually, the export carries:

- stream identity and version;
- compatibility and integrity evidence;
- source-monotonic time and deterministic equal-time order;
- source category and source-equivalent value where applicable;
- source time and the metadata required by the normal C4/C5 contract;
- explicit availability or invalidity events;
- origin metadata identifying the stream as generated synthetic material without exposing privileged truth to AirLink behavior.

The frozen export may include separate out-of-band truth or expected-output evidence for Generator verification and independent validation. That evidence is not part of the AirLink runtime source stream and must not be available to normal AirLink concerns.

Exact schemas, file formats, storage, package boundaries, and integrity mechanisms are deferred.

## Handoff to AirLink Replay

The planning-level handoff is:

```text
Scenario Generator
    -> frozen source-equivalent stream
    -> C10 replay and source delivery
    -> normal C4 / C5 boundaries
    -> normal AirLink behavior
```

C10 validates and selects an approved frozen stream, controls replay progression and delivery, and does not recalculate its values. C4/C5 receive replayed observations through the same normalized product-facing boundaries used by live sources. Generator truth, phases, formulas, and source-generation instructions do not cross this handoff.

The same AirLink replay contract may also accept a normalized recorded-real-Flight stream or a deterministic regression fixture. Those origins do not transfer generation responsibility into AirLink runtime.

## Preserved Source Material

Issue #37 and Draft PR [#44](https://github.com/AlexanderTsarkov/AirLink/pull/44) remain paused source artifacts. The following Generator-specific material is preserved conceptually for issue #47:

- scenario structure and phases;
- truth trajectory and physical model;
- geographic-coordinate generation;
- pressure generation;
- orientation and magnetic-source generation;
- source cadences;
- source-error and variation profiles;
- baseline availability events;
- deterministic generation order;
- scenario visualization and truth verification;
- formula and reference-sequence validation;
- frozen-fixture metadata and integrity evidence.

Preservation does not make PR #44 canonical, approve its formulas or exact fixture, select final Generator architecture, or authorize implementation. Product-runtime decisions from that work must be reconsidered through the corrected replay boundary when issue #37 resumes.

## Assumptions and Open Questions

Accepted assumptions:

- the mandatory simulation capability remains part of MVP 0.1;
- materialization occurs before AirLink runtime replay;
- Generator output is source-equivalent rather than privileged truth input;
- issue #47 expands this document rather than creating another boundary artifact.

Open for issue #47:

- exact non-terminal phase-boundary semantics;
- Generator architecture and technology;
- authoring and visualization workflow;
- reusable generation, verification, storage, and delivery path;
- exact frozen-stream export format and Generator-side integrity mechanism;
- which preserved PR #44 formulas, profiles, and fixture details should be accepted, revised, or discarded.

## Product Direction Alignment

**Outcome:** `Aligned with explicit simplification`.

The boundary preserves the mandatory validation capability required by Product Vision while keeping normal Flight Support behavior independent from Generator truth. The issue #45 owner decision explicitly limits issue #46 to this minimum boundary and defers full Generator design to issue #47, making the omission bounded and reversible without introducing hidden product behavior.

## Expected Promotion Target

No canonical promotion target is selected. Later owner review may retain this as controlled WIP, expand it under issue #47, or promote accepted durable content into an appropriate product-enabling or engineering document.
