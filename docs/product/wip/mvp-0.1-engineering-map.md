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
- the live-input and simulation-substitution extension prepared for owner review under GitHub issue `#34 / AL-0002-02`.

The following extension remains reserved for later bounded work:

- issue `#35 / AL-0002-03` — dependency order, risk order, decision order, deferred-decision consolidation, and candidate implementation sequence.

Approval of the issue #33 content does not constitute approval of the issue #34 extension or the complete Engineering Map. Final owner approval of the consolidated map occurs only after the bounded work of issues #34 and #35.

## Purpose

This document describes the minimum engineering structure required to treat MVP 0.1 as one coherent system without designing its final component architecture.

At the current planning depth, it defines:

- the AirLink engineering boundary for MVP 0.1;
- the major engineering concerns and their responsibility contracts;
- authoritative ownership of important runtime state and information;
- coverage of the mandatory MVP 0.1 product flows;
- external dependency and degradation boundaries;
- accepted cross-cutting responsibility rules;
- unresolved product decisions and intentionally deferred engineering decisions.

Concern identifiers in this document are planning references. They do not prescribe modules, packages, classes, services, processes, deployment units, repositories, or dependency-injection boundaries.

## Planning Depth and Completeness Standard

This document is a concern-level engineering map for the whole MVP 0.1. It is not an implementation specification and is not intended to describe every internal message, event, command, API, schema, class, module, service, or data-transfer mechanism.

For the purposes of this map, planning is complete when:

- every mandatory MVP 0.1 product flow has an explicit trigger, responsible concerns, required conceptual path, and observable result;
- every important runtime state or information category has one authoritative owner;
- required producers and consumers are connected at concern level;
- responsibility and non-ownership boundaries prevent implementation agents from inventing product semantics or transferring authority silently;
- external dependency and degradation consequences that affect product behavior are explicit;
- unresolved product decisions and intentionally deferred engineering decisions are recorded rather than guessed.

The map describes **principal conceptual handoffs** only. A handoff identifies which concern supplies information or requests a transition, which concern owns the resulting state or interpretation, and which concern requires the outcome. It does not prescribe how that handoff is technically implemented.

Missing concern-level ownership or a missing path required by an accepted MVP flow is a defect in this document. Missing implementation mechanics are not defects unless they are required to preserve product meaning, avoid a difficult-to-reverse decision, or prepare the separately selected first implementation slice.

Implementation-ready depth is intentionally reserved for the selected first vertical slice and its governing later issues. Issue #34 extends this map at whole-MVP concern level, and issue #35 will extend it with ordering and sequencing, without converting it into a complete architecture.

## Governing Context

Work under this document follows the source-of-truth order and Product-Significance Routing defined in `AGENTS.md`.

The issue #34 extension uses the issue #33 owner-approved baseline and the following task-specific context:

- `ITERATION.md`;
- `docs/product/CurrentState.md`;
- relevant canonical product documentation;
- GitHub issues #32–#34 and approved task artifacts;
- the owner-reviewed `docs/product/wip/mvp-0.1-scope.md` planning baseline;
- directly relevant Flight Mode, Flight, and Navigation WIP.

Consultation does not promote WIP into canon or make this planning artifact implementation authority. If governing sources conflict, the conflict must be reported rather than silently resolved.

---

# 1. Planning Sufficiency and Engineering Boundary

## 1.1 Planning-sufficiency assessment

The existing MVP 0.1 Scope and the owner-approved issue #33 Engineering Map are sufficient as the WIP planning baseline for the concern-level work required by issue #34.

No contradiction or omission currently blocks:

- definition of the MVP 0.1 engineering boundary;
- identification of major concerns;
- separation of Flight Mode and Flight responsibilities;
- state and information ownership;
- concern-level product-flow coverage;
- external-dependency classification;
- live-input categorization, conceptual substitution, provenance, semantic fidelity, retained simulated-Flight behavior, and mandatory observability under issue #34.

No blocking product contradiction or omission was found, no product-scope correction is required, and no additional owner decision blocks issue #34 review.

Issue #34 proceeds under five explicit owner-approved constraints:

1. mandatory simulation fidelity is semantic and behavioral rather than physically exact;
2. controlled weather and pressure simulation are required without selecting a provider or provider protocol;
3. mixed-source runs are required when each value retains live, selected, or simulated category-level provenance, pass-through or controlled-substitute handling remains a separate dimension where a C10 run governs the category, and requested and actually active provenance and handling configurations remain explicit;
4. simulated Flights use the normal recording and retention flow, remain durably distinguishable from non-simulated Flights, and support deletion individually or as the simulated-Flight category without deleting non-simulated Flights;
5. every Flight created while a C10-controlled simulation run is active is classified as simulated, regardless of the run's live, selected, or simulated provenance composition and pass-through or controlled-substitute handling composition; C10 owns the run state, C3 associates the resulting Flight-level classification, C9 preserves it durably, and C1 presents it.

These decisions settle the product-semantic boundary needed by issue #34. Architecture, source-selection mechanics, scenario representation, controls, exact simulated parameters, physical and sensor models, storage realization, diagnostics presentation, automation, and first-slice-specific simulation design remain intentionally deferred.

Some product and domain decisions remain unresolved. They are recorded in section 7 because they must not be selected silently during implementation. They do not prevent the concern-level boundary from being mapped.

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
- simulation-controlled substitution required to develop and validate accepted behavior;
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
- external tooling not incorporated into the AirLink-controlled simulation capability.

AirLink does not own those external systems. AirLink does own their integration meaning, validity and freshness interpretation, product-level degradation consequences, and pilot-visible unavailable or degraded state.

## 1.3 Simulation boundary principle

Simulation and Validation Enablement is inside the AirLink engineering boundary as a product-enabling validation capability.

Normal end-to-end simulation substitutes or controls approved external input production and uses the same downstream concerns as live input. C10 owns whether its controlled simulation run is active, the requested provenance and handling configuration for governed categories, and production of controlled selected or simulated substitutes. Upstream values used with pass-through handling may reach C4 directly, and live weather used with pass-through handling may reach C5 directly; C10 does not proxy them. C4 and C5 own the actually active AirLink-facing source state, actual live, selected, or simulated provenance, actual handling result where a C10 request applies, availability, validity, freshness, and degradation within their respective boundaries.

Every Flight created while a C10-controlled simulation run is active is classified as simulated regardless of the run's provenance or handling composition. C10 supplies the active run context when C3 creates the Flight; C3 associates the Flight-level simulation classification with Flight identity; C9 preserves the supplied classification durably; and C1 presents it. A Flight created outside a C10-controlled simulation run remains non-simulated and is not reclassified from individual input provenance or handling. Category-level input provenance and any relevant handling context remain independently preserved and are not collapsed into, or used by C9 to infer, Flight-level classification.

Source substitution changes input production and category-level provenance, not product meaning. It must not create an alternative Flight Mode, Flight lifecycle, calculation model, spatial model, record-construction path, or pilot-facing product behavior.

Minimum acceptable fidelity is semantic and behavioral. A run must be able to provide ordered and controllable time, coherent position and movement, mutually meaningful speed, altitude, vertical movement, orientation, pressure, and weather context where required, lifecycle-driving takeoff and landing conditions, Takeoff Point spatial relationships, multiple Flights in one Flight Mode period, recording and retained results, and intentional unavailable, invalid, stale, degraded, or interrupted conditions. The resulting relationships must be sufficient to exercise normal C6 detection, C7 derivation, C8 spatial awareness, C9 recording and retention, immediate Summary, and later saved review.

This boundary does not require a physical flight simulator. Aerodynamics, flight dynamics, wing, engine, pilot-control, turbulence, exact sensor physics, exact sampling, latency, numerical reproduction of a physical Flight, statistical noise, error models, and detailed synthetic-data algorithms remain deferred unless later bounded work demonstrates that one is required to preserve product meaning.

Issue #34 defines this whole-MVP boundary at concern level. Exact substitution mechanisms, scenario controls, implementation architecture, and first-slice simulator design remain reserved for the bounded work that requires them.

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
- association of Flight-level simulation classification with Flight identity at creation, using the active C10 simulation-run context;
- effective takeoff, confirmed-landing, and manual-completion boundaries;
- association of information with one Flight;
- elapsed Flight time and Flight-scoped aggregates;
- Takeoff Point identity, estimated location, and association with the Flight;
- confirmed Landing Point identity, estimated location, confirmed-landing classification, and association with the completed Flight;
- continuity of the normally completed Flight through landing confirmation, including its final recorded segment without retrospective trimming;
- runtime completion, rejection, and finalization of the individual Flight.

**Consumes**

- lifecycle authorization and operational context from C2;
- active C10 simulation-run context when a Flight is created;
- monotonic time from C4;
- derived-value and aggregate updates from C7;
- recording and retention outcomes from C9 where they affect pilot-visible completion status.

**Produces**

- active Flight identity, lifecycle context, and Flight-level simulation classification for C1, C7, C8, and C9 wherever required;
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
- availability, validity, freshness, accuracy or quality metadata, and live, selected, or simulated category-level provenance;
- actually active pass-through or controlled-substitute handling status where a C10 run governs a category;
- actually active normalized runtime-source state and configuration for the categories C4 normalizes;
- platform lifecycle and interruption signals exposed at the AirLink boundary;
- execution of concern-level acquisition demand through deferred platform and resource-management mechanisms.

Relevant input categories include position, movement and Ground Speed source information, course or Track source information, device orientation, altitude, vertical movement, pressure, wall-clock and monotonic time, platform lifecycle and interruption signals, permission and source-availability state, and current-location context.

**Consumes**

- bounded current-location acquisition demand from C5;
- Flight Mode acquisition demand from C2;
- requested simulation-run provenance and handling configuration from C10 where it affects C4-owned categories;
- available device, platform, and simulated-source information.

**Produces**

- normalized current-location context, including availability, validity, freshness, and provenance, for C5;
- normalized runtime information for C2, C3, C6, C7, C8, and C9 as required by the covered flows;
- actually active source, activation, availability, validity, freshness, provenance, and handling outcomes for consumers and C10 observability.

**Does not own**

- product lifecycle transitions;
- the product decisions that current-location weather or Flight Mode require acquisition;
- detection confirmations;
- derived semantics;
- Flight aggregates;
- presentation or historical retention;
- requested simulation-run provenance or handling configuration, or C10 simulation-run state.

For every category it normalizes, C4 preserves source identity and category-level provenance as live, selected, or simulated. Where a C10 run governs the category, C4 separately owns the actually active handling result: pass-through when an existing upstream value is used without C10 replacement, or controlled substitute when C10 supplies or controls an approved selected or simulated equivalent. Pass-through preserves upstream provenance, so a live value used with pass-through handling remains live; C4 must not infer provenance from handling. C4 also preserves source time, AirLink-observed time when meaningfully distinct, wall-clock versus monotonic semantics, availability, validity, freshness or staleness, supplied quality or accuracy, and intentional degradation. Source identity, provenance, and handling changes must be explicit and must not silently switch, merge, or upgrade one another. C10 requests handling and any controlled-equivalent provenance, but C4 owns and exposes the actually active normalized source and handling result; upstream values used with pass-through handling need not pass through C10. Outside a C10-controlled run, handling may be absent or not applicable.

## C5 — Weather Context

**Owns**

- the product need for current-location weather context in normal application and Pre-Flight use;
- geographic scoping and interpretation of current-location weather acquisition;
- current observed wind, gusts, direction, pressure or QNH, and observation or update time;
- near-term forecast when available;
- observation-versus-forecast distinction;
- actually active weather-source interpretation, availability, freshness, validity, live, selected, or simulated category-level provenance, C10-governed handling result where applicable, and degraded state.

**Consumes**

- current-conditions requests from C1;
- normalized current-location context, including availability, validity, freshness, and provenance, from C4;
- requested simulation-run weather provenance and handling configuration from C10;
- external weather-provider information;
- controlled simulated weather-provider equivalents from C10 through the same weather-input interpretation boundary used for external weather information.

**Produces**

- bounded current-location acquisition demand for C4;
- weather context and status for C1;
- pressure or QNH context for C7 when the selected altitude representation requires it;
- actually active weather-source, provenance, handling, and activation outcomes for C10 observability.

**Does not own**

- location acquisition or normalization;
- automated suitability or safety approval;
- the in-flight estimated-wind value;
- the altitude model;
- Flight lifecycle or recording;
- C10 simulation-run state or requested provenance and handling configuration.

C5 applies the same observation-versus-forecast, source-time, AirLink-observed-time, validity, freshness, and degraded-state meaning to weather with live, selected, or simulated provenance. Where C10 governs weather, C5 separately owns the actually active pass-through or controlled-substitute handling result. Live weather used unchanged remains live provenance with pass-through handling; simulated weather has simulated provenance with controlled-substitute handling. C10 requests handling and may supply a controlled simulated equivalent, but C5 owns the actually active weather interpretation and handling result, does not infer provenance from handling, and receives live weather without C10 proxying it. Simulated weather does not create a second Weather Context concern or provider-specific product behavior.

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
- active Flight, Flight-level simulation classification, and Takeoff Point context from C3;
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

C7 preserves the source dependencies and semantic status required to understand derived output. A derived value may depend on multiple sources, each of which retains its own live, selected, or simulated category-level provenance and, where needed to explain a C10 validation context, its separate pass-through or controlled-substitute handling status. Those dependencies must not be silently presented or retained as if every contributing source were live, source substitution must not select an alternative calculation meaning, and C3-supplied Flight-level simulation classification remains a separate semantic axis.

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
- active Flight, Flight-level simulation classification, and Takeoff Point context from C3;
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

C8 uses the same spatial semantics regardless of live, selected, or simulated category-level input provenance, pass-through or controlled-substitute handling where a C10 run applies, and single-source or mixed-source provenance composition. A selected starting point or route may constrain source production for validation, but it does not enable Active Navigation, transfer C8 state ownership to C10, or create a simulation-only map interpretation. C3-supplied Flight-level simulation classification remains separate from spatial input provenance and handling.

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
- durable preservation of the C3-supplied simulated or non-simulated Flight-level classification throughout progressive recording, finalization, retention, retrieval, Summary, and saved review;
- deletion of one retained simulated Flight and deletion of all retained simulated Flights as a category, without deleting non-simulated Flights;
- technical recoverability mechanisms, subject to unresolved interruption semantics.

**Consumes**

- active Flight identity, lifecycle markers, Flight-level simulation classification, Takeoff Point identity, estimated location and Flight association, final boundaries, final aggregates, and confirmed Landing Point information from C3;
- normalized track and other approved retained inputs from C4;
- selected derived values and their approved historical semantics from C7;
- category-level source and derivation provenance, plus separately relevant handling context, supplied through the normal C4/C7 recording inputs;
- scoped requests to delete one retained simulated Flight or all retained simulated Flights.

**Produces**

- recording health and retention status for C1 and C3;
- retained summary fields, C3-supplied Flight-level simulation classification, special-point information, historically preserved values, and track for C1 and C8 as required by approved presentation contracts;
- durable Flight reference when available;
- success or failure of false-detection episode deletion;
- success or failure of individual or bulk simulated-Flight deletion, without transferring record ownership to C1 or C10.

**Does not own**

- whether the pilot is airborne;
- Flight Mode or Flight lifecycle;
- boundary detection;
- confirmed Landing Point meaning or classification;
- pilot retain-versus-discard choice;
- Flight-level simulation classification or inference from recorded provenance or handling composition;
- current-value calculations or later reinterpretations;
- presentation.

C9 durably preserves approved category-level provenance as live, selected, or simulated and does not treat pass-through as an alternative provenance type. It preserves the historical context required to represent what was available or used during the original Flight; where handling status is required for that interpretation, pass-through or controlled-substitute handling remains separate from provenance. C9 must preserve the Flight-level simulation classification supplied by C3 and must not derive, infer, or reclassify it from recorded provenance or handling. One physical store or separate stores, the exact durable representation of provenance and handling under D5/D7, history filtering, visual marking, grouping, default retention, automatic cleanup, storage technology, and deletion mechanics remain deferred.

## C10 — Simulation and Validation Enablement

**Owns**

- authoritative simulation-run state, including whether a C10-controlled simulation run is active, plus scenario state and progress at a conceptual level;
- requested pass-through or controlled-substitute handling for each governed category, plus requested provenance where C10 provides a controlled equivalent;
- controlled production of approved selected or simulated substitutes;
- selected starting-point or route context used only to control approved input production;
- simulated-time control where required to preserve ordered lifecycle and value relationships;
- explicit selected or simulated category-level provenance for C10-controlled substitute values;
- intentional simulated availability, invalidity, freshness, staleness, quality, degradation, and interruption conditions;
- repeatable lifecycle-driving sequences and controlled validation support for normal lifecycle and pilot-visible outcomes;
- initiation of scoped cleanup requests for one or all retained simulated Flights through the normal request and C9-owned deletion boundary.

**Consumes**

- approved scenario and validation controls;
- requested provenance and handling configuration;
- availability or activation outcomes for sources requested with pass-through handling;
- relevant C4/C5 actually active provenance and handling outcomes needed to expose the run configuration;
- deletion outcomes from C9 when cleanup is requested.

**Produces**

- requested provenance and handling configuration for C4- and C5-owned categories;
- controlled simulated or selected equivalents through the normal C4 input boundary;
- controlled simulated weather information through the normal C5 weather-input boundary;
- active simulation-run context for C3 when a Flight is created;
- requested provenance and handling configuration, run identity, scenario state, and progress for observability;
- scoped cleanup requests whose actual record selection, deletion, and result remain owned by C9.

**Does not own**

- an alternative Flight Mode or Flight lifecycle;
- simulation-only calculation rules;
- direct Flight creation, completion, rejection, or durable-record construction;
- association or reclassification of Flight-level simulation classification after C3 creates the Flight;
- acquisition, normalization, provenance classification, or actually active runtime-source and handling configuration owned by C4;
- weather-provider integration or actually active weather-source provenance, handling, and interpretation owned by C5;
- takeoff or landing confirmation, Takeoff Point or Landing Point state, alternative derived values, direct C8 state mutation, record deletion, or provenance reclassification;
- production behavior that bypasses the normal concern paths.

C10 is not an alternative detector, calculation system, recorder, lifecycle, application, or third product pillar. It does not reclassify upstream provenance based on pass-through handling, own actually active source interpretation, or proxy upstream values used with pass-through handling. Exact controls, scenario representation, operator workflow, source-selection realization, architecture, automation, implementation technology, and tooling remain deferred.

## Live-input and conceptual-substitution categories

The table defines category-level boundaries, not APIs, sensor selection, fusion, sampling, or source hierarchy. Unless stated otherwise, the conceptual substitution point is external production before C4 normalization. C10 requests pass-through or controlled-substitute handling for governed categories and controls selected or simulated substitute production. Upstream live or selected sources used with pass-through handling may reach C4 or C5 directly. C4 and C5 own the actually active AirLink-facing source, provenance, handling result, and validity treatment; downstream concerns do not receive a simulation-only path.

| Input category | External producer and AirLink boundary | Required metadata, principal consumers, and minimum degradation consequence | Minimum pass-through or controlled-substitute capability |
| --- | --- | --- | --- |
| Position | Device/platform position production enters C4; C4 normalizes position without selecting an acquisition mechanism | Source and observed time, availability, validity, freshness, quality or accuracy, and provenance; consumed by C5, C6, C7, C8, and C9; loss makes current location and dependent spatial behavior explicitly unavailable or degraded without implying a lifecycle boundary | Pass-through handling of upstream position, or coherent selected or simulated position progression as a controlled substitute; unavailable, invalid, stale, degraded, and interrupted states are controllable |
| Movement and Ground Speed source information | Device/platform movement production enters C4; C7 owns pilot-facing Ground Speed meaning | Source and observed time, validity, freshness, quality, and provenance; consumed principally by C6, C7, C8, and C9; degraded movement must not become valid current Ground Speed or a lifecycle conclusion | Pass-through handling of upstream movement, or coherent simulated movement and speed progression as a controlled substitute, including stationary and degraded conditions |
| Course or Track source information | Device/platform movement-direction production enters C4; C7 preserves semantic identity and owns pilot-facing True-North-referenced Track-related output | Source and observed time, validity, freshness, quality, reference semantics, and provenance; consumed by C6, C7, C8, and C9; unavailable direction remains distinct from zero movement and from device orientation | Pass-through handling of upstream Track/course, or simulated Track-related progression coherent with position and movement as a controlled substitute, plus controlled unavailable or degraded direction |
| Device orientation-related source information | Device/platform orientation production enters C4; C7 owns correction, derivation, semantic identity, and pilot-facing True North output | Source and observed time, validity, freshness, quality, reference semantics, and provenance; consumed by C7 and C8; invalid orientation must not be silently replaced by Track or Heading | Pass-through handling of upstream orientation, or simulated orientation progression as a controlled substitute sufficient to exercise C7/C8 behavior, including unavailable, invalid, stale, and degraded states |
| Altitude-related information | Device/platform altitude production enters C4; C7 owns the selected pilot-facing altitude representation | Source and observed time, validity, freshness, quality or accuracy, altitude-source semantics, and provenance; consumed by C6, C7, and C9; unavailable or stale altitude degrades dependent values without inventing an altitude | Pass-through handling of upstream altitude, or simulated altitude progression as a controlled substitute coherent with position, movement, pressure context, and lifecycle transitions |
| Vertical-movement-related information | Device/platform vertical-motion or altitude progression enters C4; C7 owns pilot-facing vertical speed | Source and observed time, validity, freshness, quality, and provenance; consumed by C6, C7, and C9; degraded vertical movement must not be treated as a valid zero value | Pass-through handling of upstream vertical movement, or simulated climb, level, and descent progression as a controlled substitute coherent with altitude and sufficient for detection and derivation |
| Pressure-related information | Device/platform pressure enters C4; external weather pressure or QNH enters C5; C7 consumes the approved context when required | Source and observation time, validity, freshness, quality, measurement-versus-context semantics, and provenance; unavailable pressure explicitly limits dependent altitude behavior | Pass-through handling of upstream pressure or QNH, or controlled simulated substitutes coherent with altitude and weather, including unavailable, invalid, stale, and degraded states |
| Wall-clock time | Platform clock production enters C4 and remains distinct from duration time | Source time, AirLink-observed time where different, clock adjustments, validity, and provenance; consumed where calendar time or retained timestamps matter; clock changes must not alter monotonic durations | Live provenance with pass-through handling of the platform clock, or controlled selected or simulated wall-clock progression mapped coherently to the run |
| Monotonic time | Platform monotonic clock enters C4 and supplies duration and ordering semantics | Monotonic ordering, continuity, availability, and provenance; consumed by C2, C3, C6, C7, C9, and C10; loss or discontinuity is explicit and must not become a wall-clock assumption | Pass-through handling of a compatible upstream monotonic source, or ordered simulated progression as a controlled substitute sufficient for waiting, detection, Flight time, derivation, and recording |
| Platform lifecycle and interruption signals | Platform lifecycle production enters C4 | Signal identity, AirLink-observed time, availability, and provenance; consumed by C2, C3, and C9 as required; interruption must not masquerade as landing, completion, or rejection | Pass-through handling of upstream lifecycle signals, or controlled substitute interruption and restoration signals sufficient to expose the unresolved F14 outcome without inventing it |
| Permission and source-availability state | Platform, device, network, or provider state enters C4, or C5 for weather-provider availability | Category, observed time, availability, denial or limitation, validity impact, and provenance; consumed by every affected concern; denial or loss is explicit rather than hidden fallback | Pass-through handling of upstream state, or controlled substitute granted, denied, unavailable, restored, and degraded states by relevant category |
| Current-location context | C4 owns normalized current-location context derived from position or an explicit selected point; C5 owns its use for weather scoping | Position provenance, source and observed time, validity, freshness, quality, and selection status; consumed by C5; invalid or unavailable context must not silently request unrelated-location weather | Live upstream position with pass-through handling, or a selected point or simulated current location as a controlled substitute sufficient to scope weather explicitly |
| Weather-provider information | External provider information, or C10-controlled equivalent, enters the normal C5 weather-input interpretation boundary | Provider/source provenance, observation or update time, AirLink-observed time where different, observation-versus-forecast status, validity, freshness, availability, and degradation; consumed by C1 and conditionally C7; failure must not become suitability approval | Live provider information with pass-through handling, or controlled simulated wind speed, gusts, direction, pressure or QNH, observation/update time, and applicable near-term forecast, including stale, unavailable, invalid, and degraded states |

An individual value has live, selected, or simulated category-level provenance. Where a C10-controlled run governs a category, its handling is separately pass-through when an existing upstream value is used without C10 replacement, or controlled substitute when C10 supplies or controls an approved selected or simulated equivalent; outside such a run, handling may be absent or not applicable. Pass-through preserves upstream provenance. `Mixed` is not value provenance or handling: an actually active run/configuration is mixed-source only when relevant categories use more than one of live, selected, and simulated provenance. Thus live weather plus simulated movement and live position plus selected route context are mixed-source, while all-live pass-through and all-simulated controlled-substitute runs are each single-source by provenance. C10's requested provenance and handling configuration, C4/C5's actually active provenance and handling results, and single-source or mixed-source provenance composition must be separately observable, and switching must not occur invisibly. A Flight created while the C10-controlled run is active is simulated regardless of either composition; a Flight outside such a run is non-simulated regardless of individual input provenance or handling. Exact compatibility rules and source-selection mechanics remain deferred.

## Source-independent downstream contract

Live, selected, or simulated category-level provenance, used through pass-through or controlled-substitute handling where a C10 run applies and in either single-source or mixed-source provenance composition, must preserve the normal responsibilities below. Source quality may legitimately change availability or degraded behavior, but it does not change the product meaning of a concern.

| Concern | Source-independent responsibility |
| --- | --- |
| C2 — Flight Mode Lifecycle | Uses normal pilot intent, monotonic time, allowed detection context, and lifecycle outcomes; source substitution cannot directly enter, continue, or exit Flight Mode |
| C3 — Flight Lifecycle and Flight State | Creates, completes, rejects, or interrupts a Flight only through normal C2 authorization and C6 or pilot-originated outcomes; associates C10's active simulation-run classification with Flight identity at creation without creating a special lifecycle |
| C4 — Input Acquisition and Validity | Normalizes every approved runtime source, preserves category-level provenance and time semantics, owns actually active provenance and handling for its categories, and exposes activation, validity, freshness, availability, and degradation outcomes |
| C5 — Weather Context | Owns the actually active weather-source interpretation, provenance, and handling, and preserves normal weather meaning, location scoping, observation-versus-forecast distinction, freshness, and degradation regardless of production |
| C6 — Flight Detection | Applies the normal candidate, confirmation, rejection, and expiry responsibility to normalized inputs; C10 cannot confirm takeoff or landing |
| C7 — Flight Information Derivation | Applies the same Ground Speed, altitude, vertical-speed, wind, orientation, distance, and bearing meanings and exposes dependency provenance, relevant handling context, and semantic status; no alternative simulated calculation exists |
| C8 — Spatial Awareness and Map Context | Applies the same pilot-centred map, orientation, Takeoff Point, Current Waypoint, scale, and degraded-state meaning; selected route context does not enable Active Navigation |
| C9 — Flight Recording and Local Retention | Uses the normal progressive recording, finalization, retention, retrieval, and deletion responsibilities while durably preserving C3-supplied Flight-level simulation classification without inferring it from provenance or handling composition |

## Cross-cutting observability responsibility

Observability is a responsibility of every runtime concern rather than a separate product concern.

Each concern must expose enough information to verify its authoritative state, decisions, validity, provenance, handoffs, degradation, and outcomes. For live and simulated validation, the minimum observable set is:

- C10-requested provenance and handling configuration, whether the C10-controlled simulation run is active, conceptual run or scenario identity, state, and progress;
- C4/C5 actually active source configuration and activation outcomes, live, selected, or simulated category-level provenance, pass-through or controlled-substitute handling where applicable, observable single-source or mixed-source provenance composition, and any selected starting point or route context;
- time provenance and separately applicable handling, for example live wall-clock provenance with pass-through handling during a simulated Flight, including wall-clock versus monotonic semantics;
- relevant input availability, validity, source and observed time, freshness or staleness, quality or accuracy, and intentional degradation or interruption;
- C2 Flight Mode state and transitions, including Ready on Ground, warning, continuation, automatic exit, and explicit exit outcomes;
- C6 takeoff and landing candidate, confirmation, rejection, and expiry outcomes;
- C3 Flight creation, Flight-level simulated or non-simulated classification, completion, manual completion, rejection, interruption, final boundary, and multiple-Flight lifecycle separation;
- C7 derived-value availability, semantic status, provenance dependencies, quality, and degraded state;
- C8 pilot-visible spatial result, Takeoff Point and Current Waypoint state, orientation meaning, and degraded state;
- C9 recording health, preservation of C3-supplied Flight-level classification, progressive-record result, finalization, retention, retrieval, individual deletion, bulk simulated-Flight deletion, and false-detection deletion outcomes;
- immediate Summary availability, saved-review availability, and durable simulated-versus-non-simulated Flight distinction.

Observability must not silently change product behavior, become an alternative source of product truth or lifecycle, or preserve Flight-equivalent data that accepted false-detection behavior requires to be deleted.

Detailed logging technology, telemetry format, diagnostic UI, storage, and automation remain deferred. Issue #34 defines only this minimum mandatory observable information for whole-MVP live/simulated validation.

---

# 3. Authoritative State and Information Ownership

| ID | State or information category | Authoritative owner | Required consumers or consequence |
| --- | --- | --- | --- |
| O1 | Flight Mode operational state, transition outcome, and acquisition demand | C2 | C1 presents state/outcomes; C4 fulfills acquisition demand; C6 uses allowed detection context |
| O2 | Flight identity, runtime lifecycle state, and Flight-level simulated or non-simulated classification associated from active C10 run context at creation | C3 | C1 presents the classification; C2, C7, and C8 consume it where required; C9 preserves it durably without inference or reclassification |
| O3 | Effective takeoff, confirmed-landing, and manual-completion boundaries | C3 | C1 and C9 consume final boundaries; C7 uses the active interval; confirmed landing includes the full final segment through confirmation |
| O4 | Elapsed Flight time and Flight-scoped aggregates | C3 | C1 presents them; C9 retains approved final values |
| O5 | Takeoff Point identity and estimated location | C3 | C7 calculates relative values; C8 uses it as Current Waypoint and presents it; C9 consumes and retains it without reconstruction |
| O6 | Normalized runtime inputs; source and observed time; wall-clock and monotonic semantics; availability, validity, freshness, quality, live/selected/simulated category-level provenance, and actually active handling and configuration for C4-owned categories | C4 | C5 consumes current-location context; runtime consumers use required inputs without reclassifying or silently switching provenance or handling; C10 consumes actual provenance, handling, and activation outcomes to expose requested-versus-active configuration |
| O7 | Current-location weather acquisition demand, actually active weather-source interpretation, weather observation, forecast, pressure or QNH context, live/selected/simulated category-level provenance, applicable handling, and degradation | C5 | C4 fulfills the location demand; C1 presents weather; C7 conditionally consumes pressure or QNH; C10 consumes actual provenance, handling, and activation outcomes; weather retains the same product meaning |
| O8 | Takeoff and landing detection candidate, confirmation, and accepted detector constraint state | C6 | C2 decides whether confirmed boundaries may affect lifecycle; permanent speed-only takeoff detection is prohibited |
| O9 | Current derived Flight, True-North-referenced orientation, direction, distance, and bearing values, including independently preserved source-dependency provenance, relevant handling context, and semantic status | C7 | C1 presents current Flight values; C8 presents C7-provided orientation and relative-navigation values without recalculating them; C3 receives aggregate updates; C9 retains approved historical values without silent replacement, provenance or handling loss, or collapse into Flight-level classification |
| O10 | Active and saved spatial representation, including pilot-controlled scale and Flight compass ring or scale | C8 | C1 supplies scale-adjustment actions and presents the resulting map, compass, orientation, and awareness context |
| O11 | Progressive and durable Flight record, recording status, saved representation, historical-value preservation, durable simulated-versus-non-simulated distinction, and record deletion outcome | C9 | C1 and C8 consume saved and retention results; C9 preserves C3-supplied classification and cannot infer or redefine it; later interpretations must not silently replace original retained values; C1/C10 may initiate scoped deletion requests but cannot delete or reclassify records |
| O12 | Authoritative C10 simulation-run active state, requested provenance and pass-through/controlled-substitute handling configuration, controlled selected or simulated substitute production, simulated-time control, scenario state, and progress | C10 | C3 consumes active run context when creating a Flight; C4/C5 receive requested configuration and controlled equivalents while owning actual active provenance, handling, and source state; C10 consumes their activation and failure outcomes for observability; C2–C9 retain normal responsibility ownership |
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
| F1 | Current conditions | C1 requests current conditions from C5; C5 expresses bounded current-location acquisition demand to C4; C4 fulfills that demand and supplies normalized current-location context, including availability, validity, freshness, provenance, and applicable handling, to C5; C5 uses that context to scope weather interpretation and receives either live-provider weather with live provenance and pass-through handling or a C10-controlled simulated substitute with simulated provenance and controlled-substitute handling through the same weather-input boundary; C5 supplies observed weather, forecast when available, observation-versus-forecast distinction, freshness, validity, category-level provenance, applicable handling, and degraded state to C1 and exposes the actually active provenance and handling outcome to C10 when relevant | The pilot can understand relevant conditions for the current location before Pre-Flight through the same C5 semantics for live or simulated weather; unavailable or invalid location or weather produces an explicit limitation rather than silently using an unrelated location or source | Provider, request mechanism, concurrent-demand coordination, refresh/cache policy, forecast intervals, exact representation, fallback details |
| F2 | Minimal Pre-Flight and Flight Mode entry | C1 presents accepted acknowledgements and sends the completed acknowledgements plus explicit entry request to C2; C2 returns the resulting state to C1, issues Flight Mode acquisition demand to C4, and enables the allowed detection context for C6; C4 fulfills the demand through its acquisition boundary | Flight Mode becomes active only after explicit pilot intent; Ready on Ground waiting begins; required Flight Mode inputs become available through C4 | Exact controls, layout, platform activation, acquisition profiles, and resource-management mechanism |
| F3 | Ready on Ground waiting, warning, continuation, and automatic exit | C4 supplies monotonic time to C2; C2 supplies waiting/warning/exit state to C1; C1 may request continuation; C2 resets the waiting period or exits when required; on exit C2 disables the allowed detection context for C6, withdraws Flight Mode acquisition demand from C4, and C4 reduces or stops Flight Mode-specific acquisition while preserving other active input demands | The warning is presented, continuation is possible, and Flight Mode eventually exits if the pilot does not continue; after automatic exit no further takeoff detection may affect lifecycle until Flight Mode is entered again; resource use follows Flight Mode demand without C2 performing acquisition directly | Timeout and warning duration, presentation, exact continuation control, other reset conditions (`P5`), acquisition profiles and platform mechanism |
| F4 | Confirmed takeoff and Flight creation | C4 supplies valid runtime inputs to C6; C6 confirms takeoff under the accepted non-speed-only permanent detector constraint and supplies its estimated boundary to C2; C2 validates context and authorizes C3; C10 supplies whether its controlled simulation run is active; C3 creates the Flight and Takeoff Point, associates simulated classification when the C10 run is active or non-simulated classification otherwise, and supplies active-Flight/classification/Takeoff-Point context to C2/C1/C7/C8/C9; C8 makes Takeoff Point the Current Waypoint while keeping Active Navigation off | One Flight begins inside active Flight Mode; every Flight created during an active C10-controlled simulation run is simulated regardless of provenance or handling composition, while a Flight outside such a run remains non-simulated and is not reclassified from individual source provenance or handling; its effective start and Takeoff Point represent the accepted actual-takeoff estimate; permanent takeoff detection is not based only on speed; Takeoff Point becomes the passive navigation context; recording starts with authoritative classification and Takeoff Point available to C9 | Detector design, signal combination, confirmation semantics (`P2`), recent-history custody, retrospective estimation, thresholds, filters, and implementation of the C10-to-C3 context handoff |
| F5 | Active Flight information, elapsed time, aggregates, and progressive recording | C4 supplies inputs to C7 and time to C3; C5 supplies pressure or QNH to C7 only when required; C7 supplies current values to C1 and aggregate updates to C3; C3 supplies elapsed time and Flight context to C1; C4/C7/C3 supply approved retained information and historical semantics to C9 | Ground Speed, altitude, vertical speed, Flight time, and estimated wind are available with correct semantic status; Flight aggregates advance; progressive recording preserves the approved values and context required to represent what was available or used during the original Flight | Algorithms, validity rules, update rates, altitude/QNH model, exact retained parameter set, provenance representation, sampling and persistence mechanics |
| F6 | Takeoff Point awareness and spatial orientation | C4 supplies normalized position and other required spatial/source inputs to C8 and supplies movement/orientation source inputs to C7; C3 supplies Takeoff Point identity/location to C7 and C8; C8 retains it as Current Waypoint with Active Navigation off; C7 supplies True-North-referenced pilot-facing orientation values or candidates, their semantic/validity state, and Takeoff Point distance/bearing to C8; C1 supplies pilot map-scale adjustment actions to C8; C8 applies those actions and the later-approved orientation policy and supplies the pilot-centred map, compass ring or scale, and passive awareness presentation to C1 | The Takeoff Point remains the current passive navigation context and stays distinct and visible throughout the Flight; distance and bearing/direction use True North; map orientation uses C7-provided pilot-facing values without C8 recalculation; Track, Heading, bearing, and device orientation remain distinct; a compass ring or scale is present around the pilot; the pilot can change map scale during Flight; passive awareness does not become Active Navigation | Selection and switching among C7-provided orientation candidates, declination source/model, correction and derivation algorithms, update rate, source validity, fallback behavior, quality semantics, exact scale controls, zoom range/steps, gesture or button behavior, compass visual design, animation, recenter interaction and final orientation decision (`P6`) |
| F7 | Confirmed landing and runtime completion | C4 supplies inputs to C6; C6 confirms landing and supplies the boundary to C2; C2 validates context and authorizes C3; C3 completes/finalizes the Flight through the confirmed boundary without trimming the final segment, establishes the confirmed Landing Point, supplies final boundaries/aggregates to C1/C9, supplies Landing Point identity/location/classification to C9, and reports no active Flight to C2 | The individual Flight ends with all approved information through landing confirmation retained; the final segment is not retrospectively trimmed; the completed Flight has a distinct confirmed Landing Point; C2 immediately returns to Ready on Ground and waiting for another takeoff resumes | Landing detector, confirmation semantics (`P2`), exact Landing Point determination and storage representation |
| F8 | Immediate retained completed-Flight Summary | After either confirmed landing through F7 or manual retention through F10, C3 supplies final Flight values, Flight-level simulation classification, and the authoritative completion type to C1/C9; C9 supplies recording completeness and retention status; C1 presents their shared core summary information and simulated-versus-non-simulated distinction while C2 remains Ready on Ground; the Summary includes an explicit Flight Mode exit action that, when selected, sends the exit request from C1 to C2 through F12 | A Summary appears inside the Flight flow for every retained completed Flight, preserves its Flight-level simulation classification, identifies whether completion followed confirmed landing or manual completion, does not imply confirmed landing for a manually completed Flight, provides an explicit action within the Summary to end Flight Mode, and does not block waiting for another takeoff when that action is not selected | Exact Summary fields beyond accepted minimum, information hierarchy, layout, retention timing and error presentation, exact exit control, label, placement, confirmation behavior and visual design |
| F9 | Another Flight in the same Flight Mode period | After F7/F8 or F10/F8, C2 remains Ready on Ground, Flight Mode acquisition demand remains active, and C6 remains allowed to detect takeoff; a new F4 starts a new independent Flight; C1 closes the prior Summary when the new Flight begins | Multiple independent Flights may occur in one Flight Mode period without a Flight Session record | Exact presentation transition |
| F10 | Manual completion — retain Flight | C1 sends the manual-completion request and retain choice to C2; C2 validates context and authorizes C3; C3 completes the Flight with an explicit manual boundary, preserves its simulated or non-simulated Flight-level classification, marks the completion type as manual, and supplies final results to C1/C9; C9 preserves the supplied classification and reports recording completeness and retention status; C2 returns to Ready on Ground; the flow continues through the shared F8 Summary path | The episode is retained as a Flight, with its Flight-level simulation classification and category-level input provenance preserved; an immediate Summary is available and identifies manual completion; no confirmed landing or confirmed Landing Point is implied; Flight Mode remains active and Ready on Ground | Exact interaction, confirmation behavior, retention timing and error presentation, and Landing Point semantics (`P4`) |
| F11 | Manual completion — discard false detection | C1 sends the discard choice to C2; C2 authorizes rejection by C3; C3 reports the rejected outcome to C2/C9; C9 deletes the progressively recorded episode; C2 returns to Ready on Ground | No Flight and no hidden durable Flight-equivalent record remain; bounded observability may record only that rejection and deletion occurred | Technical deletion and cleanup mechanism; cleanup-failure recovery |
| F12 | Explicit Flight Mode exit while no Flight is active | C1 sends an exit request to C2; C2 exits, disables the allowed detection context, withdraws Flight Mode acquisition demand from C4, and returns the resulting state to C1; C4 reduces or stops Flight Mode-specific acquisition while preserving any other active input demand | Flight Mode ends separately from any individual Flight; C2 owns the operational decision and C4 owns the acquisition/resource mechanism | Exact control, presentation, acquisition profiles, and platform mechanism |
| F13 | Explicit Flight Mode exit while a Flight is active | No product flow is accepted. C1 may originate the request, but C2 must not invent refusal, forced completion, discard, or another transition | The unresolved state is exposed rather than silently implemented | Explicit owner product decision `P1` is required before implementation reaches this scenario |
| F14 | Platform interruption during an active Flight | C4 exposes the interruption or restoration signal to C3 and C9; the affected concerns expose their state and outcome to C1/C2 as required | Interruption is observable and does not silently masquerade as confirmed landing or deliberate completion | Product classification and retained outcome (`P3`), recovery guarantees, checkpointing, restoration, and storage mechanics |
| F15 | Saved-Flight access and review | C1 requests a retained Flight from C9; C9 supplies accepted summary fields, retained status, retained C3-supplied simulated-versus-non-simulated classification, retained special-point information, independently preserved live/selected/simulated category-level provenance, separately relevant historical handling context, historically preserved information, and recorded track; C8 presents the saved track and scale control; C1 presents the saved summary, Flight-level classification, source distinctions, and record status | A retained simulated or non-simulated Flight can later be opened and understood through its map track and principal summary information without losing special-point identity, Flight-level classification, category-level provenance, relevant handling context, or values that were available or used during the original Flight | Navigation to history, detailed layout, editing, replay, analytics, schema, migration and retrieval implementation |
| F16 | Simulation-driven validation | C10 owns active run state, requests pass-through or controlled-substitute handling per governed category, and supplies controlled selected or simulated substitutes; upstream live or selected sources used with pass-through handling may reach C4/C5 directly; C4/C5 own and expose actually active source state, live/selected/simulated category-level provenance, handling, and validity; mixed-source composition is determined from the actually active provenance across categories; when C10's run is active, C3 classifies every Flight it creates as simulated and supplies that classification through normal C2–C9 paths; C9 preserves it without inference | Current conditions, Pre-Flight, Flight Mode entry, Ready on Ground warning/continuation/automatic exit, explicit exit with no active Flight, takeoff candidate and confirmation, active movement, Ground Speed, altitude, vertical speed, orientation, estimated-wind input relationships and observable output, Takeoff Point awareness, landing candidate and confirmation, manual completion, false-detection discard, progressive recording, Summary, retained result, saved review, and multiple Flights can be exercised; a Flight remains simulated even when most or all sources have live or selected provenance and use pass-through handling; invalid, stale, unavailable, degraded, and interrupted states remain observable without a physical Flight; active-Flight exit remains unresolved under P1 | Controls, scenario representation, exact parameters, source-selection mechanism, architecture, automation, detailed catalogue, and first-slice-specific design |
| F17 | Retained simulated-Flight deletion | C1 or C10 initiates a request through the normal deletion boundary for one retained simulated Flight or all retained simulated Flights; C9 validates the preserved Flight-level classification, owns deletion, and returns the outcome; C1 presents the result where pilot-facing interaction applies | The requested simulated record or simulated category is removed without deleting non-simulated Flights; failure is explicit; no other concern directly mutates C9-owned records | Exact controls, authorization, confirmation, presentation, filtering, grouping, physical-store layout, cleanup policy, and deletion implementation |

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

Measured, declared, recorded, estimated, and derived information remain distinguishable where their meaning matters. An individual value retains live, selected, or simulated category-level provenance. Where a C10 run governs a category, pass-through or controlled-substitute handling is a separate dimension, and pass-through preserves upstream provenance. Mixed-source describes actually active run/configuration composition across live, selected, and simulated provenance rather than value provenance or handling. Source and observed time, wall-clock and monotonic semantics, validity, freshness, availability, quality, and degradation must not be inferred from hidden implementation details. Provenance and handling switching is explicit, and multiple provenance dependencies and relevant handling context are independently preserved during derivation and retention wherever they affect meaning. Flight-level simulated or non-simulated classification remains separate and follows the C10-run-to-C3 association rule.

## R5 — Takeoff Point is the passive Current Waypoint after takeoff

After takeoff in the MVP free-flight scenario, Takeoff Point becomes Current Waypoint and remains available after it is reached, crossed, or revisited. Active Navigation remains off. Distance and bearing/direction awareness do not enable route guidance or Active Navigation.

## R6 — False-detection discard is destructive at the Flight-record level

A pilot-rejected false-detection episode produces no Flight and no hidden durable Flight-equivalent record. C9 is responsible for deleting any progressively recorded episode data. The technical deletion mechanism remains deferred.

## R7 — Summary continuity does not prescribe one technical object

The immediate Summary and later saved-Flight review use the same core Flight information and preserve the same semantics. This rule does not require a shared DTO, read model, database object, service, or other technical realization.

## R8 — Local-first degradation preserves lifecycle meaning

Map, weather, network, or storage degradation may reduce available information or retention quality, but it must not silently redefine whether Flight Mode is active, whether a Flight exists, or whether landing was confirmed.

## R9 — Simulation reuses normal product responsibilities

Simulation substitutes or controls approved input production and validation control. C10 owns requested provenance and pass-through/controlled-substitute handling configuration and supplies controlled selected or simulated substitutes without proxying upstream inputs used with pass-through handling; C4 and C5 own actually active provenance, handling, normalization, and interpretation within their boundaries. C2–C9 retain their normal lifecycle, detection, derivation, spatial, recording, retention, deletion, and presentation meaning. Deliberately invalid or degraded input may produce different availability or degraded outcomes, but it does not create simulation-only product semantics.

## R10 — Pilot-facing navigation directions use True North

All pilot-facing navigation directions are referenced to True North. A magnetic source may support ground device orientation, but it must be corrected before pilot-facing use. Track, Heading, bearing, and device orientation remain semantically distinct and must not be presented as an unlabeled generic direction.

The declination source or model, correction algorithm, update rate, validity rules, fallback behavior, and display formatting remain deferred.

## R11 — Historical Flight information is not silently rewritten

Replay-supporting information retained for a Flight preserves the values, semantic status, validity, live/selected/simulated provenance, separately relevant handling context, and calculation context required to represent what was available or used during the original Flight.

Later algorithm or interpretation changes may produce a distinct later interpretation, but they must not silently replace the retained historical values.

The exact retained parameter set, sampling rules, provenance representation, representation of pass-through or controlled-substitute handling where historically required, calculation-version context, storage format, migration mechanism, and later-interpretation representation remain deferred.

## R12 — Confirmed landing preserves the final Flight segment

A normally completed Flight includes all approved information retained through landing confirmation. Confirmed landing finalization must not retrospectively trim the final segment in order to approximate an earlier landing boundary.

The detector, confirmation rule, Landing Point determination method, and technical finalization mechanism remain deferred.

## R13 — Permanent takeoff detection is not speed-only

Takeoff detection may use speed as one signal, but the permanent detector must not rely on speed as its only basis. The exact signal set, algorithm, thresholds, filters, and confirmation behavior remain deferred.

## R14 — Unused pre-takeoff history is transient

A bounded recent history may support retrospective takeoff-boundary estimation. Measurements not incorporated into a confirmed Flight are overwritten and are not retained as Flight data or as a hidden Flight-equivalent record.

The buffer duration, custody, data categories, and implementation mechanism remain deferred.

## R15 — Flight-level simulation classification survives normal retention

A Flight created while a C10-controlled simulation run is active is classified as simulated regardless of the run's provenance or handling composition. C3 associates that classification with Flight identity at creation; the Flight then follows the normal recording, completion, Summary, retention, retrieval, and saved-review paths while remaining durably distinguishable from a non-simulated Flight. C9 preserves but never infers or redefines the classification from provenance or handling. C9 owns deletion of individual or all simulated Flights; a category-wide simulated-Flight deletion must not delete non-simulated Flights. Live/selected/simulated category-level provenance and separately relevant handling context remain independently preserved.

## R16 — Minimum simulation fidelity preserves product meaning

MVP 0.1 requires coherent semantic and behavioral relationships sufficient to exercise accepted flows, derivation, spatial meaning, degradation, and retained outcomes. It does not require physically exact flight dynamics, aerodynamics, sensor physics, statistical noise, latency, sampling, or numerical reproduction of a physical Flight.

---

# 7. Open Decisions and Explicit Deferrals

## 7.1 Product and domain decisions requiring explicit owner resolution

| ID | Unresolved decision | Why it remains open | Required before |
| --- | --- | --- | --- |
| P1 | Behavior of an explicit Flight Mode exit request while a Flight is active | The Flight Mode WIP explicitly leaves the required pilot and lifecycle behavior open | Any implementation slice exposes or must handle this action |
| P2 | Exact semantic distinction and transition rule between detected and confirmed takeoff or landing | Current WIP requires automatic lifecycle support but defers the exact boundary semantics | Detector and lifecycle implementation-ready planning |
| P3 | Product classification and retained outcome for a Flight interrupted without confirmed landing or deliberate manual completion | Current sources do not accept whether the episode is retained, deleted, recoverable, incomplete, or represented another way | Interruption recovery, restoration, or persistence implementation |
| P4 | Landing Point existence and classification for a manually retained Flight | A manual boundary must not falsely imply confirmed landing | Persisting or presenting a Landing Point for manual completion |
| P5 | Conditions other than pilot continuation that reset the Ready on Ground inactivity period | Flight Mode WIP leaves these conditions open | Detailed inactivity behavior for a selected slice |
| P6 | Final primary in-flight orientation behavior | Track-up versus estimated Heading-up remains deferred for experimentation and pilot feedback | Final Flight spatial behavior is selected for implementation |

These decisions are not implementation-agent choices. When a selected slice reaches one of them, the affected work must stop for the governing owner decision.

## 7.2 Engineering decisions intentionally deferred

| ID | Decision group | Includes | Governing later work |
| --- | --- | --- | --- |
| D1 | Platform and input contracts | Android APIs, permissions, execution behavior, concern-level acquisition-demand coordination, resource profiles, sampling, timestamps, freshness, validity, source hierarchy, sensor fusion | Selected-slice planning and bounded technical decisions |
| D2 | Flight detection and boundary-derived points | Detector design within the accepted non-speed-only constraint, signal combination, thresholds, filters, confirmation windows, recent-history duration and custody within the transient-history constraint, retrospective takeoff-boundary estimation, confirmed Landing Point determination, false-positive and false-negative recovery | Selected-slice planning after required product decisions |
| D3 | Derived information | Altitude/QNH model, vertical speed, estimated wind within the accepted non-gust scope, direction values, quality/stability semantics, precision, smoothing, update rates | Selected-slice and parameter-contract work |
| D4 | Orientation and map behavior | Selection and switching among C7-provided orientation values or candidates, declination source/model, magnetic-to-True correction and Heading-derivation algorithms, update rate, source validity, fallback behavior, display formatting and labels, exact compass-ring design, scale-control mechanism, zoom range/steps, gesture/button behavior, animation, recenter interaction, final orientation policy and implementation | Selected-slice planning and pilot validation |
| D5 | Recording and replay-supporting data | Exact retained parameter set, sampling intervals, live/selected/simulated provenance and validity representation, separate representation and historical preservation of pass-through/controlled-substitute handling where required, calculation-version context, preservation mechanism, active-Flight recording buffering, checkpointing, recovery, storage capacity, retention policy, special-point storage representation, schema, migration and format | Selected-slice persistence planning and later logging work |
| D6 | Summary and presentation | Exact Summary fields beyond accepted minimum, information hierarchy, formatting, units, controls, warning presentation, non-flight placement and presentation of estimated-wind limitations, manual-versus-confirmed completion formatting, exact Summary exit-action control, label, placement and confirmation behavior, saved-review layout | Selected-slice UX and product planning |
| D7 | Simulation and observability realization | Framework architecture, substitution implementation, source hierarchy and switching, pass-through/controlled-substitute handling representation, mixed-source provenance-composition mechanism, scenario and selected-route representation, control surface, operator workflow, exact simulated parameters, physical and sensor realism, sample rates, timing implementation, deterministic versus stochastic behavior, noise and error models, latency, acceleration, pause, step, rewind or replay controls, automation integration, diagnostics presentation, logging or telemetry technology, test framework, scenario or run persistence, storage separation by Flight-level classification, history filtering and visual marking, retention and cleanup policy or implementation, and first-slice-specific simulation design | Selected-slice planning and the bounded later work that requires each decision |
| D8 | Dependencies, risks, decisions, and implementation sequence | Concern dependency order, external constraints, risk-reduction order, difficult-to-reverse decision classification and timing, future slice sequence | Issue #35 |

Deferral means that the decision must be made deliberately in the bounded work that requires it. Deferral does not authorize an implementation agent to select product semantics or difficult-to-reverse architecture silently.

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
- **Explicit simplification:** MVP 0.1 is mapped only at concern, authoritative-ownership, mandatory-flow, input-category, semantic-fidelity, observability, external-dependency-category, and deferral level. Simulation preserves product meaning without requiring a physically exact flight or sensor model.
- **Approval authority:** the planning depth and simplification are authorized by `ITERATION.md`, issues #33 and #34, the owner-reviewed MVP 0.1 planning boundary, and the five owner-approved issue #34 constraints. Acceptance of the issue #34 extension remains subject to owner review.
- **Boundedness:** the map applies only to MVP 0.1 engineering planning under AL-0002.
- **Reversibility:** source substitution changes input production and may change explicit category-level provenance, while pass-through/controlled-substitute handling remains separately visible and C10-run-to-C3 Flight classification and downstream C2–C9 responsibilities remain concern-level; no final components, APIs, schemas, providers, algorithms, storage engines, simulation controls, or complete architecture are selected.
- **Intentionally deferred:** physical and sensor realism beyond product meaning, detailed scenario and source-selection realization, first-slice simulator design, wider Flight Support domains, Pilot Ecosystem, connected operation, cloud, web, iOS, later lifecycle capabilities, and future architecture.
- **Product behavior and authority:** no new behavior is inferred beyond the owner-approved issue constraints, and this WIP planning artifact remains non-canonical and non-authoritative for implementation.
- **Outcome:** `Aligned with explicit simplification`.

Any new product-semantic simplification, irreversible constraint, or expansion beyond these bounds requires a separate owner decision.

---

# 10. Review Contract

Review this map at concern and mandatory-flow level.

A valid review should verify that:

1. every accepted MVP 0.1 flow is represented in section 4;
2. every important state or information category has one authoritative owner in section 3;
3. every mandatory flow has a complete trigger–demand–producer–owner–consumer path at concern level where external acquisition is required;
4. no concern silently assumes authority owned by another concern;
5. every relevant unresolved product question is explicitly recorded in section 7.1;
6. accepted semantic product rules are not reclassified as deferred engineering choices;
7. deferred implementation mechanics remain deferred;
8. no final architecture, API, schema, provider, algorithm, or complete internal message graph is implied;
9. every required live-input category has one explicit AirLink normalization or interpretation boundary and a minimum equivalent;
10. each provenance enumeration is limited to live, selected, and simulated; pass-through always describes handling, preserves upstream provenance, and remains distinct from controlled-substitute handling; mixed-source composition is determined only from actually active provenance and remains independent of handling;
11. C10 owns active simulation-run state, C3 associates Flight-level simulation classification at creation, C9 preserves it without inference, C1 presents it, and C2–C9 otherwise retain normal product responsibility;
12. semantic fidelity is sufficient for the full set of required MVP 0.1 validation outcomes without implying physical fidelity;
13. weather and pressure simulation, invalidity, staleness, unavailability, degradation, and interruption are represented;
14. retained simulated Flights remain distinguishable from non-simulated Flights through Summary and saved review and can be deleted individually or as a category without deleting non-simulated Flights;
15. mandatory observability distinguishes C10-requested provenance and handling, C4/C5 actually active provenance and handling, single-source or mixed-source provenance composition, Flight-level classification, inputs, lifecycle, detection, derivation, spatial results, recording, retention, deletion, Summary, and saved review;
16. issue #35 dependency, risk, decision-order, and future-slice work has not begun.

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
- engineering deferrals D1–D8 remain assigned to the appropriate later bounded work;
- the document remains WIP, non-canonical, and non-authoritative for implementation;
- no implementation or final architecture has been introduced.

---

# 12. Issue #34 Acceptance Check

Issue #34 content is ready for owner review when the owner confirms that:

- the existing MVP 0.1 Scope and issue #33 baseline are sufficient and no blocking contradiction or additional owner decision remains;
- the five owner-approved constraints in section 1.1 are represented without reinterpretation;
- the live-input table covers position, movement, Track/course, orientation, altitude, vertical movement, pressure, wall-clock and monotonic time, platform lifecycle, permission and availability state, current location, and weather;
- every required equivalent enters the normal C4 or C5 boundary, C10-requested provenance and handling remain distinct from C4/C5 actually active provenance and handling, and upstream values used with pass-through handling need not be proxied through C10;
- each value retains live, selected, or simulated category-level provenance; pass-through always describes handling and preserves upstream provenance rather than reclassifying it;
- mixed-source remains a run/configuration property determined only from actually active live, selected, or simulated provenance, while handling composition and multiple derivation dependencies remain independent;
- minimum semantic fidelity covers accepted lifecycle, detection, derivation, spatial, recording, Summary, retained review, repeated-Flight, and degraded-input outcomes;
- C10 remains Simulation and Validation Enablement and does not absorb C2–C9 responsibilities;
- every Flight created during an active C10-controlled simulation run is classified as simulated regardless of provenance or handling composition through an explicit C10 → C3 → C9 → C1 path, while a Flight outside such a run remains non-simulated and is not reclassified from individual input provenance or handling;
- C9 preserves C3-supplied Flight-level classification without deriving, inferring, or redefining it from provenance or handling;
- manual completion retains the episode as either a simulated or non-simulated Flight without changing the accepted retain-versus-false-detection-discard semantics;
- simulated Flights use normal C9 recording and retention, remain durably distinguishable from non-simulated Flights, and support scoped individual and bulk deletion without deleting non-simulated Flights;
- mandatory observability is sufficient to inspect every required source, state, decision, outcome, and retained distinction without becoming product truth or retaining a rejected Flight-equivalent record;
- D7 explicitly defers implementation mechanics and first-slice-specific simulation design;
- Product Direction alignment remains `Aligned with explicit simplification`;
- no implementation, architecture, provider, schema, format, algorithm, complete test strategy, or issue #35 work has been introduced.

Owner acceptance of this section approves the issue #34 extension as planning input. It does not approve the consolidated Engineering Map, promote this WIP artifact, authorize implementation, or activate AL-0003.

---

# 13. Reserved Extension Point

## 13.1 Issue #35 — Dependencies, risks, decisions, and future slices

Issue #35 will extend and consolidate this map with the approved:

- concern-level dependency order;
- external constraints affecting implementation order;
- major engineering risks and risk-reduction order;
- difficult-to-reverse decision classification and timing;
- consolidated deferred-decision register;
- high-level candidate sequence of future implementation iterations;
- findings that constrain first-slice candidate selection.

This extension must not convert the map into a complete backlog, final architecture, or detailed plan for every future slice.
