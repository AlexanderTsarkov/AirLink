# MVP 0.1 First Slice Implementation Plan

## Status and Authority

Related issue: [#37 — AL-0002-05: Prepare the selected first vertical slice for implementation](https://github.com/AlexanderTsarkov/AirLink/issues/37).

This document is the newly authored, implementation-ready **WIP engineering plan** for the owner-selected first AirLink vertical slice. It is:

- non-canonical;
- not product implementation authority by itself;
- bounded to AL-0002 planning;
- based on current `main`, issues #37 and #45–#47, and the owner decisions recorded in this plan;
- subject to owner review and acceptance through the fresh Draft PR;
- an input to later AL-0003 issue preparation, not activation of AL-0003.

Merge and owner acceptance would approve this plan as a planning input. They would not begin implementation, create an implementation issue, approve a complete application architecture, make WIP canonical, or authorize Scenario Generator implementation.

Draft PR [#44](https://github.com/AlexanderTsarkov/AirLink/pull/44) is a paused, non-canonical source artifact. Its useful decisions are accounted for in the traceability appendix. Its text is not authoritative and is not the basis of this document's structure.

## Source and Decision Basis

This plan follows:

- [`ITERATION.md`](../../../ITERATION.md);
- canonical [`CurrentState.md`](../CurrentState.md) and [`ProductVision.md`](../vision/ProductVision.md);
- the product-significance process in [`ProductGovernance.md`](../policy/ProductGovernance.md) and alignment context in [`ProductDirection.md`](../policy/ProductDirection.md);
- the owner-reviewed [`MVP 0.1 Scope`](mvp-0.1-scope.md);
- the owner-approved [`MVP 0.1 Engineering Map`](mvp-0.1-engineering-map.md);
- the owner-selected [`First Vertical Slice Selection`](mvp-0.1-first-slice-selection.md);
- relevant [`Flight Mode`](flight-mode-model.md), [`Flight`](flight-model.md), and [`Navigation`](navigation-model.md) WIP;
- the minimum [`Scenario Generator Boundary`](scenario-generator.md);
- issue [#37](https://github.com/AlexanderTsarkov/AirLink/issues/37);
- issue [#45](https://github.com/AlexanderTsarkov/AirLink/issues/45) and its Owner Decision Record;
- issue [#46](https://github.com/AlexanderTsarkov/AirLink/issues/46) and merged PR [#49](https://github.com/AlexanderTsarkov/AirLink/pull/49);
- issue [#47](https://github.com/AlexanderTsarkov/AirLink/issues/47);
- the complete Draft PR #44 diff, review history, and 29 unresolved review threads;
- the final owner decisions supplied for issue #37.

Current `main` is authoritative where these sources overlap. The final owner decisions recorded here supersede conflicting PR #44 assumptions.

---

# 1. Purpose and Selected Slice

The selected slice remains:

> **Simulation-driven Map Flight Core with early estimated wind**

The historical name is retained for traceability. Under the accepted runtime boundary, “simulation-driven” means that a pre-materialized frozen source-equivalent stream is replayed into AirLink. AirLink runtime does not generate the simulated Flight.

The slice exercises one bounded Flight from `Ready on Ground` through confirmed takeoff, active map-centred Flight, confirmed landing, in-memory record finalization, and Summary. It combines a real pilot-visible result with early evidence for lifecycle, replay, source semantics, experimental detection, derivation, orientation, estimated wind, recording, and independent validation.

The slice is implementation-ready when AL-0003 can begin without inventing product semantics, transferring Scenario Generator responsibility into AirLink, or selecting a complete application architecture.

# 2. Pilot-Visible Outcome

The normal successful path is:

1. A development build enters a temporary Flight Screen path with a prepared replay-backed Flight Mode context.
2. Before replay delivery begins, Flight Mode is `Ready on Ground`, no Flight exists, unavailable values remain unavailable, and the map is North-up.
3. A bounded non-flight explanation states that in-Flight wind is estimated, short-term changes cannot be reliably separated among pilot input, climb or descent, wing behavior or configuration, turbulence, and actual wind variation, and in-Flight gust estimation is not included.
4. The user selects `Start`. This begins source-equivalent delivery only.
5. While still on the ground, current map position, barometric Altitude MSL, weather-source wind, and compact replay controls are presented. Valid incoming Device Magnetic Azimuth is converted by C7 to Device True Azimuth and rotates the map; unavailable or invalid ground-orientation context uses North-up fallback.
6. C6 qualifies and confirms experimental takeoff from normal C4/C5-derived inputs. C2 authorizes C3 to create the Flight and Takeoff Point. C9 initializes the bounded in-memory record.
7. During Flight, C1 presents C7's pilot-facing Ground Speed output together with a centred map, understandable True North, barometric Altitude MSL, Height above Takeoff when available, Vertical Speed, elapsed Flight time, flown distance, and an accepted estimated wind using the bounded windsock-like comprehension experiment.
8. C8 uses C7's valid True-North Track output for Track-up. Invalid or unavailable C7 Track produces a North-up degraded fallback; C8 does not reinterpret raw C4 Track, and Device True Azimuth is not an airborne fallback.
9. C6 confirms experimental landing from normal inputs. C2 authorizes C3 to complete the Flight and create the Landing Point. C9 retains the confirmation tail and produces a terminal recording outcome.
10. A successful Summary appears only when C9 has a `finalized_complete` record. The Summary identifies the result as `Synthetic test Flight`, presents recording quality/status, and is recomputed from the finalized record.
11. C2 remains `Ready on Ground` within Flight Mode, but the first-slice harness does not permit a second Flight in the same development session.
12. `Reset` explicitly discards the completed in-memory record, tears down the non-active development session through coordinated concern-owned resets, and creates a new development session. Replaying the same fixture keeps the stream identity but receives a new `developmentSessionId`.

If recording cannot produce a successful record, the pilot-visible outcome preserves:

```text
Flight completed
Flight record unavailable
```

Recording failure never rewrites completed Flight lifecycle truth.

# 3. Included Behavior

The slice includes:

- temporary development entry into an already prepared replay-backed Flight Mode context;
- one immutable calculation profile per development session;
- one automatically selected, read-only frozen source-equivalent stream;
- C10 stream validation, replay activation, Start, Pause, `1×`/`2×`, cursor/progression, deterministic ordering, batching, delay, exact redelivery, collision handling, approved delivery transforms, replay diagnostics, and replay-state Reset;
- normal C4/C5 interpretation of replayed source-equivalent information;
- pre-delivery North-up behavior and source-driven ground orientation after Start;
- experimental takeoff and landing detection;
- C2/C3 lifecycle authority and one Flight per development session;
- bounded transient pre-Flight history and authoritative takeoff-side handoff;
- barometric Altitude MSL, Height above Takeoff, Vertical Speed, estimated wind, landing estimated Airspeed, elapsed time, and distance;
- a pilot-centred real basemap with bounded scale and explicit spatial degradation;
- progressive in-memory C9 recording with separate completeness and quality;
- terminal recording outcomes and Summary-from-finalized-record behavior;
- a controlled interruption validation path only through the unresolved P3 boundary;
- replaceable structured diagnostics;
- independent validation against out-of-band validation reference evidence;
- coordinated fresh development-session Reset.

# 4. Explicit Non-Goals

The slice does not include:

- product code, Flutter scaffolding, fixtures, tests, executable prototypes, or Generator implementation under this planning issue;
- Home, full Pre-Flight, onboarding, permanent startup/navigation, or a complete help system;
- live Android or iOS source integration, permissions, background execution, process recovery, or real-Flight validation;
- durable persistence, a database, files as Flight storage, schema migration, saved Flights, reopening after restart, or durable deletion;
- a second Flight in one first-slice development session;
- active-Flight Reset, active-Flight exit, manual completion, false-detection discard, or automatic Flight Mode inactivity exit;
- a final product outcome for an interrupted active Flight;
- final production or safety takeoff/landing thresholds;
- final wind estimator, final numerical quality thresholds, or a safety claim based on estimated wind;
- final whole-product framework, code-sharing strategy, map provider, tile architecture, or application architecture;
- map pan, user zoom, map-layer selection, prefetch, pre-seeding, bulk download, or offline map packages;
- actual flown-track rendering, zero-wind reference paths, Takeoff/Landing Point markers, route guidance, Route Navigation, or Active Navigation;
- complete Flight Screen design or final visual hierarchy;
- scenario authoring, phases, truth state, physical modelling, source-value generation, source-error generation, baseline cadence/availability materialization, Generator verification, or expected-answer generation;
- a complete replay-storage architecture, complete provenance schema, complete backlog, or one-issue-per-increment commitment;
- activation of AL-0003 or creation of its implementation issues.

These omissions are explicit first-slice simplifications. They do not redefine the broader MVP 0.1 boundary or reject future AirLink domains.

# 5. Terminology and Semantic Distinctions

## 5.1 Runtime source and Flight classification axes

The following fields are independent:

```text
flightClassification = syntheticTestFlight
deliveryMode = replay
replayOrigin = generatedSynthetic
replayStreamId
replayStreamVersion
category-level value provenance
delivery handling
```

- C3 assigns `flightClassification`.
- C9 stores the C3 assignment and never infers or changes it.
- Replay mode does not imply synthetic classification.
- An individual observation's provenance does not define Flight classification.
- Delay, batching, transformation, or exact redelivery are delivery handling, not classification.
- Future live Flights, normalized replay of recorded real Flights, and deterministic regression fixtures can use the same fields without changing existing meanings.

The bounded completed-Flight label is:

```text
Synthetic test Flight
```

Future deletion of synthetic test records must use authoritative Flight classification, never delivery mode.

## 5.2 Motion and direction

- **normalized GS observation:** C4-owned source-equivalent speed over the ground with normalized unit, source time, validity, quality, and provenance.
- **AirLink Ground Speed (GS):** C7-owned pilot-facing Ground Speed meaning and output status, expressed in `km/h` for this slice.
- **normalized Track observation:** C4-owned source-equivalent movement direction with normalized True-North reference, source time, validity, quality, and provenance.
- **AirLink Track:** C7-owned True-North Track meaning and output status consumed by C8 for Track-up.
- **Device Magnetic Azimuth:** source orientation relative to Magnetic North.
- **Device True Azimuth:** C7-derived device orientation relative to True North.
- **Airspeed (AS):** speed relative to the air mass. Generator truth AS is never available to AirLink runtime.
- **takeoff Airspeed proxy:** C6's bounded pre-takeoff estimate derived from source-equivalent GS, Track, and usable weather wind.
- **landing estimated AS:** magnitude of ground velocity minus the last accepted estimated-wind vector.
- **Heading:** aircraft orientation through the air. Generator Air Heading is not a runtime source and Device True Azimuth is not Air Heading.
- **Bearing:** direction from one geographic point to another; it is not Track, Heading, or device orientation.

C7 need not numerically transform GS or Track when C4 has already normalized the required unit/reference. C7 still owns their AirLink product meaning, availability/quality output status, and pilot-facing handoff. C4 retains source validity, quality, provenance, and timing ownership.

## 5.3 Wind

- **weather wind:** C5-interpreted external or replayed weather observation, using meteorological `from` direction.
- **truth wind:** Generator-only physical state and possible out-of-band reference evidence; it is unavailable to C1–C10.
- **estimated wind:** C7 output inferred from accepted normalized GNSS observations.
- **headwind component:** nonnegative vector projection of usable weather wind against current Track for the takeoff Airspeed proxy.

Weather wind, truth wind, and estimated wind are never interchangeable.

## 5.4 Time

- **sourceMonotonicTime:** immutable time attached to a source-equivalent event; it governs source order, detector holds, derivation windows, and replay-equivalence checks.
- **AirLink-observed monotonic time:** when C4/C5 accepts delivery; it may differ under delay or batching and is retained when needed for delivery evidence.
- **wall-clock/civil time:** source-equivalent calendar time used only where civil meaning is required.
- **playback time:** C10 delivery progression; it does not change source timestamps.
- **host timer/wall time:** implementation execution timing; it is not a detector or wind-evaluation clock.

## 5.5 Lifecycle, recording, and quality

- C2 owns Flight Mode authorization.
- C3 owns Flight lifecycle truth and Flight classification.
- C6 owns detector state and outcomes.
- C9 owns recording state and record usability.
- **record completeness** describes structural retention of mandatory recording-contract data.
- **quality** describes input/data quality and is `nominal` or `degraded`.

A correctly retained source outage may produce degraded quality while the record remains structurally complete. Missing mandatory record data makes the record incomplete.

# 6. Live/Replay Inputs and Product Outputs

## 6.1 Normal input categories

The selected frozen stream supplies source-equivalent events for C4/C5 normalization:

- GNSS position, GS, Track, and associated quality/accuracy;
- atmospheric pressure;
- Device Magnetic Azimuth and associated quality;
- weather wind speed and meteorological `from` direction;
- QNH;
- civil time where required;
- explicit source availability, invalidity, interruption, and restoration transitions.

Live platform sources will later enter the same C4/C5-facing contracts. This slice implements no live source.

## 6.2 Runtime controls

C10 accepts:

- approved stream activation;
- `Start`;
- `Pause`;
- playback speed `1×` or `2×`;
- approved delay, batching, exact-redelivery, and status-transform configuration used by validation cases;
- replay-state `Reset` only when the Development Session Coordinator has established eligibility.

No control selects Generator phase, truth, formulas, source cadences, physical conditions, or expected outcomes.

## 6.3 Product and retained outputs

Outputs include:

- C2 Flight Mode state and authorization outcomes;
- C3 Flight identity, `syntheticTestFlight` classification, lifecycle, effective boundaries, Takeoff Point, and Landing Point;
- C6 detector state, reset reasons, provisional boundaries, and confirmed outcomes;
- C7 AirLink Ground Speed and True-North Track outputs plus accepted derived outputs and their quality/availability;
- C8 map/orientation state and explicit degradation;
- C9 recording state, quality, in-memory record, and terminal outcome;
- a successful Summary only from `finalized_complete`;
- coordinated development-session state;
- structured diagnostics and validation comparison results outside normal product behavior.

# 7. C1–C10 Responsibility Boundaries

Concern identifiers remain planning references, not required modules or packages.

## C1 — Pilot Interaction and Operational Flow

C1 presents the temporary entry, wind-limitation explanation, Flight Screen, compact replay controls, Flight state, C7 Ground Speed and other current values, degraded states, recording outcome, and Summary. It sends user replay-control intent to the development harness/C10 boundary. It does not read raw C4 GS/Track for pilot-facing display or infer lifecycle, classification, source validity, derivations, recording completeness, or validation results.

## C2 — Flight Mode Lifecycle

C2 owns the development Flight Mode state, `Ready on Ground`, awareness of the active Flight, and authorization for confirmed detector outcomes to affect C3. It returns to `Ready on Ground` after completed landing. It does not detect boundaries, create a Flight, record data, or reset C10/C3/C9 directly.

## C3 — Flight Lifecycle and Flight State

C3 owns Flight identity, active/completed state, effective boundaries, Takeoff/Landing Point identity and association, elapsed Flight time, and:

```text
flightClassification = syntheticTestFlight
```

C3 receives active replay-session context but does not infer classification from observations. At Flight creation it supplies C8 with the Flight ID, Takeoff Point ID, effective-boundary source position/state, and Flight association; C8 uses that handoff to establish Takeoff Point as Current Waypoint while Active Navigation remains off. C3 supplies authoritative creation/completion context to C9 and consumes recording outcomes only for pilot-visible completion status. Recording failure never changes C3 lifecycle truth.

## C4 — Input Acquisition and Validity

C4 receives replayed C4-facing events through the same boundary intended for live sources. It owns normalized GS and Track observations and all other normalized values, source and observed time, availability, validity, freshness, quality/accuracy, category-level provenance, delivery handling, continuity/gap state, and idempotent handling of exact redelivery. It supplies normalized observations and status to C6/C7/C9 and position where required by C8. It does not own pilot-facing GS/Track meaning, consume Generator truth, or infer Flight classification.

## C5 — Weather Context

C5 receives replayed weather/QNH through its normal input interpretation boundary and owns weather meaning, units, source/update time, freshness, validity, availability, provenance, and degradation. It supplies usable weather context to C1, C6, and C7 without becoming lifecycle or estimated-wind authority.

## C6 — Flight Detection

C6 owns experimental takeoff and landing candidate state, source-time holds, cancellation, timeout, effective-boundary anchors, and confirmed outcomes. It may consume the normalized C4/C5 inputs required by the detector and calculates the takeoff Airspeed proxy and landing estimated AS. It does not own pilot-facing GS/Track semantics, lifecycle, recording, or Generator physical transitions.

## C7 — Flight Information Derivation

C7 consumes C4's normalized GS and Track observations while preserving C4's validity, quality, provenance, and timing status. C7 owns the AirLink semantic Ground Speed output used by C1 and the True-North Track output used by C8, even when no numerical unit/reference conversion is required. C7 also owns barometric Altitude MSL, Height above Takeoff, Vertical Speed, Device True Azimuth, estimated wind, accepted output quality/availability, and calculation context. It performs normal product calculations from C3/C4/C5 context and never consumes scenario phase, physical liftoff/touchdown, truth wind, truth AS, or expected answers.

## C8 — Spatial Awareness and Map Context

C8 owns the pilot-centred map, package-specific zoom, fixed visible ground width, orientation-mode presentation, True North indication, passive Current Waypoint state after takeoff, attribution realization, provider isolation, and spatial degradation. It consumes C7's True-North Track output for Track-up and must not bypass C7 or reinterpret raw C4 Track. It does not perform C7 calculations or receive fixture/scenario metadata directly.

## C9 — Flight Recording and In-Memory Retention

C9 owns bounded transient recent history, recording initialization, append health, finalization, record completeness, record quality, the finalized in-memory record, and Summary source data. It preserves C3 classification and normal-boundary provenance. It does not create a hidden pre-Flight, infer lifecycle/classification, recalculate C7 outputs, or delete state as a side effect of C10 Reset.

## C10 — Replay and Source Delivery Enablement

C10 owns only:

- replay activation and replay-session state;
- Start and Pause;
- playback speed;
- replay-state Reset;
- cursor and progression;
- deterministic event ordering;
- batching;
- delay;
- exact redelivery;
- collision handling;
- approved delivery transforms;
- replay diagnostics;
- delivery into normal C4/C5 boundaries.

C10 does not own scenario phases, truth, physical or source-generation formulas, deterministic source-value generation, baseline cadences, source errors, source availability/fault materialization, Generator verification, lifecycle, detection, derivation, recording, Summary, or privileged truth.

# 8. Replay and Frozen-Stream Contract

## 8.1 Validation package

The first-slice validation package contains exactly four separate artifacts:

1. **runtime fixture manifest**;
2. **frozen runtime source-equivalent event stream**;
3. **out-of-band validation reference evidence**, with its own reference identity and payload digest;
4. **validation-only association and isolation evidence**, which associates the stream with the reference and proves that normal C1–C10 runtime cannot discover or access the reference location or content.

Issue #37 plan approval does not require a concrete package. A conforming concrete package is mandatory:

- before C10/downstream integration validation; and
- before acceptance of the implemented slice.

Full completion of issue #47 is not a blocker. Only a specifically missing required artifact from this package may block those validation stages.

## 8.2 Bounded serialization decision

For the first slice only:

- the runtime fixture manifest is deterministic UTF-8 JSON;
- the runtime event stream is UTF-8 NDJSON with one event per line;
- out-of-band validation reference evidence is one deterministic UTF-8 JSON envelope containing `referenceEvidenceId`, `referenceEvidenceVersion`, its deterministic payload, and a SHA-256 digest calculated over that payload;
- the validation-only association/isolation artifact is deterministic UTF-8 JSON containing the stream-to-reference association and isolation results;
- the runtime manifest's SHA-256 digest covers the runtime stream only;
- canonical field ordering is not a runtime requirement, but Generator/export verification must produce stable bytes for the frozen package.

This is a replaceable fixture interchange choice, not a complete replay-storage architecture or permanent Generator format.

## 8.3 Manifest contract

The manifest declares:

- `fixturePackageContractVersion`;
- `streamId`;
- `streamVersion`;
- `compatibilityContractId`;
- `replayOrigin = generatedSynthetic`;
- SHA-256 digest and byte length of the runtime stream;
- source-time unit;
- first and last source-monotonic times;
- terminal behavior: explicit `endOfStream`, with no implicit lifecycle meaning;
- categories and category contract versions present;
- permitted units and reference semantics;
- approved delivery-validation cases and their referenced event identities;

Compatibility is fail-closed. C10 rejects an unknown contract version, incompatible target, missing mandatory field, invalid digest, unordered stream, any duplicate identity in the frozen baseline stream, or invalid terminal declaration before delivery. Exact redelivery is a C10 delivery action over one validated baseline event; it is not a duplicate line in the frozen stream.

The runtime fixture manifest contains only runtime-stream metadata, compatibility/integrity information, categories, source-time range, terminal behavior, and approved delivery-case declarations required by C10. It contains no validation-reference identity, digest, path, filename, URI, URL, package key, lookup key, or other reference-location hint.

### Validation-only association and isolation evidence

The validation-only association/isolation artifact associates `streamId`/`streamVersion` and runtime-stream digest with the independent `referenceEvidenceId`/`referenceEvidenceVersion` and reference-payload digest. The validation harness receives this association after runtime output capture. C10 and the runtime bundle receive only the runtime fixture manifest and runtime event stream; they receive neither the reference artifact nor artifact 4.

Artifact 4 also records the parser, dependency, asset, and configuration checks proving that no runtime surface exposes the reference identity, location, or content. This four-artifact separation is a bounded validation contract, not a storage or distribution architecture.

## 8.4 Event contract and identity

Every runtime event has:

- `streamId`;
- `sourceCategory`;
- `sourceMonotonicTime`;
- `equalTimeSequence`;
- `eventKind = observation | status`;
- category-specific value fields and explicit units when `observation`;
- availability, validity, source state, freshness inputs, and quality/accuracy applicable to the category;
- category-level value provenance;
- source civil timestamp when the category requires civil meaning;
- C4/C5-facing metadata required to interpret the event.

The minimum replay-event identity is exactly:

```text
streamId
+ sourceCategory
+ sourceMonotonicTime
+ equalTimeSequence
```

`sourceMonotonicTime` is serialized as a nonnegative integer in the manifest-declared unit. `equalTimeSequence` is a nonnegative integer that establishes one total frozen order among all events sharing a source-monotonic time. The ordered pair `(sourceMonotonicTime, equalTimeSequence)` must be strictly increasing across the stream.

The runtime stream contains no:

- scenario phase;
- truth position, motion, wind, AS, altitude, liftoff, or touchdown;
- physical or source-generation formula;
- source-error profile;
- expected detector result;
- expected derived value;
- expected Summary value.

## 8.5 Delivery semantics

- `Start` delivers from the current cursor.
- `Pause` freezes C10 progression and emits no later event; it has no lifecycle meaning.
- `1×` and `2×` scale delivery intervals only.
- Delay changes observed delivery time only.
- Batching changes delivery opportunity only and preserves frozen order.
- Exact redelivery preserves the complete identity, payload, and metadata. C4/C5 handle it idempotently; histories, holds, derivations, and recording do not advance twice.
- The same identity with different payload or metadata is a collision. C10 fails closed for the affected stream and does not deliver the conflicting event as valid.
- An approved status transform may suppress a named baseline delivery or inject an explicit status event with its own valid four-part identity. It cannot calculate a source value, alter source time, reuse an identity with changed content, expose truth, or create ambiguous order.
- Contract validation and collision checks precede delivery transforms; delay, batching, and redelivery follow transformation.
- End of stream does not imply takeoff, landing, Flight completion, Flight Mode exit, finalization, or Summary.

The manifest's validation cases cover baseline, `2×`, Pause/Resume, delay, batching, exact redelivery, collision, and the controlled interruption transform without embedding expected answers in the runtime stream.

# 9. Development-Session Lifecycle and Reset

## 9.1 Session identity and immutable context

Every fresh session has a new:

```text
developmentSessionId
```

The same fixture replay retains the same `streamId` and `streamVersion`. Each session also fixes:

```text
calculationProfileId
calculationProfileVersion
```

Formulas, constants, detector thresholds, estimator configuration, provider implementation, and calculation contracts cannot change inside a Flight.

## 9.2 Development Session Coordinator

A bounded **Development Session Coordinator** is a first-slice harness responsibility, not a new permanent product concern. It:

- creates and publishes the session-scoped concern context;
- provides the prepared context to the Flight Screen;
- owns the `developmentSessionId`;
- routes replay-control intent;
- checks Reset eligibility;
- coordinates concern-owned teardown/reset without taking ownership of concern state.

## 9.3 Reset eligibility

Reset is allowed only:

- before an active Flight exists; or
- after C3 has completed the Flight and C9 has produced a terminal recording outcome.

Reset is unavailable during an active Flight or pending C9 finalization.

## 9.4 Coordinated reset protocol

The coordinator:

1. asks C3 and C9 for eligibility;
2. pauses/quiesces C10 delivery;
3. asks each concern to validate that its own state can be closed;
4. seals the old session context so no mixed teardown state is observable;
5. asks C9 to explicitly discard any completed in-memory record;
6. asks each concern to clear only its own session-local state:
   - C10 clears replay cursor, progression, transforms, and replay diagnostics;
   - C9 clears its current recorder/record after explicit discard;
   - C3 clears completed Flight/session-local lifecycle context;
   - C2 clears development Flight Mode/session state;
   - other concerns clear their own transient session state;
7. creates a new context with a new `developmentSessionId`, the same selected stream identity, and an immutable calculation profile;
8. publishes the new context only after every required initialization succeeds.

C10 Reset never deletes a Flight or recording directly. A completed record is discarded through an explicit C9 operation.

Reset is fail-closed. If teardown or new-session creation fails, no partially reset context is published. The old context remains sealed and unusable, replay controls remain disabled, and the failure is observable. Restarting the development build is an acceptable recovery for this harness; silently exposing mixed old/new concern state is not.

# 10. Flight Lifecycle and Authority Chains

## 10.1 Before Flight creation

C2 is `Ready on Ground`; no Flight identity or authoritative Flight record exists. C9 may hold only the bounded transient recent history in section 13.

## 10.2 Takeoff authority

```text
C6 detects
-> C2 authorizes
-> C3 creates Flight and Takeoff Point
-> C9 initializes recording
```

C3 assigns `syntheticTestFlight` at creation from the approved development/replay context, not from replay mode or observations.

C3 then supplies C8:

- `flightId`;
- Takeoff Point identity;
- effective-boundary position, accuracy/state, and source-observation reference;
- explicit Takeoff Point-to-Flight association.

C8 acknowledges the handoff by establishing the Takeoff Point as passive Current Waypoint. It does not enable Active Navigation or invent a Route.

## 10.3 Active Flight

C3 remains lifecycle authority. C7 derives values, C8 presents spatial state, and C9 progressively records. A source or map problem does not itself complete, reject, split, or delete the Flight.

## 10.4 Landing authority

```text
C6 detects
-> C2 validates active Flight
-> C3 completes Flight and creates Landing Point
-> C9 retains confirmation tail and finalizes
```

C3 establishes lifecycle completion regardless of C9 success. C2 returns to `Ready on Ground`. The harness prevents another Flight until fresh-session Reset.

# 11. Experimental Takeoff Detector

All thresholds are experimental first-slice parameters, not production or safety thresholds.

## 11.1 Accepted evaluation input

Takeoff state advances only on a newly accepted C4-normalized GNSS observation with valid source-monotonic time and valid GS. C6 consumes this detector input directly without taking ownership of C7's pilot-facing Ground Speed/Track semantics. Exact redelivery does not advance state. Gap, required-input invalidity, or source-monotonic discontinuity clears all candidate state.

## 11.2 Qualification

Qualification is:

```text
GS > 7.0 km/h
continuously for 1.0 s source time
```

The first qualifying observation becomes:

- the provisional effective takeoff boundary;
- the provisional Takeoff Point source anchor;
- the beginning of the relevant retained-history range.

Before qualification completes:

```text
GS <= 7.0 km/h
```

resets qualification.

## 11.3 Takeoff Airspeed proxy

The physical `25 km/h` reference is nominal wing Airspeed for sufficient lift. It is not a GS threshold.

For each evaluated observation:

```text
usableWeatherHeadwindComponentMps
=
max(
  0,
  weatherWindSpeedMps
  * cos(shortest angular difference between
        weatherWindFromDegTrue
        and trackDegTrue)
)
```

```text
takeoffAirspeedProxyKmh
=
GS
+ 0.75
  * usableWeatherHeadwindComponentMps
  * 3.6
```

The weather direction is meteorological `from`. C6 uses C4's normalized GNSS Track from the same evaluated observation and C5's weather context. Weather correction is usable only when:

- weather-wind speed and direction are available, valid, and fresh;
- Track is available, valid, and fresh;
- C4 supplies finite, available course accuracy; and
- the inclusive gate passes:

```text
courseAccuracyDeg <= 10.0
```

Exactly `10.0` degrees is accepted. A value below `10.0` is accepted; a value just above `10.0`, unavailable accuracy, or non-finite accuracy produces zero weather correction. Any missing, stale, or invalid required weather/Track value also produces zero correction. This is an experimental first-slice detector input-quality gate, not a production GNSS policy. C4 remains authoritative for the normalized Track observation and its accuracy/status; C6 owns only application of this detector gate.

Crosswind gives no positive headwind correction. Tailwind never lowers required GS.

The proxy does not use Device orientation, candidate displacement, Generator Air Heading, truth wind, truth AS, or scenario phase.

## 11.4 Confirmation, cancellation, and timeout

Confirmation is:

```text
takeoffAirspeedProxyKmh >= 25 km/h
continuously for 1.0 s source time
```

The qualification-completing observation may be evaluated once as the first confirmation-hold observation. It cannot complete a new one-second hold at the same timestamp.

Low-speed cancellation is:

```text
GS <= 7.0 km/h
continuously for 1.0 s source time
```

Candidate timeout is:

```text
15.0 s from provisional effective boundary
```

On an exact confirmation/timeout tie:

```text
confirmation first
timeout second
```

The confirmed outcome carries the effective boundary observation reference, confirmation source time, detector contract/configuration identifiers, and diagnostics. It never carries physical liftoff or Generator truth.

# 12. Experimental Landing Detector

All thresholds are experimental first-slice parameters.

## 12.1 Estimated AS

Landing uses:

```text
estimated air velocity
=
ground velocity
- last accepted estimated wind
```

Estimated AS is the vector magnitude. Estimated wind is mandatory; there is no GS-only landing fallback.

For this detector calculation, C6 consumes C4-normalized GS/Track source observations and their status. It does not redefine the C7 Ground Speed/Track outputs used by product presentation.

For landing only:

```text
stationaryGsThresholdKmh = 1.0
```

- `GS > 1.0 km/h`: valid Track is required to construct ground velocity.
- `GS <= 1.0 km/h`: ground velocity is `(0,0)` and Track is not required.
- missing or invalid GS is not stationary.
- no synthetic Track is created for C8 or C9.

## 12.2 Entry and confirmation

Entry is:

```text
estimated AS < 25 km/h
AND
GS < 4 km/h
```

The first newly accepted GNSS observation satisfying entry becomes the provisional effective landing boundary. Confirmation requires the predicate continuously for `15.0 s` source time.

Leaving the entry predicate clears accumulated confirmation time and the provisional boundary. Candidate identity may remain in the hysteresis band, but elapsed confirmation time is never reused.

## 12.3 Hard cancellation and invalidity

Hard cancellation is:

```text
estimated AS > 28.0 km/h
OR
GS > 7.0 km/h
continuously for 1.0 s source time
```

Comparisons are strict. Equality does not satisfy hard cancellation.

A gap, invalid GNSS, hard-invalidated wind, or source-time discontinuity clears all landing candidate state.

## 12.4 Effective boundary and confirmation tail

The Landing Point and Summary metric endpoint use the effective landing-boundary observation. C9 retains all approved data through actual confirmation time as detector evidence. The confirmation tail is tagged outside the metric interval and is never silently trimmed or counted in Summary metrics.

# 13. Bounded Recent-History Contract

Before Flight creation, C9 may hold only bounded transient pre-Flight history. It creates no Flight identity, hidden Flight, or authoritative Flight record.

The approved transient categories are:

- normalized GNSS position, GS, Track, accuracy/quality, state, and identity;
- normalized atmospheric pressure and state;
- Device Magnetic Azimuth and state;
- C5 weather wind and QNH context used by takeoff/altitude calculations;
- accepted C7 barometric Altitude MSL needed to establish the effective-boundary baseline;
- availability, invalidity, continuity, and source-time transitions affecting those categories.

The buffer preserves these categories from the earliest unresolved qualification anchor through current time. Its maximum normative horizon is:

```text
15.0 s source time
```

On confirmation, C3 supplies the authoritative effective boundary. C9 initializes only:

```text
[effectiveTakeoffBoundary, takeoffConfirmationTime]
```

Data before the boundary does not enter the Flight record. The wind estimator remains inactive until C3 creates the Flight. After authorized creation, C7 receives or reconstructs only the approved retained GNSS history in:

```text
[effectiveTakeoffBoundary, takeoffConfirmationTime]
```

C7 processes that history deterministically in source order as Flight-scoped estimator input, then continues the same history with newly accepted active-Flight observations. Observations before the authoritative effective boundary never enter wind windows or determine schedule alignment. This retrospective handoff does not create a pre-existing Flight.

Cancellation, timeout, invalidity, or discontinuity discards no-longer-needed transient history. During landing, C9 already owns the active record and retains the confirmation tail through actual confirmation.

# 14. Runtime Derivations and Calculation Context

Only simulated source-value generation belongs outside AirLink runtime. C6, C7, and Summary continue to perform product calculations.

## 14.1 Immutable calculation profile

Each development session fixes:

```text
calculationProfileId
calculationProfileVersion
```

The profile references semantic calculation contracts including:

```text
isaTropospherePressureAltitudeV1
olsAltitudeSlope3sMin20Span2sV1
haversineMeanEarthR6371008_8V1
barometricHeightAboveTakeoffV1
magneticToTrueAzimuthEastPositiveV1
validSegmentDistanceOverCoveredTimeV1
```

Detector contracts, thresholds, windows, estimator configuration, and provider implementation IDs are also versioned. Shared constants/configuration are retained once in Flight-level calculation context.

## 14.2 Barometric Altitude MSL

C7 uses pressure and QNH in `hPa`:

```text
altitudeMslM
=
44330.76923076923
* (1 - (pressureHpa / qnhHpa)^0.1902632365)
```

Both inputs must be finite, positive, valid, and sufficiently fresh under their normal C4/C5 contracts. The ratio is dimensionless. Unavailable input produces unavailable altitude, not zero. Generator inverse-pressure calculation is not part of this plan.

Contract:

```text
isaTropospherePressureAltitudeV1
```

## 14.3 Height above Takeoff

At the authoritative effective takeoff boundary:

```text
takeoffBoundaryDerivedAltitudeMsl
```

is taken from the valid C7-derived altitude associated with that boundary observation/range. Height is:

```text
currentDerivedAltitudeMsl
- takeoffBoundaryDerivedAltitudeMsl
```

It is unavailable if no valid effective-boundary baseline exists. Confirmation-time altitude is not a substitute. This value is barometric Height above Takeoff, not terrain AGL.

Contract:

```text
barometricHeightAboveTakeoffV1
```

## 14.4 Vertical Speed

On each newly accepted valid pressure/altitude sample, C7 performs unweighted ordinary least squares over distinct accepted derived Altitude MSL samples in:

```text
[endpoint source time - 3.0 s, endpoint source time]
```

For samples `(ti, hi)`:

```text
verticalSpeedMps
=
sum((ti - meanT) * (hi - meanH))
/ sum((ti - meanT)^2)
```

Output requires:

- at least 20 distinct samples;
- at least `2.0 s` source-time span;
- finite nonzero denominator.

There is no interpolation, host-time input, phase input, or duplicate redelivery sample. A pressure gap, invalidity, or source-monotonic discontinuity clears the fit window and makes VS unavailable until fresh minimum history exists.

Contract:

```text
olsAltitudeSlope3sMin20Span2sV1
```

## 14.5 Device True Azimuth

C7 receives Device Magnetic Azimuth from C4 and declination from a replaceable provider using valid position and civil-time context. East-positive conversion is:

```text
deviceTrueAzimuthDeg
=
normalize360(deviceMagneticAzimuthDeg + declinationEastDeg)
```

Contract:

```text
magneticToTrueAzimuthEastPositiveV1
```

The provider identity/version and applied declination are retained when the derived value is retained. Provider-specific types do not escape the C7 boundary.

## 14.6 Ground Speed and Track semantics

C4 supplies normalized GS and Track observations with source timing, availability, validity, freshness, quality/accuracy, and provenance. C7 exposes:

- AirLink Ground Speed value/unit and output status for C1;
- AirLink True-North Track value/reference and output status for C8.

When C4 has already normalized `km/h` and True-North reference, C7 may preserve the numeric value unchanged. This does not transfer semantic ownership to C4: C7 owns the product output identity and status, while C4 remains authoritative for source validity and metadata. C8 never reads raw C4 Track for Track-up. C6 may independently consume the normalized C4/C5 detector inputs it requires.

# 15. Estimated-Wind Derivation and Evaluation Schedule

## 15.1 Inputs and vector model

The estimator is inactive before C3 creates the Flight. After authorized Flight creation, C7 uses only the approved retained GNSS history beginning at the authoritative effective takeoff boundary and subsequent newly accepted active-Flight GNSS observations with valid GS, valid True-North Track, source monotonic time, and acceptable input quality.

```text
groundVelocityEastMps  = (GS / 3.6) * sin(trackDegTrue)
groundVelocityNorthMps = (GS / 3.6) * cos(trackDegTrue)
```

The model is:

```text
ground velocity = wind + air-relative velocity
```

For approximately stable horizontal AS magnitude, ground-velocity samples lie near a circle. The fitted centre estimates wind and the radius estimates AS. Stable AS is an observation property, not a Generator phase or altitude/VS label.

## 15.2 Fit and candidate windows

The bounded first estimator uses:

1. Pratt or Taubin circle initialization;
2. geometric radial least-squares refinement.

Kåsa-only fitting is not accepted for incomplete arcs. Pratt versus Taubin is reversible implementation tuning decided by deterministic numerical tests.

Candidate windows are:

```text
30 s / 60 s / 90 s / 120 s
```

A window is eligible only within one continuous valid GNSS segment and after its source-time span reaches its nominal duration. The shortest accepted window is the freshest accepted estimate.

Acceptance requires:

- acceptable input validity/quality;
- acceptable radial residual;
- adequate angular coverage;
- acceptable conditioning/geometric observability;
- acceptable centre uncertainty.

Diagnostics include radial RMSE, normalized RMSE/radius, a maximum or percentile residual, robust residual such as MAD, angular coverage, covariance/uncertainty, and a condition number or equivalent. Low residual alone is insufficient.

## 15.3 Flight-scoped activation and source-time evaluation schedule

Wind evaluation is driven only by accepted normalized GNSS observations and source monotonic time while a C3-created Flight is active.

On Flight creation, C7 processes the approved retained range `[effectiveTakeoffBoundary, takeoffConfirmationTime]` once in deterministic source order. The first eligible accepted GNSS observation at or after the authoritative effective takeoff boundary establishes:

```text
windScheduleEpochTime = observation.sourceMonotonicTime
nextEvaluationBoundary = windScheduleEpochTime + 1.0 s
```

Observations before the effective boundary, including pre-Start and unrelated ground observations, are excluded and cannot shift the epoch or any evaluation endpoint. Subsequently accepted observations after takeoff confirmation continue the same active-Flight history and schedule.

On each newly accepted active-Flight GNSS observation:

1. add it to eligible history;
2. if its source time reaches or crosses `nextEvaluationBoundary`, perform one evaluation using that observation as the endpoint;
3. advance `nextEvaluationBoundary` to the first epoch-aligned one-second boundary strictly greater than the current observation source time.

If several boundaries were crossed, exactly one evaluation occurs. No catch-up fit is created without a new observation. At most one evaluation occurs for one source timestamp.

Exact redelivery adds no history and triggers no evaluation. A gap, invalidity, or source-monotonic discontinuity invalidates dependent history and clears the schedule; the next eligible recovered observation begins a new continuity epoch using the same rule.

The same ordered source stream must produce equivalent endpoints and domain results under `1×`, `2×`, Pause/Resume, delayed delivery, batching, and exact redelivery.

No Generator phase, altitude phase, VS phase, host timer, playback speed, or sample count starts, stops, aligns, or gates estimator execution.

## 15.4 Accepted state

C7 exposes:

- `current`: the latest scheduled evaluation accepted a new estimate;
- `retained`: a previously accepted estimate remains usable after a later rejected candidate;
- `unavailable`: no accepted estimate exists or a hard invalidation cleared it.

A rejected candidate does not overwrite an accepted estimate. Hard invalidation clears landing eligibility.

The estimator retains separately:

```text
windEstimatorContractId
windEstimatorImplementationId
windEstimatorConfigurationVersion
```

Configuration identifies initialization, refinement, candidate windows, numerical thresholds, quality gates, uncertainty limits, and evaluation-schedule version.

Fixture-specific expected acceptance time is out-of-band validation reference evidence, never runtime phase knowledge.

# 16. Map and Spatial Behavior

## 16.1 Framework and renderer boundary

Flutter is the bounded application framework for this first slice, not a final whole-product commitment. Domain, lifecycle, detection, replay, derivation, recording, and Summary logic remains testable Dart independent of:

- widgets;
- `BuildContext`;
- Flutter view lifetime;
- map-view lifetime;
- plugin-specific types.

C8 uses:

```text
flutter_map
```

as the bounded renderer. OpenStreetMap Standard raster tiles are a replaceable online development provider for this slice.

## 16.2 Provider constraints

C8 contains:

- provider URLs and HTTP behavior;
- renderer/provider-specific types;
- attribution implementation;
- package lifecycle details;
- tile request and cache behavior.

Requirements:

- visible `© OpenStreetMap contributors` attribution;
- an identifying application User-Agent;
- normal HTTP caching semantics;
- current-view requests only;
- no prefetch;
- no pre-seeding;
- no bulk download;
- no offline package;
- explicit degraded spatial state;
- no permanent provider commitment.

## 16.3 Scale

The pilot marker is at the geometric centre of the full logical map viewport. That full logical viewport includes the area behind the temporary development panel.

The fixed scale is:

```text
2000 m +/- 2%
```

Visible ground width is the ground distance between the geographic positions under the left and right edges of the full logical viewport along its horizontal centreline. C8 computes the renderer-specific fractional zoom from the full logical viewport width and the current centre latitude.

A layout-size change may update fractional zoom only to preserve the same physical width. Rotation changes orientation, not scale. Ground/Flight state, replay/live mode, provenance, fixture metadata, scenario metadata, and C10 cannot select another scale. No fixture field may control zoom.

## 16.4 Orientation

Before replay delivery:

- no replay orientation observation exists;
- map is North-up;
- unavailable values remain unavailable.

After Start and before confirmed takeoff:

- valid Device True Azimuth produces ground device-orientation-up presentation;
- valid incoming compass changes rotate the map;
- invalid/unavailable ground orientation uses `North-up fallback`.

After confirmed takeoff:

- valid C7 True-North Track output produces `Track-up`;
- invalid/unavailable C7 Track output produces `North-up degraded fallback`;
- C8 does not bypass C7 or reinterpret raw C4 Track;
- Device True Azimuth is not an airborne fallback.

True North remains understandable in every mode.

## 16.5 Spatial degradation

If provider, network, coverage, tiles, or renderer is unavailable, C8 shows an explicit neutral degraded spatial state and `Map unavailable`. It must not display misleading current coverage or silently switch providers. Map degradation does not change Flight Mode, Flight lifecycle, detector state, derivations whose non-map inputs remain valid, recording, finalization, or Summary.

## 16.6 Ground and estimated-wind presentation

Before takeoff, the bounded presentation shows:

- current map position;
- barometric Altitude MSL;
- weather-source wind;
- compact replay controls.

Weather-source wind occupies the primary information location later used by C1 for C7's Ground Speed output. Unavailable, valid zero, stale, degraded, and uncertain states remain distinguishable where supplied by the normal contracts.

After C7 accepts estimated wind, a simplified windsock-like representation appears in the compass/orientation context:

- the aerodrome-windsock analogy makes the into-wind landing direction understandable;
- visible length or sections communicate magnitude;
- a numeric value remains visible;
- display attempts approximately `0.5 m/s` granularity.

The display granularity is not a calculation-accuracy claim. Exact geometry, section styling, gradients, warning thresholds, blinking, placement, and dimensions remain bounded presentation tuning.

## 16.7 Compact replay panel

The development-only panel shows:

- Start/Pause;
- Reset with section 9 eligibility;
- `1×`/`2×`;
- replay position or elapsed source time.

It exposes no Generator phase, truth, or expected result. Detailed diagnostics remain outside the normal pilot-facing panel.

## 16.8 Estimated-wind limitation explanation

While no Flight is active, the `Ready on Ground` Flight Screen provides a labelled `Estimated wind info` action adjacent to the weather-wind context. It is available before Start and remains a non-flight help action while the screen is `Ready on Ground`.

Activating it opens a dismissible, non-blocking information surface without starting, pausing, resetting, or otherwise changing replay/Flight state. The minimum text is:

> In-flight wind is estimated from Flight data. Short-term changes may come from pilot input, climb or descent, wing behavior or configuration, turbulence, or actual wind variation. AirLink cannot reliably separate these effects. In-flight gust estimation is not included in MVP 0.1.

Dismissing the surface returns to the unchanged `Ready on Ground` state. The active-Flight presentation does not require a persistent static warning and must not imply direct wind truth or gust estimation.

# 17. Recording Outcomes and Retained-Result Contract

## 17.1 Recording state and handoffs

Logical recording states are:

```text
unavailable
active_complete
active_incomplete
failed
finalized_complete
finalized_incomplete
```

Explicit handoff results are:

- initialization: `initialized` or `unavailable`;
- append health: `active_complete`, `active_incomplete`, or `failed`;
- finalization: `finalized_complete`, `finalized_incomplete`, or `finalization_failed`.

`finalization_failed` maps to a failed usable-record outcome without changing C3 lifecycle truth.

The bounded transition rules are:

| Current state | Condition/result | Next state | Rule |
| --- | --- | --- | --- |
| no recorder | initialization `unavailable` | `unavailable` | No usable active recorder was initialized for this Flight; no finalized usable record can result |
| no recorder | initialization `initialized` | `active_complete` | Initial successful append state |
| `active_complete` | mandatory append succeeds | `active_complete` | Completeness remains intact |
| `active_complete` | transient append failure with retry still possible | `active_complete` | Retry is allowed only before mandatory loss is declared; finalization waits for success, declared loss, or failure |
| `active_complete` | mandatory recording-contract item is irretrievably missed | `active_incomplete` | The established gap is irreversible |
| `active_incomplete` | later append succeeds | `active_incomplete` | Later data cannot hide established mandatory loss |
| `active_complete` or `active_incomplete` | recorder becomes unusable | `failed` | Terminal for the usable record |
| `active_complete` | finalization succeeds | `finalized_complete` | The only successful complete finalization |
| `active_complete` | finalization fails | `failed` | Handoff result is `finalization_failed`; no usable finalized record |
| `active_incomplete` | finalization succeeds | `finalized_incomplete` | Incomplete record remains ineligible for successful Summary |
| `active_incomplete` | finalization fails | `failed` | Handoff result is `finalization_failed`; no usable finalized record |
| `failed` | any later append/finalization attempt | `failed` | Failed cannot produce a finalized usable record |

The transient retry is an internal pending condition, not another logical recording state. `unavailable`, `failed`, `finalized_complete`, and `finalized_incomplete` are terminal for this Flight's recorder. Once mandatory data is conclusively lost, no later append may return the record to `active_complete`.

Quality is separate:

```text
quality = nominal | degraded
```

Loss of mandatory recording-contract data makes the record incomplete. A retained outage/invalidity interval may make quality degraded while structural completeness remains complete.

Only `finalized_complete` is eligible for successful Summary. A degraded `finalized_complete` record is clearly presented as degraded. `finalized_incomplete`, unavailable, and failed outcomes do not produce a successful Summary.

Quality changes never alter the completeness transition graph. No recording transition changes C3 lifecycle truth.

## 17.2 C3 to C9 creation handoff

Before the first ordinary retainable active-Flight update, C3 supplies:

- `developmentSessionId`;
- `flightId`;
- `flightClassification = syntheticTestFlight`;
- replay stream ID/version and replay origin as separate context;
- active lifecycle state;
- effective takeoff boundary and source-observation reference;
- takeoff confirmation source time;
- Takeoff Point identity, position, accuracy/state, classification, and Flight association;
- detector contract/configuration identifiers;
- approved bounded-history range;
- immutable calculation-profile identifiers.

If C9 cannot retain mandatory identity/boundary/classification context, initialization returns `unavailable`. The Flight still exists.

## 17.3 C3 to C9 completion handoff

C3 supplies:

- `flightId`;
- completed lifecycle state;
- effective landing boundary and source-observation reference;
- landing confirmation source time;
- Landing Point identity, position, accuracy/state, confirmed-landing classification, and Flight association;
- detector contract/configuration identifiers;
- final confirmation-tail range.

C9 retains this context and the final tail before finalization. Missing mandatory completion context yields an incomplete or failed usable-record outcome, not a lifecycle change.

## 17.4 Flight-level envelope

The in-memory record retains once:

- development-session and Flight identity;
- authoritative Flight classification;
- delivery mode, replay origin, stream ID/version as separate fields;
- effective/confirmation boundaries;
- Takeoff and Landing Point representations;
- calculation profile and shared constants/configuration;
- recording state and quality;
- continuity/gap intervals;
- metric interval and confirmation-tail evidence interval.

## 17.5 Normalized source layer

For every approved retained event in the Flight range, retain sufficient normal-boundary information:

- four-part replay identity or stable observation reference;
- source category;
- value and unit;
- source monotonic time and equal-time sequence;
- availability;
- validity;
- freshness/freshness inputs;
- quality/accuracy;
- course accuracy where Track supplies it;
- category-level provenance;
- delivery handling;
- interruption/gap transitions;
- AirLink-observed delivery time where needed.

The first slice retains all accepted approved events in memory without downsampling. Exact redelivery is referenced once. Data before effective takeoff is excluded; detector evidence after effective landing is retained but tagged outside Summary metrics.

## 17.6 Derived/domain layer

For each retained accepted derivation, retain:

- value and unit;
- availability/quality state;
- effective source time or source-time range;
- semantic calculation contract ID;
- implementation/configuration version where needed;
- retained input observation references or input range.

Retained C7 product outputs include Ground Speed and True-North Track value/status with references to their C4 observations where required. Detector context retains the takeoff `courseAccuracyDeg <= 10.0` gate result without moving source-accuracy ownership out of C4.

C9 does not recalculate, repair, or improve C7 values. Rejected wind candidates remain diagnostics unless needed to interpret an accepted outcome.

# 18. Summary

Summary is created only from a `finalized_complete` C9 record. It never uses transient widget state or copies the active runtime aggregate as the authoritative completed result.

The Summary displays:

- `Synthetic test Flight`;
- duration from effective boundaries;
- distance from retained GNSS positions and continuity boundaries;
- valid covered time;
- average GS;
- maximum valid GS;
- maximum derived Altitude MSL;
- maximum derived Height above Takeoff;
- maximum valid climb VS;
- maximum valid descent VS;
- last accepted estimated wind;
- recording quality/status.

Summary uses retained accepted C7 outputs for Ground Speed, altitude, VS, and estimated wind. It does not run a second wind estimator or VS estimator.

Maximum valid GS uses retained C7 AirLink Ground Speed outputs. C1 never reconstructs pilot-facing GS from raw C4 observations.

## 18.1 Distance and covered time

Active distance and finalized Summary distance are independent calculations using:

```text
haversineMeanEarthR6371008_8V1
```

and the same continuity policy:

- sum adjacent accepted positions only within one uninterrupted valid segment;
- do not bridge availability, invalidity, interruption, or source-time-discontinuity boundaries;
- the first recovered position is an anchor and contributes no cross-gap distance;
- exact redelivery contributes no second point or interval.

Pair eligibility is determined by this continuity-segment policy before applying the pairwise formula. For eligible accepted geographic positions `p1` and `p2`, convert latitude/longitude to radians and use:

```text
R = 6371008.8 m

deltaPhi = phi2 - phi1
deltaLambda = shortest signed longitude difference in radians

h
=
sin(deltaPhi / 2)^2
+ cos(phi1) * cos(phi2) * sin(deltaLambda / 2)^2

h = clamp(h, 0, 1)

centralAngle
=
2 * atan2(sqrt(h), sqrt(max(0, 1 - h)))

distanceM = R * centralAngle
```

The semantic contract is:

```text
haversineMeanEarthR6371008_8V1
```

Neither active nor finalized distance may use truth distance, local Generator East/North displacement, GS integration, interpolation, dead reckoning, or route reconstruction.

Valid covered time is the sum of source-time intervals for the same accepted within-segment position pairs. Average GS is:

```text
valid segment distance / valid covered time
```

using:

```text
validSegmentDistanceOverCoveredTimeV1
```

The active and finalized calculations remain independent and are separately testable so divergence or recording omission is detectable.

## 18.2 Derived extrema and Height above Takeoff

Altitude and Height extrema use retained valid derived outputs only. VS extrema use retained valid outputs of `olsAltitudeSlope3sMin20Span2sV1`; unavailable intervals do not create zero values.

Height above Takeoff uses the authoritative effective-boundary derived-altitude baseline. If unavailable, the metric is unavailable rather than substituted.

# 19. Degradation Boundaries

| Condition | Required result | Must not happen |
| --- | --- | --- |
| Pre-delivery | North-up; source values unavailable | Synthesized orientation/value |
| Ground orientation unavailable | North-up fallback | Treat Track or zero as Device True Azimuth |
| Airborne Track unavailable | North-up degraded fallback | Device True Azimuth fallback |
| Weather/Track unusable for takeoff correction | Zero weather correction | Tailwind/crosswind lowering required proxy GS |
| Pressure/QNH unavailable | Altitude/VS dependency unavailable as applicable | Zero or Generator truth altitude |
| GNSS gap/invalidity/discontinuity | Reset detector/wind dependent history; break distance segment | Interpolation, catch-up fit, lifecycle outcome |
| Map/provider unavailable | Explicit C8 degradation; non-map path continues | Lifecycle/detector/recording change |
| C10 collision/incompatibility | Fail closed before conflicting valid delivery | Best-effort continuation with ambiguous event |
| Recording quality degradation with complete structure | `finalized_complete`, `quality=degraded` may show degraded Summary | Mark structurally incomplete solely because input quality degraded |
| Missing mandatory retained data | Incomplete/failed usable record; no successful Summary | Nominal or successful Summary |
| Recording unavailable/failure | Flight completion remains true; show record unavailable | Cancel/reopen/reject Flight |

## 19.1 Controlled interruption boundary

The controlled interruption case is limited to:

- explicit source-equivalent availability/interruption events delivered through C10 to C4;
- downstream knowledge only through normal C4-derived state;
- C3 exposing that an active Flight encountered the boundary;
- C9 exposing technical gap, degradation, or incompleteness state.

No final P3 product classification is assigned. The case does not decide continuation, retention, finalization, Summary availability, false-takeoff discard, landing, Flight Mode exit, or production recovery.

# 20. Observability and Diagnostics

Observability is replaceable validation support, not an alternate product authority.

Minimum structured snapshots/events expose:

- Development Session Coordinator state, eligibility, teardown steps, new session ID, and fail-closed reset outcome;
- C10 stream identity/version/origin/integrity, playback state, cursor, source progression, four-part event identity, ordering, batching, delay, transforms, redelivery, collisions, and delivery outcomes;
- C4/C5 active mode, normalized GS/Track observations, category provenance, delivery handling, source/observed time, availability, validity, freshness, quality/accuracy, and continuity;
- C2 state and authorization outcomes;
- C6 qualification, candidate, takeoff course-accuracy gate input/result, hold, cancellation/timeout, reset reason, effective boundary, confirmation time, and detector version;
- C3 Flight identity, classification, lifecycle, special points, and recording-outcome handoffs;
- C7 AirLink Ground Speed and True-North Track output status, calculation profile, Flight-scoped estimator activation/range, schedule epoch/boundaries/endpoints, windows, quality metrics, accepted/rejected state, derivation availability, and retained input references;
- C8 orientation mode/source, proof that Track-up consumed C7 Track rather than raw C4 Track, True North state, scale, provider state, attribution state, and degradation;
- C9 recent-history range, initialization result, append retry/loss declaration, state transition, completeness, quality, retained counts/ranges, finalization result, and explicit discard;
- Summary source record identity, independent aggregate results, and comparison evidence.

Normal runtime diagnostics contain no Generator phase, truth, physical liftoff/touchdown, expected detector result, or expected Summary value. Out-of-band comparisons are performed by the validation harness after runtime output is produced.

# 21. Independent Validation Strategy

## 21.1 Independence rule

Expected results are not generated by calling the runtime implementation under test. The validation producer uses an independently implemented method, reviewed reference calculation, or analytically derived evidence. Normal C1–C10 runtime is structurally unable to access the reference.

Structural proof includes:

- reference files are outside bundled runtime assets;
- runtime packages have no dependency/import path to reference readers or Generator packages;
- C10 accepts only the runtime fixture manifest and runtime-stream inputs;
- the runtime manifest has no field containing a reference identity, digest, path, filename, URI, URL, package/lookup key, or other location hint;
- no runtime parser, dependency, asset, configuration, or generated resource exposes reference location or content;
- the validation-only association occurs outside runtime after outputs are captured;
- build/asset inventory verifies reference files are absent from the application bundle;
- an automated dependency/asset test fails if a runtime target gains reference access;
- comparison code runs in a test-only harness after runtime outputs are captured.

## 21.2 Required validation groups

The implemented slice must provide:

1. manifest, checksum, compatibility, order, identity, terminal, and malformed-stream tests;
2. `1×`, `2×`, Pause/Resume, delay, batching, and exact-redelivery equivalence tests;
3. changed-payload identity collision fail-closed tests;
4. classification-axis tests proving replay does not imply synthetic classification and C9 does not infer classification;
5. takeoff tests for qualification, same-observation ordering, direct/partial headwind, crosswind, tailwind, unusable weather/Track, cancellation, timeout, tie priority, gaps, and redelivery, including `courseAccuracyDeg` below `10.0`, exactly `10.0`, just above `10.0`, unavailable, and non-finite;
6. landing tests for moving/stationary rules, missing Track, missing GS, exact confirmation, hysteresis reset, strict hard cancellation, wind requirement, gaps, and redelivery;
7. altitude formula and invalid-input tests;
8. VS fit, minimum history, batching, gap, discontinuity, and redelivery tests;
9. Flight-scoped wind activation tests proving the estimator is inactive before C3 creates the Flight; pre-Start and pre-effective-boundary observations cannot enter windows or shift evaluation endpoints; deterministic source-order reconstruction of `[effectiveTakeoffBoundary, takeoffConfirmationTime]`; the first eligible accepted observation at or after the effective boundary anchors the initial epoch; continuation with newly accepted observations; recovered-epoch reset; scheduler endpoint equivalence; sample count cannot gate or align evaluation; and numerical estimator tests for ideal, noisy, incomplete-arc, poorly conditioned, and outlier cases;
10. proof that wind acceptance depends on observations/quality gates rather than Generator phase/truth;
11. C4/C7/C1/C8 ownership tests proving C4 retains normalized GS/Track validity/quality/provenance/timing, C7 exposes semantic Ground Speed/Track output status, C1 displays C7 Ground Speed, C8 uses C7 Track without raw-C4 reinterpretation, plus pre-delivery, ground Device True Azimuth, ground fallback, airborne Track-up, airborne fallback, ground position/altitude/weather-wind presentation, and weather-wind-to-GS transition cases;
12. full-logical-viewport scale tests covering the centred pilot, area behind the development panel, left/right centreline ground-width measurement, centre-latitude fractional zoom, layout resizing, rotation independence, prohibited scale selectors, attribution, request policy, map degradation, windsock direction/magnitude/numeric/`0.5 m/s` granularity, and replay-position/elapsed-source-time readout;
13. recording-transition tests covering initialization unavailable/initialized, initial `active_complete`, transient retry, irreversible transition to `active_incomplete`, no return to complete, terminal `failed`, allowed finalization edges, quality independence, confirmation-tail retention, Summary eligibility, and no C3 lifecycle mutation;
14. independent active-versus-finalized distance and Summary tests covering pair eligibility before the complete clamped haversine formula, exact `R = 6371008.8 m`, shortest longitude difference, and prohibition of truth/local-East-North/GS-integration/interpolation/dead-reckoning/route-reconstruction shortcuts;
15. controlled interruption tests that stop at the P3 boundary;
16. coordinated Reset eligibility, explicit record discard, new session ID, same stream ID, and fail-closed failure tests;
17. runtime-reference isolation and bundle-content tests proving the runtime manifest and every runtime parser, dependency, asset, and configuration expose neither validation-reference content nor identity/digest/path/filename/URI/URL/package/lookup location hints, while the validation-only artifact associates the stream and reference outside runtime;
18. wall-clock-adjustment and source-monotonic-discontinuity tests proving Flight duration, detector holds/timeouts, wind scheduling, and retained ordering use source/normalized monotonic semantics rather than mutable wall clock, host time, or playback speed;
19. C3-to-C8 Takeoff Point identity/position/Flight-association and passive-Current-Waypoint acknowledgement tests proving Active Navigation remains off;
20. `Estimated wind info` location, exact minimum text, non-blocking/dismiss behavior, no-state-change behavior, no persistent in-Flight-warning requirement, and no truth/gust implication tests.

## 21.3 Concrete artifact blocker

There is no concrete fixture-package blocker to owner approval of this plan. Before C10/downstream integration validation and implemented-slice acceptance, the four-artifact validation package in section 8.1 is mandatory. If any one artifact is missing then, that artifact—not all of issue #47—is the blocker.

# 22. Bounded Technical Decisions

The following decisions are limited to this slice:

1. Flutter is the application framework for the slice.
2. Product/domain logic is testable Dart isolated from Flutter and plugin types.
3. `flutter_map` is the C8 renderer.
4. OpenStreetMap Standard raster tiles are the replaceable online development provider.
5. The map width is `2000 m +/- 2%`, calculated inside C8.
6. The fixture interchange uses deterministic JSON manifest, NDJSON runtime stream, and SHA-256 integrity evidence.
7. The Flight record is in memory only and retains all approved events/derivations without downsampling.
8. Calculation contracts and complex estimator implementation/configuration are semantically versioned.
9. Structured diagnostics are a replaceable read-only snapshot/event mechanism, not a permanent telemetry architecture.
10. The Development Session Coordinator is a temporary harness responsibility.

These decisions do not select a permanent framework/provider, durable schema, production replay store, complete module graph, dependency-injection framework, or final application architecture.

# 23. Implementation Decomposition for AL-0003

This decomposition is planning input only. It does not activate AL-0003, create issues, authorize code, define a complete backlog, or require one issue per increment/PR.

## First bounded implementation issue — Development foundation and pre-delivery state

The first AL-0003 implementation issue must be smaller than the complete Ready-on-Ground milestone. Its bounded scope is:

- Flutter development foundation and development target;
- a Development Session context with `developmentSessionId` and immutable calculation-profile identity;
- temporary development entry into pre-delivery `Ready on Ground`;
- neutral replay contract/model seams and Start/Pause/`1×`/`2×`/Reset control intent without normal C4/C5 source delivery;
- a neutral North-up spatial placeholder, not `flutter_map` or an online tile provider;
- tests proving no Flight exists and no source value is available or delivered before Start.

The observable result is a development build showing a coherent pre-delivery `Ready on Ground` state and controls without implying Flight creation or source availability.

This first issue excludes:

- runtime-stream parsing/integrity integration beyond the neutral contract seam;
- normal C4/C5 ground delivery;
- C7 Ground Speed, Track, altitude, or orientation output;
- real map rendering or OSM requests;
- detector, recording, or Summary behavior.

No issue is created by this plan.

## Early milestone / Increment 1 — Replay-backed Ready on Ground

The larger early milestone may span multiple small independently reviewed Draft PRs. It is complete when:

- the bounded Flutter development entry opens the Flight Screen in `Ready on Ground`;
- the wind limitation explanation is available;
- the validated frozen-stream boundary, four-part identity, C10 Start/Pause/`1×`/`2×`, and normal C4/C5 delivery path exist for required ground categories;
- pre-delivery North-up and unavailable state are visible;
- after Start, C7 exposes semantic Ground Speed/Track status and valid Device True Azimuth rotates the presentation;
- current map position, barometric Altitude MSL, weather-source wind, and replay position/elapsed source time are presented while on the ground;
- fixed scale, attribution, User-Agent/request policy, and map degradation are exercised;
- no Flight exists and no detector/record/summary behavior is claimed.

Suggested bounded follow-ups within the milestone are:

1. **Normal ground delivery and C7 semantic outputs:** validate/activate the stream, deliver required categories through normal C4/C5 boundaries, expose C7 Ground Speed/True-North Track product status, barometric altitude, and Device True Azimuth on the neutral spatial presentation.
2. **Real C8 map adapter:** replace the placeholder with isolated `flutter_map`/OSM rendering, exact full-logical-viewport scale, attribution/request policy, C7 orientation inputs, and explicit map degradation.

These follow-ups may be separate issues or multiple small Draft PRs under an approved bounded issue. The milestone is not a requirement that one issue or PR implement all of its behavior.

## Increment 2 — Takeoff and authoritative Flight creation

Add transient recent history, takeoff qualification/proxy/confirmation, the inclusive `courseAccuracyDeg <= 10.0` detector gate, C2/C3 authority, Flight classification, Takeoff Point/passive Current Waypoint state, C9 initialization outcomes, and the Flight-scoped C7 handoff that admits only `[effectiveTakeoffBoundary, takeoffConfirmationTime]` history.

## Increment 3 — Active Flight derivations

Add C1 consumption of C7 Ground Speed, C8 consumption of C7 Track, elapsed time, active/finalized-independent distance with the complete haversine contract, pressure altitude, Height above Takeoff, VS, Flight-scoped wind schedule/estimator, retained calculation context, and active presentation.

## Increment 4 — Landing, recording finalization, and Summary

Add landing estimated AS/detection, Landing Point, confirmation-tail retention, recording terminal outcomes, finalized record, and successful/degraded/unavailable Summary behavior.

## Increment 5 — Delivery/degradation matrix and fresh-session Reset

Complete batching, delay, redelivery, collision, controlled interruption boundary, recording failure cases, map/provider failures, coordinated Reset, independent validation package comparisons, and acceptance evidence.

# 24. Implementation-Readiness Criteria

Implementation may be prepared for AL-0003 only when:

- this plan is owner-approved and merged;
- the separate AL-0003 transition is approved and active;
- the first bounded implementation issue references only its applicable plan sections;
- the first issue is limited to section 23's development-foundation/pre-delivery scope and does not absorb the complete early milestone;
- no protected product/governance artifact must change to start;
- the runtime fixture/manifest/reference/isolation contracts are accepted, even if the concrete package is not yet produced;
- calculation profile, semantic contract IDs, detector semantics, recording states, and classification axes are fixed as described;
- framework/map decisions are understood as slice-bounded;
- package versions can be selected at implementation time without changing these contracts;
- reference evidence remains inaccessible to runtime by construction;
- the first issue has explicit tests, observable result, non-goals, and stop conditions;
- any concrete package needed by the increment exists before its integration validation.

# 25. Definition of Done for the Implemented Slice

The later implemented slice is done only when:

- the pilot-visible path in section 2 works in a development build;
- C10 consumes a validated frozen stream and never generates source values;
- live/replay-compatible C4/C5-facing boundaries and all classification axes remain distinct;
- C4 retains normalized GS/Track source-state ownership while C7 supplies pilot-facing Ground Speed/Track semantics to C1/C8;
- takeoff and landing detectors obey the exact experimental source-time contracts;
- runtime derivations use the fixed calculation profile and never consume Generator truth;
- wind endpoints/results are delivery-equivalent across the required delivery modes;
- C8 meets full-logical-viewport scale, C7-Track orientation, attribution, request, and degradation requirements;
- C9 follows the bounded transition graph and produces the required recording states, quality, retained layers, and explicit discard behavior;
- only `finalized_complete` produces successful Summary;
- Summary is independently recomputed from the finalized record;
- controlled interruption stops at the P3 boundary;
- fresh-session Reset is coordinated and fail-closed;
- the four-artifact package exists and normal runtime cannot access its reference evidence;
- all validation groups pass;
- no durable persistence, live platform integration, Generator implementation, or deferred product behavior has been introduced;
- documentation records any accepted tuning and final calculation configuration versions.

# 26. Bounded Implementation Tuning

Before acceptance evidence is frozen, implementation may tune only reversible choices that do not change owner-approved semantics:

- Pratt versus Taubin initialization;
- numerical optimizer convergence details;
- numerical thresholds for residual, angular coverage, conditioning, and uncertainty;
- presentation spacing, typography, and non-semantic animation;
- diagnostic serialization/details;
- package patch/minor versions compatible with the approved boundary.

Tuning must not change:

- source-time schedule;
- detector thresholds, comparisons, holds, timeout, tie order, or stationary rule;
- classification meaning;
- recording state/quality meaning;
- map scale;
- orientation fallback policy;
- Summary eligibility or formulas;
- runtime/reference isolation.

Once out-of-band validation reference evidence is frozen, any changed wind estimator configuration requires:

- a new `windEstimatorConfigurationVersion`; and
- regenerated reference evidence.

The same version rule applies to any calculation-profile change that alters expected results.

# 27. Explicitly Deferred Decisions

Deferred work includes:

- final whole-product framework and code-sharing strategy;
- final map/provider, offline maps, caching architecture, and user map controls;
- production Android/iOS source integration, permission, lifecycle, background, and recovery models;
- durable Flight identity, storage, schemas, migrations, saved review, retention, and deletion UX;
- multiple Flights in one implemented session;
- active-Flight exit, manual completion, false-detection discard, and inactivity timeout behavior;
- final P3 interruption continuation/retention/finalization/Summary/recovery semantics;
- production detector algorithms and safety thresholds;
- final wind estimator and safety/usefulness acceptance;
- final Flight Screen and passive Takeoff Point presentation;
- Scenario Generator phases, formulas, truth, cadence/error profiles, architecture, technology, UX, and reusable productization;
- normalized recorded-real-Flight production;
- complete replay storage/selection UI and a complete backlog.

# 28. Stop Conditions

Implementation planning or later implementation must stop if work would:

- make C10 generate or reinterpret simulated source values;
- give runtime access to Generator phase, truth, physical transitions, formulas, or expected answers;
- infer Flight classification from replay mode, origin, observations, or delivery handling;
- change an owner-approved detector, recording, Reset, orientation, Summary, or validation semantic;
- assign a final P3 outcome;
- add durable persistence or live platform integration;
- make Flutter, `flutter_map`, or OSM a permanent whole-product commitment;
- introduce a complete architecture, schema, API, or backlog;
- require a protected governance change not separately authorized;
- expose mixed old/new state during Reset;
- use the same runtime implementation to generate expected validation evidence;
- discover a material conflict among current canon, iteration scope, issue scope, or Product Direction.

# 29. Product Direction Alignment

**Trigger:** preparation of a product-significant implementation plan covering Flight lifecycle, major Flight presentation behavior, safety-relevant semantic distinctions, and bounded technical choices.

**Direction advanced:** the plan prepares one coherent, pilot-visible Flight Support outcome using normal lifecycle, derivation, spatial, recording, and completion responsibilities.

**Explicit simplification:** one replay-driven synthetic test Flight, temporary development entry, in-memory retention, experimental detectors/estimator, no live source, and no durable storage. These are authorized by `ITERATION.md`, issue #37, the selected-slice record, and the final owner decisions.

**Bounded and reversible:** runtime consumes source-equivalent inputs through normal boundaries; framework/provider choices are isolated; calculation contracts are versioned; omitted future domains remain explicit; Generator work remains independently schedulable.

**Long-term concepts deferred:** broader preparation/history, durable retention, live mobile behavior, later Flight Support domains, Pilot Ecosystem, and complete Generator productization.

**No hidden behavior:** the plan assigns no final P3 outcome, uses no privileged truth, introduces no alternative replay lifecycle, and does not promote WIP to canon.

**Outcome:** `Aligned with explicit simplification`.

No Product Vision, Product Direction, Current State, iteration, or governance revision is required.

# 30. Final Owner Decision Record

The following issue #37 decisions are final and are implemented by the cited plan sections:

| Group | Final decision | Plan sections |
| --- | --- | --- |
| 1 | Plan approval may precede a concrete package; four separate artifacts are mandatory before integration validation and slice acceptance; only a specific missing artifact may block | 8, 21 |
| 2 | C3 assigns `syntheticTestFlight`; replay/delivery/provenance axes remain independent; Summary says `Synthetic test Flight` | 5.1, 7, 17, 18 |
| 3 | Recording completeness and quality are independent; transition rules make mandatory loss irreversible; only `finalized_complete` yields successful Summary; failure preserves Flight completion | 17, 18, 19 |
| 4 | C10 Reset is replay-only; the temporary Development Session Coordinator performs fail-closed concern-owned fresh-session reset and explicit record discard | 9 |
| 5 | Estimated-wind activation is Flight-scoped from the authoritative effective boundary; evaluation is driven by accepted GNSS observations and epoch-aligned source time with one evaluation per endpoint and no catch-up | 13, 15.3 |
| 6 | Flutter, `flutter_map`, and OSM Standard are bounded slice choices; Dart domain isolation, attribution, request policy, degradation, and fixed full-logical-viewport `2000 m +/- 2%` width are required | 16, 22 |
| 7 | Pre-delivery is North-up; valid ground Device True Azimuth rotates after Start; airborne uses Track-up with North-up degraded fallback and no compass fallback | 14.5, 16.4 |
| 8 | Takeoff uses experimental GS qualification, discounted weather-headwind Airspeed proxy, inclusive `courseAccuracyDeg <= 10.0` gate, exact holds/cancellation/timeout/tie, and C6→C2→C3→C9 authority | 11 |
| 9 | Landing requires estimated wind, stationary-vector rule, exact entry/hold/hysteresis/hard cancellation, and effective-boundary metric end with retained confirmation tail | 12 |
| 10 | C9 pre-Flight history is transient, bounded to 15 seconds, and only the authoritative effective-boundary-to-confirmation range enters the record | 13 |
| 11 | C6/C7/Summary keep product calculations; only simulated source-value generation moves to Generator | 7, 11–15, 18 |
| 12 | Every session has immutable calculation profile; semantic calculation and estimator IDs/configuration are retained and versioned | 9.1, 14.1, 15.4, 17 |
| 13 | C9 retains normalized source and derived/domain layers with observation references and calculation context; it never recalculates C7 values | 17 |
| 14 | Summary exists only from `finalized_complete`, recomputes from retained C7 data, and uses an independent complete haversine calculation | 18 |
| 15 | Validation evidence is independently produced and associated only by the validation harness; its identity, location, and content remain structurally inaccessible to C1–C10 | 8, 20, 21 |

No decision group above remains open.

# 31. Draft PR #44 Decision Traceability

## 31.1 Disposition classes

This appendix uses exactly:

- `retained for AirLink product runtime`;
- `rewritten under the replay-only boundary`;
- `moved/preserved for Scenario Generator / #47`;
- `replaced by the #46 replay/materialization contract`;
- `discarded as obsolete or over-scoped`;
- `still requiring owner decision`.

The last class has zero rows: all issue #37 owner-decision groups are resolved, and remaining Generator phase semantics belong to #47 rather than blocking this plan.

## 31.2 Major PR #44 content

Each row is one meaningful PR #44 section or decision group. Mixed sections are split so runtime and Generator formulas are not collapsed.

| ID | PR #44 source | Disposition | Destination and reason |
| --- | --- | --- | --- |
| T01 | README index entry | discarded as obsolete or over-scoped | Replace with the fresh plan entry only |
| T02 | Status, authority, purpose, source basis | rewritten under the replay-only boundary | This document's Status, Source Basis, and Purpose use current authority |
| T03 | §1 Selected Slice | rewritten under the replay-only boundary | §1 keeps the selection but defines simulation-driven as materialized replay |
| T04 | §2 pilot-visible acceptance | retained for AirLink product runtime | §2 preserves the coherent waiting-to-Summary path |
| T05 | §2 engineering acceptance | rewritten under the replay-only boundary | §§8 and 20–21 keep reference identity/location outside the runtime manifest/bundle and use validation-only association |
| T06 | §3.1 one Flight per development session | retained for AirLink product runtime | §§2 and 9 preserve the harness restriction |
| T07 | §3.2 removal of passive Current Waypoint | discarded as obsolete or over-scoped | Current selection keeps the logical passive Current Waypoint |
| T08 | §3.3 reduced C8 presentation | rewritten under the replay-only boundary | §16 keeps only approved scale, orientation, provider, and degradation contracts |
| T09 | §3.4 no durable retained Flight | retained for AirLink product runtime | §§4 and 17 keep in-memory-only retention |
| T10 | §4 included behavior | rewritten under the replay-only boundary | §3 replaces runtime simulation with frozen replay and corrected orientation/recording |
| T11 | §5 non-goals | rewritten under the replay-only boundary | §4 adds Generator/runtime-generation exclusions |
| T12 | §6.1 horizontal motion terminology | rewritten under the replay-only boundary | §5.2 removes truth AS/Air Heading from runtime |
| T13 | §6.2 vertical/altitude terminology | retained for AirLink product runtime | §§5.2 and 14 preserve VS/MSL/Height distinctions |
| T14 | §6.3 wind terminology | rewritten under the replay-only boundary | §5.3 puts truth wind out of band |
| T15 | §6.4 scenario/virtual time model | replaced by the #46 replay/materialization contract | §§5.4 and 8 use frozen source time and delivery time |
| T16 | §7.1 inputs | rewritten under the replay-only boundary | §6 accepts frozen events and delivery controls, not scenario controls |
| T17 | §7.2 outputs | rewritten under the replay-only boundary | §6 separates replay, classification, retained, and diagnostic outputs |
| T18 | §8 C1 | retained for AirLink product runtime | §7 preserves presentation responsibility |
| T19 | §8 C2 | retained for AirLink product runtime | §7 preserves Flight Mode authority |
| T20 | §8 C3 hardcoded simulated meaning | rewritten under the replay-only boundary | §§5.1 and 7 require C3 `syntheticTestFlight` assignment with independent axes |
| T21 | §8 C4/C5 simulator-source boundaries | replaced by the #46 replay/materialization contract | §§7–8 use normal C4/C5-facing replay delivery |
| T22 | §8 C6/C7 runtime calculation | retained for AirLink product runtime | §§5.2, 7, 11–15 retain C6 detector inputs and restore C7 ownership of AirLink Ground Speed/True-North Track semantics |
| T23 | §8 C8 | rewritten under the replay-only boundary | §§7 and 16 require C8 to consume C7 Track without reinterpreting raw C4 Track and apply final orientation/provider decisions |
| T24 | §8 C9 | rewritten under the replay-only boundary | §17 supplies final state, quality, classification, and two-layer contracts |
| T25 | §8 old C10 Simulation and Validation Enablement | replaced by the #46 replay/materialization contract | §§7–8 use Replay and Source Delivery Enablement |
| T26 | §9 development entry/initial ground | rewritten under the replay-only boundary | §§2 and 9 use prepared replay context and pre-delivery distinction |
| T27 | §9 Start as scenario progression | replaced by the #46 replay/materialization contract | §8.5 Start begins frozen event delivery |
| T28 | §9 takeoff/active/landing authority | retained for AirLink product runtime | §§10–12 preserve normal authority chains |
| T29 | §9 completed state and Reset | rewritten under the replay-only boundary | §9 separates replay Reset from coordinated session teardown/discard |
| T30 | §10 Flight Screen product role | retained for AirLink product runtime | §§2 and 16 preserve map-centred pilot-visible outcome |
| T31 | §10 detailed layer/overlay geometry | discarded as obsolete or over-scoped | Complete layout remains outside §16's bounded behavior |
| T32 | §10 fixed `2000 m +/- 2%` scale | retained for AirLink product runtime | §16.3 defines the full logical viewport, centred pilot, edge-to-edge centreline measurement, centre-latitude fractional zoom, and invariant selectors |
| T33 | §10 active distance and value semantics | retained for AirLink product runtime | §§2 and 18 retain independent active/finalized metrics and the complete normative haversine formula |
| T34 | §10 windsock experiment | rewritten under the replay-only boundary | §2 retains estimated-wind presentation outcome without fixing complete geometry |
| T35 | §10 minimal one-line wind notice | discarded as obsolete or over-scoped | §2 uses the owner-required fuller limitation explanation |
| T36 | §10 pilot-visible degradation | retained for AirLink product runtime | §19 preserves bounded degraded states |
| T37 | §11 source/observed/wall time | rewritten under the replay-only boundary | §§5.4 and 8 preserve distinctions without runtime scenario clocks |
| T38 | §11 duplicate/collision identity | replaced by the #46 replay/materialization contract | §8 uses the four-part identity |
| T39 | §11 generated civil-time jump | moved/preserved for Scenario Generator / #47 | #47 may decide source civil-time materialization; runtime only consumes it |
| T40 | §11 source cadences | moved/preserved for Scenario Generator / #47 | Baseline cadence materialization belongs to Generator |
| T41 | §11 source-time predicate holds | retained for AirLink product runtime | §§11–12 define exact observation/source-time holds |
| T42 | §12.1–12.8 scenario asset, phases, truth, coordinates, motion, errors, cadences | moved/preserved for Scenario Generator / #47 | Scenario Generator owns all source-generation internals |
| T43 | §12.9 mixed fault variant and delivery behavior | rewritten under the replay-only boundary | §8 keeps only frozen status events/approved C10 transforms |
| T44 | §12.10–12.13 physical launch/Flight/touchdown/scale | moved/preserved for Scenario Generator / #47 | Physical truth never enters runtime |
| T45 | §12.14 C10-owned privileged truth prohibition | rewritten under the replay-only boundary | §§8 and 21 move truth/reference evidence and even its location fully outside C1–C10 |
| T46 | §13 takeoff candidate/holds/authority | rewritten under the replay-only boundary | §11 applies final Airspeed-proxy semantics, inclusive `courseAccuracyDeg <= 10.0` gate, and exact owner rules |
| T47 | §14 Flight lifecycle | retained for AirLink product runtime | §10 preserves lifecycle/recording independence |
| T48 | §15 runtime pressure-altitude formula | retained for AirLink product runtime | §14.2 uses `isaTropospherePressureAltitudeV1` |
| T49 | §15 inverse pressure generation | moved/preserved for Scenario Generator / #47 | Generator owns simulated pressure materialization |
| T50 | §15 Height above Takeoff and VS | retained for AirLink product runtime | §§14.3–14.4 retain corrected baseline and OLS contracts |
| T51 | §16 wind vector/fit/windows/quality gates | retained for AirLink product runtime | §§13 and 15 retain non-conflicting estimator decisions while making activation/history strictly Flight-scoped from the effective boundary |
| T52 | §16 approximate recomputation cadence | rewritten under the replay-only boundary | §15.3 supplies exact active-Flight accepted-GNSS source-time schedule and recovered epochs |
| T53 | §17 landing detector | retained for AirLink product runtime | §12 records final owner rules |
| T54 | §18 ground orientation throughout pre-takeoff | rewritten under the replay-only boundary | §16.4 distinguishes pre-delivery from post-Start source-driven ground orientation |
| T55 | §18 airborne Track-up/no compass fallback | retained for AirLink product runtime | §16.4 preserves it with immediate North-up degraded fallback |
| T56 | §18 stale-orientation grace | discarded as obsolete or over-scoped | Owner decision requires fallback when invalid/unavailable; no grace is fixed |
| T57 | §18 map degradation/attribution | retained for AirLink product runtime | §16 keeps provider isolation, attribution, and degradation |
| T58 | §19 complete/degraded/failed recording model | rewritten under the replay-only boundary | §17 defines exact initialization, irreversible incompleteness, retry, failure, finalization, and quality-independent transitions |
| T59 | §19 normalized/derived retained layers | rewritten under the replay-only boundary | §17 removes Generator context and retains C4 source state, C7 GS/Track output status, course-gate evidence, and normal-boundary provenance |
| T60 | §19 special points and C3/C9 handoffs | rewritten under the replay-only boundary | §§17.2–17.4 keep logical requirements without a durable schema |
| T61 | §20 Summary-from-record principle | retained for AirLink product runtime | §18 preserves it |
| T62 | §20 Summary fields/calculations | rewritten under the replay-only boundary | §18 applies final fields, Height baseline, and independent distance |
| T63 | §20 failed-record presentation | retained for AirLink product runtime | §§2 and 17 preserve lifecycle truth and unavailable record meaning |
| T64 | §21 five-second outage continuation/degraded Summary | discarded as obsolete or over-scoped | §19.1 stops at unresolved P3 boundary |
| T65 | §21 map unavailable | retained for AirLink product runtime | §§16.5 and 19 preserve non-map continuation |
| T66 | §22 scenario phase/truth diagnostics | rewritten under the replay-only boundary | §§8 and 20–21 use replay diagnostics and validation-only comparison without exposing reference identity/location to runtime |
| T67 | §22 shared diagnostic snapshots | retained for AirLink product runtime | §20 keeps replaceable structured observability |
| T68 | §22 Generator/parser/source-generation evidence | moved/preserved for Scenario Generator / #47 | Generator verification remains outside AirLink acceptance |
| T69 | §23 Flutter decision | retained for AirLink product runtime | §§16 and 22 make it slice-bounded |
| T70 | §23 adapter boundaries | retained for AirLink product runtime | §§14, 16, and 22 isolate provider/plugin types |
| T71 | §23 `flutter_map`/OSM | retained for AirLink product runtime | §§16 and 22 apply final provider constraints after the neutral first-issue placeholder |
| T72 | §24 greenfield assumption | retained for AirLink product runtime | §23 defines a small development-foundation/pre-delivery first issue from no implementation |
| T73 | §25 runtime increments | rewritten under the replay-only boundary | §23 separates the first bounded issue from a multi-PR Ready-on-Ground milestone and later product increments |
| T74 | §25 deterministic simulator increment | moved/preserved for Scenario Generator / #47 | No Generator implementation belongs in AL-0003 AirLink increments |
| T75 | §25 issue/PR flexibility | retained for AirLink product runtime | §23 explicitly permits the early milestone to span multiple bounded Draft PRs |
| T76 | §26 scope/stop conditions | retained for AirLink product runtime | §28 preserves and strengthens stops |
| T77 | §27 post-slice Flutter qualification path | discarded as obsolete or over-scoped | Later Android/iOS work is not designed by this slice |
| T78 | §28 readiness | rewritten under the replay-only boundary | §24 uses package contracts and current authority |
| T79 | §29 implemented-slice DoD | rewritten under the replay-only boundary | §25 removes runtime Generator requirements |
| T80 | §30 tuning | rewritten under the replay-only boundary | §26 separates product/replay tuning from Generator work |
| T81 | §31 deferrals | rewritten under the replay-only boundary | §27 records current deferrals and P3 boundary |
| T82 | §32 AL-0003 boundary | rewritten under the replay-only boundary | §23 prepares but does not activate implementation |
| T83 | §33 Product Direction alignment | rewritten under the replay-only boundary | §29 records current triggered alignment |
| T84 | §34.1 minimal wind notice | discarded as obsolete or over-scoped | Superseded by final owner explanation requirement |
| T85 | §34.2 defer passive Current Waypoint | discarded as obsolete or over-scoped | Superseded by current selected-slice logical state |
| T86 | §34.3 assign P3 outage outcome | discarded as obsolete or over-scoped | Superseded by controlled interruption stop boundary |
| T87 | §34.4 magnetic source/runtime conversion | rewritten under the replay-only boundary | §§14.5 and 16.4 separate generated source from runtime derivation/presentation |
| T88 | §34.5 scenario-v1 timing/format | moved/preserved for Scenario Generator / #47 | Not a runtime fixture and not pre-approved Generator design |
| T89 | §34.6 special-point retention/handoffs | rewritten under the replay-only boundary | §17 adopts corrected logical contract |
| T90 | §34.7 phase-independent wind | retained for AirLink product runtime | §15 forbids phase gating |
| T91 | §34.8 exact `FLT`/`DST` interaction | discarded as obsolete or over-scoped | Complete layout/interaction is not required for readiness |
| T92 | §34.9 exact wind UI geometry | rewritten under the replay-only boundary | Pilot-visible estimate remains; final geometry is not fixed |
| T93 | §34.10 no expanded diagnostics overlay | retained for AirLink product runtime | §20 keeps replaceable non-product diagnostics |
| T94 | §34.11 stationary landing rule | retained for AirLink product runtime | §12.1 records final owner rule |
| T95 | §34.12 takeoff headwind correction | rewritten under the replay-only boundary | §11.3 expresses it as discounted Airspeed proxy with the exact inclusive course-accuracy gate, never GS=AS |
| T96 | §34.13 Generator geography | moved/preserved for Scenario Generator / #47 | Coordinate materialization is Generator-owned |
| T97 | §34.13 runtime haversine | retained for AirLink product runtime | §18.1 restores exact radius, radian/shortest-longitude/clamped-haversine formula, eligibility order, exclusions, and independent calculations |
| T98 | §34.14 runtime-generated outage transitions | replaced by the #46 replay/materialization contract | §8 requires frozen status event or approved identity-bearing transform |
| T99 | §34.15 ground weather-to-GS presentation | retained for AirLink product runtime | §2 preserves ground/weather then active Flight values without fixing full layout |
| T100 | §34.16 fixed map scale | retained for AirLink product runtime | §16.3 records the exact full-logical-viewport physical-width and fractional-zoom contract |
| T101 | §34.17 renderer/provider | retained for AirLink product runtime | §16 records final bounded decision |
| T102 | §34.18 virtual civil time generation | moved/preserved for Scenario Generator / #47 | Runtime preserves source civil time only |
| T103 | §34.19 terminal phase JSON | moved/preserved for Scenario Generator / #47 | Phase schema remains #47 work |
| T104 | §34.20 source-error order | moved/preserved for Scenario Generator / #47 | Source generation remains outside runtime |
| T105 | §34.21 detector source-time holds | retained for AirLink product runtime | §§11–12 record exact final semantics |
| T106 | §34.22 versioned VS OLS | retained for AirLink product runtime | §14.4 records the contract |
| T107 | §34.23 fixture metadata/course gate | rewritten under the replay-only boundary | §§8 and 11 separate materialized C4 metadata from C6 interpretation and restore the exact inclusive `courseAccuracyDeg <= 10.0` gate |
| T108 | §34.24 truth-profile/error mapping | moved/preserved for Scenario Generator / #47 | #47 owns formula/profile mapping |

## 31.3 Current unresolved PR #44 review findings

All 29 PR #44 review threads remain formally unresolved; 22 are outdated and 7 are current. This table accounts for each finding even where an owner reply or later commit addressed its PR-era concern.

| Finding | Concern | Disposition | Resolution/destination |
| --- | --- | --- | --- |
| R01 | GNSS interruption assigned continuation/degraded Summary across P3 | rewritten under the replay-only boundary | §19.1 keeps only the authorized boundary |
| R02 | Pressure/QNH units and formula undefined | retained for AirLink product runtime | §14.2 defines units/formula/contract |
| R03 | Monotonic discontinuity lacked fail-closed semantics | retained for AirLink product runtime | §§11–15 and 19 clear dependent state |
| R04 | QNH provenance/calculation context omitted | retained for AirLink product runtime | §§17.5–17.6 retain normal and calculation context |
| R05 | Duplicate timestamp/coalescing ambiguity | replaced by the #46 replay/materialization contract | §8 four-part identity/redelivery/collision rules |
| R06 | Scenario schema/phases/formulas/cadences missing | moved/preserved for Scenario Generator / #47 | Generator planning owns them |
| R07 | Special-point representation and handoffs incomplete | retained for AirLink product runtime | §§17.2–17.4 define logical handoffs |
| R08 | Active-Flight distance omitted | retained for AirLink product runtime | §§2 and 18 require active and finalized calculations |
| R09 | Windsock-like comprehension experiment omitted | retained for AirLink product runtime | §2 retains estimated-wind pilot presentation without final geometry |
| R10 | Expanded diagnostics inspector over-scoped | discarded as obsolete or over-scoped | §20 uses replaceable diagnostics |
| R11 | Landing could not complete with stationary Track unavailable | retained for AirLink product runtime | §12.1 defines zero-vector stationary rule |
| R12 | Temporary payload/workflow files risked surviving merge | discarded as obsolete or over-scoped | Fresh PR contains only plan/index |
| R13 | Takeoff correction lacked direction/fallback | rewritten under the replay-only boundary | §11.3 fixes same-observation Track, meteorological direction, inclusive course-accuracy gate, and zero fallback |
| R14 | Distance bridged GNSS outage | retained for AirLink product runtime | §18.1 defines continuity segments, eligibility-first calculation, and the complete haversine formula |
| R15 | Truth East/North lacked geographic conversion | moved/preserved for Scenario Generator / #47 | Runtime consumes materialized coordinates |
| R16 | Silence did not expose interruption through C4 | replaced by the #46 replay/materialization contract | §§8.5 and 19.1 require explicit status events/transforms |
| R17 | Ground UI used wrong weather/GS meaning | retained for AirLink product runtime | §2 preserves ground weather versus Flight GS meaning |
| R18 | Map scale had incompatible targets | retained for AirLink product runtime | §16.3 fixes `2000 m +/- 2%` over the full logical viewport with centred pilot and fractional-zoom invariants |
| R19 | Virtual wall-clock under Pause/2× was undefined | rewritten under the replay-only boundary | §§5.4 and 8 keep frozen source/civil time independent of delivery |
| R20 | Open-ended terminal phase JSON undefined | moved/preserved for Scenario Generator / #47 | #47 owns phase representation |
| R21 | Declared source errors not normatively applied | moved/preserved for Scenario Generator / #47 | Generator owns source-error materialization |
| R22 | Detector holds were sample-count ambiguous | retained for AirLink product runtime | §§11–12 define source-time holds/ties/redelivery |
| R23 | VS window/algorithm/minimum history undefined | retained for AirLink product runtime | §14.4 defines OLS contract |
| R24 | Map package/provider/licensing decision deferred | retained for AirLink product runtime | §16 records final bounded choice |
| R25 | Fixture validity/freshness/accuracy/provenance unspecified | rewritten under the replay-only boundary | §§8 and 11 define runtime metadata, keep reference location out of the manifest, and apply exact course accuracy |
| R26 | Variation profiles lacked exact phase assignment | moved/preserved for Scenario Generator / #47 | #47 owns phase/profile mapping |
| R27 | Shared non-terminal phase boundary ownership undefined | moved/preserved for Scenario Generator / #47 | Explicit #47 open question; no runtime dependency |
| R28 | Wind recomputation was approximately host-timed | rewritten under the replay-only boundary | §§13 and 15.3 record Flight-scoped history, effective-boundary epoch, recovery reset, and final source-time schedule |
| R29 | Recording outcomes/handoffs were incomplete | rewritten under the replay-only boundary | §17 records final state, irreversible transition, retry, quality, finalization, and handoff model |

None of these findings creates an unresolved issue #37 blocker.

## 31.4 Disposition counts

The major-content table counts 108 traceability units:

| Disposition | Count |
| --- | ---: |
| retained for AirLink product runtime | 39 |
| rewritten under the replay-only boundary | 39 |
| moved/preserved for Scenario Generator / #47 | 13 |
| replaced by the #46 replay/materialization contract | 6 |
| discarded as obsolete or over-scoped | 11 |
| still requiring owner decision | 0 |
| **Total** | **108** |

The separate unresolved-review table counts:

| Disposition | Count |
| --- | ---: |
| retained for AirLink product runtime | 13 |
| rewritten under the replay-only boundary | 6 |
| moved/preserved for Scenario Generator / #47 | 6 |
| replaced by the #46 replay/materialization contract | 2 |
| discarded as obsolete or over-scoped | 2 |
| still requiring owner decision | 0 |
| **Total** | **29** |

Generator-specific PR #44 material is preserved through the existing [`Scenario Generator Boundary`](scenario-generator.md) and issue [#47](https://github.com/AlexanderTsarkov/AirLink/issues/47). This plan does not expand that document or approve PR #44's exact Generator formulas, phases, profiles, fixture, or architecture.
