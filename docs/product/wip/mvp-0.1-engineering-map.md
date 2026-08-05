# MVP 0.1 Engineering Map

## Status and Authority

This document is a **WIP engineering-planning artifact** for `AL-0002: MVP 0.1 Engineering Planning`.

It is:

- non-canonical;
- not a product specification;
- not a final component architecture;
- not implementation authority;
- developed incrementally through bounded AL-0002 issues;
- subject to explicit owner review and approval.

The current document contains:

- the owner-approved engineering-boundary and responsibility-map baseline prepared under GitHub issue `#33 / AL-0002-01`;
- the owner-approved and merged live-input and simulation-substitution extension prepared under GitHub issue `#34 / AL-0002-02`;
- the dependency, risk, decision-order, deferral, and high-level sequencing extension prepared under GitHub issue `#35 / AL-0002-03` and PR `#41`;
- the owner-approved runtime replay versus Scenario Generator correction from issue `#45`, applied under issue `#46 / AL-0002-05B`.

Creation of the issue #35 extension does not itself approve that extension or the consolidated MVP 0.1 Engineering Map. Explicit owner acceptance and merge of PR #41 records approval of the issue #35 extension and of the consolidated map as an AL-0002 planning input. That approval does not promote the document to canon, make it implementation authority, select the first vertical slice, start issue #36, activate AL-0003, or authorize implementation.

## Purpose

This document describes the minimum engineering structure required to treat MVP 0.1 as one coherent system without designing its final component architecture.

At the current planning depth, it defines:

- the AirLink engineering boundary for MVP 0.1;
- the major engineering concerns and their responsibility contracts;
- authoritative ownership of important runtime state and information;
- coverage of the mandatory MVP 0.1 product flows;
- external dependency and degradation boundaries;
- accepted cross-cutting responsibility rules;
- concern-level semantic, runtime-information, external, and implementation-order dependencies;
- external constraints that affect implementation order;
- engineering risks and their reduction order;
- unresolved product decisions, difficult-to-reverse decision timing, and intentionally deferred engineering decisions;
- a high-level risk-ordered sequence of future implementation waves;
- constraints for later first-slice candidate selection.

Concern identifiers in this document are planning references. They do not prescribe modules, packages, classes, services, processes, deployment units, repositories, or dependency-injection boundaries.

## Planning Depth and Completeness Standard

This document is a concern-level engineering map for the whole MVP 0.1. It is not an implementation specification and is not intended to describe every internal message, event, command, API, schema, class, module, service, or data-transfer mechanism.

For the purposes of this map, planning is complete when:

- every mandatory MVP 0.1 product flow has an explicit trigger, responsible concerns, required conceptual path, and observable result;
- every important runtime state or information category has one authoritative owner;
- required producers and consumers are connected at concern level;
- responsibility and non-ownership boundaries prevent implementation agents from inventing product semantics or transferring authority silently;
- external dependency and degradation consequences that affect product behavior are explicit;
- semantic and runtime-information dependencies are distinguishable from implementation order;
- material risks, decision gates, latest safe decision points, and difficult-to-reverse choices are visible before implementation reaches them;
- future implementation can be organized as bounded vertical slices that produce observable product outcomes rather than as concern-by-concern construction;
- unresolved product decisions and intentionally deferred engineering decisions are recorded rather than guessed.

The map describes **principal conceptual handoffs** only. A handoff identifies which concern supplies information or requests a transition, which concern owns the resulting state or interpretation, and which concern requires the outcome. It does not prescribe how that handoff is technically implemented.

Missing concern-level ownership or a missing path required by an accepted MVP flow is a defect in this document. Missing implementation mechanics are not defects unless they are required to preserve product meaning, avoid a difficult-to-reverse decision, or prepare the separately selected first implementation slice.

Implementation-ready depth is intentionally reserved for the selected first vertical slice and its governing later issues. Issues #33 and #34 establish the accepted boundary, responsibility, and live/simulation foundation. Issue #35 adds whole-MVP dependency, risk, decision-order, and high-level sequence guidance without converting the map into a complete architecture, backlog, or implementation plan.

## Governing Context

Work under this document follows the source-of-truth order and Product-Significance Routing defined in `AGENTS.md`.

The issue #35 extension uses the owner-approved and merged issue #33 and #34 outputs and the following task-specific context:

- `ITERATION.md`;
- `docs/product/CurrentState.md`;
- relevant canonical product documentation;
- GitHub issues #32 and #35 and approved task artifacts;
- the owner-reviewed `docs/product/wip/mvp-0.1-scope.md` planning baseline;
- directly relevant Flight Mode, Flight, and Navigation WIP.

Consultation does not promote WIP into canon or make this planning artifact implementation authority. If governing sources conflict, the conflict must be reported rather than silently resolved.

---

# 1. Planning Sufficiency and Engineering Boundary

## 1.1 Planning-sufficiency assessment

The existing MVP 0.1 Scope and the owner-approved and merged issue #33 and #34 Engineering Map content are sufficient as the WIP planning baseline for the dependency, risk, decision-order, deferral, and high-level sequencing work required by issue #35.

No contradiction or omission currently blocks:

- definition of the MVP 0.1 engineering boundary;
- identification of major concerns;
- separation of Flight Mode and Flight responsibilities;
- state and information ownership;
- concern-level product-flow coverage;
- external-dependency classification;
- live-input categorization, replay substitution, provenance, semantic fidelity, retained replayed-Flight behavior, and mandatory observability;
- concern-level dependency classification, external ordering constraints, risk reduction, decision timing, deferral consolidation, and high-level future sequencing under issue #35.

No blocking product contradiction or omission was found, no product-scope correction is required, and no additional owner decision blocks issue #35 review.

Issue #45 corrects the responsibility realization of the issue #34 simulation boundary while preserving its product purpose:

1. mandatory simulation fidelity remains semantic and behavioral rather than requiring AirLink runtime to calculate a physically exact Flight;
2. controlled weather and pressure observations remain required, but they are materialized before replay rather than generated by AirLink runtime;
3. live and replayed source-equivalent observations converge through the same C4/C5-facing boundaries while source mode, source origin, value provenance, delivery handling, and actually active input state remain explicit and non-interchangeable;
4. replayed Flights use normal lifecycle, recording, retention, Summary, and review responsibilities;
5. C10 supplies active replay-session context to C3, but the exact retained Flight-level replay/source-origin classification and its Summary presentation are deferred to resumed issue #37 and must not be inferred from individual value provenance.

Scenario representation, truth/physical models, deterministic source-value generation, baseline cadence/error/availability materialization, and Generator verification move to the separate [Scenario Generator boundary](scenario-generator.md). Replay-source selection, delivery controls, source-mode and origin taxonomy, normalized handoff, storage realization, diagnostics presentation, and first-slice-specific runtime contracts remain bounded planning concerns.

Issue #35 proceeds under four additional explicit owner decisions:

1. future implementation planning uses bounded end-to-end vertical slices ordered by product outcome and risk reduction, not concern-by-concern, layer-by-layer, or infrastructure-first construction;
2. the first implementation slice includes only the minimum C10 replay and source-delivery capability required to exercise and observe that slice, and replay capability grows incrementally inside later product slices;
3. Android constraints inform boundaries and difficult-to-reverse choices from the beginning, while concrete live Android source integration follows a coherent and acceptably working replay-driven application path;
4. estimated wind is an early major product and engineering risk: the first slice must preserve compatible input, time, derivation, replay, observability, and retention boundaries, and controlled wind validation must occur within the first several implementation iterations rather than after general MVP completion.

These owner decisions order future planning without selecting the first slice, application architecture, mobile framework, Android APIs, storage design, provider, algorithm, complete replay subsystem, complete Generator, or final component realization.

Some product and domain decisions remain unresolved. They are recorded in section 7 because they must not be selected silently during implementation. They do not prevent the concern-level dependency and sequencing map from being completed, provided each future slice stops at an applicable unresolved decision boundary.

The MVP 0.1 Scope remains non-canonical, is not a detailed specification, and does not gain implementation authority through this assessment.

## 1.2 AirLink engineering boundary

The MVP 0.1 engineering boundary includes all AirLink-controlled responsibility for:

- the preparation–Flight–completion–review product flow;
- Flight Mode and individual Flight lifecycle state;
- interpretation and use of runtime inputs;
- calculated and derived Flight information;
- pilot-facing operational, Flight, spatial, Summary, and saved-review presentation;
- progressive local recording, completed-Flight retention, and later retrieval;
- validity, freshness, provenance, availability, and degradation semantics;
- replay and source delivery required to develop and validate accepted behavior from materialized streams;
- runtime observability required to verify concern decisions and outcomes.

The following remain outside the AirLink engineering boundary:

- Android operating-system internals;
- device hardware and sensor internals;
- GNSS infrastructure;
- weather-provider internals;
- map-provider and rendering-engine internals;
- network infrastructure;
- concrete database, filesystem, and storage-engine internals;
- development, CI, and deployment infrastructure;
- external tooling not incorporated into AirLink replay capability;
- Scenario Generator authoring, truth/physical modelling, deterministic source-value generation, baseline source cadence/error/availability materialization, visualization, verification, and frozen-stream export.

AirLink does not own those external systems. AirLink does own their integration meaning, validity and freshness interpretation, product-level degradation consequences, and pilot-visible unavailable or degraded state.

## 1.3 Live and replay source boundary principle

C10 — Replay and Source Delivery Enablement is inside the AirLink runtime boundary as a product-enabling capability. Scenario Generator materialization is a separate subproduct responsibility.

Live platform sources and replayed source-equivalent streams enter the same normal product-facing boundaries:

```text
live platform sources -----\
                            -> C4 / C5 -> normal AirLink behavior
replayed source streams ---/
```

C10 owns selection and activation of an approved frozen stream, replay-session state, replay progression, and deterministic delivery into C4/C5. It does not calculate source values. Live sources reach C4/C5 without C10 proxying them. C4 and C5 own the actually active AirLink-facing source state, normalization, provenance, handling result, availability, validity, freshness, and degradation within their boundaries.

A replay stream may originate from an offline-generated synthetic Flight, a recorded real Flight normalized to the source-event contract, or a deterministic regression fixture. Delivery mode (`live` or `replay`), replay source origin, category-level value provenance, and delivery handling are distinct axes and must not be inferred from one another. C10 supplies active replay-session context to C3 where runtime or retained meaning requires it. Resumed issue #37 must decide the exact Flight-record and Summary representation without treating every replay origin as equivalent.

Replay changes source delivery, not product meaning. It must not create an alternative Flight Mode, Flight lifecycle, detector, calculation model, spatial model, record-construction path, or pilot-facing product behavior. AirLink runtime does not receive scenario phases, truth state, physical formulas, source-generation instructions, or privileged expected answers.

Minimum acceptable fidelity remains semantic and behavioral. A materialized stream must be able to carry ordered source-equivalent time, position, movement, speed, pressure, orientation, weather, lifecycle-driving conditions, and explicit unavailable, invalid, stale, degraded, or interrupted events as required by the validating slice. Those observations exercise normal C6 detection, C7 derivation, C8 spatial awareness, C9 recording and retention, Summary, and saved review.

Exact frozen-stream format, compatibility and integrity mechanism, replay storage, technical adapter architecture, and first-slice implementation contracts remain reserved for the bounded work that requires them.

---

# 2. Concern Contracts

The following ten concerns define the minimum useful responsibility map for MVP 0.1.

## C1 — Pilot Interaction and Operational Flow

**Owns**

- normal application and pilot interaction flow;
- current-conditions and minimal Pre-Flight presentation;
- non-flight explanation of the estimated nature and limitations of in-flight wind information;
- pilot acknowledgements and explicit actions;
- pilot-facing presentation of Flight Mode state, current Flight information, spatial information, Summary, saved Flights, and degraded capability states;
- pilot-facing distinction between simulated and non-simulated retained Flights where those records are presented;
- manual-completion retain-versus-discard choice.

**Consumes**

- weather and freshness information from C5;
- Flight Mode state and transition outcomes from C2;
- active Flight context, Flight-level simulation classification, elapsed time, final boundaries, and final aggregates from C3;
- current derived Flight values and explanatory estimated-wind semantics from C7;
- spatial presentation from C8;
- recording, retention, deletion, and saved-record results from C9.

**Produces**

- requests to load or refresh current conditions;
- Pre-Flight acknowledgements;
- requests to enter, continue, or exit Flight Mode;
- requests to manually complete a Flight;
- explicit choice to retain the episode as a Flight or discard it as a false detection;
- pilot map-scale adjustment actions;
- saved-Flight selection and review actions;
- requests to delete one retained simulated Flight or all retained simulated Flights as a category.

**Does not own**

- lifecycle state;
- detection;
- calculations;
- spatial semantics;
- recording or persistence.

## C2 — Flight Mode Lifecycle

**Owns**

- the operational context in which zero or more Flights may occur;
- Flight Mode entry, Ready on Ground, active-Flight awareness, inactivity warning, continuation, and exit;
- authorization for confirmed automatic boundaries or explicit manual actions to affect an individual Flight;
- prevention of Flight creation outside an allowed Flight Mode state.

**Consumes**

- pilot requests from C1;
- monotonic time and platform-state information from C4;
- confirmed takeoff and landing boundaries from C6;
- Flight creation, completion, rejection, interruption, and no-active-Flight outcomes from C3.

**Produces**

- authoritative Flight Mode state and transition outcomes for C1;
- Flight Mode acquisition demand for C4;
- authorization and relevant operational context for C3;
- allowed detection context for C6.

**Does not own**

- the lifecycle or internal state of an individual Flight;
- input-acquisition mechanisms or resource profiles;
- detection algorithms;
- Flight calculations or aggregates;
- durable records;
- presentation.

## C3 — Flight Lifecycle and Flight State

**Owns**

- one airborne episode after authorization by C2;
- Flight identity and active, completed, manually completed, rejected, or interrupted runtime state;
- association of the approved Flight-level source/replay classification with Flight identity at creation, using active C10 replay-session context where applicable;
- effective takeoff, confirmed-landing, and manual-completion boundaries;
- association of information with one Flight;
- elapsed Flight time and Flight-scoped aggregates;
- Takeoff Point identity, estimated location, and association with the Flight;
- confirmed Landing Point identity, estimated location, confirmed-landing classification, and association with the completed Flight;
- continuity of the normally completed Flight through landing confirmation, including its final recorded segment without retrospective trimming;
- runtime completion, rejection, and finalization of the individual Flight.

**Consumes**

- lifecycle authorization and operational context from C2;
- active C10 replay-session context when a Flight is created;
- monotonic time from C4;
- derived-value and aggregate updates from C7;
- recording and retention outcomes from C9 where they affect pilot-visible completion status.

**Produces**

- active Flight identity, lifecycle context, and approved Flight-level source/replay classification for C1, C7, C8, and C9 wherever required;
- Takeoff Point identity, estimated location, and Flight association for C7, C8, and C9;
- confirmed Landing Point identity, location, and classification for C9;
- final lifecycle boundaries and aggregates, including all information retained through confirmed landing, for C1 and C9;
- completion, rejection, interruption, and no-active-Flight outcomes for C2.

**Does not own**

- the wider Flight Mode period;
- boundary detection;
- instantaneous-value calculation;
- spatial presentation;
- durable storage;
- pilot interaction.

## C4 — Input Acquisition and Validity

**Owns**

- the AirLink-facing representation of externally produced runtime information;
- normalized values, source timestamps, wall-clock and monotonic time;
- availability, validity, freshness, accuracy or quality metadata, and category-level provenance;
- actually active live or replay source mode and delivery-handling status where applicable;
- actually active normalized runtime-source state and configuration for the categories C4 normalizes;
- platform lifecycle and interruption signals exposed at the AirLink boundary;
- execution of concern-level acquisition demand through deferred platform and resource-management mechanisms.

Relevant input categories include position, movement and Ground Speed source information, course or Track source information, device orientation, altitude, vertical movement, pressure, wall-clock and monotonic time, platform lifecycle and interruption signals, permission and source-availability state, and current-location context.

**Consumes**

- bounded current-location acquisition demand from C5;
- Flight Mode acquisition demand from C2;
- requested replay-session delivery configuration from C10 where it affects C4-owned categories;
- available device, platform, and replay-source information.

**Produces**

- normalized current-location context, including availability, validity, freshness, and provenance, for C5;
- normalized runtime information for C2, C3, C6, C7, C8, and C9 as required by the covered flows;
- actually active source mode, activation, availability, validity, freshness, provenance, and delivery-handling outcomes for consumers and C10 observability.

**Does not own**

- product lifecycle transitions;
- the product decisions that current-location weather or Flight Mode require acquisition;
- detection confirmations;
- derived semantics;
- Flight aggregates;
- presentation or historical retention;
- requested replay delivery configuration or C10 replay-session state.

For every category it normalizes, C4 preserves source identity, category-level provenance, source mode, and delivery handling as separate meanings. Live platform observations reach C4 directly. Replayed observations retain their frozen source identity, source time, provenance, validity, availability, freshness metadata, and replay event identity while C4 adds AirLink-observed delivery time where meaningfully distinct. C4 must not infer source origin or Flight-level classification from replay handling. Source and observed time, wall-clock and monotonic semantics, availability, validity, freshness or staleness, supplied quality or accuracy, and intentional degradation remain explicit. Switching between live and replay sources cannot occur invisibly.

## C5 — Weather Context

**Owns**

- the product need for current-location weather context in normal application and Pre-Flight use;
- geographic scoping and interpretation of current-location weather acquisition;
- current observed wind, gusts, direction, pressure or QNH, and observation or update time;
- near-term forecast when available;
- observation-versus-forecast distinction;
- actually active weather-source interpretation, availability, freshness, validity, category-level provenance, live or replay source mode, delivery handling where applicable, and degraded state.

**Consumes**

- current-conditions requests from C1;
- normalized current-location context, including availability, validity, freshness, and provenance, from C4;
- requested replay-session weather delivery configuration from C10;
- external weather-provider information;
- replayed source-equivalent weather observations from C10 through the same weather-input interpretation boundary used for external weather information.

**Produces**

- bounded current-location acquisition demand for C4;
- weather context and status for C1;
- pressure or QNH context for C7 when the selected altitude representation requires it;
- actually active weather-source, source mode, provenance, delivery-handling, and activation outcomes for C10 observability.

**Does not own**

- location acquisition or normalization;
- automated suitability or safety approval;
- the in-flight estimated-wind value;
- the altitude model;
- Flight lifecycle or recording;
- C10 replay-session state or requested delivery configuration.

C5 applies the same observation-versus-forecast, source-time, AirLink-observed-time, validity, freshness, and degraded-state meaning to live and replayed weather. C5 owns the actually active weather interpretation and delivery result, does not infer provenance or source origin from replay handling, and receives live weather without C10 proxying it. Replayed weather does not create a second Weather Context concern or provider-specific product behavior.

## C6 — Flight Detection

**Owns**

- takeoff and landing candidates;
- confirmed takeoff and landing outcomes;
- rejected or expired candidates;
- estimated actual transition-boundary information;
- the accepted constraint that permanent takeoff detection must not rely on speed as its only basis;
- detection uncertainty and diagnostic context where required.

**Consumes**

- normalized runtime inputs from C4;
- allowed detection context from C2;
- a bounded recent history of valid inputs when required for retrospective boundary estimation.

**Produces**

- confirmed or rejected boundary outcomes and relevant context for C2;
- observable detection state.

**Does not own**

- Flight Mode state;
- Flight creation, completion, rejection, or finalization;
- Takeoff Point, Landing Point, or Flight aggregate mutation;
- durable recording.

## C7 — Flight Information Derivation

**Owns**

- current Ground Speed;
- current altitude representation;
- current vertical speed;
- current or rolling estimated wind;
- the semantic explanation that estimated wind is derived and cannot reliably separate short-term pilot input, climb or descent, wing behavior, turbulence, and actual wind variation;
- course-, orientation-, distance-, and bearing-related calculations assigned to this concern;
- True North reference semantics for pilot-facing navigation-direction outputs;
- validity, quality, stability, availability, and measured-versus-estimated-versus-derived semantics for its values.

**Consumes**

- normalized runtime inputs from C4;
- active Flight, approved Flight-level source/replay classification, and Takeoff Point context from C3;
- pressure or QNH from C5 when required by the selected altitude representation.

**Produces**

- current pilot-facing Flight values for C1;
- non-flight explanatory estimated-wind semantics for C1;
- aggregate updates and final aggregate values for C3;
- True-North-referenced pilot-facing orientation values or candidates, including corrected device orientation where applicable, GPS Track, estimated Heading when available, semantic identity, availability, validity, quality, and provenance, for C8;
- Takeoff Point distance, True-North-referenced bearing or direction, and relevant validity state for C8;
- selected time-varying values, with the semantic status and provenance required by the approved historical-retention contract, for C9.

**Does not own**

- Flight identity or lifecycle;
- elapsed Flight time or association of aggregates with a Flight;
- presentation;
- durable records;
- map representation.

C7 preserves the source dependencies and semantic status required to understand derived output. A derived value may depend on multiple sources, each of which retains its own category-level provenance, live/replay source mode, and separately relevant delivery handling. Those dependencies must not be silently presented or retained as if every contributing source were live, replay must not select an alternative calculation meaning, and C3-supplied Flight-level source/replay classification remains a separate semantic axis.

## C8 — Spatial Awareness and Map Context

**Owns**

- pilot-centred spatial presentation;
- current-position and active-track representation;
- map orientation presentation and pilot-controlled map scale;
- a compass ring or scale around the pilot as a mandatory Flight-orientation presentation capability;
- the bounded free-flight navigation context in which Takeoff Point becomes Current Waypoint after takeoff while Active Navigation remains off;
- Takeoff Point map representation and passive-awareness presentation;
- pilot-facing use of True North while keeping Track, Heading, bearing, and device orientation semantically distinct;
- saved-track presentation;
- spatial unavailable or degraded state.

**Consumes**

- normalized position and other required spatial source information from C4;
- active Flight, approved Flight-level source/replay classification, and Takeoff Point context from C3;
- True-North-referenced pilot-facing orientation values or candidates, including their semantic identity, availability, validity, quality, and provenance, from C7;
- distance, True-North-referenced bearing or direction, and validity information from C7;
- pilot map-scale adjustment actions from C1;
- retained track and spatial record information from C9 for saved-Flight review.

**Produces**

- current navigation context and active/saved spatial presentation for C1.

**Does not own**

- Takeoff Point or Landing Point identity or location;
- current-value calculations;
- Flight lifecycle;
- active route guidance or route planning;
- durable track storage.

C8 uses the same spatial semantics regardless of live or replay source mode, category-level input provenance, delivery handling, and source composition. Generator scenario or truth metadata is never a C8 input, does not create a Current Route, does not enable Route Navigation or Active Navigation, and does not transfer route or navigation state into C8. C8 receives only normal spatial observations and presents the resulting position, Track, Takeoff Point, orientation, and passive-awareness context. C3-supplied Flight-level source/replay classification remains separate from spatial input provenance and delivery handling.

## C9 — Flight Recording and Local Retention

**Owns**

- initialization and progressive recording for an active Flight;
- recording health and degraded state;
- local storage of completed and retained Flights;
- retained Takeoff Point and confirmed Landing Point information as part of the completed Flight record;
- retention of all approved information through the confirmed-landing boundary without retrospectively trimming the final segment;
- preservation of the values, semantic status, validity, provenance, and calculation context required to represent what was available or used during the original Flight;
- retained track, time-varying information, accepted summary fields, completion status, and durable Flight reference;
- retrieval of retained Flights;
- deletion of a progressively recorded episode rejected as a false detection;
- durable preservation of the C3-supplied Flight-level source/replay classification throughout progressive recording, finalization, retention, retrieval, Summary, and saved review, once resumed issue #37 approves that classification contract;
- deletion of one retained synthetic-simulation Flight or the approved synthetic-simulation category without deleting other Flights, subject to the classification mapping resolved by resumed issue #37;
- technical recoverability mechanisms, subject to unresolved interruption semantics.

**Consumes**

- active Flight identity, lifecycle markers, approved Flight-level source/replay classification, Takeoff Point identity, estimated location and Flight association, final boundaries, final aggregates, and confirmed Landing Point information from C3;
- normalized track and other approved retained inputs from C4;
- selected derived values and their approved historical semantics from C7;
- category-level source and derivation provenance, plus separately relevant handling context, supplied through the normal C4/C7 recording inputs;
- approved C1 requests to delete one retained synthetic-simulation Flight or the approved category.

**Produces**

- recording health and retention status for C1 and C3;
- retained summary fields, C3-supplied Flight-level source/replay classification, special-point information, historically preserved values, and track for C1 and C8 as required by approved presentation contracts;
- durable Flight reference when available;
- success or failure of false-detection episode deletion;
- success or failure of approved synthetic-simulation Flight deletion, without transferring record ownership to C1 or C10.

**Does not own**

- whether the pilot is airborne;
- Flight Mode or Flight lifecycle;
- boundary detection;
- confirmed Landing Point meaning or classification;
- pilot retain-versus-discard choice;
- Flight-level source/replay classification or inference from recorded provenance or delivery composition;
- current-value calculations or later reinterpretations;
- presentation.

C9 preserves the historical context required to represent what was available or used during the Flight, including approved category-level provenance, source mode, and separately relevant delivery handling. It must preserve the Flight-level source/replay classification supplied by C3 and must not derive, infer, or reclassify it from recorded value provenance or delivery composition. Resumed issue #37 must approve the minimum replay provenance needed in the first-slice record and Summary. One physical store or separate stores, exact durable representation, history filtering, visual marking, grouping, retention, cleanup, storage technology, and deletion mechanics remain deferred.

## C10 — Replay and Source Delivery Enablement

**Owns**

- replay-source selection, compatibility/integrity validation, and activation;
- authoritative replay-session state and source origin;
- Start, Pause, playback speed, and Reset;
- replay cursor and source-monotonic progression;
- deterministic delivery ordering, including explicit equal-time order;
- delivery batching, delay, and exact redelivery;
- collision detection and fail-closed handling;
- explicitly approved availability or invalidity delivery transforms;
- replay and source-delivery diagnostics;
- delivery of frozen source-equivalent observations through normal C4/C5-facing boundaries.

**Consumes**

- an approved frozen source-equivalent stream;
- replay controls and approved delivery-transform configuration;
- C4/C5 activation and delivery outcomes needed for diagnostics.

**Produces**

- replayed source-equivalent observations and explicit availability or invalidity events for C4/C5;
- active replay-session context for C3 where runtime classification requires it;
- replay identity, source origin, cursor, progression, delivery-transform, collision, and outcome diagnostics.

**Does not own**

- scenario authoring or phase definitions;
- truth state, trajectory, motion, environment, or physical modelling;
- generation of latitude, longitude, Ground Speed, Track, pressure, orientation, weather, or any other source value;
- baseline source cadences, source-error profiles, or baseline availability-event materialization;
- Generator visualization, truth verification, formulas, or expected answers;
- an alternative Flight Mode, Flight lifecycle, detector, calculation, map, recorder, Summary, or product flow;
- C4/C5 normalization, validity, freshness, availability interpretation, or actually active source meaning;
- direct Flight creation, completion, rejection, record construction, record deletion, or downstream state mutation.

### Minimum frozen stream and replay contract

A frozen stream identifies its stream ID, version, compatibility target, origin (`generated synthetic`, `normalized recorded real Flight`, or `deterministic regression fixture`), and integrity evidence. Each event identifies its stream, source category, source-monotonic time, deterministic sequence within equal source time, source-equivalent value where applicable, source timestamp where distinct, and the metadata required by the applicable C4/C5 boundary. Availability and invalidity changes are explicit events rather than hidden Generator state.

Source-monotonic time defines event order and source semantics. AirLink-observed delivery time records when C4/C5 receives an event and never rewrites source time. Start begins delivery at the current cursor. Pause freezes replay progression and emits no later event; it is not a lifecycle action. Playback speed scales delivery intervals only and does not alter frozen source timestamps, values, ordering, or metadata. Reset returns the cursor and replay progression to the stream start and clears session-scoped delivery transforms and diagnostics; it does not mutate an active Flight, delete a record, or define Flight Mode behavior. The selected first slice keeps its stricter rule that Reset is unavailable during an active Flight and creates a fresh development session.

Equal-time events are delivered by their explicit frozen sequence. Batching may give several events one delivery opportunity but preserves their source order and identity. Delay changes delivery schedule only. Exact redelivery re-emits the same event identity and payload; C4/C5 handling is idempotent and does not advance source windows or retain a duplicate. Reuse of one event identity with different value or metadata is a collision: C10 fails closed for the affected stream, does not deliver the conflicting event as valid, and exposes the outcome.

Approved availability or invalidity delivery transforms may replace delivery status for an identified event or inject a specifically identified status transition. They cannot recalculate a source value, invent baseline cadence or error behavior, alter source identity or source time, or expose Generator truth. Contract validation and collision checks occur before delivery; explicit transforms are then applied; delay, batching, and redelivery affect only delivery. Every transform remains visible in diagnostics and distinguishable from the frozen baseline.

The conceptual validation path is:

`Generator materialization → frozen source-equivalent stream → C10 replay delivery → normal C4/C5 boundaries → normal C6–C9 behavior`

Any out-of-band truth or expected-output evidence is unavailable to normal runtime concerns. Exact serialization, storage, adapter architecture, transport, and implementation technology remain deferred.

## Live-input and replay-source categories

The table defines category-level boundaries, not APIs, sensor selection, fusion, sampling, replay storage, or source hierarchy. Live production and frozen replay production both occur before C4/C5 normalization. C10 delivers already materialized replay events and never produces their values. C4 and C5 own the actually active AirLink-facing source, provenance, delivery result, and validity treatment; downstream concerns do not receive a replay-only product path.

| Input category | External producer and AirLink boundary | Required metadata, principal consumers, and minimum degradation consequence | Minimum live or replay capability |
| --- | --- | --- | --- |
| Position | Device/platform live position or a replayed source-equivalent observation enters C4; C4 normalizes position without selecting an acquisition mechanism | Source and observed time, availability, validity, freshness, quality or accuracy, provenance, and source mode; consumed by C5, C6, C7, C8, and C9; loss makes current location and dependent spatial behavior explicitly unavailable or degraded without implying a lifecycle boundary | Live position or materialized replay position progression; explicit unavailable, invalid, stale, degraded, and interrupted events are supported |
| Movement and Ground Speed source information | Device/platform live movement or a replayed source-equivalent observation enters C4; C7 owns pilot-facing Ground Speed meaning | Source and observed time, validity, freshness, quality, provenance, and source mode; consumed principally by C6, C7, C8, and C9; degraded movement must not become valid current Ground Speed or a lifecycle conclusion | Live movement or materialized replay movement and speed observations, including stationary and degraded conditions |
| Course or Track source information | Device/platform live movement direction or a replayed source-equivalent observation enters C4; C7 preserves semantic identity and owns pilot-facing True-North-referenced Track-related output | Source and observed time, validity, freshness, quality, reference semantics, provenance, and source mode; consumed by C6, C7, C8, and C9; unavailable direction remains distinct from zero movement and device orientation | Live Track/course or materialized replay Track observations coherent with their stream, plus explicit unavailable or degraded direction |
| Device orientation-related source information | Device/platform live orientation or a replayed source-equivalent observation enters C4; C7 owns correction, derivation, semantic identity, and pilot-facing True North output | Source and observed time, validity, freshness, quality, reference semantics, provenance, and source mode; consumed by C7 and C8; invalid orientation must not be silently replaced by Track or Heading | Live orientation or materialized replay orientation observations sufficient to exercise C7/C8 behavior, including unavailable, invalid, stale, and degraded states |
| Altitude-related information | Device/platform live altitude-related information or a replayed source-equivalent observation enters C4; C7 owns the selected pilot-facing altitude representation | Source and observed time, validity, freshness, quality or accuracy, altitude-source semantics, provenance, and source mode; consumed by C6, C7, and C9; unavailable or stale altitude degrades dependent values without inventing an altitude | Live altitude-related observations or materialized replay observations coherent with the stream's movement, pressure context, and lifecycle-driving conditions |
| Vertical-movement-related information | Device/platform live vertical-motion information or a replayed source-equivalent observation enters C4; C7 owns pilot-facing vertical speed | Source and observed time, validity, freshness, quality, provenance, and source mode; consumed by C6, C7, and C9; degraded vertical movement must not be treated as valid zero | Live observations or materialized replay observations sufficient for normal detection and derivation |
| Pressure-related information | Device/platform live pressure enters C4; external live weather pressure or QNH enters C5; replayed equivalents enter the same respective boundaries | Source and observation time, validity, freshness, quality, measurement-versus-context semantics, provenance, and source mode; unavailable pressure explicitly limits dependent altitude behavior | Live pressure/QNH or materialized replay equivalents, including explicit unavailable, invalid, stale, and degraded states |
| Wall-clock time | Platform live clock or a replayed source-equivalent civil-time observation enters C4 and remains distinct from duration time | Source time, AirLink-observed time where different, clock adjustments, validity, provenance, and source mode; consumed where calendar time or retained timestamps matter; clock changes must not alter monotonic durations | Live platform clock or materialized replay civil-time observations mapped coherently to source time |
| Monotonic time | Platform live monotonic clock or replay source-monotonic progression enters C4 and supplies duration and ordering semantics | Monotonic ordering, continuity, availability, provenance, and source mode; consumed by C2, C3, C6, C7, C9, and C10; loss or discontinuity is explicit and must not become a wall-clock assumption | Live monotonic source or ordered materialized replay progression sufficient for waiting, detection, Flight time, derivation, and recording |
| Platform lifecycle and interruption signals | Live platform lifecycle signals or replayed source-equivalent availability/interruption events enter C4 | Signal identity, source and AirLink-observed time, availability, provenance, and source mode; consumed by C2, C3, and C9 as required; interruption must not masquerade as landing, completion, or rejection | Live signals or materialized replay events sufficient to expose the unresolved F14 outcome without inventing it |
| Permission and source-availability state | Live platform, device, network, or provider state enters C4/C5; replayed availability events enter the same applicable boundary | Category, source and observed time, availability, denial or limitation, validity impact, provenance, and source mode; denial or loss is explicit rather than hidden fallback | Live state or explicit materialized replay granted, denied, unavailable, restored, and degraded events |
| Current-location context | C4 owns normalized current-location context from live or replayed position or an explicit selected point; C5 owns its use for weather scoping | Position provenance, source mode, source and observed time, validity, freshness, quality, and selection status; invalid or unavailable context must not silently request unrelated-location weather | Live position, an explicit selected point, or a materialized replay current-location observation sufficient to scope weather explicitly |
| Weather-provider information | External live provider information or replayed source-equivalent weather enters the normal C5 weather-input interpretation boundary | Provider/source provenance, source mode, observation or update time, AirLink-observed time where different, observation-versus-forecast status, validity, freshness, availability, and degradation; failure must not become suitability approval | Live provider information or materialized replay wind, gust, direction, pressure/QNH, update-time, and forecast observations, including explicit stale, unavailable, invalid, and degraded states |

Delivery mode is `live` or `replay`. Replay origin is separately `generated synthetic`, `normalized recorded real Flight`, or `deterministic regression fixture`. Each runtime value also preserves its approved category-level provenance and delivery handling; none of these axes may be inferred from another. A run may combine live and replay categories only through an explicitly approved source configuration, and switching must not occur invisibly. Selected-point provenance remains distinguishable from replay delivery. C10 exposes requested replay configuration while C4/C5 expose actually active source mode, provenance, delivery result, and source composition. The exact retained Flight-level replay/source-origin classification is deferred to resumed issue #37.

## Source-independent downstream contract

Live or replay delivery, any approved category-level provenance and source origin, and any permitted source composition must preserve the normal responsibilities below. Source quality may legitimately change availability or degraded behavior, but it does not change the product meaning of a concern.

| Concern | Source-independent responsibility |
| --- | --- |
| C2 — Flight Mode Lifecycle | Uses normal pilot intent, monotonic time, allowed detection context, and lifecycle outcomes; source substitution cannot directly enter, continue, or exit Flight Mode |
| C3 — Flight Lifecycle and Flight State | Creates, completes, rejects, or interrupts a Flight only through normal C2 authorization and C6 or pilot-originated outcomes; associates approved C10 replay-session context with Flight identity without creating a special lifecycle |
| C4 — Input Acquisition and Validity | Normalizes every approved live or replay source, preserves source identity, mode, provenance and time semantics, owns actually active interpretation for its categories, and exposes activation, validity, freshness, availability, and degradation outcomes |
| C5 — Weather Context | Owns the actually active weather-source interpretation, source mode, provenance, and delivery result, and preserves normal weather meaning, location scoping, observation-versus-forecast distinction, freshness, and degradation |
| C6 — Flight Detection | Applies the normal candidate, confirmation, rejection, and expiry responsibility to normalized inputs; C10 cannot confirm takeoff or landing |
| C7 — Flight Information Derivation | Applies the same Ground Speed, altitude, vertical-speed, wind, orientation, distance, and bearing meanings and exposes dependency provenance, relevant delivery context, and semantic status; no alternative replay calculation exists |
| C8 — Spatial Awareness and Map Context | Applies the same pilot-centred map, orientation, Takeoff Point, Current Waypoint, scale, and degraded-state meaning to normal spatial inputs; Generator scenario/truth metadata is not a downstream input and does not create a Current Route or enable Route Navigation or Active Navigation |
| C9 — Flight Recording and Local Retention | Uses the normal progressive recording, finalization, retention, retrieval, and deletion responsibilities while preserving the approved C3-supplied Flight-level source/replay classification without inferring it from value provenance or delivery composition |

## Cross-cutting observability responsibility

Observability is a responsibility of every runtime concern rather than a separate product concern.

Each concern must expose enough information to verify its authoritative state, decisions, validity, provenance, handoffs, degradation, and outcomes. For live and replay validation, the minimum observable set is:

- C10 replay-session identity, source-stream identity/version/origin, integrity outcome, cursor, progression, playback state, delivery order, transforms, collision state, and delivery diagnostics;
- C4/C5 actually active runtime-source configuration and activation outcomes, live/replay mode, category-level provenance, delivery handling, and observable source composition;
- source-monotonic time and AirLink-observed delivery time, including wall-clock versus monotonic semantics;
- relevant input availability, validity, source and observed time, freshness or staleness, quality or accuracy, and intentional degradation or interruption;
- C2 Flight Mode state and transitions, including Ready on Ground, warning, continuation, automatic exit, and explicit exit outcomes;
- C6 takeoff and landing candidate, confirmation, rejection, and expiry outcomes;
- C3 Flight creation, approved Flight-level source/replay classification, completion, manual completion, rejection, interruption, final boundary, and multiple-Flight lifecycle separation;
- C7 derived-value availability, semantic status, provenance dependencies, quality, and degraded state, including independently calculated estimated wind;
- C8 pilot-visible spatial result, Takeoff Point and Current Waypoint state, orientation meaning, and degraded state;
- C9 recording health, preservation of C3-supplied Flight-level classification, progressive-record result, finalization, retention, retrieval, and false-detection deletion outcomes;
- immediate Summary and saved-review availability.

Observability must not silently change product behavior, become an alternative source of product truth or lifecycle, or preserve Flight-equivalent data that accepted false-detection behavior requires to be deleted.

Out-of-band Generator truth or expected-output evidence may support independent validation, but it is not a C10 runtime input and is unavailable to C1–C9 normal behavior. Detailed logging technology, telemetry format, diagnostic UI, storage, and automation remain deferred.

---

# 3. Authoritative State and Information Ownership

| ID | State or information category | Authoritative owner | Required consumers or consequence |
| --- | --- | --- | --- |
| O1 | Flight Mode operational state, transition outcome, and acquisition demand | C2 | C1 presents state/outcomes; C4 fulfills acquisition demand; C6 uses allowed detection context |
| O2 | Flight identity, runtime lifecycle state, and approved Flight-level source/replay classification associated from active source context at creation | C3 | C1 presents the approved classification; C2, C7, and C8 consume it where required; C9 preserves it without inference or reclassification; resumed issue #37 defines replay/source-origin representation |
| O3 | Effective takeoff, confirmed-landing, and manual-completion boundaries | C3 | C1 and C9 consume final boundaries; C7 uses the active interval; confirmed landing includes the full final segment through confirmation |
| O4 | Elapsed Flight time and Flight-scoped aggregates | C3 | C1 presents them; C9 retains approved final values |
| O5 | Takeoff Point identity and estimated location | C3 | C7 calculates relative values; C8 uses it as Current Waypoint and presents it; C9 consumes and retains it without reconstruction |
| O6 | Normalized runtime inputs; source and observed time; wall-clock and monotonic semantics; availability, validity, freshness, quality, category-level provenance, source mode, and delivery handling for C4-owned categories | C4 | C5 consumes current-location context; runtime consumers use required inputs without reclassifying or silently switching source mode, provenance, or handling; C10 consumes replay activation and delivery outcomes for diagnostics |
| O7 | Current-location weather acquisition demand, actually active weather-source interpretation, weather observation, forecast, pressure or QNH context, category-level provenance, source mode, delivery handling, and degradation | C5 | C4 fulfills the location demand; C1 presents weather; C7 conditionally consumes pressure or QNH; C10 consumes replay activation and delivery outcomes; weather retains the same product meaning |
| O8 | Takeoff and landing detection candidate, confirmation, and accepted detector constraint state | C6 | C2 decides whether confirmed boundaries may affect lifecycle; permanent speed-only takeoff detection is prohibited |
| O9 | Current derived Flight, True-North-referenced orientation, direction, distance, and bearing values, including independently preserved source-dependency provenance, relevant handling context, and semantic status | C7 | C1 presents current Flight values; C8 presents C7-provided orientation and relative-navigation values without recalculating them; C3 receives aggregate updates; C9 retains approved historical values without silent replacement, provenance or handling loss, or collapse into Flight-level classification |
| O10 | Active and saved spatial representation, including pilot-controlled scale and Flight compass ring or scale | C8 | C1 supplies scale-adjustment actions and presents the resulting map, compass, orientation, and awareness context |
| O11 | Progressive and durable Flight record, recording status, saved representation, historical-value preservation, approved Flight-level source/replay distinction, and record deletion outcome | C9 | C1 and C8 consume saved and retention results; C9 preserves C3-supplied classification and cannot infer or redefine it; later interpretations must not silently replace original retained values |
| O12 | Authoritative C10 replay-session state, stream identity/version/origin/integrity outcome, cursor and source-monotonic progression, playback controls, delivery order and transforms, collision state, and delivery diagnostics | C10 | C3 consumes only approved active replay context when creating a Flight; C4/C5 receive source-equivalent events while retaining normalization and source-meaning ownership; C2–C9 receive no Generator phase, truth, formula, or expected-answer authority |
| O13 | Confirmed Landing Point identity, estimated location, and confirmed-landing classification | C3 | C9 retains it as part of the completed Flight record; presentation may consume it only through an approved Summary or spatial contract |
| O14 | MVP free-flight Current Waypoint and passive-navigation state | C8 | Takeoff Point becomes Current Waypoint after takeoff; C1 presents the context; Active Navigation remains off |

## Ownership interpretation rules

- Each entry has one authoritative owner.
- A presentation concern may format an authoritative value but must not silently recalculate or upgrade its semantic status.
- A recording concern may retain an authoritative value but must not redefine its lifecycle or calculation meaning.
- A detection concern may confirm a boundary but must not perform the lifecycle transition.
- A consumer may express acquisition demand or report an outcome to an owner but must not mutate another concern's authoritative state or perform its mechanism directly.

---

# 4. Mandatory Product-Flow Coverage

Each flow below describes the minimum concern-level path required by the accepted MVP 0.1 outcome. Exact messages, APIs, events, schemas, controls, and implementation mechanisms remain deferred.

| ID | Mandatory flow | Principal conceptual path | Required observable result | Explicitly deferred or unresolved |
| --- | --- | --- | --- | --- |
| F1 | Current conditions | C1 requests current conditions from C5; C5 expresses bounded current-location acquisition demand to C4; C4 fulfills that demand and supplies normalized current-location context to C5; C5 receives either live-provider weather or replayed source-equivalent weather through the same weather-input boundary and supplies observation-versus-forecast distinction, source mode, provenance, freshness, validity, availability, and degraded state to C1 | The pilot can understand relevant conditions through the same C5 semantics for live or replay weather; unavailable or invalid location or weather produces an explicit limitation rather than silently using an unrelated location or source | Provider, request mechanism, concurrent-demand coordination, refresh/cache policy, forecast intervals, exact representation, fallback details |
| F2 | Minimal Pre-Flight and Flight Mode entry | C1 presents accepted acknowledgements and sends the completed acknowledgements plus explicit entry request to C2; C2 returns the resulting state to C1, issues Flight Mode acquisition demand to C4, and enables the allowed detection context for C6; C4 fulfills the demand through its acquisition boundary | Flight Mode becomes active only after explicit pilot intent; Ready on Ground waiting begins; required Flight Mode inputs become available through C4 | Exact controls, layout, platform activation, acquisition profiles, and resource-management mechanism |
| F3 | Ready on Ground waiting, warning, continuation, and automatic exit | C4 supplies monotonic time to C2; C2 supplies waiting/warning/exit state to C1; C1 may request continuation; C2 resets the waiting period or exits when required; on exit C2 disables the allowed detection context for C6, withdraws Flight Mode acquisition demand from C4, and C4 reduces or stops Flight Mode-specific acquisition while preserving other active input demands | The warning is presented, continuation is possible, and Flight Mode eventually exits if the pilot does not continue; after automatic exit no further takeoff detection may affect lifecycle until Flight Mode is entered again; resource use follows Flight Mode demand without C2 performing acquisition directly | Timeout and warning duration, presentation, exact continuation control, other reset conditions (`P5`), acquisition profiles and platform mechanism |
| F4 | Confirmed takeoff and Flight creation | C4 supplies valid normalized inputs to C6; C6 confirms takeoff under the accepted non-speed-only permanent detector constraint and supplies its estimated boundary to C2; C2 validates context and authorizes C3; C10 supplies active replay-session context when applicable; C3 creates the Flight and Takeoff Point, associates the approved Flight-level source/replay classification, and supplies active-Flight/classification/Takeoff-Point context to C2/C1/C7/C8/C9; C8 makes Takeoff Point the Current Waypoint while keeping Active Navigation off | One Flight begins inside active Flight Mode through the same lifecycle for live and replay sources; its effective start and Takeoff Point represent the accepted actual-takeoff estimate; permanent takeoff detection is not based only on speed; recording starts with authoritative classification and Takeoff Point available to C9 | Detector design, signal combination, confirmation semantics (`P2`), recent-history custody, retrospective estimation, thresholds, filters, exact replay/source-origin classification in resumed issue #37, and implementation of the C10-to-C3 context handoff |
| F5 | Active Flight information, elapsed time, aggregates, and progressive recording | C4 supplies inputs to C7 and time to C3; C5 supplies pressure or QNH to C7 only when required; C7 supplies current values to C1 and aggregate updates to C3; C3 supplies elapsed time and Flight context to C1; C4/C7/C3 supply approved retained information and historical semantics to C9 | Ground Speed, altitude, vertical speed, Flight time, and estimated wind are available with correct semantic status; Flight aggregates advance; progressive recording preserves the approved values and context required to represent what was available or used during the original Flight | Algorithms, validity rules, update rates, altitude/QNH model, exact retained parameter set, provenance representation, sampling and persistence mechanics |
| F6 | Takeoff Point awareness and spatial orientation | C4 supplies normalized position and other required spatial/source inputs to C8 and supplies movement/orientation source inputs to C7; C3 supplies Takeoff Point identity/location to C7 and C8; C8 retains it as Current Waypoint with Active Navigation off; C7 supplies True-North-referenced pilot-facing orientation values or candidates, their semantic/validity state, and Takeoff Point distance/bearing to C8; C1 supplies pilot map-scale adjustment actions to C8; C8 applies those actions and the later-approved orientation policy and supplies the pilot-centred map, compass ring or scale, and passive awareness presentation to C1 | The Takeoff Point remains the current passive navigation context and stays distinct and visible throughout the Flight; distance and bearing/direction use True North; map orientation uses C7-provided pilot-facing values without C8 recalculation; Track, Heading, bearing, and device orientation remain distinct; a compass ring or scale is present around the pilot; the pilot can change map scale during Flight; passive awareness does not become Active Navigation | Selection and switching among C7-provided orientation candidates, declination source/model, correction and derivation algorithms, update rate, source validity, fallback behavior, quality semantics, exact scale controls, zoom range/steps, gesture or button behavior, compass visual design, animation, recenter interaction and final orientation decision (`P6`) |
| F7 | Confirmed landing and runtime completion | C4 supplies inputs to C6; C6 confirms landing and supplies the boundary to C2; C2 validates context and authorizes C3; C3 completes/finalizes the Flight through the confirmed boundary without trimming the final segment, establishes the confirmed Landing Point, supplies final boundaries/aggregates to C1/C9, supplies Landing Point identity/location/classification to C9, and reports no active Flight to C2 | The individual Flight ends with all approved information through landing confirmation retained; the final segment is not retrospectively trimmed; the completed Flight has a distinct confirmed Landing Point; C2 immediately returns to Ready on Ground and waiting for another takeoff resumes | Landing detector, confirmation semantics (`P2`), exact Landing Point determination and storage representation |
| F8 | Immediate retained completed-Flight Summary | After either confirmed landing through F7 or manual retention through F10, C3 supplies final Flight values, approved Flight-level source/replay classification, and the authoritative completion type to C1/C9; C9 supplies recording completeness and retention status; C1 presents their shared core summary information while C2 remains Ready on Ground; the Summary includes an explicit Flight Mode exit action through F12 | A Summary appears inside the Flight flow for every retained completed Flight, preserves the approved classification, identifies confirmed or manual completion correctly, provides the accepted Flight Mode exit action, and does not block waiting for another takeoff | Exact replay/source-origin presentation in resumed issue #37; Summary fields beyond accepted minimum; information hierarchy, layout, retention timing, error presentation, and exact exit control |
| F9 | Another Flight in the same Flight Mode period | After F7/F8 or F10/F8, C2 remains Ready on Ground, Flight Mode acquisition demand remains active, and C6 remains allowed to detect takeoff; a new F4 starts a new independent Flight; C1 closes the prior Summary when the new Flight begins | Multiple independent Flights may occur in one Flight Mode period without a Flight Session record | Exact presentation transition |
| F10 | Manual completion — retain Flight | C1 sends the manual-completion request and retain choice to C2; C2 validates context and authorizes C3; C3 completes the Flight with an explicit manual boundary, preserves its approved Flight-level source/replay classification, marks the completion type as manual, and supplies final results to C1/C9; C9 preserves the supplied classification and reports recording completeness and retention status; C2 returns to Ready on Ground; the flow continues through F8 | The episode is retained with its approved Flight-level classification and category-level input provenance preserved; an immediate Summary identifies manual completion; no confirmed Landing Point is implied; Flight Mode remains active and Ready on Ground | Exact interaction, confirmation behavior, replay/source-origin presentation, retention timing and error presentation, and Landing Point semantics (`P4`) |
| F11 | Manual completion — discard false detection | C1 sends the discard choice to C2; C2 authorizes rejection by C3; C3 reports the rejected outcome to C2/C9; C9 deletes the progressively recorded episode; C2 returns to Ready on Ground | No Flight and no hidden durable Flight-equivalent record remain; bounded observability may record only that rejection and deletion occurred | Technical deletion and cleanup mechanism; cleanup-failure recovery |
| F12 | Explicit Flight Mode exit while no Flight is active | C1 sends an exit request to C2; C2 exits, disables the allowed detection context, withdraws Flight Mode acquisition demand from C4, and returns the resulting state to C1; C4 reduces or stops Flight Mode-specific acquisition while preserving any other active input demand | Flight Mode ends separately from any individual Flight; C2 owns the operational decision and C4 owns the acquisition/resource mechanism | Exact control, presentation, acquisition profiles, and platform mechanism |
| F13 | Explicit Flight Mode exit while a Flight is active | No product flow is accepted. C1 may originate the request, but C2 must not invent refusal, forced completion, discard, or another transition | The unresolved state is exposed rather than silently implemented | Explicit owner product decision `P1` is required before implementation reaches this scenario |
| F14 | Platform interruption during an active Flight | C4 exposes the interruption or restoration signal to C3 and C9; the affected concerns expose their state and outcome to C1/C2 as required | Interruption is observable and does not silently masquerade as confirmed landing or deliberate completion | Product classification and retained outcome (`P3`), recovery guarantees, checkpointing, restoration, and storage mechanics |
| F15 | Saved-Flight access and review | C1 requests a retained Flight from C9; C9 supplies accepted summary fields, retained status, approved C3-supplied Flight-level source/replay classification, special-point information, category-level provenance, separately relevant delivery context, historically preserved information, and recorded track; C8 presents the saved track and scale control; C1 presents the saved summary and record status | A retained Flight can later be opened and understood without losing special-point identity, approved classification, category-level provenance, relevant delivery context, or original values | Exact replay/source-origin representation and presentation in resumed issue #37; navigation, layout, editing, replay presentation, analytics, schema, migration, and retrieval implementation |
| F16 | Replay-driven validation | The Scenario Generator or another approved producer materializes a frozen source-equivalent stream before runtime. C10 validates, selects, and deterministically delivers its observations and explicit status events through normal C4/C5 boundaries. C4/C5 own active source interpretation, validity, freshness, availability, provenance, and degradation; C6–C9 operate through normal responsibilities. Generator truth or expected-output evidence remains out of band and unavailable to product behavior | Accepted lifecycle, derivation, spatial, recording, Summary, retained-result, saved-review, and degradation behavior can be exercised without a physical Flight; replay does not create alternate product semantics or expose privileged Generator state | Exact frozen-stream serialization/storage, adapter architecture, replay provenance in Flight records and Summary, minimum validation oracle, and concrete fixture requirement for resumed issue #37; Generator architecture and detailed materialization under issue #47 |
| F17 | Retained synthetic-simulation Flight deletion | C1 initiates an approved request through the normal deletion boundary for one retained synthetic-simulation Flight or the approved category; C9 validates the preserved Flight-level classification, owns deletion, and returns the outcome | The requested classified record or category is removed without deleting other Flights; failure is explicit; no other concern directly mutates C9-owned records | Mapping of replay source origin to retained classification under resumed issue #37; exact controls, authorization, confirmation, presentation, filtering, grouping, cleanup policy, and deletion implementation |

## Flow-coverage interpretation

- A flow row is complete when its concern-level path and product result are explicit, even when its implementation mechanism remains deferred.
- A row that reaches an unresolved product decision must stop at that decision rather than invent a transition.
- Current and saved presentations may use different layouts, but they must preserve the semantic identity and provenance of the authoritative information they present.
- A completed Flight and its durable retention are distinct outcomes. Storage failure must not imply that the pilot remains airborne.
- A recent pre-takeoff history may support retrospective boundary estimation. Samples not incorporated into a confirmed Flight remain transient, are overwritten, and are not retained as Flight data.
- Concern-level acquisition demand states which product context requires inputs; C4 owns how concurrent demands are fulfilled, reduced, or stopped.

---

# 5. External Dependency and Degradation Boundaries

| Dependency category | External responsibility | AirLink responsibility | Minimum degradation invariant |
| --- | --- | --- | --- |
| Android lifecycle and permissions | Process scheduling, permission system, execution limits, device resource constraints | Interpret lifecycle and permission state; expose unavailable or interrupted state; route platform signals through C4 | Platform interruption or denied access must not be hidden or silently converted into a lifecycle conclusion |
| Device location, motion, altitude, pressure, and time | Hardware and platform production of samples and metadata | C4 fulfills concern-level acquisition demand and normalizes resulting inputs while preserving timestamps, validity, freshness, quality, and provenance | Invalid, unavailable, or stale information must not be treated as valid current information; one concern withdrawing demand must not silently remove inputs still required by another concern |
| Weather provider | Observation, forecast, and provider internals | C5 uses C4-owned current-location context to scope weather acquisition and preserves observation/forecast distinction, freshness, availability, provenance, and degraded state | Weather failure or unavailable location context must not become an automated safety decision or silently produce unrelated-location weather |
| Map capability | Tiles, rendering engine, projection and provider internals | C8 preserves AirLink spatial semantics, current/track/Takeoff Point presentation, compass ring or scale, pilot-controlled scale interaction, and degraded state | Loss of map capability must not terminate or redefine Flight Mode, Flight lifecycle, detection, calculation, recording, or Current Waypoint identity |
| Local storage | Filesystem/database primitives and storage-engine internals | C9 owns record meaning, recording status, retained result, historical-value preservation, retrieval, and required deletion outcome | Storage failure must be explicit and must not imply that the Flight remains active; a discarded false detection must not appear as a saved Flight; later processing must not silently replace retained original values |
| Network | Connectivity infrastructure | AirLink interprets availability and freshness and may use cached or preloaded context | Core active-Flight lifecycle, current local information, and progressive local recording must not require permanent connectivity |
| System and monotonic clocks | Platform clock sources | C4 exposes wall-clock and monotonic time with source semantics | Durations, detection windows, and inactivity periods must not rely solely on mutable wall-clock continuity |

## External constraints affecting implementation order

External dependencies do not prescribe a provider or technical stack, but their semantics constrain when implementation choices remain safe. Android feasibility is considered from the first slice wherever it affects boundaries or difficult-to-reverse decisions. Concrete live-source mechanisms are intentionally connected only after a coherent replay-driven product path exists.

| External constraint | Why it affects order | Latest safe point | Failures that must remain explicit | Authoritative concerns |
| --- | --- | --- | --- | --- |
| Android lifecycle and permissions | Lifecycle state and denied, limited, or revoked access affect availability, interruption, acquisition demand, and whether a product flow can continue | Preserve lifecycle, permission, and availability semantics in the first slice; choose concrete mechanisms before Phase 4 live integration | Denial, revocation, process interruption, restoration, and unavailable source; none may masquerade as landing, completion, rejection, or valid current data | C4 owns normalized platform and availability state; C2/C3 own lifecycle meaning; C9 owns recording outcome |
| Foreground and background execution | Continuous acquisition and recording may be constrained when the app is obscured, backgrounded, or interrupted | Preserve acquisition-demand and interruption boundaries in replay-driven slices; decide the Android execution model before live continuous acquisition and validate it before real-flight readiness | Suspended acquisition, stopped execution, delayed samples, continuity gaps, restoration failure, and recording degradation | C4 owns input/execution state; C2 owns Flight Mode demand; C3 owns Flight state; C9 owns recording health and retention |
| Device GNSS, movement, orientation, altitude, pressure, and clocks | Source availability, timestamps, quality, reference semantics, and combinations directly affect C6–C9 behavior | Preserve C4 normalization, time, provenance, validity, and degradation contracts from the first slice; connect and validate each live category before the dependent Phase 4 behavior is claimed | Unavailable, invalid, stale, low-quality, discontinuous, or semantically incompatible values and source switches | C4 owns normalized runtime-source state; C6 owns detection; C7 owns derived meaning; C8 owns spatial presentation |
| Weather-provider availability and network behavior | Current-location scoping, observation versus forecast, freshness, and offline behavior affect Pre-Flight context and conditional QNH use | Preserve C5 interpretation and degradation boundaries before a slice consumes weather; select or integrate a provider only when that slice requires live weather | Network loss, provider failure, unrelated-location data, stale observation, missing forecast, and unavailable pressure/QNH | C5 remains authoritative for weather interpretation; C4 owns current-location context; C1 presents limitations |
| Map availability | Map loss can remove a presentation aid without changing lifecycle, detection, calculation, or recording truth | Preserve C8 degraded behavior before the first spatial slice; choose a provider only when map rendering is implemented | Missing tiles or rendering, stale/offline coverage, projection or orientation failure, and unavailable saved-track presentation | C8 owns AirLink spatial meaning and degradation; C2/C3/C6/C7/C9 remain authoritative for their non-map responsibilities |
| Local-storage reliability | Early record loss can permanently destroy Flight history and future replay evidence; partial writes may misrepresent retention | Define the logical retained-data and historical-preservation contract before the first durable Flight result; choose schema and recovery strategy before production persistence hardening | Initialization, progressive-write, finalization, retrieval, migration, capacity, deletion, and recovery failures | C9 owns recording, durable meaning, retrieval, deletion, and health; C3 owns lifecycle classification and boundaries |
| System and monotonic clocks | Mutable wall time cannot safely define durations, detection windows, ordering, or inactivity; discontinuity affects replay and diagnosis | Separate wall-clock and monotonic semantics in the first slice and decide the durable timestamp model before retained time history depends on it | Clock adjustment, monotonic discontinuity, missing timestamps, reordered samples, and invalid source time | C4 owns clock-source semantics; C2/C3/C6/C7/C9 consume time only for their accepted responsibilities |
| Interruption and restoration | A process or source interruption during a Flight exposes unresolved P3 semantics and tests whether recording and lifecycle remain distinguishable | Make interruption observable from the first applicable slice; resolve P3 before recovery behavior or production persistence hardening is implemented | Interrupted, restored, partially restored, unrecoverable, and recording-incomplete outcomes; interruption is never silently landing or deliberate completion | C4 exposes interruption/restoration; C3 owns Flight classification/state; C9 owns technical recovery and retained outcome after product meaning is decided |
| Battery and resource use | High-rate acquisition and long-running execution may make a technically correct path unusable or unsafe for real-flight validation | Preserve concern-level demand boundaries early; measure and harden resource behavior after live integration and before real-flight readiness | Throttling, reduced availability, device pressure, excessive drain, and resource-driven acquisition or recording gaps | C2/C5 express bounded demand; C4 owns fulfillment and resource-facing source state; affected concerns expose degradation |

Specific providers, APIs, libraries, storage engines, databases, formats, caches, acquisition coordinators, resource profiles, declination sources, and recovery mechanisms are not selected by this document.

---

# 6. Accepted Cross-Cutting Rules

## R1 — Flight Mode and Flight remain distinct

Flight Mode is the operational context in which zero or more independent Flights may occur. Ending one Flight does not by itself end Flight Mode. No persisted Flight Session is introduced.

## R2 — Detection and transition ownership remain distinct

C6 identifies and confirms possible lifecycle boundaries. C2 decides whether a confirmed automatic boundary or explicit manual action is allowed to affect lifecycle in the current operational context. C3 owns the resulting individual-Flight transition.

## R3 — Calculation, lifecycle association, presentation, and retention remain distinct

C7 calculates current and rolling values. C3 owns their association with one Flight and owns Flight-scoped aggregates. C1/C8 present authoritative information. C9 retains approved historical information. None of these responsibilities silently absorbs another.

## R4 — Semantic distinctions and provenance are preserved

Measured, declared, recorded, estimated, and derived information remain distinguishable where their meaning matters. Live/replay delivery mode, replay source origin, category-level value provenance, delivery handling, and Flight-level classification are separate axes. Source and observed time, wall-clock and monotonic semantics, validity, freshness, availability, quality, and degradation must not be inferred from hidden implementation details. Source switching is explicit, and multiple provenance dependencies and relevant delivery context are independently preserved during derivation and retention wherever they affect meaning. Generator scenario/truth metadata is not runtime provenance and does not enter normal AirLink behavior.

## R5 — Takeoff Point is the passive Current Waypoint after takeoff

After takeoff in the MVP free-flight scenario, Takeoff Point becomes Current Waypoint and remains available after it is reached, crossed, or revisited. Active Navigation remains off. Distance and bearing/direction awareness do not enable route guidance or Active Navigation.

## R6 — False-detection discard is destructive at the Flight-record level

A pilot-rejected false-detection episode produces no Flight and no hidden durable Flight-equivalent record. C9 is responsible for deleting any progressively recorded episode data. The technical deletion mechanism remains deferred.

## R7 — Summary continuity does not prescribe one technical object

The immediate Summary and later saved-Flight review use the same core Flight information and preserve the same semantics. This rule does not require a shared DTO, read model, database object, service, or other technical realization.

## R8 — Local-first degradation preserves lifecycle meaning

Map, weather, network, or storage degradation may reduce available information or retention quality, but it must not silently redefine whether Flight Mode is active, whether a Flight exists, or whether landing was confirmed.

## R9 — Replay reuses normal product responsibilities

C10 delivers frozen source-equivalent observations without calculating their values. C4 and C5 own actually active source interpretation, provenance, delivery handling, normalization, validity, freshness, and availability within their boundaries, and C2–C9 retain normal lifecycle, detection, derivation, spatial, recording, retention, deletion, and presentation meaning. Deliberately invalid or degraded replay events may produce different availability or degraded outcomes, but they do not create replay-only product semantics. Generator scenario phases, truth, and formulas are unavailable to runtime behavior.

## R10 — Pilot-facing navigation directions use True North

All pilot-facing navigation directions are referenced to True North. A magnetic source may support ground device orientation, but it must be corrected before pilot-facing use. Track, Heading, bearing, and device orientation remain semantically distinct and must not be presented as an unlabeled generic direction.

The declination source or model, correction algorithm, update rate, validity rules, fallback behavior, and display formatting remain deferred.

## R11 — Historical Flight information is not silently rewritten

Replay-supporting information retained for a Flight preserves the values, semantic status, validity, live/selected/simulated provenance, separately relevant handling context, and calculation context required to represent what was available or used during the original Flight.

Later algorithm or interpretation changes may produce a distinct later interpretation, but they must not silently replace the retained historical values.

The exact retained parameter set, sampling rules, provenance and source-mode representation, delivery handling where historically required, calculation-version context, storage format, migration mechanism, and later-interpretation representation remain deferred.

## R12 — Confirmed landing preserves the final Flight segment

A normally completed Flight includes all approved information retained through landing confirmation. Confirmed landing finalization must not retrospectively trim the final segment in order to approximate an earlier landing boundary.

The detector, confirmation rule, Landing Point determination method, and technical finalization mechanism remain deferred.

## R13 — Permanent takeoff detection is not speed-only

Takeoff detection may use speed as one signal, but the permanent detector must not rely on speed as its only basis. The exact signal set, algorithm, thresholds, filters, and confirmation behavior remain deferred.

## R14 — Unused pre-takeoff history is transient

A bounded recent history may support retrospective takeoff-boundary estimation. Measurements not incorporated into a confirmed Flight are overwritten and are not retained as Flight data or as a hidden Flight-equivalent record.

The buffer duration, custody, data categories, and implementation mechanism remain deferred.

## R15 — Flight-level source/replay classification survives normal retention

C3 associates the approved Flight-level source/replay classification with Flight identity at creation. The Flight then follows normal recording, completion, Summary, retention, retrieval, and saved-review paths. C9 preserves but never infers or redefines the classification from event provenance, replay origin, or delivery composition. The existing requirement to distinguish and safely delete retained synthetic-simulation Flights remains; resumed issue #37 must define how replay mode and source origin map to the first-slice record and Summary without misclassifying normalized recorded-real-Flight sources.

## R16 — Materialized simulation fidelity preserves product meaning

MVP 0.1 requires coherent semantic and behavioral relationships sufficient to exercise accepted flows, derivation, spatial meaning, degradation, and retained outcomes. The Scenario Generator materializes those relationships into source-equivalent observations. C10 preserves their deterministic order and delivery through normal boundaries, while C7 independently derives estimated wind. Generator truth may support out-of-band comparison only. AirLink runtime does not calculate flight dynamics, aerodynamics, sensor physics, trajectory, source errors, or simulated source values.

---

# 7. Decision Classification and Consolidated Deferrals

## 7.1 Difficult-to-reverse decision classification

Decision class describes authority and reversibility, not implementation order. A high-classification decision is made only when a bounded vertical slice requires it; it is not a reason to build a horizontal foundation first.

| Class | Meaning | Examples in this map | Required treatment |
| --- | --- | --- | --- |
| A — owner-controlled product semantics | Defines pilot-facing meaning, lifecycle, retention meaning, or another accepted product distinction | P1–P6; completion and interruption meaning; Flight retention or deletion meaning; pilot-facing semantic distinctions | Explicit owner decision before implementation reaches the boundary; an agent must stop rather than infer an answer |
| B — material and difficult to reverse | Establishes a durable technical contract or system boundary whose later replacement would be costly or could destroy historical meaning | Logical retained-data contract; historical-value preservation; timestamp and monotonic-time model; durable Flight identity and classification; provenance and validity preservation; application architecture or framework when first required; storage schema and migration strategy; Android execution model; realization of runtime-input and substitution boundaries | A bounded decision section or separate decision issue before implementation depends on it; issue #35 classifies but does not choose it |
| C — material but replaceable behind preserved contracts | Selects a substantial mechanism that can evolve without changing accepted product semantics or Class B boundaries | Detection and wind-estimation algorithms; weather or map provider; smoothing; source hierarchy; simulation controls; detailed UI information hierarchy | Decide in the bounded slice that first requires it, preserve observability and contracts, and avoid premature whole-MVP selection |
| D — local implementation choice | Affects only a local realization and can change without altering accepted behavior or a preserved contract | Class names; package organization; local helper abstractions; internal DTOs; behavior-preserving implementation detail | Choose locally inside an authorized implementation issue; do not elevate it into product or architecture authority |

No final architecture or Class B choice is selected by this document. A Class B decision may be prepared only in the later bounded work that demonstrates the need and authority.

## 7.2 Owner-controlled product decisions

P1–P6 remain unresolved. The timing below establishes the latest safe decision point; it does not answer any decision.

| ID | Decision and type | Authority | Trigger and latest safe decision point | Dependencies and reversibility | Status and governing future work |
| --- | --- | --- | --- | --- | --- |
| P1 | Behavior of an explicit Flight Mode exit request while a Flight is active; Class A lifecycle semantics | Owner | Before a slice exposes or handles Flight Mode exit during an active Flight | Depends on C1 request, C2 authorization, C3 completion/interruption meaning, and C9 retention outcome; a guessed transition could corrupt lifecycle and retained meaning | Unresolved; stop the applicable slice and obtain an owner decision before implementing F13 |
| P2 | Exact semantic distinction and transition rule between detected and confirmed takeoff or landing; Class A lifecycle semantics | Owner | Before implementation-ready planning of permanent automatic detection and lifecycle transitions | Constrains C6 outcomes, C2 authorization, C3 boundaries, Takeoff/Landing Point meaning, and C9 history; early reversible experiments may not claim final semantics | Unresolved; governing decision for D2 and permanent F4/F7 behavior |
| P3 | Product classification and retained outcome for a Flight interrupted without confirmed landing or deliberate manual completion; Class A lifecycle and retention semantics | Owner | Before interruption recovery behavior and production persistence hardening | Constrains C3 state, C9 recovery and retention, C1 presentation, and restoration; technical recoverability must not decide product meaning | Unresolved; governing decision for F14 and the recovery parts of D1/D5 |
| P4 | Landing Point existence and classification for a manually retained Flight; Class A special-point semantics | Owner | Before any retained or pilot-facing manual-completion Landing Point representation | Depends on C3 manual boundary, C9 retained special-point contract, and C1/C8 presentation; a confirmed Landing Point must never be implied falsely | Unresolved; governing decision for F10 and relevant D2/D5/D6 work |
| P5 | Conditions other than pilot continuation that reset the Ready on Ground inactivity period; Class A Flight Mode semantics | Owner | Before final inactivity-reset behavior is planned or implemented | Depends on C2 monotonic-time and transition behavior and C1 warning/continuation flow; timeout experiments must remain reversible | Unresolved; governing decision for final F3 behavior |
| P6 | Final primary in-flight orientation behavior; Class A spatial-presentation semantics | Owner | Before final in-flight spatial-orientation policy is selected, while reversible Track-up and estimated-Heading-up experiments may occur earlier | Depends on C4 source state, C7 semantic identity and validity, and C8 presentation; experiments must preserve Track, Heading, bearing, and device-orientation distinctions | Unresolved; governing decision for final F6 policy and D4 |

Implementation must stop at the applicable boundary when the required P1–P6 owner decision has not been made.

## 7.3 Engineering decision groups intentionally deferred

| ID | Decision group and classification | Authority | Trigger and latest safe decision point | Dependencies and reversibility | Status and governing future work |
| --- | --- | --- | --- | --- | --- |
| D1 | Platform and input contracts: Android APIs, permissions, execution behavior, concern-level acquisition-demand coordination, resource profiles, sampling, timestamps, freshness, validity, source hierarchy, and sensor fusion; Class B for execution, time, acquisition-demand, and common live/replay boundary realization; Class C for replaceable source hierarchy and resource tuning | Bounded technical decision with required owner approval for material Class B choices | Decide only the subset required before the selected slice depends on it; decide the concrete Android execution model before Phase 4 continuous live acquisition | Depends on C2/C5 demand, C4/C5 normalization, clocks, permissions, lifecycle, provenance, and C10 replay delivery; mechanisms remain replaceable only behind preserved contracts | Deferred to selected-slice planning, Phase 4 integration, and bounded technical decisions |
| D2 | Flight detection and boundary-derived points: detector design within the non-speed-only constraint, signal combination, thresholds, filters, confirmation windows, recent-history duration and custody within the transient-history constraint, retrospective takeoff-boundary estimation, confirmed Landing Point determination, and false-positive or false-negative recovery; Class C mechanism governed by Class A P2/P4 semantics | Bounded slice work after applicable owner decisions | Before a slice claims permanent automatic takeoff/landing or retained boundary-derived points | Depends on C4 inputs, C6 detection, C2 authorization, C3 boundaries, transient recent history, and C9 retention; algorithms may evolve behind stable semantics and observability | Deferred to selected-slice detector planning after P2 and P4 where applicable |
| D3 | Derived information: altitude/QNH model, vertical speed, estimated wind within the accepted non-gust scope, direction values, quality and stability semantics, precision, smoothing, and update rates; primarily Class C algorithms and parameter contracts, with historically retained meaning constrained by Class B D5 | Bounded slice and parameter-contract work | Before a slice presents, validates, or retains each derived value; estimated-wind decisions occur early enough for Phase 2 validation | Depends on C4 inputs, C5 pressure/QNH when applicable, C3 context, C7 semantics, C9 historical preservation, replay delivery observability, and separate out-of-band validation evidence where approved | Deferred incrementally; estimated wind is an early risk target, not a post-MVP deferral |
| D4 | Orientation and map behavior: selection and switching among C7-provided orientation candidates, declination source or model, magnetic-to-True correction and Heading derivation, update rate, source validity, fallback, formatting and labels, compass-ring design, map-scale controls, zoom, gesture or button behavior, animation, recenter interaction, final orientation policy, and implementation; Class C mechanisms governed by Class A P6 final policy | Bounded slice and pilot-validation work after any required owner decision | Before each spatial behavior is implemented; P6 is required only before the final policy, allowing reversible earlier experiments | Depends on C4 orientation and movement inputs, C7 True-North outputs and semantic identity, and C8 presentation; providers and algorithms remain replaceable | Deferred to spatial slices and pilot validation |
| D5 | Recording and replay-supporting data: exact retained parameters and sampling, provenance and validity representation, separately relevant handling context, calculation-version context, historical preservation, active-Flight buffering, checkpointing, recovery, capacity, retention policy, special-point representation, schema, migration, and format; Class B for logical retained-data, historical preservation, Flight identity/classification, schema, and migration; Class C for tuning and replaceable mechanisms | Bounded technical decision with owner approval for material Class B choices | Define the minimum logical retained contract before the first durable Flight result; decide production schema, migration, checkpointing, and recovery before persistence hardening depends on them | Depends on C3 identity/boundaries/classification, C4/C7 retained information and provenance, C9 semantics, P3 interruption meaning, and P4 when manual Landing Point is represented; omitted history cannot be reconstructed | Deferred to first applicable retained-result planning and later persistence hardening |
| D6 | Summary and presentation: exact Summary fields beyond the accepted minimum, information hierarchy, formatting, units, controls, warning presentation, non-flight explanation of estimated-wind limitations, manual-versus-confirmed completion formatting, exact Summary exit action, and saved-review layout; mainly Class C information hierarchy and controls, with Class A escalation if new product meaning is required | Bounded UX and product planning | Before the applicable pilot-facing slice is implementation-ready | Depends on C1 presentation, C3 completion/classification, C7 values, C8 spatial result, C9 retention status, and P1/P4/P6 where applicable; layouts remain replaceable while semantics remain stable | Deferred incrementally to relevant vertical slices |
| D7 | Replay and observability realization: frozen-stream identity/version/compatibility/integrity representation; common live/replay adapter boundary; source-mode, origin, provenance, and delivery-context representation; source selection and switching; Start, Pause, speed, Reset, cursor, ordering, batching, delay, redelivery, collision, approved status transforms; replay storage; diagnostics, logging or telemetry; test framework; Flight-level replay classification and first-slice-specific design. Class B applies where realization fixes a difficult-to-reverse source, time, architecture, integrity, or retained-classification boundary; otherwise controls and tooling are Class C | Bounded technical decision inside each vertical slice | Decide only the minimum subset before each slice uses it; the first slice must include enough materialized input, replay state, source time, deterministic delivery, observability, and approved Flight classification to validate its path | Depends on C10 replay delivery, normal C4/C5 boundaries, C3 classification, C6–C9 normal behavior, and strict exclusion of Generator truth; Generator architecture and materialization belong to the separate Scenario Generator boundary and issue #47 | Deferred per slice; no standalone replay infrastructure iteration or Generator implementation is authorized |

Deferral means that each decision is made deliberately in the bounded future work that first requires it. It does not authorize an implementation agent to select product semantics or difficult-to-reverse architecture silently. The former D8 placeholder is removed because issue #35 now records dependencies, risks, decision timing, and sequence directly in sections 13 and 14.

---

# 8. Explicit MVP 0.1 Engineering Exclusions

MVP 0.1 has no mandatory dependency on:

- cloud backend, account, authentication, or server-side processing;
- synchronization between users, devices, or clients;
- another AirLink user, social behavior, crew coordination, or multi-user operation;
- connected aircraft equipment or an external flight computer;
- route planning, route progression, or active route navigation;
- landing-zone logic or landing assistance;
- in-flight gust estimation;
- replay presentation, advanced analytics, or completed-Flight editing;
- permanent network connectivity;
- web or iOS clients;
- the complete Pilot Ecosystem;
- a complete future AirLink architecture.

These exclusions are bounded MVP simplifications. They do not reject or redefine future AirLink domains.

---

# 9. Product Direction Alignment

- **Direction advanced:** the first coherent local Flight Support outcome spanning preparation, Flight, completion, retention, and later review.
- **Explicit simplification:** MVP 0.1 is mapped only at concern, authoritative-ownership, mandatory-flow, input-category, semantic-fidelity, observability, external-constraint, dependency, risk, decision-timing, and high-level product-wave level. Future implementation is organized through bounded vertical product outcomes rather than a complete backlog or concern-by-concern build sequence.
- **Approval authority:** the planning depth and simplification are authorized by `ITERATION.md`, issues #32–#35, the owner-reviewed MVP 0.1 planning boundary, and the owner decision record in issue #45. Issue #45 supersedes only the runtime-generation aspects of earlier simulation planning.
- **Boundedness:** the map applies only to MVP 0.1 engineering planning under AL-0002.
- **Reversibility:** each implementation wave expands only the necessary subset of C1–C10; replay grows inside product slices; Android constraints remain visible before live integration; source mode, replay origin, runtime provenance, delivery handling, and Flight classification remain separate; no final components, APIs, schemas, providers, algorithms, stores, or complete architecture are selected.
- **Early risk treatment:** estimated wind remains a central intended Flight Support value and is scheduled for controlled risk reduction through replayed source-equivalent observations and independent out-of-band validation evidence.
- **Intentionally deferred:** concrete Android live-source integration until a coherent replay-driven core exists; exact replay architecture and retained replay provenance to resumed issue #37; Generator architecture, formulas, fidelity, and delivery path to issue #47; Route, Equipment, Airspace, wider Flight Support, Pilot Ecosystem, cloud, web, iOS, and future architecture.
- **Product-pillar boundary:** mandatory simulation enables validation; Scenario Generator is a separate enabling subproduct boundary, not a third product pillar, while C10 remains an AirLink runtime concern.
- **Product behavior and authority:** no new behavior is inferred beyond the owner-approved issue constraints, and this WIP planning artifact remains non-canonical and non-authoritative for implementation.
- **Outcome:** `Aligned with explicit simplification`.

Any new product-semantic simplification, irreversible constraint, or expansion beyond these bounds requires a separate owner decision.

---

# 10. Review Contract

Review this map at concern, mandatory-flow, dependency, risk, decision-timing, and high-level sequence level.

A valid review should verify that:

1. every accepted MVP 0.1 flow is represented in section 4;
2. every important state or information category has one authoritative owner in section 3;
3. every mandatory flow has a complete trigger–demand–producer–owner–consumer path at concern level where external acquisition is required;
4. no concern silently assumes authority owned by another concern;
5. every relevant unresolved product question is explicitly recorded in section 7.2;
6. accepted semantic product rules are not reclassified as deferred engineering choices;
7. deferred implementation mechanics remain deferred;
8. no final architecture, API, schema, provider, algorithm, or complete internal message graph is implied;
9. every required source category has one common live/replay C4/C5 normalization or interpretation boundary;
10. delivery mode, replay origin, category-level provenance, delivery handling, source time, observed time, and Flight-level classification remain separate;
11. C10 owns replay and source delivery only, while Generator phases, truth, formulas, and source-value generation remain outside AirLink runtime;
12. the frozen-stream and replay contract defines identity, compatibility/integrity, source-monotonic time, equal-time ordering, status events, Start, Pause, speed, Reset, batching, delay, redelivery, collision, and approved delivery transforms at planning level;
13. C2–C9 retain normal responsibility for all live and replay inputs;
14. resumed issue #37 owns exact retained replay/source-origin provenance and Summary presentation, while issue #47 owns fuller Generator definition;
15. mandatory observability distinguishes replay delivery state from C4/C5 source interpretation and keeps any out-of-band Generator truth unavailable to normal behavior;
16. semantic, runtime-information, external, and implementation-order dependencies remain distinct, and the runtime concern graph is not presented as a module-build sequence;
17. implementation order uses bounded vertical slices that produce observable product outcomes and introduce only the necessary subset of concerns;
18. the first slice includes minimum C10 replay capability inside the slice rather than requiring standalone replay infrastructure;
19. Android feasibility constrains early boundaries while concrete live-source integration remains later than a coherent replay-driven product path;
20. risk severity, risk-reduction order, and implementation order remain distinct;
21. estimated wind remains an early risk-reduction target without Generator truth entering C7 as an answer;
22. P1–P6 remain unresolved with explicit latest safe decision points, D1–D7 remain deferred to bounded future work, and no obsolete D8 deferral remains;
23. decision Classes A–D classify authority and reversibility without selecting architecture or another Class B realization;
24. the future sequence remains a high-level set of product waves compatible with issue #36 rather than a complete backlog or first-slice selection.

Do not treat the absence of implementation mechanics as a defect unless that absence leaves an accepted product flow, ownership boundary, difficult-to-reverse decision, accepted semantic invariant, or required first-slice dependency undefined.

---

# 11. Issue #33 Baseline Acceptance Record

Owner approval and merge of the issue #33 baseline confirmed that:

- the MVP 0.1 Scope is sufficient for this planning level;
- the engineering boundary in section 1 is accepted;
- concern contracts C1–C10 are accepted as responsibilities rather than final components;
- ownership entries O1–O14 are accepted;
- mandatory flows F1–F16 cover the accepted MVP outcome without inventing unresolved product behavior;
- external dependency and degradation boundaries are accepted;
- cross-cutting rules R1–R14 preserve the approved product distinctions;
- product decisions P1–P6 remain explicitly unresolved and are not delegated to implementation;
- engineering deferrals D1–D7 remain assigned to appropriate later bounded work; the issue #33 baseline's former D8 reservation is now completed by issue #35 content in sections 13 and 14;
- the document remains WIP, non-canonical, and non-authoritative for implementation;
- no implementation or final architecture has been introduced.

---

# 12. Issue #34 Extension and Issue #45 Correction Record

Issue #34 established the common normal-boundary principle, category-level provenance, source-independent downstream behavior, normal recording, and mandatory observability. Those accepted outcomes remain.

Issue #45 supersedes the parts of issue #34 that assigned scenario state, truth, physical/source-value generation, cadence/error/availability materialization, or validation truth to C10. Those responsibilities now belong to the separate Scenario Generator boundary. C10 remains in the map as **Replay and Source Delivery Enablement** and delivers already materialized observations through normal C4/C5 boundaries.

The existing synthetic-simulation Flight distinction and deletion requirement remain accepted, but resumed issue #37 must define how replay mode and replay source origin map to retained Flight and Summary provenance, especially for normalized recorded-real-Flight streams. This correction does not authorize implementation, promote this WIP artifact, or activate AL-0003.

---

# 13. Issue #35 — Dependencies, Risks, Decisions, and Future Slices

## 13.1 Concern-level dependency model

C1–C10 are responsibility boundaries, not final components. Four dependency types must remain distinguishable:

| Dependency type | Meaning in this map | Ordering consequence |
| --- | --- | --- |
| Semantic dependency | One concern needs product meaning or authority owned by another concern before it can interpret an outcome correctly | The supplying meaning must be accepted before dependent implementation reaches it; this does not require the owner concern to be completed first |
| Runtime information dependency | One concern consumes state or information produced by another during an accepted flow | The vertical slice must connect the necessary producer, owner, consumer, validity, and degradation path; it does not prescribe an internal call, event, or message architecture |
| External dependency | AirLink relies on platform, device, provider, map, network, storage, or clock behavior outside its ownership | The slice must preserve AirLink's interpretation and explicit failure boundary; concrete integration occurs only when the slice requires it |
| Implementation-order dependency | A risk, product decision, or difficult-to-reverse contract must be reduced or decided before later implementation can safely depend on it | Implementation order is expressed through bounded vertical slices and decision gates, never by treating the runtime concern graph as a module-build sequence |

The runtime concern graph is not a construction sequence. A concern may be introduced minimally in one slice and expanded in later slices. Concern dependencies describe semantic and runtime responsibility relationships; they do not require all of C4, C9, C10, or another concern to be built as an isolated foundation before C1–C3 product behavior.

Major dependency structures are:

| Structure | Principal dependency relationships | Implementation-order interpretation |
| --- | --- | --- |
| Runtime input backbone | C4 normalizes live or replayed runtime inputs and exposes validity, timing, provenance, availability, and degradation. C5 owns weather scoping and interpretation. C10 delivers approved frozen replay events but does not replace C4/C5 ownership of source state and meaning | Each slice introduces only the input categories and replay behavior needed for its observable outcome; no complete input platform or replay subsystem is built first |
| Lifecycle spine | Conceptually preserve `C1 pilot action → C2 Flight Mode authorization → C6 boundary confirmation → C2 authorization → C3 Flight transition → C9 recording/retention` | This records authority and semantic dependency, not a mandatory technical call sequence. A slice may exercise a bounded subset while preserving every reached ownership boundary |
| Active-Flight information | C7 depends on C4 runtime inputs, C3 Flight context, and conditionally C5 pressure/QNH. C8 depends on C4 spatial inputs, C3 Flight and Takeoff Point context, C7 orientation/bearing outputs, and C9 retained spatial information for saved review | Derived and spatial behavior is added only with the normal context, provenance, and validity path required by the slice; C8 does not recalculate C7 meaning and saved review does not redefine C9 history |
| Validation | C10 crosses replay activation, deterministic delivery, source-mode context, and observability boundaries. C3 associates approved classification at Flight creation; C9 preserves it. All product behavior continues through C2–C9 | Minimum C10 replay capability belongs inside the first and later vertical slices. It must not become an alternative lifecycle, calculation system, recorder, or standalone infrastructure wave |

## 13.2 Engineering-risk register and reduction order

Risk severity describes consequence. Reduction order describes when evidence or a decision gate is needed. Implementation order describes the bounded product slice that supplies that evidence. These are not interchangeable: a severe risk may require an early compatible boundary or experiment without requiring its complete capability in the first slice.

The register is ordered by earliest required risk-reduction attention. Adjacent items may be reduced in the same vertical slice, and estimated-wind work must begin within the first several implementation iterations.

| Reduction order | Risk and severity | Consequence and affected concerns or flows | Recommended reduction strategy | Required decision gate and ordering rationale | First-slice constraint |
| --- | --- | --- | --- | --- | --- |
| 1 | Irreversible loss of historically necessary Flight data — Critical | Early Flights cannot later support faithful review or replay; C3/C4/C7/C9 and F5/F8/F15 are affected | Define the minimum logical retained-data and historical-preservation contract before the first durable result; validate that original semantic status, provenance, classification, boundaries, and needed time-varying values survive | Class B D5 decision before persistence depends on it; P3/P4 before their exceptional retained meanings. Irrecoverable omission makes this earlier than storage optimization | If the slice retains a Flight result, it must preserve the minimum approved history and classification rather than a disposable summary-only record |
| 2 | Unresolved Flight and Flight Mode lifecycle semantics — Critical | A guessed transition can corrupt Flight identity, completion, retention, repeated-Flight behavior, or pilot intent across C1/C2/C3/C6/C9 and F2–F14 | Select a slice path that uses only accepted transitions; expose reached unresolved boundaries and stop there | P1–P5 at their section 7 latest safe points; Class B identity and timestamp choices only when required. Product meaning precedes mechanism | The slice must show real runtime state and may not invent active-Flight exit, interruption, manual Landing Point, detection-confirmation, or inactivity semantics |
| 3 | Untrustworthy replay or observability — High | Passing demonstrations could use alternate semantics, hidden Generator truth, incorrect provenance, or uninspectable delivery; C3–C10 and F16/F17 are affected | Put minimum deterministic C10 replay state, source time, ordering, classification context, degradation delivery, and concern-owned observability inside the first slice; keep Generator truth out of runtime | Relevant D7 subset before the slice runs; any Class B source/time/integrity realization receives bounded decision treatment | Mandatory: enough C10 and observability to exercise the slice through normal boundaries; no standalone replay subsystem |
| 4 | Takeoff and landing detection — High | False or late boundaries can create, omit, truncate, or misclassify Flights and special points across C2/C3/C4/C6/C9 and F4/F7/F11 | Start with controlled lifecycle-driving inputs and observable candidates/outcomes; compare reversible detector approaches before permanent semantics and hardening | P2 before permanent automatic detection; P4 before manual Landing Point representation; D2 remains Class C behind accepted boundaries | A first slice need not solve the permanent detector, but any automatic boundary it uses must be bounded, observable, and must not claim unresolved semantics |
| 5 | Estimated-wind feasibility and usefulness — High | A central Flight Support value may prove unstable, misleading, or incompatible with retained/source boundaries; C4/C7/C9/C10 and F5/F16 are affected | Preserve required input, time, derivation, provenance, Generator-truth separation, comparison diagnostics, and retained context from the first slice; run controlled validation in an early following wave | Relevant D3/D5/D7 decisions before wind output is presented or retained. Early evidence is required because late failure would undermine core product value | The first slice need not implement complete wind estimation, but it must not create incompatible boundaries or postpone the risk until general MVP completion |
| 6 | Orientation and spatial semantics — High | Collapsing Track, Heading, bearing, device orientation, or True North can mislead the pilot and constrain future navigation; C4/C7/C8 and F6/F15 are affected | Preserve semantic identities and validity from the input boundary; use reversible simulated experiments and degraded cases before final presentation policy | P6 before final policy; D4 decisions per spatial slice. Semantic separation precedes provider or rendering optimization | If spatial behavior is included, it must use normal C4/C7/C8 boundaries and remain compatible with later P6 resolution |
| 7 | Local persistence and interruption recovery — High | Partial recording, process loss, or failed restoration may silently lose or misstate a Flight; C3/C4/C9 and F5/F14/F15 are affected | Validate progressive recording status and explicit failure with controlled interruption; harden recovery after product classification is decided | P3 before recovery semantics; Class B D5 schema/recovery and D1 execution decisions before production hardening | Minimal retention must expose success or failure; the slice must not imply that storage failure changes airborne state or completion meaning |
| 8 | Android lifecycle and continuous-acquisition behavior — High | A replay-correct path may fail under real permissions, execution limits, source loss, or backgrounding; C2–C4/C6/C9 and active-Flight flows are affected | Keep lifecycle, acquisition-demand, time, provenance, and interruption boundaries Android-feasible from the beginning; integrate live sources after a coherent replay path, then harden before real-flight validation | Class B D1 Android execution model before Phase 4 continuous acquisition; P3 before interruption recovery meaning. Concrete integration is later because stable product behavior should exist first | No live Android source is required, but the first slice may not assume uninterrupted wall time, permanent availability, or unconstrained execution |
| 9 | External provider coupling — Medium | Weather or map failure or replacement could leak provider meaning into product semantics or block local Flight behavior; C5/C8 and F1/F6/F15 are affected | Preserve provider-neutral AirLink interpretation, freshness, availability, and degradation boundaries; defer selection until a consuming slice needs it | Relevant Class C provider choice and any Class B boundary decision before integration; local lifecycle and recording must remain provider-independent | The first slice must not select providers unless explicitly required, and no provider may become lifecycle or calculation authority |
| 10 | Infrastructure-first planning without product outcomes — High | Time may be spent completing layers, storage, scaffolding, replay infrastructure, or Generator tooling while no pilot-visible outcome or integrated risk evidence exists | Require each future iteration to deliver a bounded end-to-end outcome using only necessary concern subsets, minimum replay, observability, and retained result where meaningful | Issue #36 rejects infrastructure-only candidates; later charters retain this gate | Mandatory: the first slice cannot be storage-only, scaffolding, dependency injection, a map/application shell, standalone replay infrastructure, or a standalone Generator |

## 13.3 Bounded minimum replay concept

Early end-to-end validation uses one approved frozen source-equivalent stream, not an in-runtime simulator. The stream may represent the selected paramotor movement profile and carry only the source observations and explicit status events required by the slice. C10 selects and replays that materialized input with deterministic source-time order and bounded delivery controls.

The separate [Scenario Generator boundary](scenario-generator.md) owns any phase model, truth motion, physical calculation, source cadence, source error, availability baseline, and frozen-stream export required to create a synthetic stream. AirLink runtime receives none of those generation instructions or privileged truth.

The validation relationship is:

`Generator materialization → frozen source-equivalent observations → C10 replay → normal C4/C5 inputs → independent C7 estimated wind`

Out-of-band expected evidence may compare the derived result after normal processing, but it is not available to C7 or other product behavior. Exact stream serialization, replay storage, adapter architecture, and concrete fixture remain deferred to resumed issue #37; full Generator definition remains deferred to issue #47.

## 13.4 High-level future implementation sequence

The sequence is a set of risk-ordered product waves, not a committed roadmap, fixed issue list, complete backlog, or authority to implement. A wave contains multiple bounded vertical slices where needed, a later wave may begin before every behavior in an earlier wave is complete, and issue #36 remains responsible for candidate comparison and explicit first-slice selection.

### Phase 1 — Replay-driven product core

Bounded vertical slices progressively connect pilot-facing action, Flight Mode and Flight state, materialized replay inputs, observable lifecycle behavior, completion, a minimal retained result, and approved replay/source classification. Each slice introduces only the necessary subset of C1–C10 and minimum C10 replay capability.

### Phase 2 — Early estimated-wind risk reduction

Within the first several implementation iterations, introduce only the replay and runtime capabilities needed to test Ground Track, Ground Speed, independent C7 estimation, out-of-band comparison evidence, and retained calculation context where required. This wave may overlap with adjacent lifecycle, spatial, recording, or saved-result slices. It does not require complete wind behavior or complete Generator tooling.

### Phase 3 — Broader replay-based MVP behavior

Progressively cover missing replay-driven product behavior such as Takeoff Point and spatial awareness, multiple Flights, Summary, waiting and automatic exit, manual completion and discard, current conditions, Pre-Flight, degraded cases, and saved review. These outcomes remain separate bounded slices where appropriate.

### Phase 4 — Android live-input integration

After a coherent replay-based application path exists, integrate real position, movement, orientation, altitude, pressure, time sources, lifecycle signals, permissions, and availability states. Preserve the C4/C5 and downstream boundaries already exercised by replay.

### Phase 5 — Real-flight readiness

Before any bounded real-flight validation, address foreground/background behavior, interruption and restoration, acquisition continuity, recording reliability, battery and resource behavior, source degradation, detector behavior, diagnostics, live-versus-simulated semantic consistency, and cleanup or separation of simulated Flights. This phase establishes readiness evidence; it does not define actual real-flight test procedures.

## 13.5 First-slice selection constraints

Issue #35 constrains but does not perform issue #36. A valid first-slice candidate must:

- produce a real pilot-visible result;
- include runtime state;
- use normal concern boundaries;
- include the minimum replay and source-delivery capability needed by the slice;
- provide sufficient observability;
- produce a minimal retained result where meaningful;
- reduce at least one material risk;
- remain bounded and reversible;
- avoid requiring resolution of every P1–P6 decision;
- avoid infrastructure-only work;
- avoid standalone replay infrastructure or a standalone Generator;
- avoid a map shell, storage-only task, application shell, scaffolding task, or dependency-injection task;
- avoid silently choosing complete architecture or a Class B realization outside bounded authority;
- preserve the possible future independence of Route, Equipment, Airspace, Pilot Ecosystem, web, iOS, cloud, and connected domains.

Issue #36 must compare candidates and obtain explicit owner selection. This document neither compares nor selects them.

---

# 14. Issue #35 Acceptance Check

The acceptance record for issue #35 is explicit owner acceptance and merge of PR #41 after confirmation that:

- issue #33 baseline and issue #34 extension are recorded as accepted and merged, and issue #35 approval is recorded only through explicit owner acceptance and merge of PR #41;
- semantic, runtime-information, external, and implementation-order dependencies are explicit and remain distinct;
- C1–C10 remain concern responsibilities rather than final components, and the runtime graph is not a module-build sequence;
- the runtime input backbone, lifecycle spine, active-Flight information dependencies, and validation boundary preserve accepted ownership without defining APIs or internal message architecture;
- external Android, device, provider, map, storage, clock, interruption, and resource constraints identify ordering effect, latest safe point, explicit failures, and authoritative concerns without selecting a provider or technology;
- the risk register distinguishes severity, reduction order, and implementation order and covers historical data, lifecycle semantics, simulation trust, Android behavior, detection, estimated wind, spatial semantics, persistence/recovery, provider coupling, and infrastructure-first planning;
- minimum C10 replay capability is part of the first vertical slice and grows incrementally rather than becoming a standalone infrastructure iteration;
- the bounded replay concept consumes materialized source-equivalent streams and does not expose Generator scenario/truth metadata to runtime behavior or AirLink Route/navigation state;
- Generator truth remains out of band and separate from C7's independent estimate;
- estimated wind is an early risk target within the first several implementation iterations and is not postponed until general MVP completion;
- Android constraints are visible from the first slice where relevant, concrete live integration follows a coherent replay-driven core, and Android reliability precedes real-flight readiness;
- P1–P6 remain unresolved with explicit latest safe decision points and stop boundaries;
- decision Classes A–D describe authority and reversibility without selecting a Class B decision or final architecture;
- D1–D7 are consolidated with triggers and governing later work, and D8 no longer claims completed issue #35 work is deferred;
- the five implementation phases remain high-level risk-ordered product waves rather than a complete backlog or fixed issue sequence;
- first-slice constraints are explicit while candidate comparison and selection remain reserved for issue #36;
- Product Direction alignment remains `Aligned with explicit simplification`;
- no application architecture, mobile framework, Android API, provider, database, schema, algorithm, exact simulation equation or format, complete UI, implementation code, or executable prototype is introduced.

Creation of this extension and acceptance check does not itself record approval. Explicit owner acceptance and merge of PR #41 records approval of the issue #35 extension and the consolidated MVP 0.1 Engineering Map as an AL-0002 planning input. That approval does not promote the document to canon, make it implementation authority, select the first vertical slice, start issue #36, activate AL-0003, or authorize product implementation.
