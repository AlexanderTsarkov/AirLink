# MVP 0.1 Engineering Map

## Status

This document is a **WIP engineering-planning artifact** for `AL-0002: MVP 0.1 Engineering Planning`.

It is:

- non-canonical;
- not a product specification;
- not a final component architecture;
- not implementation authority;
- developed incrementally through bounded AL-0002 issues;
- subject to explicit owner review and approval.

Current section ownership:

- engineering boundary, major concerns, responsibility boundaries, state and information ownership, conceptual handoffs, external dependencies, exclusions, and initial deferred decisions — prepared under GitHub issue `#33 / AL-0002-01`;
- live-input and simulation-substitution boundaries — reserved for `#34 / AL-0002-02`;
- dependency order, risk order, decision order, deferred-decision consolidation, and candidate implementation sequence — reserved for `#35 / AL-0002-03`.

Approval of the sections prepared under issue #33 does not constitute approval of the complete Engineering Map. Final owner approval of the full Engineering Map occurs only after the bounded work of issues #34 and #35 is complete.

## Purpose

This document describes the minimum engineering structure required to treat MVP 0.1 as one coherent system while avoiding premature design of its final architecture.

At the current planning depth, it defines:

- the AirLink engineering boundary for MVP 0.1;
- the major engineering concerns;
- the responsibility and non-ownership boundaries between those concerns;
- authoritative ownership of important runtime state and information;
- the principal conceptual handoffs;
- categories of external dependencies;
- degradation expectations that affect system responsibility;
- explicit exclusions;
- decisions intentionally deferred to later AL-0002 work or implementation iterations.

The concerns in this document are not assumed to become modules, services, packages, layers, repositories, classes, processes, or deployment units.

## Governing Context

This document must be interpreted through the repository source-of-truth order:

1. the explicit owner task or decision;
2. `ITERATION.md`;
3. `docs/product/CurrentState.md` and relevant canonical documentation under `docs/`;
4. the relevant GitHub issue and approved task artifacts;
5. WIP documents only when the task explicitly concerns them or their use is otherwise authorized by the governing task;
6. legacy material only as source material, never as current truth by default.

Product Direction and Product Governance are conditionally consulted alignment context. They apply only when the Product-Significance Routing rules in `AGENTS.md` require them; consultation does not make them task authority or implementation requirements.

If these sources conflict, work must stop and the conflict must be reported rather than silently resolved.

The Product Vision remains canonical.

The MVP 0.1 Scope remains an owner-reviewed, non-canonical WIP product boundary. It does not become canonical or gain implementation authority through this document.

Flight Mode, Flight, Navigation, and other WIP documents remain supporting inputs rather than automatic product truth.

## Planning-Sufficiency Review of the MVP 0.1 Scope

### Assessment

The existing MVP 0.1 Scope is sufficient as the WIP product-level baseline for the engineering-boundary and responsibility work required by issue #33.

No contradiction or omission has been identified that currently blocks:

- definition of the MVP 0.1 engineering boundary;
- identification of major engineering concerns;
- separation of Flight Mode and Flight responsibilities;
- state and information ownership;
- conceptual handoffs;
- external-dependency classification;
- later planning of live/simulated substitution under issue #34.

### Corrections required

No product-scope correction is required as part of issue #33.

The remaining open matters identified during this work are engineering-planning questions, not unresolved product contradictions.

### Authority consequence

The MVP 0.1 Scope remains:

- sufficient for AL-0002 planning;
- non-canonical;
- not a detailed specification;
- not implementation authority;
- unchanged by this planning-sufficiency assessment.

---

# 1. MVP 0.1 Engineering Boundary

## 1.1 Boundary definition

The MVP 0.1 engineering boundary includes all AirLink-controlled:

- product behavior;
- operational and Flight lifecycle state;
- interpretation and use of input information;
- calculated and derived Flight information;
- pilot-facing presentation;
- local recording and retention;
- saved-Flight access and review;
- handling of validity, freshness, provenance, availability, and degradation;
- simulation-controlled substitution required to develop and validate the accepted product flow;
- runtime observability required to verify that behavior.

The boundary covers the complete accepted preparation–Flight–completion–review outcome, not only the in-flight display.

## 1.2 External systems

The following remain outside the AirLink engineering boundary:

- Android operating-system internals;
- device hardware and sensor internals;
- GNSS infrastructure;
- specific weather providers;
- specific map providers;
- network infrastructure;
- concrete database, filesystem, or storage implementations;
- development, CI, and deployment infrastructure;
- external tooling not incorporated into the AirLink-controlled simulation capability.

AirLink does not own the internal operation of those systems.

AirLink does own:

- the integration boundary;
- interpretation of returned information;
- source and provenance semantics;
- validity and freshness handling;
- degradation consequences;
- fallback behavior at product level;
- presentation of unavailable, stale, invalid, estimated, or incomplete information;
- preservation of accepted lifecycle and recording behavior when an external capability is degraded.

## 1.3 Simulation boundary principle

The Flight Simulation capability is inside the AirLink engineering boundary.

It must develop vertically with product behavior rather than as a separate universal simulator built in advance.

Issue #33 establishes only its engineering responsibility and non-ownership boundaries.

Its concrete:

- architecture;
- scenario representation;
- virtual-time mechanism;
- control surface;
- implementation technology;
- automation strategy;
- data-generation mechanism

remain deferred until issue #34 and the implementation-ready planning of the selected vertical slice.

---

# 2. Major Engineering Concerns

The following ten concerns define the minimum useful responsibility map for MVP 0.1.

## 2.1 Pilot Interaction and Operational Flow

Owns:

- normal application flow;
- access to current conditions;
- minimal explicit Pre-Flight interaction;
- pilot acknowledgements;
- explicit request to enter Flight Mode;
- explicit request to exit Flight Mode;
- pilot response to inactivity warnings;
- pilot-facing presentation of current operational state;
- post-landing interaction;
- access to saved Flights;
- presentation of degraded or unavailable capability states.

Does not own:

- Flight Mode state transitions;
- Flight creation or finalization;
- takeoff or landing detection;
- calculation of Flight information;
- persistence of Flight records;
- raw external-input acquisition.

## 2.2 Flight Mode Lifecycle

Owns the operational context in which one or more Flights may occur.

Owns:

- inactive versus active Flight Mode;
- explicit entry into Flight Mode;
- waiting before the first Flight;
- authorization for automatic takeoff and landing detection to affect product lifecycle;
- awareness that an active Flight exists or does not exist;
- waiting after a completed Flight;
- readiness for another Flight;
- inactivity warning before automatic exit;
- continuation of the waiting period;
- explicit exit;
- automatic exit;
- prevention of new Flight creation outside an allowed Flight Mode state.

Does not own:

- the lifecycle or internal state of an individual Flight;
- detection algorithms;
- Flight aggregates;
- durable Flight recording;
- Takeoff Point presentation.

## 2.3 Flight Lifecycle and Flight State

Owns one airborne episode.

Owns:

- creation of a new Flight after authorization by Flight Mode;
- Flight identity;
- active versus finalized Flight state;
- effective takeoff boundary;
- effective landing boundary;
- association of state and information with one specific Flight;
- Flight-scoped aggregates;
- Takeoff Point identity and its association with the Flight;
- finalization of one Flight;
- transfer of a finalized or interrupted Flight result to retention.

Does not own:

- the wider Flight Mode period;
- takeoff or landing detection decisions;
- raw input production;
- map rendering;
- durable storage mechanisms;
- presentation of saved Flights.

## 2.4 Input Acquisition and Validity

Owns the AirLink-facing representation of externally produced runtime information.

Owns:

- acquisition of available device and platform inputs;
- normalization at the AirLink boundary;
- timestamps;
- source availability;
- validity;
- freshness;
- provenance;
- distinction between live and simulated information;
- explicit unavailable, stale, invalid, or degraded states.

Relevant input categories include:

- position;
- movement;
- orientation;
- altitude-related information;
- pressure-related information where available;
- platform lifecycle signals;
- system and monotonic time.

Does not own:

- product lifecycle transitions;
- takeoff or landing confirmation;
- Flight aggregates;
- pilot-facing semantics of calculated values;
- historical retention.

## 2.5 Weather Context

Owns weather information used before or around Flight.

Owns:

- current wind;
- gust information;
- wind direction;
- atmospheric pressure or QNH required by the accepted altitude use case;
- observation or update time;
- near-term forecast when available;
- distinction between observation and forecast;
- weather freshness, availability, validity, and degraded state.

Does not own:

- the in-flight estimated-wind value;
- safety approval of a Flight;
- automated suitability decisions;
- Flight lifecycle;
- Flight recording.

## 2.6 Flight Detection

Owns identification and confirmation of possible Flight lifecycle boundaries.

Owns:

- takeoff candidates;
- confirmed takeoff;
- rejected or expired takeoff candidates;
- landing candidates;
- confirmed landing;
- rejected or expired landing candidates;
- estimated actual transition boundary information;
- detection uncertainty and diagnostic context where required.

Does not own:

- Flight Mode state;
- Flight creation;
- Flight finalization;
- durable recording;
- direct modification of Takeoff Point or Flight aggregates.

## 2.7 Flight Information Derivation

Owns calculation and interpretation of instantaneous or rolling Flight information.

Owns, where applicable:

- Ground Speed;
- altitude representation;
- vertical speed;
- current or rolling estimated wind;
- course- or orientation-related values;
- distance and bearing calculations supplied to the relevant consumer;
- quality, validity, or stability information for derived values;
- explicit distinction between measured, declared, estimated, recorded, and derived information.

Does not own:

- Flight identity;
- Flight start or end;
- attribution of aggregates to a particular Flight;
- durable historical records;
- pilot interaction;
- map representation.

## 2.8 Spatial Awareness and Map Context

Owns spatial product representation.

Owns:

- pilot-centered map context;
- current-position representation;
- map orientation behavior;
- map scale and pilot control of map scale;
- active track representation;
- saved track representation;
- Takeoff Point map representation;
- distance and direction or bearing presentation for the Takeoff Point;
- spatial degraded state when map capability is unavailable.

Does not own:

- Takeoff Point identity or association with a Flight;
- Flight lifecycle;
- active route guidance;
- route planning;
- raw location acquisition;
- durable track storage.

## 2.9 Flight Recording and Local Retention

Owns creation and retention of the durable or recoverable Flight record.

Owns:

- initialization of recording for an active Flight;
- progressive recording during Flight;
- recording of relevant time-varying information;
- recording health and degradation state;
- recoverability of already recorded information after interruption;
- finalization of a completed Flight record;
- retention of interrupted or incomplete Flight records;
- finalized Flight summary values;
- durable Flight identity or reference;
- retrieval of retained Flights;
- historical fidelity of retained information.

Does not own:

- whether a Flight is currently airborne;
- takeoff or landing detection;
- Flight Mode lifecycle;
- presentation of the saved Flight;
- calculation ownership of current derived values.

## 2.10 Simulation and Validation Enablement

Owns the AirLink-controlled capability required to exercise and observe product behavior without a real Flight.

Owns:

- production of simulated equivalents for approved input categories;
- simulation scenario state;
- simulation progress;
- simulated time where required;
- explicit simulated provenance;
- controlled validation of lifecycle and pilot-visible outcomes;
- support for input-driven simulation;
- explicitly bounded diagnostic transition injection;
- observability needed to distinguish simulated source state from downstream product behavior.

Does not own:

- an alternative Flight lifecycle;
- an alternative Flight Mode lifecycle;
- separate simulation-only calculation rules;
- direct creation or finalization of Flights during normal end-to-end simulation;
- direct durable-record construction;
- production behavior that bypasses normal responsibility boundaries.

---

# 3. Cross-Cutting Responsibility: Runtime Observability and Diagnostics

Runtime observability and diagnostics are cross-cutting responsibilities rather than a separate major concern.

Each runtime concern must expose enough observable information to validate its own decisions and handoffs.

Required observable categories include, at minimum:

- Flight Mode transitions;
- Flight lifecycle transitions;
- active Flight identity;
- detection candidates and confirmations;
- input provenance;
- input validity and freshness;
- derived-value validity or quality;
- recording health;
- record finalization outcome;
- incomplete or interrupted Flight status;
- simulation state;
- use of controlled diagnostic transition injection;
- degradation of external capabilities.

Observability must not silently change product behavior.

Detailed logging technology, telemetry format, diagnostic UI, storage, and automation remain deferred.

---

# 4. Responsibility Principles

## 4.1 Single authoritative owner

Each significant state has one authoritative owner.

Other concerns may:

- read the state;
- receive a representation of the state;
- derive information from it;
- request a transition;
- report an outcome.

They must not directly mutate another concern’s authoritative state.

## 4.2 Calculation ownership versus lifecycle ownership

Calculation ownership does not imply lifecycle ownership.

Flight Information Derivation computes instantaneous and rolling values.

Flight Lifecycle determines:

- whether a Flight exists;
- which Flight the values belong to;
- when Flight-scoped aggregation starts;
- when Flight-scoped aggregation ends;
- which aggregate values belong to the finalized Flight.

Flight Recording owns the historical and durable representation.

## 4.3 Detection versus transition ownership

Flight Detection identifies and confirms a lifecycle boundary.

It does not create or finalize a Flight.

Flight Mode authorizes whether a confirmed takeoff may initiate a Flight.

Flight Lifecycle creates and owns the Flight after that authorization.

## 4.4 Runtime completion versus durable retention

A Flight may be logically completed because landing has been confirmed even if durable record finalization is still pending or has failed.

The following states remain distinct:

- active Flight;
- completed Flight;
- successfully retained Flight;
- interrupted retained Flight;
- completed but incompletely retained Flight;
- recording-degraded Flight.

A storage failure must not imply that the pilot remains airborne.

---

# 5. Authoritative State and Information Ownership

## 5.1 Flight Mode state

Authoritative owner: **Flight Mode Lifecycle**

Examples:

- inactive;
- entering;
- waiting before first Flight;
- active Flight present;
- waiting after Flight;
- inactivity warning pending;
- exiting.

## 5.2 Active Flight identity and lifecycle state

Authoritative owner: **Flight Lifecycle and Flight State**

Examples:

- Flight identity;
- active;
- finalizing;
- finalized;
- interrupted;
- effective takeoff boundary;
- effective landing boundary.

## 5.3 Detection state

Authoritative owner: **Flight Detection**

Examples:

- takeoff candidate;
- landing candidate;
- candidate stability;
- confirmation state;
- rejection state;
- estimated transition boundary;
- diagnostic confidence or uncertainty.

## 5.4 Runtime input state

Authoritative owner: **Input Acquisition and Validity**

Examples:

- latest normalized location sample;
- altitude-related sample;
- movement sample;
- orientation sample;
- source timestamp;
- freshness;
- validity;
- availability;
- provenance.

## 5.5 Weather state

Authoritative owner: **Weather Context**

Examples:

- observed wind;
- gust;
- direction;
- QNH or pressure;
- forecast values;
- observation time;
- update time;
- freshness;
- provider availability.

## 5.6 Instantaneous and rolling derived information

Authoritative owner: **Flight Information Derivation**

Examples:

- current Ground Speed;
- current altitude representation;
- current vertical speed;
- current estimated wind;
- rolling quality or stability;
- current direction or bearing calculation.

## 5.7 Flight-scoped aggregates

Authoritative owner: **Flight Lifecycle and Flight State**

Examples:

- elapsed Flight time;
- total Flight distance;
- average speed;
- maximum speed;
- maximum altitude;
- values explicitly associated with one Flight.

Flight Information Derivation supplies calculated inputs or updated aggregate results, but Flight Lifecycle owns their association with the Flight.

## 5.8 Takeoff Point

Split ownership is accepted.

### Flight Lifecycle owns

- Takeoff Point identity;
- estimated location;
- creation moment;
- association with one Flight;
- continued identity if revisited or crossed.

### Spatial Awareness owns

- map representation;
- screen representation;
- distance presentation;
- direction or bearing presentation;
- spatial relationship to the current pilot position.

Spatial Awareness must not modify Takeoff Point identity or location.

## 5.9 Active track

Ownership is divided by responsibility:

- Input Acquisition owns individual normalized position inputs;
- Flight Lifecycle owns association of accepted track information with one Flight;
- Flight Recording owns progressive historical recording and durable retention;
- Spatial Awareness owns active and saved track presentation.

## 5.10 Completed or interrupted Flight record

Authoritative owner: **Flight Recording and Local Retention**

The retained record owns the durable historical representation of:

- recorded track;
- recorded time-varying information;
- summary values;
- lifecycle boundaries when known;
- completion status;
- incomplete or interrupted status;
- recording degradation;
- durable Flight reference.

## 5.11 Simulation scenario state

Authoritative owner: **Simulation and Validation Enablement**

Examples:

- current scenario;
- scenario progress;
- simulated time;
- simulated-source state;
- simulated provenance;
- diagnostic injection state.

Downstream concerns must not maintain separate simulation-specific versions of normal product state.

---

# 6. Principal Conceptual Handoffs

These handoffs describe meaning and responsibility transfer. They do not define exact APIs, events, commands, schemas, classes, or transport mechanisms.

## 6.1 Pilot Interaction → Flight Mode Lifecycle

Meaning:

- pilot requests entry into Flight Mode;
- pilot provides required Pre-Flight acknowledgements;
- pilot requests exit;
- pilot responds to an inactivity warning.

Flight Mode decides whether and how the operational transition occurs.

## 6.2 Weather Context → Pilot Interaction

Provides:

- observed weather information;
- forecast information when available;
- freshness;
- observation or update time;
- availability;
- validity;
- degraded state.

Pilot Interaction owns presentation, not weather-state mutation.

## 6.3 Input Acquisition and Validity → Runtime consumers

Provides:

- normalized value;
- timestamp;
- validity;
- freshness;
- provenance;
- availability or degradation state.

Consumers include:

- Flight Detection;
- Flight Information Derivation;
- Spatial Awareness;
- Flight Recording where relevant;
- runtime observability.

A consumer must not infer live versus simulated origin from hidden implementation details.

## 6.4 Flight Detection → Flight Mode Lifecycle

Provides:

- confirmed takeoff;
- confirmed landing;
- rejected or expired candidate where relevant;
- estimated actual transition boundary;
- uncertainty or diagnostic context where required.

Detection does not command direct Flight creation or record finalization.

## 6.5 Flight Mode Lifecycle → Flight Lifecycle

After confirmed takeoff, Flight Mode:

- verifies that the system is in an allowed waiting state;
- authorizes the start of a new Flight;
- supplies the relevant operational context.

Flight Lifecycle then creates and owns the Flight.

This is the accepted C2 responsibility boundary.

## 6.6 Flight Lifecycle → Flight Mode Lifecycle

Provides:

- Flight created;
- active Flight exists;
- Flight completed;
- Flight finalized at runtime level;
- no active Flight remains;
- unresolved finalization or interruption state where relevant.

Flight Mode then returns to waiting, remains active, or continues its exit flow.

## 6.7 Flight Lifecycle → Flight Information Derivation

Provides:

- Flight identity;
- Flight started;
- effective takeoff boundary;
- active-Flight state;
- finalization start;
- effective landing boundary.

This defines the period in which calculated information and aggregates belong to the Flight.

## 6.8 Flight Information Derivation → Flight Lifecycle

Provides:

- current derived information;
- updated Flight-scoped aggregate information;
- final aggregate values;
- quality, validity, or limitation state.

Derivation does not mutate Flight identity or lifecycle state.

## 6.9 Flight Lifecycle → Spatial Awareness

Provides:

- active Flight identity;
- active versus finalized status;
- Takeoff Point identity and location;
- association of track information with the Flight;
- finalized retained reference where required for later review.

Spatial Awareness owns representation, not Flight semantics.

## 6.10 Flight Lifecycle and runtime information → Flight Recording

Recording begins with the active Flight rather than after landing.

Provides progressively:

- Flight identity;
- effective Flight start;
- lifecycle markers;
- accepted time-varying information;
- derived information selected for retention;
- track information;
- aggregate updates where required;
- completion or interruption state;
- effective Flight end when known;
- final aggregate values.

The exact buffering, checkpoint, and transaction mechanisms are deferred.

## 6.11 Flight Recording → Flight Lifecycle and Pilot Interaction

Provides:

- recording initialized;
- recording healthy or degraded;
- already recorded information remains recoverable;
- finalization succeeded or failed;
- Flight retained;
- Flight retained as incomplete or interrupted;
- durable Flight identity or reference.

A Flight may complete operationally before successful durable finalization.

## 6.12 Flight Recording → Saved-Flight presentation

Provides:

- retained Flight list information;
- selected Flight record;
- recorded track;
- summary information;
- completion or interruption status;
- data-quality or degradation information required for correct understanding.

## 6.13 Simulation and Validation → Input boundary

For normal end-to-end simulation, Simulation substitutes external input production.

It may provide simulated equivalents of:

- position;
- movement;
- orientation;
- altitude-related inputs;
- time progression;
- weather where required;
- platform interruption or degradation conditions where required.

The information then follows the same conceptual validity, provenance, lifecycle, derivation, recording, and presentation boundaries as live input.

Simulation must not directly:

- create a Flight;
- finalize a Flight;
- create a Takeoff Point;
- construct a retained Flight record;
- bypass Flight Detection in the normal input-driven validation path.

## 6.14 Controlled diagnostic transition injection

A separate, explicitly identified diagnostic capability may inject confirmed takeoff or landing transitions to validate downstream concerns independently.

Such injection:

- must be observably identified;
- is not the normal end-to-end simulation path;
- does not validate Flight Detection;
- must not be presented as evidence that live-input detection works;
- must not create separate downstream product logic.

---

# 7. Recording and Interruption Semantics

## 7.1 Recording start

Flight Recording begins when an active Flight is created.

It does not wait until landing.

## 7.2 Progressive retention

Relevant time-varying Flight information is recorded progressively.

The engineering expectation is that already recorded Flight information remains recoverable if:

- the battery is exhausted;
- the application process terminates;
- the operating system interrupts the application;
- the application crashes;
- another unexpected interruption occurs.

Exact durability timing and guarantees remain deferred.

## 7.3 Interrupted Flight

If recording stops before a confirmed landing and normal finalization:

- the already recorded part should remain available where technically recoverable;
- the record must be identified as incomplete or interrupted;
- the absence of a confirmed landing boundary must remain explicit;
- AirLink must not invent a landing;
- the user must be able to distinguish the interrupted record from a complete Flight;
- retained information should make the point of interruption understandable.

## 7.4 Recording degradation

If durable recording becomes unavailable during an active Flight:

- Flight lifecycle continues;
- detection continues where input permits;
- pilot-facing core Flight behavior continues;
- recording enters an explicit degraded state;
- loss or incompleteness of retained information must not be hidden.

---

# 8. External Dependencies

This section identifies dependency categories only. It does not select technologies or providers.

## 8.1 Android platform lifecycle

External responsibilities include:

- application and process lifecycle;
- foreground and background state;
- permission model;
- resource constraints;
- battery constraints;
- execution restrictions;
- process termination behavior.

AirLink responsibilities include:

- handling lifecycle changes;
- preserving recoverable Flight information;
- restoring understandable state after restart;
- explicit behavior when required permission is unavailable;
- preventing silent loss of an active or interrupted Flight record.

## 8.2 Device location and motion capabilities

External responsibilities include production of available:

- location samples;
- timestamps;
- accuracy or quality information;
- movement information;
- course where available;
- orientation or motion information;
- source-availability state.

AirLink must not assume:

- continuous availability;
- stable update frequency;
- reliable course at low speed;
- identical hardware across devices;
- perfect timestamp behavior;
- uninterrupted sensor access.

## 8.3 Altitude and pressure-related capabilities

Potential external information includes:

- GNSS altitude;
- device barometric pressure;
- provider-delivered pressure or QNH;
- associated quality information.

Issue #33 does not select an altitude source, source hierarchy, or fusion model.

AirLink must preserve source and provenance distinctions where they affect meaning.

Unavailable or derived altitude information must not be silently represented as a direct exact measurement.

## 8.4 Weather provider category

Expected categories include:

- current wind;
- gust;
- direction;
- pressure or QNH;
- observation or update time;
- near-term forecast where available without disproportionate complexity.

AirLink owns:

- freshness interpretation;
- unavailable state;
- stale-data state;
- observation versus forecast distinction;
- presentation that does not represent provider data as a safety guarantee.

## 8.5 Map and geographic presentation capability

Potential external capability includes:

- map rendering or tiles;
- coordinate projection;
- geographic viewport behavior;
- cache or offline behavior;
- attribution constraints.

AirLink owns:

- pilot-centered presentation;
- current-position representation;
- track presentation;
- Takeoff Point presentation;
- map-scale interaction;
- degraded behavior.

Loss of map capability must not terminate:

- Flight Mode;
- active Flight;
- Flight Detection;
- recording;
- core numeric Flight information.

## 8.6 Local storage capability

External responsibility includes local durable-storage primitives.

AirLink owns:

- storage-readiness interpretation;
- progressive recording;
- recoverability expectations;
- record integrity;
- finalization semantics;
- interrupted-record status;
- degraded-recording status;
- later retrieval.

No database, file format, serialization method, or transaction mechanism is selected here.

## 8.7 System and monotonic time

The system must conceptually distinguish:

- wall-clock time for dates and civil-time presentation;
- monotonic elapsed time for durations, detection windows, inactivity periods, and runtime intervals.

Flight duration and runtime timing must not rely solely on wall-clock continuity.

Exact Android APIs remain deferred.

## 8.8 Network capability

Network access is an external capability, not a continuous prerequisite for an active Flight.

After entering Flight Mode, core behavior must not require permanent connectivity.

Network may support:

- preloaded weather context;
- cached or preloaded map context;
- opportunistic refresh.

Loss of network must not stop:

- Flight lifecycle;
- Flight Detection;
- derived core Flight information;
- recording;
- local retention.

---

# 9. Degradation Expectations

## 9.1 Map degradation

Accepted behavior:

- active Flight continues;
- recording continues;
- core numeric information remains available where inputs permit;
- map presentation becomes explicitly degraded or unavailable;
- no false map content is substituted silently.

## 9.2 Network degradation

Accepted behavior:

- active Flight remains local-first;
- no permanent network dependency exists;
- previously available or cached context may remain usable subject to freshness semantics;
- opportunistic refresh may fail without terminating Flight behavior.

## 9.3 Storage readiness and failure

Before Flight, AirLink should perform a bounded storage-readiness assessment.

If a storage problem is known before Flight:

- the user receives an explicit warning or degraded-state indication;
- exact blocking policy remains a later bounded decision if required.

If storage fails during Flight:

- Flight lifecycle continues;
- recording status becomes explicitly degraded;
- already retained information should remain recoverable where possible;
- incomplete retention must be visible.

## 9.4 Input degradation

Unavailable, stale, invalid, or low-quality input must remain distinguishable.

Issue #33 does not define exact thresholds or detailed fallback behavior.

A downstream concern must not silently treat invalid input as valid current information.

---

# 10. Explicit MVP 0.1 Engineering Exclusions

MVP 0.1 has no mandatory dependency on:

- cloud backend;
- user account;
- authentication;
- server-side processing;
- synchronization between devices;
- another AirLink user;
- social capabilities;
- multi-user operation;
- crew coordination;
- connected aircraft equipment;
- external flight computer;
- route-based mission planning;
- active route navigation;
- remote analytics;
- permanent network connectivity;
- web client;
- iOS client;
- replay presentation;
- complete Pilot Ecosystem;
- complete future AirLink architecture.

These exclusions do not erase future product directions. They prevent MVP 0.1 planning from silently implementing or constraining them.

---

# 11. Deferred Decisions

The following decisions are intentionally not made under issue #33.

## 11.1 Platform and application realization

- Android framework choices;
- application architecture;
- minimum Android version;
- exact permission-handling mechanisms;
- background-execution strategy;
- dependency-injection approach;
- package and module structure;
- concurrency model.

## 11.2 Providers and infrastructure

- weather provider;
- map provider;
- database or file-storage technology;
- serialization format;
- cache implementation;
- network client;
- telemetry or logging technology.

## 11.3 Input and validity details

- exact platform APIs;
- sampling rates;
- freshness thresholds;
- validity thresholds;
- source-priority rules;
- fallback algorithms;
- sensor fusion;
- orientation-source switching;
- smoothing.

## 11.4 Flight Detection details

- takeoff algorithm;
- landing algorithm;
- thresholds;
- filters;
- confirmation windows;
- retrospective boundary estimation;
- false-positive recovery;
- false-negative recovery;
- interruption handling.

## 11.5 Derived-information details

- estimated-wind algorithm;
- wind-calculation window;
- confidence or stability metric;
- altitude model;
- pressure correction;
- vertical-speed algorithm;
- aggregate precision;
- update rates.

## 11.6 Recording details

- retained parameter list;
- sample intervals;
- checkpoint frequency;
- buffering;
- transaction model;
- write strategy;
- recovery mechanism;
- storage-capacity policy;
- retention policy;
- historical-value preservation rules;
- record format.

## 11.7 Simulation details

- simulation architecture;
- scenario format;
- scenario-authoring mechanism;
- virtual clock;
- input-generation mechanism;
- simulation control interface;
- diagnostic control interface;
- automation framework;
- test-runner integration.

## 11.8 Presentation details

- visual hierarchy;
- detailed screen layout;
- exact warning interaction;
- detailed degraded-state presentation;
- units and formatting;
- notification mechanisms;
- detailed map behavior;
- detailed saved-Flight presentation.

---

# 12. Decisions Required in Later AL-0002 Work

The following matters must not be silently delegated to an implementation agent.

They must be resolved in issue #34, #36, #37, or a separately authorized bounded decision issue when required:

- live versus simulated substitution points;
- minimum simulated input set;
- minimum simulation fidelity;
- provenance representation;
- mandatory runtime observability;
- first-slice lifecycle boundary;
- first-slice recording durability;
- first-slice interruption recovery;
- incomplete Flight record semantics at implementation-ready depth;
- material provider choices required by the selected slice;
- difficult-to-reverse persistence decisions;
- exact first-slice validation strategy;
- necessary technical decisions for AL-0003;
- explicit deferrals applicable to AL-0003.

---

# 13. Product Direction Alignment

## Direction advanced

This Engineering Map advances the first meaningful Flight Support outcome by defining the minimum engineering structure required to implement the accepted preparation–Flight–completion–review product flow incrementally.

## Explicit simplifications

- concerns are defined as responsibilities, not final components;
- full-MVP planning remains at boundary and ownership level;
- implementation mechanisms remain deferred;
- simulation is required but not prematurely designed as a universal platform;
- no complete architecture or backlog is created;
- no technology or provider is selected without a demonstrated need;
- no product implementation begins.

## Reversibility

The decisions in this section are designed to preserve reversibility because they define:

- semantic ownership;
- lifecycle separation;
- information provenance;
- responsibility boundaries;
- external dependency consequences.

They do not prescribe specific modules, classes, libraries, storage engines, providers, schemas, or deployment architecture.

## Outcome

`Aligned with explicit simplification`.

---

# 14. Issue #33 Completion Assessment

Issue #33 may be considered complete after owner review confirms that:

- the MVP 0.1 Scope is sufficient for planning;
- no blocking product contradiction remains;
- the engineering boundary is accepted;
- the ten major concerns are accepted;
- responsibility and non-ownership boundaries are accepted;
- authoritative state and information ownership is accepted;
- Flight Mode and Flight remain distinct;
- the C2 Flight-start authorization boundary is accepted;
- Takeoff Point split ownership is accepted;
- instantaneous derivation, Flight-scoped aggregation, and durable recording ownership are accepted;
- conceptual handoffs are accepted;
- progressive Flight Recording and interrupted-record semantics are accepted;
- input-driven simulation and bounded diagnostic transition injection are accepted;
- external-dependency categories are accepted;
- map, network, and storage degradation expectations are accepted;
- exclusions and deferred decisions are accepted;
- this document remains WIP and non-canonical;
- no implementation or final architecture has been introduced.

## Remaining AL-0002 work

After issue #33, the Engineering Map remains incomplete.

The next bounded work is:

1. issue #34 — live-input and simulation-substitution boundaries;
2. issue #35 — dependencies, risks, decisions, and future implementation sequence;
3. owner approval of the complete MVP 0.1 Engineering Map.
