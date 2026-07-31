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

The current document contains the engineering-boundary and responsibility-map work prepared under GitHub issue `#33 / AL-0002-01`.

The following extensions remain reserved for later bounded work:

- issue `#34 / AL-0002-02` — live-input and simulation-substitution boundaries, provenance, minimum simulation fidelity, and mandatory observability;
- issue `#35 / AL-0002-03` — dependency order, risk order, decision order, deferred-decision consolidation, and candidate implementation sequence.

Approval of the issue #33 content does not constitute approval of the complete Engineering Map. Final owner approval of the consolidated map occurs only after the bounded work of issues #34 and #35.

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

Implementation-ready depth is intentionally reserved for the selected first vertical slice and its governing later issues. Issues #34 and #35 extend this map without converting it into a complete architecture.

## Governing Context

Work under this document follows the source-of-truth order and Product-Significance Routing defined in `AGENTS.md`.

Task-specific context for issue #33 consists of:

- `ITERATION.md`;
- `docs/product/CurrentState.md`;
- relevant canonical product documentation;
- GitHub issue #33 and approved task artifacts;
- the owner-reviewed `docs/product/wip/mvp-0.1-scope.md` planning baseline;
- directly relevant Flight Mode, Flight, and Navigation WIP.

Consultation does not promote WIP into canon or make this planning artifact implementation authority. If governing sources conflict, the conflict must be reported rather than silently resolved.

---

# 1. Planning Sufficiency and Engineering Boundary

## 1.1 Planning-sufficiency assessment

The existing MVP 0.1 Scope is sufficient as the WIP product-level baseline for the concern and responsibility work required by issue #33.

No contradiction or omission currently blocks:

- definition of the MVP 0.1 engineering boundary;
- identification of major concerns;
- separation of Flight Mode and Flight responsibilities;
- state and information ownership;
- concern-level product-flow coverage;
- external-dependency classification;
- later live/simulation planning under issue #34.

No product-scope correction is required under issue #33.

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

Normal end-to-end simulation substitutes approved external input production and uses the same downstream concerns as live input. It must not create an alternative Flight Mode, Flight lifecycle, calculation model, record-construction path, or pilot-facing product behavior.

Issue #33 defines only this responsibility boundary. Substitution points, simulated equivalents, provenance, minimum fidelity, observability, scenario control, and architecture remain reserved for issue #34 and the selected-slice planning work.

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
- manual-completion retain-versus-discard choice.

**Consumes**

- weather and freshness information from C5;
- Flight Mode state and transition outcomes from C2;
- active Flight context, elapsed time, final boundaries, and final aggregates from C3;
- current derived Flight values and explanatory estimated-wind semantics from C7;
- spatial presentation from C8;
- recording, retention, deletion, and saved-record results from C9.

**Produces**

- requests to load or refresh current conditions;
- Pre-Flight acknowledgements;
- requests to enter, continue, or exit Flight Mode;
- requests to manually complete a Flight;
- explicit choice to retain a real Flight or discard a false detection;
- pilot map-scale adjustment actions;
- saved-Flight selection and review actions.

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
- effective takeoff, confirmed-landing, and manual-completion boundaries;
- association of information with one Flight;
- elapsed Flight time and Flight-scoped aggregates;
- Takeoff Point identity, estimated location, and association with the Flight;
- confirmed Landing Point identity, estimated location, confirmed-landing classification, and association with the completed Flight;
- continuity of the normally completed Flight through landing confirmation, including its final recorded segment without retrospective trimming;
- runtime completion, rejection, and finalization of the individual Flight.

**Consumes**

- lifecycle authorization and operational context from C2;
- monotonic time from C4;
- derived-value and aggregate updates from C7;
- recording and retention outcomes from C9 where they affect pilot-visible completion status.

**Produces**

- active Flight identity and lifecycle context for C1, C7, C8, and C9;
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
- availability, validity, freshness, accuracy or quality metadata, and provenance;
- distinction between live and simulated source information;
- platform lifecycle and interruption signals exposed at the AirLink boundary;
- execution of concern-level acquisition demand through deferred platform and resource-management mechanisms.

Relevant input categories include position, movement, orientation, altitude-related information, pressure-related information where available, time, and platform-state signals.

**Consumes**

- bounded current-location acquisition demand from C5;
- Flight Mode acquisition demand from C2;
- available device, platform, and simulated-source information.

**Produces**

- normalized current-location context, including availability, validity, freshness, and provenance, for C5;
- normalized runtime information for C2, C3, C6, C7, C8, and C9 as required by the covered flows;
- observable source and validity state.

**Does not own**

- product lifecycle transitions;
- the product decisions that current-location weather or Flight Mode require acquisition;
- detection confirmations;
- derived semantics;
- Flight aggregates;
- presentation or historical retention.

## C5 — Weather Context

**Owns**

- the product need for current-location weather context in normal application and Pre-Flight use;
- geographic scoping and interpretation of current-location weather acquisition;
- current observed wind, gusts, direction, pressure or QNH, and observation or update time;
- near-term forecast when available;
- observation-versus-forecast distinction;
- weather availability, freshness, validity, provenance, and degraded state.

**Consumes**

- current-conditions requests from C1;
- normalized current-location context, including availability, validity, freshness, and provenance, from C4;
- external weather-provider information.

**Produces**

- bounded current-location acquisition demand for C4;
- weather context and status for C1;
- pressure or QNH context for C7 when the selected altitude representation requires it.

**Does not own**

- location acquisition or normalization;
- automated suitability or safety approval;
- the in-flight estimated-wind value;
- the altitude model;
- Flight lifecycle or recording.

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
- active Flight and Takeoff Point context from C3;
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
- active Flight and Takeoff Point context from C3;
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
- technical recoverability mechanisms, subject to unresolved interruption semantics.

**Consumes**

- active Flight identity, lifecycle markers, Takeoff Point identity, estimated location and Flight association, final boundaries, final aggregates, and confirmed Landing Point information from C3;
- normalized track and other approved retained inputs from C4;
- selected derived values and their approved historical semantics from C7.

**Produces**

- recording health and retention status for C1 and C3;
- retained summary fields, special-point information, historically preserved values, and track for C1 and C8 as required by approved presentation contracts;
- durable Flight reference when available;
- success or failure of false-detection episode deletion.

**Does not own**

- whether the pilot is airborne;
- Flight Mode or Flight lifecycle;
- boundary detection;
- confirmed Landing Point meaning or classification;
- pilot retain-versus-discard choice;
- current-value calculations or later reinterpretations;
- presentation.

## C10 — Simulation and Validation Enablement

**Owns**

- AirLink-controlled simulated-source production for approved input categories;
- simulation scenario state and progress;
- simulated time where required;
- explicit simulated provenance;
- controlled validation support for lifecycle and pilot-visible outcomes.

**Consumes**

- approved scenario and validation controls.

**Produces**

- simulated equivalents through the normal C4 input boundary;
- observable simulation state.

**Does not own**

- an alternative Flight Mode or Flight lifecycle;
- simulation-only calculation rules;
- direct Flight creation, completion, rejection, or durable-record construction;
- production behavior that bypasses the normal concern paths.

## Cross-cutting observability responsibility

Observability is a responsibility of every runtime concern rather than a separate product concern.

Each concern must expose enough information to verify its authoritative state, decisions, validity, provenance, handoffs, degradation, and outcomes. Observability must not silently change product behavior or preserve data that accepted product behavior requires to be deleted.

Detailed logging technology, telemetry format, diagnostic UI, storage, and automation remain deferred. Issue #34 will define the minimum mandatory observability required for live/simulated validation.

---

# 3. Authoritative State and Information Ownership

| ID | State or information category | Authoritative owner | Required consumers or consequence |
| --- | --- | --- | --- |
| O1 | Flight Mode operational state, transition outcome, and acquisition demand | C2 | C1 presents state/outcomes; C4 fulfills acquisition demand; C6 uses allowed detection context |
| O2 | Flight identity and runtime lifecycle state | C3 | C1, C2, C7, C8, and C9 consume the relevant context |
| O3 | Effective takeoff, confirmed-landing, and manual-completion boundaries | C3 | C1 and C9 consume final boundaries; C7 uses the active interval; confirmed landing includes the full final segment through confirmation |
| O4 | Elapsed Flight time and Flight-scoped aggregates | C3 | C1 presents them; C9 retains approved final values |
| O5 | Takeoff Point identity and estimated location | C3 | C7 calculates relative values; C8 uses it as Current Waypoint and presents it; C9 consumes and retains it without reconstruction |
| O6 | Normalized runtime inputs, time, validity, freshness, and provenance | C4 | C5 consumes current-location context; other runtime consumers use required inputs without reclassifying source state silently |
| O7 | Current-location weather acquisition demand, weather observation, forecast, and pressure or QNH context | C5 | C4 fulfills the location demand; C1 presents weather; C7 conditionally consumes pressure or QNH |
| O8 | Takeoff and landing detection candidate, confirmation, and accepted detector constraint state | C6 | C2 decides whether confirmed boundaries may affect lifecycle; permanent speed-only takeoff detection is prohibited |
| O9 | Current derived Flight, True-North-referenced orientation, direction, distance, and bearing values | C7 | C1 presents current Flight values; C8 presents C7-provided orientation and relative-navigation values without recalculating them; C3 receives aggregate updates; C9 retains approved historical values without silent replacement |
| O10 | Active and saved spatial representation, including pilot-controlled scale and Flight compass ring or scale | C8 | C1 supplies scale-adjustment actions and presents the resulting map, compass, orientation, and awareness context |
| O11 | Progressive and durable Flight record, recording status, saved representation, and historical-value preservation | C9 | C1 and C8 consume saved and retention results; later interpretations must not silently replace original retained values |
| O12 | Simulation scenario and simulated-source state | C10 | C4 receives simulated equivalents; observability distinguishes the source |
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
| F1 | Current conditions | C1 requests current conditions from C5; C5 expresses bounded current-location acquisition demand to C4; C4 fulfills that demand and supplies normalized current-location context, including availability, validity, freshness, and provenance, to C5; C5 uses that context to scope external weather acquisition and supplies observed weather, forecast when available, freshness, validity, and degraded state to C1 | The pilot can understand relevant conditions for the current location before Pre-Flight; unavailable or invalid location produces an explicit weather limitation rather than silently using an unrelated location | Provider, request mechanism, concurrent-demand coordination, refresh/cache policy, forecast intervals, exact representation, fallback details |
| F2 | Minimal Pre-Flight and Flight Mode entry | C1 presents accepted acknowledgements and sends the completed acknowledgements plus explicit entry request to C2; C2 returns the resulting state to C1, issues Flight Mode acquisition demand to C4, and enables the allowed detection context for C6; C4 fulfills the demand through its acquisition boundary | Flight Mode becomes active only after explicit pilot intent; Ready on Ground waiting begins; required Flight Mode inputs become available through C4 | Exact controls, layout, platform activation, acquisition profiles, and resource-management mechanism |
| F3 | Ready on Ground waiting, warning, continuation, and automatic exit | C4 supplies monotonic time to C2; C2 supplies waiting/warning/exit state to C1; C1 may request continuation; C2 resets the waiting period or exits when required; on exit C2 withdraws Flight Mode acquisition demand and C4 reduces or stops Flight Mode-specific acquisition while preserving other active input demands | The warning is presented, continuation is possible, and Flight Mode eventually exits if the pilot does not continue; resource use follows Flight Mode demand without C2 performing acquisition directly | Timeout and warning duration, presentation, exact continuation control, other reset conditions (`P5`), acquisition profiles and platform mechanism |
| F4 | Confirmed takeoff and Flight creation | C4 supplies valid runtime inputs to C6; C6 confirms takeoff under the accepted non-speed-only permanent detector constraint and supplies its estimated boundary to C2; C2 validates context and authorizes C3; C3 creates the Flight and Takeoff Point and supplies active-Flight/Takeoff-Point context to C2/C1/C7/C8/C9; C8 makes Takeoff Point the Current Waypoint while keeping Active Navigation off | One Flight begins inside active Flight Mode; its effective start and Takeoff Point represent the accepted actual-takeoff estimate; permanent takeoff detection is not based only on speed; Takeoff Point becomes the passive navigation context; recording starts with the authoritative Takeoff Point available to C9 | Detector design, signal combination, confirmation semantics (`P2`), recent-history custody, retrospective estimation, thresholds, filters |
| F5 | Active Flight information, elapsed time, aggregates, and progressive recording | C4 supplies inputs to C7 and time to C3; C5 supplies pressure or QNH to C7 only when required; C7 supplies current values to C1 and aggregate updates to C3; C3 supplies elapsed time and Flight context to C1; C4/C7/C3 supply approved retained information and historical semantics to C9 | Ground Speed, altitude, vertical speed, Flight time, and estimated wind are available with correct semantic status; Flight aggregates advance; progressive recording preserves the approved values and context required to represent what was available or used during the original Flight | Algorithms, validity rules, update rates, altitude/QNH model, exact retained parameter set, provenance representation, sampling and persistence mechanics |
| F6 | Takeoff Point awareness and spatial orientation | C4 supplies normalized position and other required spatial/source inputs to C8 and supplies movement/orientation source inputs to C7; C3 supplies Takeoff Point identity/location to C7 and C8; C8 retains it as Current Waypoint with Active Navigation off; C7 supplies True-North-referenced pilot-facing orientation values or candidates, their semantic/validity state, and Takeoff Point distance/bearing to C8; C1 supplies pilot map-scale adjustment actions to C8; C8 applies those actions and the later-approved orientation policy and supplies the pilot-centred map, compass ring or scale, and passive awareness presentation to C1 | The Takeoff Point remains the current passive navigation context and stays distinct and visible throughout the Flight; distance and bearing/direction use True North; map orientation uses C7-provided pilot-facing values without C8 recalculation; Track, Heading, bearing, and device orientation remain distinct; a compass ring or scale is present around the pilot; the pilot can change map scale during Flight; passive awareness does not become Active Navigation | Selection and switching among C7-provided orientation candidates, declination source/model, correction and derivation algorithms, update rate, source validity, fallback behavior, quality semantics, exact scale controls, zoom range/steps, gesture or button behavior, compass visual design, animation, recenter interaction and final orientation decision (`P6`) |
| F7 | Confirmed landing and runtime completion | C4 supplies inputs to C6; C6 confirms landing and supplies the boundary to C2; C2 validates context and authorizes C3; C3 completes/finalizes the Flight through the confirmed boundary without trimming the final segment, establishes the confirmed Landing Point, supplies final boundaries/aggregates to C1/C9, supplies Landing Point identity/location/classification to C9, and reports no active Flight to C2 | The individual Flight ends with all approved information through landing confirmation retained; the final segment is not retrospectively trimmed; the completed Flight has a distinct confirmed Landing Point; C2 immediately returns to Ready on Ground and waiting for another takeoff resumes | Landing detector, confirmation semantics (`P2`), exact Landing Point determination and storage representation |
| F8 | Immediate retained completed-Flight Summary | After either confirmed landing through F7 or manual retention through F10, C3 supplies final Flight values and the authoritative completion type to C1/C9; C9 supplies recording completeness and retention status; C1 presents their shared core summary information while C2 remains Ready on Ground | A Summary appears inside the Flight flow for every retained completed Flight, identifies whether completion followed confirmed landing or manual completion, does not imply confirmed landing for a manually completed Flight, and does not block waiting for another takeoff | Exact Summary fields beyond accepted minimum, information hierarchy, layout, retention timing and error presentation |
| F9 | Another Flight in the same Flight Mode period | After F7/F8 or F10/F8, C2 remains Ready on Ground, Flight Mode acquisition demand remains active, and C6 remains allowed to detect takeoff; a new F4 starts a new independent Flight; C1 closes the prior Summary when the new Flight begins | Multiple independent Flights may occur in one Flight Mode period without a Flight Session record | Exact presentation transition |
| F10 | Manual completion — retain real Flight | C1 sends the manual-completion request and retain choice to C2; C2 validates context and authorizes C3; C3 completes the Flight with an explicit manual boundary, marks the completion type as manual, and supplies final results to C1/C9; C9 supplies recording completeness and retention status; C2 returns to Ready on Ground; the flow continues through the shared F8 Summary path | A real Flight is retained; an immediate Summary is available and identifies manual completion; no confirmed landing or confirmed Landing Point is implied; Flight Mode remains active and Ready on Ground | Exact interaction, confirmation behavior, retention timing and error presentation, and Landing Point semantics (`P4`) |
| F11 | Manual completion — discard false detection | C1 sends the discard choice to C2; C2 authorizes rejection by C3; C3 reports the rejected outcome to C2/C9; C9 deletes the progressively recorded episode; C2 returns to Ready on Ground | No Flight and no hidden durable Flight-equivalent record remain; bounded observability may record only that rejection and deletion occurred | Technical deletion and cleanup mechanism; cleanup-failure recovery |
| F12 | Explicit Flight Mode exit while no Flight is active | C1 sends an exit request to C2; C2 exits, disables the allowed detection context, withdraws Flight Mode acquisition demand from C4, and returns the resulting state to C1; C4 reduces or stops Flight Mode-specific acquisition while preserving any other active input demand | Flight Mode ends separately from any individual Flight; C2 owns the operational decision and C4 owns the acquisition/resource mechanism | Exact control, presentation, acquisition profiles, and platform mechanism |
| F13 | Explicit Flight Mode exit while a Flight is active | No product flow is accepted. C1 may originate the request, but C2 must not invent refusal, forced completion, discard, or another transition | The unresolved state is exposed rather than silently implemented | Explicit owner product decision `P1` is required before implementation reaches this scenario |
| F14 | Platform interruption during an active Flight | C4 exposes the interruption or restoration signal to C3 and C9; the affected concerns expose their state and outcome to C1/C2 as required | Interruption is observable and does not silently masquerade as confirmed landing or deliberate completion | Product classification and retained outcome (`P3`), recovery guarantees, checkpointing, restoration, and storage mechanics |
| F15 | Saved-Flight access and review | C1 requests a retained Flight from C9; C9 supplies accepted summary fields, retained status, retained special-point information, historically preserved information, and recorded track; C8 presents the saved track and scale control; C1 presents the saved summary and record status | A retained Flight can later be opened and understood through its map track and principal summary information without losing special-point identity or silently replacing values that were available or used during the original Flight | Navigation to history, detailed layout, editing, replay, analytics, schema, migration and retrieval implementation |
| F16 | Simulation-driven validation | C10 supplies approved simulated equivalents through C4; C4 preserves simulated provenance; normal C2–C9 paths execute without simulation-only lifecycle or product logic | Ground waiting, takeoff, active Flight values, Takeoff Point awareness, landing, Summary, persistence, saved review, and multiple Flights can be validated without a real Flight | Full substitution boundary, minimum equivalents, fidelity, provenance contract, observability, controls, automation, and architecture belong to issue #34 |

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

Measured, declared, recorded, estimated, and derived information remain distinguishable where their meaning matters. Live and simulated provenance, validity, freshness, availability, and degradation must not be inferred from hidden implementation details.

## R5 — Takeoff Point is the passive Current Waypoint after takeoff

After takeoff in the MVP free-flight scenario, Takeoff Point becomes Current Waypoint and remains available after it is reached, crossed, or revisited. Active Navigation remains off. Distance and bearing/direction awareness do not enable route guidance or Active Navigation.

## R6 — False-detection discard is destructive at the Flight-record level

A pilot-rejected false-detection episode produces no Flight and no hidden durable Flight-equivalent record. C9 is responsible for deleting any progressively recorded episode data. The technical deletion mechanism remains deferred.

## R7 — Summary continuity does not prescribe one technical object

The immediate Summary and later saved-Flight review use the same core Flight information and preserve the same semantics. This rule does not require a shared DTO, read model, database object, service, or other technical realization.

## R8 — Local-first degradation preserves lifecycle meaning

Map, weather, network, or storage degradation may reduce available information or retention quality, but it must not silently redefine whether Flight Mode is active, whether a Flight exists, or whether landing was confirmed.

## R9 — Simulation reuses normal product responsibilities

Simulation substitutes approved input production and validation control. It does not create simulation-only lifecycle, calculation, storage, or presentation behavior.

## R10 — Pilot-facing navigation directions use True North

All pilot-facing navigation directions are referenced to True North. A magnetic source may support ground device orientation, but it must be corrected before pilot-facing use. Track, Heading, bearing, and device orientation remain semantically distinct and must not be presented as an unlabeled generic direction.

The declination source or model, correction algorithm, update rate, validity rules, fallback behavior, and display formatting remain deferred.

## R11 — Historical Flight information is not silently rewritten

Replay-supporting information retained for a Flight preserves the values, semantic status, validity, provenance, and calculation context required to represent what was available or used during the original Flight.

Later algorithm or interpretation changes may produce a distinct later interpretation, but they must not silently replace the retained historical values.

The exact retained parameter set, sampling rules, provenance representation, calculation-version context, storage format, migration mechanism, and later-interpretation representation remain deferred.

## R12 — Confirmed landing preserves the final Flight segment

A normally completed Flight includes all approved information retained through landing confirmation. Confirmed landing finalization must not retrospectively trim the final segment in order to approximate an earlier landing boundary.

The detector, confirmation rule, Landing Point determination method, and technical finalization mechanism remain deferred.

## R13 — Permanent takeoff detection is not speed-only

Takeoff detection may use speed as one signal, but the permanent detector must not rely on speed as its only basis. The exact signal set, algorithm, thresholds, filters, and confirmation behavior remain deferred.

## R14 — Unused pre-takeoff history is transient

A bounded recent history may support retrospective takeoff-boundary estimation. Measurements not incorporated into a confirmed Flight are overwritten and are not retained as Flight data or as a hidden Flight-equivalent record.

The buffer duration, custody, data categories, and implementation mechanism remain deferred.

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
| D5 | Recording and replay-supporting data | Exact retained parameter set, sampling intervals, provenance and validity representation, calculation-version context, preservation mechanism, active-Flight recording buffering, checkpointing, recovery, storage capacity, retention policy, special-point storage representation, schema, migration and format | Selected-slice persistence planning and later logging work |
| D6 | Summary and presentation | Exact Summary fields beyond accepted minimum, information hierarchy, formatting, units, controls, warning presentation, non-flight placement and presentation of estimated-wind limitations, manual-versus-confirmed completion formatting, saved-review layout | Selected-slice UX and product planning |
| D7 | Simulation and observability | Live/simulated substitution points, simulated equivalents, fidelity, provenance, mandatory observable behavior, controls, scenarios, automation | Issue #34, then selected-slice planning |
| D8 | Dependencies, risks, decisions, and implementation sequence | Concern dependency order, external constraints, risk-reduction order, difficult-to-reverse decisions, future slice sequence | Issue #35 |

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
- **Explicit simplification:** MVP 0.1 is mapped only at concern, authoritative-ownership, mandatory-flow, external-dependency-category, and deferral level.
- **Approval authority:** the planning depth and simplification are authorized by `ITERATION.md`, issue #33, and the owner-reviewed MVP 0.1 planning boundary. Acceptance of this specific draft remains subject to owner review.
- **Boundedness:** the map applies only to MVP 0.1 engineering planning under AL-0002.
- **Reversibility:** no final components, APIs, schemas, providers, algorithms, storage engines, or complete architecture are selected.
- **Intentionally deferred:** wider Flight Support domains, Pilot Ecosystem, connected operation, cloud, web, iOS, later lifecycle capabilities, and future architecture.
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
8. no final architecture, API, schema, provider, algorithm, or complete internal message graph is implied.

Do not treat the absence of implementation mechanics as a defect unless that absence leaves an accepted product flow, ownership boundary, difficult-to-reverse decision, accepted semantic invariant, or required first-slice dependency undefined.

---

# 11. Issue #33 Acceptance Check

Issue #33 content is ready for owner review when the owner confirms that:

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

# 12. Reserved Extension Points

## 12.1 Issue #34 — Live and simulated input boundary

Issue #34 will extend this map with the approved:

- live-input categories and minimum simulated equivalents;
- conceptual substitution points;
- provenance, validity, freshness, and distinguishability expectations;
- minimum Flight Simulation Framework responsibility;
- mandatory observable lifecycle and pilot-visible outcomes;
- simulation concerns that remain deferred.

This extension must preserve C1–C10 ownership and use the normal F1–F16 product paths unless a conflict or missing product decision is explicitly reported.

## 12.2 Issue #35 — Dependencies, risks, decisions, and future slices

Issue #35 will extend and consolidate this map with the approved:

- concern-level dependency order;
- external constraints affecting implementation order;
- major engineering risks and risk-reduction order;
- difficult-to-reverse decision classification and timing;
- consolidated deferred-decision register;
- high-level candidate sequence of future implementation iterations;
- findings that constrain first-slice candidate selection.

This extension must not convert the map into a complete backlog, final architecture, or detailed plan for every future slice.
