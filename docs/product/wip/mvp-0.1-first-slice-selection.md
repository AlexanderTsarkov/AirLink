# MVP 0.1 First Vertical Slice Selection

## Status and Authority

Related issue: [#36 — AL-0002-04: Compare and select the first MVP 0.1 end-to-end vertical slice](https://github.com/AlexanderTsarkov/AirLink/issues/36).

This document is an **owner-selected AL-0002 engineering-planning artifact**. It is non-canonical, is not implementation authority, and is not an implementation-ready plan. Merge records repository acceptance of the owner selection; it does not authorize implementation or promote this artifact into canonical product definition.

[Issue #37](https://github.com/AlexanderTsarkov/AirLink/issues/37) owns the later implementation-ready plan for the selected slice. This document is an accepted planning input to that work and may remain as a historical selection record rather than being promoted into canonical product documentation.

## Purpose

This artifact compares four reasonable first end-to-end slices and records one bounded owner selection for detailed planning before AL-0003. The selected slice connects pilot-facing behavior, runtime state, simulated source inputs, observable behavior, a retained result, and validation without a real Flight.

The comparison selects a coherent product and risk-reduction step, not the largest feature set or the lowest-effort technical foundation.

## Source Basis

This artifact uses only the following repository and task sources:

- [`ITERATION.md`](../../../ITERATION.md);
- [`CurrentState.md`](../CurrentState.md);
- canonical [`ProductVision.md`](../vision/ProductVision.md);
- the product-policy roles and alignment procedure in [`policy/README.md`](../policy/README.md) and [`ProductGovernance.md`](../policy/ProductGovernance.md);
- [`ProductDirection.md`](../policy/ProductDirection.md);
- the product and WIP indexes in [`docs/product/README.md`](../README.md) and [`wip/README.md`](README.md);
- the owner-reviewed [`MVP 0.1 Scope`](mvp-0.1-scope.md);
- the owner-approved [`MVP 0.1 Engineering Map`](mvp-0.1-engineering-map.md), including its issue #35 dependency, risk, decision-order, and sequencing extension;
- relevant boundaries from the [`Flight Mode`](flight-mode-model.md), [`Flight`](flight-model.md), and [`Navigation`](navigation-model.md) WIP documents;
- GitHub issues [#35](https://github.com/AlexanderTsarkov/AirLink/issues/35), [#36](https://github.com/AlexanderTsarkov/AirLink/issues/36), and the planning boundary in [#37](https://github.com/AlexanderTsarkov/AirLink/issues/37);
- explicit owner decisions supplied for this task.

No Google Drive, historical project, or other legacy material was used.

## Evaluation Framework

All candidates are evaluated qualitatively against the same dimensions. Ratings indicate relative fit for the first slice, not precise scores or implementation estimates.

- **Pilot-visible value:** whether the slice produces a coherent result the pilot can see and understand.
- **Critical risk reduction:** how strongly it reduces the earliest lifecycle, simulation, spatial, wind, retention, and platform risks identified by the Engineering Map.
- **Responsibility and handoff coverage:** whether it exercises normal concern ownership rather than alternative simulation-only product paths.
- **Simulation and no-real-flight validation:** whether deterministic evidence can validate the result without a real Flight.
- **Retained result:** whether the run produces a result beyond transient UI counters.
- **Dependency coverage:** whether it establishes useful, compatible boundaries for later slices.
- **Technical-decision burden:** how many material choices must be made before implementation can begin.
- **Implementation size:** the relative breadth and depth of the slice.
- **Reversibility:** whether the slice avoids prematurely fixing difficult-to-reverse product or engineering choices.

## Candidates Considered

### Candidate A — Lifecycle-led simulated Flight

A deterministic simulated movement sequence drives automatic takeoff and landing detection, an active Flight lifecycle, basic Flight metrics, minimal instrument-style presentation, and progressive in-memory recording through C9. C9 finalizes one in-memory Flight record that preserves the minimum ordered history needed for Candidate A's lifecycle and metrics scope, lifecycle boundaries, C3-supplied Flight-level `simulated` classification, required semantic status, validity, provenance and calculation context, recording completeness and outcome, and the aggregates from which Summary is derived. It includes no durable persistence, meaningful map-centered spatial experience, or early estimated-wind validation; because estimated wind is excluded, no estimated-wind history is required.

Its strength is a smaller lifecycle-focused slice. Its weakness is that it under-tests the intended spatial Flight presentation and the central estimated-wind risk.

### Candidate B — Map-centered simulated Flight

Candidate B inherits Candidate A's valid progressive C9 recording and finalized in-memory Flight-record baseline, then adds a real basemap, a pilot-centered map, compass and orientation context, and core Flight values. Its ordered history expands only as required to preserve its spatial and orientation scope. It does not meaningfully calculate or validate estimated wind and therefore need not retain estimated-wind history.

Its strength is establishment of the spatial Flight foundation. Its weakness is that it still postpones an important product and engineering risk.

### Candidate C — Simulation-driven Map Flight Core with early estimated wind

Candidate C provides one complete bounded simulated Flight from ground waiting to Summary. It includes a real map and spatial orientation, automatic takeoff and landing detection, pressure/QNH-derived altitude, derived vertical speed, core Flight metrics, an early estimated-wind calculation with simplified in-Flight presentation, and progressive in-memory recording finalized as a Flight record that preserves the minimum logical history and C3-supplied Flight-level `simulated` classification required for compatibility with later durable retention. It includes no durable persistence.

This is the owner-selected candidate.

### Candidate D — Thin preparation-to-saved-Flight journey

Candidate D spans a broader but shallower journey through preparation, Flight, progressive C9 recording, durable save, and reopening. Its durable Flight record must preserve the minimum ordered history, lifecycle boundaries, C3-supplied Flight-level classification, semantic context, recording outcome, and Summary-from-record contract applicable to its scope. It reduces depth in map behavior, simulation, detectors, and estimated wind, while requiring early storage and historical-contract decisions.

Its strength is broader lifecycle coverage. Its weakness is that it introduces difficult-to-reverse persistence and workflow decisions before the highest-risk Flight behavior is validated and spreads the first implementation effort too thinly.

## Comparison

| Evaluation dimension | Candidate A | Candidate B | Candidate C | Candidate D |
| --- | --- | --- | --- | --- |
| Pilot-visible value | **Moderate:** coherent lifecycle and metrics, but instrument-like and spatially incomplete | **High:** a recognizable map-centered Flight experience | **High:** a coherent map-centered Flight with meaningful derived information and Summary | **Moderate:** broader journey, but each Flight behavior is shallow |
| Critical risk reduction | **Moderate:** lifecycle and detector learning; little map or wind learning | **High:** lifecycle, detector, map, and orientation learning; wind remains deferred | **Very high:** combines lifecycle, simulation trust, map/orientation, derivation, and early wind evidence | **Moderate:** persistence risk is addressed early, but the highest Flight risks remain weakly exercised |
| Responsibility and handoff coverage | **Moderate:** reaches C2, C3, C4, C6, C7, C9, C10 and pilot presentation through a bounded lifecycle-and-recording path | **High:** inherits Candidate A's C9 path and adds normal C8 spatial responsibility | **Very high:** exercises the bounded lifecycle, C4/C5 input meaning, C6 detection, C7 derivation, C8 spatial context, progressive C9 recording, C10 validation, and pilot-facing flow | **Broad but shallow:** reaches more preparation, durable C9 retention, and reopening workflow without enough depth in core Flight handoffs |
| Simulation and no-real-flight validation | **High:** deterministic lifecycle evidence is straightforward | **High:** deterministic lifecycle and spatial evidence | **Very high:** deterministic lifecycle, spatial, pressure, and truth-wind comparison evidence through normal boundaries | **Moderate:** the breadth is demonstrable, but reduced simulator depth weakens evidence |
| Retained result | **Moderate:** progressive in-memory C9 recording finalized as a Flight record with Candidate A's minimum ordered history, lifecycle boundaries, C3-supplied Flight-level `simulated` classification, recording outcome, and Summary derived from the record; no durable persistence | **Moderate:** inherits Candidate A's valid retained result and adds only history required by its spatial and orientation scope; no durable persistence | **High:** progressive in-memory recording finalized as a Flight record with minimum history and C3-supplied Flight-level `simulated` classification; no durable persistence | **Very high:** progressive recording, durable save, and reopening with the minimum valid retained-result contract, at the cost of early historical and storage contracts |
| Dependency coverage | **Moderate:** useful lifecycle spine but weak spatial and wind foundation | **High:** useful lifecycle and spatial foundation | **Very high:** covers the most important compatible lifecycle, source, derivation, spatial, and validation boundaries | **Broad:** covers workflow and storage, but defers depth in risk-bearing Flight dependencies |
| Technical-decision burden | **Low to moderate:** includes only the bounded in-memory recording and retained-data decisions required by its lifecycle and metrics scope | **Moderate:** inherits Candidate A's bounded retained-data burden and adds map technology and orientation choices | **High but bounded:** adds map, detector, pressure, vertical-speed, wind, time, provenance/handling, and slice-specific retained-data decisions | **Very high:** adds durable persistence, schema, migration, historical preservation, and reopening decisions |
| Implementation size | **Smallest** | **Medium** | **Largest bounded core** | **Broadest overall journey** |
| Reversibility | **High**, but risks requiring later spatial rework | **High**, provided orientation remains experimental | **High:** no durable schema; algorithms, provider, framework, and detailed UI remain replaceable | **Lower:** early durable contracts and workflow decisions are costly to revise |

Candidates A and B are valid retained-result slices rather than disposable Summary-only alternatives. Candidate A is smaller, but it does not reduce enough of the map and wind risk. Candidate B establishes the spatial foundation, but still postpones estimated-wind learning. Candidate D covers more of the product journey, but introduces premature durable-persistence, historical-contract, and workflow decisions while diluting attention across the core Flight behavior.

Candidate C is preferred because it offers the best balance of coherent pilot-visible value, lifecycle validation, simulation trust, map and orientation learning, early wind-risk reduction, and reversibility. Its selection is not based on containing more features: its additional scope is concentrated on mutually dependent Flight behavior that can be validated together, while durable storage and broader workflow remain deliberately absent.

## Owner Selection

The owner selects Candidate C:

> **Simulation-driven Map Flight Core with early estimated wind.**

The selection is accepted with explicit simplification. It selects the first implementation-slice candidate for detailed planning; it does not authorize implementation. Issue #37 must turn this selection into an implementation-ready plan.

Issue #36 selects no application framework, map provider, whole-product technical strategy, final architecture, exact algorithm, threshold, implementation contract, or detailed UI. Issue #37 may prepare only the bounded choices needed to make this slice implementation-ready, subject to the approval boundaries stated below.

## Selected Slice Outcome

The selected end-to-end user-observable result is:

1. A development build starts through a temporary entry path.
2. One bundled, read-only simulation scenario is loaded automatically.
3. A simulated Flight Mode context is created outside the Flight Screen.
4. The Flight Screen opens in `Ready on Ground`.
5. No Flight exists yet.
6. The user starts simulation input progression.
7. Normal source-equivalent data changes over time.
8. C6 confirms takeoff from normal inputs rather than a simulator-declared lifecycle event, distinguishing confirmation time from its estimated effective takeoff boundary, and C2 authorizes C3 to create the Flight.
9. C3 establishes the Flight identity, C3-supplied Flight-level `simulated` classification, authoritative effective takeoff boundary, and Takeoff Point identity, estimated location, and Flight association, then supplies that creation context to C8 and C9; C8 establishes Takeoff Point as the passive Current Waypoint while Active Navigation remains off.
10. As part of that handoff, C9 performs in-memory recording initialization before the first ordinary active-Flight update expected to be retained. Any initialized recording starts with the authoritative C3 context, and the successful, degraded, or failed initialization outcome is observable.
11. The map and Flight information update during the simulated Flight, or C8 exposes a pilot-visible spatial unavailable or degraded state while the non-map Flight path continues; C9 progressively retains the approved C3/C4/C7 history while exposing recording health, completeness, and outcome.
12. Estimated wind becomes available after sufficient data exists.
13. Landing is detected from normal inputs.
14. C3 completes the individual Flight through confirmed landing, creates the confirmed Landing Point, and supplies the required Landing Point information to C9; recording preserves the final segment through that boundary, C2 returns to `Ready on Ground`, and Flight Mode remains active.
15. C9 exposes the finalization outcome as it finalizes the progressive recording as the in-memory Flight record for the current run.
16. A successful Flight Summary is shown inside the continuing Flight Mode flow from the complete finalized Flight record; an incomplete, degraded, unavailable, or failed recording outcome is exposed rather than presented as successful retention.
17. No second Flight can begin in the same first-slice development session. After the completed-Flight outcome, the user can Reset, discard the current in-memory record, and repeat the scenario in a fresh development session.

This outcome defines observable behavior without selecting screen pixels, technical components, APIs, or detector thresholds.

## Temporary Development Entry

The following constraints are accepted for this slice:

- the app may temporarily open directly into the Flight Screen;
- this is a development shortcut, not the permanent AirLink startup model;
- Flight Screen is not Home, Pre-Flight, or the permanent application root;
- Flight Screen does not load the simulation scenario itself;
- Flight Screen receives an already prepared simulated Flight Mode context;
- opening the screen does not start a Flight;
- `Start simulation` starts source-input progression only;
- a pilot-facing explanation of the estimated nature and limitations of in-Flight wind information is available in a bounded non-flight context while no Flight is active; issue #37 selects the exact location and presentation mechanics;
- Reset is available before an active Flight exists and after a completed-Flight outcome, but is unavailable while a Flight is active;
- Reset tears down the non-active development session and creates a fresh run; it is not Flight interruption, manual completion, false-detection discard, Flight Mode exit, confirmed landing, or another lifecycle outcome;
- after confirmed landing, C2 is `Ready on Ground`, Flight Mode remains active, and Summary appears inside that continuing flow until Reset creates the fresh development session;
- future Home and Pre-Flight flows must be able to enter the same Flight flow without redefining its internal semantics;
- an empty Home, complete navigation graph, coordinator framework, or workflow engine is not required.

## Simulation Scenario

The slice uses exactly one bundled scenario. It is authored outside the application as a declarative, versioned, read-only data asset and is loaded automatically. There is no scenario selector, editor, remote distribution, or scenario-specific branching embedded in simulator code.

The scenario is deterministic and phase-based. It may include ground waiting, acceleration, a takeoff profile, climb, level segments, relative turns, descent, final approach, landing, ground stop, deterministic Heading variation, truth wind, initial truth conditions, and fixed initial map zoom. Scenario phase, flight-pattern template, truth trajectory, truth airspeed, air-relative Heading used as truth, generated ground-vector truth, and other scenario-only values remain privileged C10 control or truth metadata rather than product inputs.

Initial Heading is derived relative to truth wind so takeoff occurs into wind. The relative maneuver profile rotates with the initial Heading, and the final direction may also be into wind. Exact return to the start point is not required. This introduces neither an autopilot nor a navigation controller.

Issue #36 does not select the final scenario file format.

## Source-Equivalent Simulation Boundary

The selected slice preserves the Engineering Map responsibility boundary. C10 — Simulation and Validation Enablement owns scenario state and controlled substitute production, but does not become an alternative lifecycle, detector, calculation system, spatial model, recorder, or product flow.

The simulator may emit normal-source equivalents through the normal C4 — Input Acquisition and Validity or C5 — Weather Context boundaries, including:

- wall-clock time where civil timestamp meaning is required;
- monotonic time for controlled progression, ordering, durations, elapsed Flight time, detector windows, and retained-history ordering;
- controlled platform interruption and, where the bounded validation case includes it, restoration state;
- position;
- Ground Speed source information;
- Track;
- atmospheric pressure;
- source validity, availability, freshness, and provenance;
- simulated weather QNH;
- simulated weather wind.

For the selected pressure path, the simulator supplies atmospheric pressure rather than final pilot-facing altitude. Simulated weather supplies QNH. AirLink calculates barometric altitude and derives vertical speed from altitude history through normal C7 — Flight Information Derivation responsibility.

C4 owns the normalized clock semantics exposed to consumers. C10 may produce the controlled simulated equivalent required for deterministic progression, but it does so through the normal C4 boundary. Source time and AirLink-observed time remain distinct wherever their difference affects validity, freshness, ordering, diagnosis, or retained meaning.

At least one separate validation case must expose a controlled interruption while the simulated Flight is active. C10 or the validation harness may initiate the condition, but it enters AirLink through the normal C4 platform/input boundary. C4 exposes the interruption and, where included in the bounded case, restoration to the affected normal concerns. C10 does not directly mutate C2 or C3 lifecycle state or C9 recording meaning. The conceptual path is:

`controlled interruption condition → normal C4 platform/input boundary → affected normal concerns → observable C3 and C9 state or outcome`

The normal successful user-observable scenario may remain uninterrupted. The separate case may reuse the bundled scenario as a controlled variation, test, or developer-validation run; it requires neither another bundled scenario nor a product-facing simulator mode or expanded diagnostics UI.

The slice preserves three independent axes:

1. each AirLink-facing runtime value has live, selected, or simulated category-level provenance;
2. when C10 governs a category, its requested handling is pass-through or controlled substitute;
3. C3 supplies Flight-level `simulated` classification from the active C10 run context when the Flight is created.

Provenance and handling must not be inferred from one another. Pass-through preserves upstream provenance. C10 owns requested run configuration and controlled substitute production; C4 and C5 own and expose the actually active provenance, handling, availability, validity, freshness, and source outcome within their boundaries. Flight-level classification is not inferred from individual runtime-value provenance or handling composition, and C9 preserves the C3-supplied classification without reclassification. Retained history preserves provenance and separately relevant handling context wherever each is required for historical meaning. This artifact does not select a full provenance schema or technical representation.

The simulator must not emit:

- takeoff or landing detected;
- Flight started or completed;
- Flight elapsed time or flown distance;
- calculated altitude as an AirLink result;
- vertical speed as an AirLink result;
- Takeoff Point or Landing Point;
- estimated wind;
- Flight Summary values.

All privileged C10 scenario truth and control metadata—including truth wind, truth airspeed, air-relative Heading used as scenario truth, scenario phase, the flight-pattern template, truth trajectory, generated ground-vector truth, initial truth conditions, and any other scenario-only value—may be used only to generate approved normal source-equivalent runtime inputs, for independent validation comparison, or for bounded development observability. None may be consumed directly by C6, C7, C8, C9, or pilot-facing product logic.

If an additional value is required as a real runtime input, issue #37 must identify it and obtain the required approval for the selected slice. C4-owned values, including Heading-related and airspeed-related values, must enter through the normal C4 boundary. Controlled weather equivalents must continue through the normal C5 boundary. Any other approved source-equivalent must enter through the applicable accepted C4 or C5 boundary. The applicable boundary owns and exposes availability, validity, timing, provenance, handling, and source outcome, and downstream consumers must not receive a privileged scenario-truth identity. C7 estimated wind uses only the approved normal-boundary runtime inputs available through the selected product path. Truth values may be compared with C7 output only after independent estimation, preserving the Engineering Map relationship:

`scenario truth wind → C10-generated approved source equivalents → normal C4/C5 boundaries → independent C7 estimated wind → validation comparison`

Concern identifiers are planning references only. They do not select modules, services, classes, or architecture.

## Time Semantics

The selected slice uses monotonic time for simulation progression, sample and retained-history ordering, durations, elapsed Flight time, detector windows, and other interval behavior. Wall-clock time is used only where civil timestamp meaning is required. Source time and AirLink-observed time remain distinguishable under the Source-Equivalent Simulation Boundary when their difference affects meaning.

The following invariants apply:

- wall-clock adjustment must not alter Flight duration, detector windows, or retained-sample ordering;
- Pause freezes only C10-controlled simulated progression and does not become a Flight lifecycle action;
- `1×` and `2×` change simulated progression rate, not wall-clock semantics;
- a monotonic discontinuity, unavailable monotonic source, or invalid clock state is explicit and must not silently produce a valid duration, detector result, or retained ordering;
- C4 owns normalized clock semantics for consumers, while C10 supplies only the controlled simulated equivalent needed for deterministic progression through C4.

This selection does not choose timestamp data types, clock APIs, timer libraries, durable timestamp schema, or exact discontinuity-recovery behavior.

## Flight Lifecycle

The first detector is bounded and experimental, not the final production detector. C6 — Flight Detection infers takeoff and landing from normal simulated inputs; C10 does not send lifecycle events.

- before confirmed takeoff, Flight Mode remains in ground waiting and no Flight exists;
- after C6 confirms takeoff, C2 — Flight Mode Lifecycle authorizes C3 — Flight Lifecycle and Flight State to begin an active simulated Flight;
- C3 establishes the Flight identity, C3-supplied Flight-level `simulated` classification, authoritative effective takeoff boundary, and Takeoff Point identity, estimated location, and association with the active Flight, and supplies that creation context to C8 and C9; C8 establishes Takeoff Point as the passive Current Waypoint while Active Navigation remains off;
- as part of the same creation handoff, C9 performs in-memory recording initialization before the first ordinary active-Flight update expected to be retained; any initialized recording starts with that authoritative context, and the successful, degraded, or failed initialization outcome is observable;
- after confirmed landing, C2 authorizes C3 to complete the individual Flight through the confirmed boundary; C3 logically creates the confirmed Landing Point, owns its identity, estimated location, confirmed-landing classification, and association with the completed Flight, and supplies that information to C9; C2 returns to `Ready on Ground`, Flight Mode remains active, and Summary appears inside the continuing Flight Mode flow;
- C9 retains all approved information through confirmed landing, including the final segment, and exposes the outcome when finalizing the in-memory Flight record;
- recording degradation or failure does not cancel confirmed landing, reactivate the Flight, or convert it into a rejected or nonexistent Flight;
- the first-slice development harness does not begin a second Flight in the same development session;
- Reset is available only before an active Flight exists or after a completed-Flight outcome, never while a Flight is active;
- Reset tears down a non-active development session and creates a fresh run; after completion it may discard the current in-memory Flight record because durable persistence is outside the slice;
- Reset is not Flight interruption, manual completion, false-detection discard, Flight Mode exit, confirmed landing, or another Flight lifecycle outcome.

The one-Flight-per-development-session restriction is an explicit first-slice harness simplification only. It does not redefine the accepted broader rule that one Flight Mode may contain multiple independent Flights, and completing the Flight does not automatically exit Flight Mode. Repeating the scenario requires Reset after the completed outcome and therefore starts a fresh development session.

The separate interruption validation case stops at the unresolved P3 boundary. When C4 exposes interruption, C3 exposes that the active Flight encountered that boundary without assigning the final product classification, and C9 exposes the resulting recording gap, degradation, incompleteness, or other bounded technical recording outcome without deciding whether the episode is ultimately retained as a Flight. C2, C3, C4, and C9 keep their normal ownership. Interruption or restoration must not silently become confirmed landing, manual completion, a rejected Flight, false-detection discard, Flight Mode exit, successful recording continuity, or valid uninterrupted source data. Restoration observability, when included, does not establish a continuation guarantee or final recovery behavior.

C6's takeoff confirmation time, its estimated effective takeoff boundary, and the bounded recent pre-confirmation history are distinct. If the effective boundary is earlier than confirmation, C9 incorporates only the approved retained categories from that boundary through confirmation as part of initialization. Inputs before the effective boundary and any unused pre-confirmation history remain transient and are overwritten; they do not create a hidden Flight record before C3 creates the Flight. This selection does not choose detector thresholds or windows, the detector algorithm, history-buffer duration or custody, the exact effective sample, or final P2 semantics. Issue #37 must plan the bounded experimental mechanism, ownership, approved retained categories, and acceptance evidence without claiming final production semantics.

Pilot-facing detector state may remain limited to waiting for takeoff, active Flight, an optional brief landing-confirmation state, and completed Summary. Detailed diagnostics, thresholds, final detection semantics, and the final algorithm belong to issue #37 or authorized implementation work.

## Map and Orientation

The selected slice includes:

- a real geographic basemap;
- a centered pilot marker;
- fixed zoom, with the exact zoom and area selected during implementation planning;
- North-up presentation while on the ground before valid movement;
- Track-up presentation after valid Track is available;
- a compass or orientation indication that keeps True North understandable;
- explicit semantic distinction between Heading and Track;
- the logical C8 state in which Takeoff Point is the passive Current Waypoint after confirmed takeoff while Active Navigation remains off.

Track-up is a reversible first-slice behavior, not resolution of the final orientation policy. A real Android magnetic compass, pan, user zoom controls, map-layer selection, and offline-map scope are excluded.

The logical Current Waypoint state is included, but pilot-facing special-point navigation presentation is excluded. The slice therefore excludes the actual flown-track line, zero-wind reference path, Takeoff Point marker, Landing Point marker, map presentation of Landing Point, distance or bearing to Takeoff Point, and other visual passive-navigation presentation. Because that presentation is excluded, the slice does not require C7 Takeoff Point distance or bearing calculations. The logical state is not a Route, route guidance, Route Navigation, or Active Navigation.

C8 must expose an explicit pilot-visible spatial unavailable or degraded state when map tiles, the provider, rendering, or coverage is unavailable, or when orientation input or valid Track is unavailable or invalid. Map or orientation degradation must not end Flight Mode, create or complete a Flight, change confirmed takeoff or landing, stop C6 detection, redefine C3 lifecycle, stop C7 derivation where its required non-map inputs remain valid, stop C9 recording, or prevent the completed-Flight outcome or Summary. No map or orientation provider becomes lifecycle, calculation, detection, or recording authority.

Offline maps, cached-map architecture, provider redundancy, final fallback visual design, and user map-layer controls remain outside the slice.

## Ground Presentation

Before takeoff, the screen presents only values with current meaning:

- current map position;
- barometric altitude;
- weather-source wind;
- compact simulation controls.

Weather-source wind occupies the primary top-row location that later displays Flight speed. Presentation distinguishes unavailable, valid zero, and—where supported—available but stale, degraded, or uncertain states. Zero must not substitute for unavailable data.

## Estimated-Wind Limitation Explanation

The first slice must provide a pilot-facing explanation in a bounded non-flight context, such as the temporary development entry, `Ready on Ground` before simulation progression begins, a compact help or information presentation reachable while no Flight is active, or another bounded non-flight location selected by issue #37. Developer documentation, logs, or test output alone do not satisfy this requirement.

The explanation communicates that the in-Flight wind value is an estimate and that short-term changes cannot be reliably separated from pilot input, climb or descent, changes in wing behavior or configuration, turbulence, or actual wind variation or gusts. It also states that in-Flight gust estimation is not part of MVP 0.1. The value must not be presented as more authoritative or precise than those accepted semantics allow.

This explanation does not require a persistent static warning on the active Flight Screen and must not compete permanently with the in-Flight wind presentation. Exact copy, layout, control, interaction, and bounded location are implementation-ready work for issue #37; the requirement to provide the explanation is fixed here.

## Active Flight Information

During the simulated Flight, the minimum presented information is:

- Ground Speed in km/h;
- barometric altitude in m;
- vertical speed in m/s;
- elapsed Flight time;
- flown distance in metric units;
- estimated wind speed in m/s;
- estimated wind direction;
- current map and orientation context.

Exact typography, hierarchy, and layout remain outside issue #36.

## Weather Wind and Estimated Wind

On the ground, weather-source wind is shown in the upper primary-information area instead of Flight speed. During Flight, estimated wind is presented within the compass or orientation context only after the estimator has a valid result. Before that, the UI may show that estimation is unavailable or warming up.

Weather-source wind and estimated wind remain distinguishable by meaning and provenance. Issue #36 does not decide whether a future estimator may use weather information as a prior or fusion input.

The active-Flight presentation must not imply that estimated wind is direct wind truth, that it includes gust estimation, or that short-term changes have a more authoritative interpretation than the non-flight explanation allows. A persistent active-Flight warning is not required.

## Simplified Wind Presentation

The first slice should attempt a simplified windsock-like presentation inside the compass area:

- use the visual analogy of an aerodrome windsock;
- orient it so the into-wind landing direction is intuitively understandable;
- use visible length or sections to communicate wind speed;
- retain a visible numeric value;
- attempt approximately `0.5 m/s` display granularity.

Final geometry, section rendering, gradients, safety thresholds, blinking, warning policy, placement, and dimensions are not fixed. The attempted display granularity is not a claim of calculation accuracy or authority. The purpose is comprehension learning, not approval of the final Flight Screen design.

## Compact Simulation Panel

Only a compact panel is included. It contains:

- Start/Pause;
- Reset, enabled only before an active Flight exists or after a completed-Flight outcome;
- `1×` and `2×` speed;
- current simulation phase;
- simulation elapsed time.

Use `Pause`, not product-semantic `Stop`. Pause freezes C10-controlled monotonic simulation progression without completing, interrupting, rejecting, or otherwise changing the Flight lifecycle. `1×` and `2×` affect that simulated progression only. Reset is unavailable during an active Flight and never acts as an active-Flight lifecycle control. The panel occupies the lower area beneath the compass that may later serve other Flight or Route presentation. An expanded diagnostics overlay is out of scope because its information architecture and placement require separate design.

## Validation Observability

The slice includes sufficient bounded observability to prove that normal concern ownership and handoffs are used. Tests, logs, bounded developer output, or another replaceable mechanism chosen by issue #37 must make the following observable:

- active C10 run state, scenario phase, and controlled progression;
- C10-requested provenance and handling;
- C4/C5 actually active provenance, handling, availability, validity, and freshness;
- controlled interruption initiation and its entry through the normal C4 platform/input boundary;
- C4 interruption identity, observed time, availability effect, and restoration state where the bounded case includes restoration;
- wall-clock and monotonic semantics where relevant;
- C6 takeoff and landing candidate and confirmed outcomes, including distinct takeoff confirmation time and estimated effective takeoff boundary;
- C2 authorization outcomes reached by the slice;
- C3 Flight lifecycle transitions and the creation context supplied to C9, including Flight identity, Flight-level classification, authoritative effective takeoff boundary, and Takeoff Point information;
- C7 estimator inputs received through normal boundaries, estimated-wind output, validity, and quality state;
- independent comparison between C7 estimated wind and the C10 truth oracle after estimation;
- C8 normal versus unavailable or degraded outcome;
- C9 initialization before the first ordinary retainable active-Flight update, successful, degraded, or failed initialization outcome, approved bounded recent-history incorporation or discard outcome, progressive append or retention, completeness, finalization, and retention outcome;
- C2/C3 state at the unresolved interruption boundary and C9 recording-gap, degradation, incompleteness, or other bounded technical outcome, with evidence that interruption was not converted into another lifecycle or successful-retention result;
- Summary derivation from the finalized Flight record.

Scenario phase and other privileged truth or control metadata may be observed for validation but do not become product inputs or alternative concern authority. This observability is validation support rather than product UI. It does not require an expanded on-screen diagnostics overlay, production telemetry platform, cloud logging, analytics, full replay tooling, or final observability architecture.

## Retained Result and Flight Summary

The retained result is not a Summary-only object. After C6 confirms takeoff and C2 authorizes creation, C3 supplies C9 with the Flight identity, C3-supplied Flight-level `simulated` classification, authoritative effective takeoff boundary, and Takeoff Point identity, estimated location, and Flight association. C9 performs recording initialization as part of that creation handoff, before the first ordinary active-Flight update expected to be retained. Any initialized recording begins with those authoritative values; a degraded or failed attempt to do so remains observable. During the active Flight, selected historical information passes through the accepted C3/C4/C7 → C9 responsibility boundary. A minimal C9 implementation progressively retains that information in memory; it does not become a database, storage engine, or complete durable-retention subsystem. C9 owns recording health, completeness, and the in-memory retention outcome and exposes the outcomes of recording initialization, progressive append or retention, and finalization to C3 and the pilot-facing flow where they affect the completed-Flight result. Independent UI counters must not replace this recording path.

After confirmed landing, C9 finalizes the progressive recording into one in-memory Flight record for the current run, retaining all approved information through the confirmed-landing boundary. Recording must not stop at the first landing candidate, trim history retrospectively to approximate an earlier landing point, or omit the final segment between landing-detection activity and confirmed landing. That record preserves at least these logical categories:

- temporary Flight identity within the current application run;
- Flight lifecycle boundaries;
- takeoff confirmation time and the authoritative effective takeoff boundary as distinct meanings;
- the authoritative confirmed-landing completion status applicable to this slice;
- the C3-supplied Flight-level `simulated` classification;
- Takeoff Point identity, estimated location, and Flight association;
- confirmed Landing Point identity, estimated location, confirmed-landing classification, and Flight association;
- C9 recording health, completeness, and in-memory retention outcome;
- an ordered time-varying history sufficient to represent and validate the selected slice;
- monotonic ordering and duration semantics, plus wall-clock timestamps only where civil meaning is required;
- semantic status of retained values where meaning requires it;
- availability and validity information where meaning requires it;
- source and derivation provenance where meaning requires it, kept distinct from separately relevant pass-through or controlled-substitute handling context;
- calculation context required to interpret historically retained derived values;
- final Flight aggregates used by Summary.

If the effective takeoff boundary precedes confirmation, the initialized record incorporates the approved retained categories available in bounded recent history from that boundary through confirmation. Information before the effective boundary and unused pre-confirmation history remain transient, are overwritten, and never constitute a hidden Flight record. The bounded history exists only to support the experimental retrospective boundary; it does not authorize retention of unapproved categories.

The ordered time-varying history must be sufficient to preserve the selected slice's source and derived Flight behavior through confirmed landing, including its final segment, and validate position and movement; Ground Speed and Track; atmospheric pressure and relevant QNH context; calculated barometric altitude; derived vertical speed; estimated wind; and lifecycle and timing boundaries. Retained ordering and durations use monotonic semantics and cannot be rewritten by wall-clock adjustment. Runtime-value provenance and separately relevant handling remain distinct from Flight-level `simulated` classification; C9 preserves the classification supplied by C3 without deriving or reclassifying it from recorded source composition. This does not require retaining every raw runtime value. This artifact selects neither an exact schema nor an exhaustive retained parameter list. The exact retained parameters, sampling frequency, history-reduction rules, downsampling, compression, buffering, custody, and representation are implementation-ready work for issue #37 within the fixed selection boundary.

The Flight Summary is not the retained Flight record. It appears after confirmed landing while C2 is `Ready on Ground` and Flight Mode remains active. Its aggregates come from the finalized in-memory Flight record rather than independent UI-owned counters, and the completed-Flight presentation preserves the record's C3-supplied Flight-level `simulated` classification, authoritative confirmed-landing completion status, and C9 recording health, completeness, and retention outcome. A missing, incomplete, degraded, or failed record must not be presented as successfully retained, and Summary must not mask recording failure or synthesize a successful result from independent UI counters. Where no complete finalized record exists, the pilot-facing completed-Flight flow exposes that the record is incomplete, degraded, unavailable, or failed as applicable; the exact presentation remains deferred.

Recording failure does not redefine lifecycle truth: it does not cancel confirmed landing, make the Flight active again, or convert the Flight into a rejected or nonexistent Flight.

The finalization and Summary requirements above describe the normal confirmed-landing path. In the separate interruption validation case, C9 exposes the technical recording consequence but does not decide whether the interrupted episode is retained, completed, rejected, discarded, deleted, restored, or assigned a Summary. Those product outcomes and any recovery meaning remain unresolved under P3.

The minimum Summary contains:

- Flight duration;
- flown distance;
- average Ground Speed;
- maximum Ground Speed;
- maximum altitude.

The in-memory Flight record remains available for the current completed run and Summary. After the completed-Flight outcome, Reset may discard it while tearing down the non-active development session and creating a fresh run. Reset cannot discard it during an active Flight because Reset is unavailable then. The record does not survive application-process termination and is not written to a database, file, or durable store.

No durable persistence is included. The selected slice has no database or storage engine, durable schema, migrations, durable Flight identifier, saved Flight list, reopening after restart, deletion of durable records, saved-Flight review, interruption recovery, or production storage health and recovery behavior.

## Platform Direction

AirLink's intended mobile product supports Android and iOS. Initial implementation and early platform validation are Android-first, while first-slice product semantics and non-platform logic must not be Android-specific. The first slice preserves the platform-interruption and source-availability boundary through C4 without implementing production Android lifecycle, foreground/background execution, process recovery, or continuity behavior. Concrete iOS integration is deferred.

Issue #36 selects no application framework, code-sharing strategy, map technology, or map provider. This artifact does not choose Flutter, two native applications, a whole-product framework or sharing strategy, a permanent map-provider standard, or a complete application architecture. Issue #37 may prepare and obtain owner approval only for the bounded application/framework and map-technology/provider choices demonstrably necessary for this slice; those choices do not establish a whole-product strategy or permanent standard.

## First-Slice Explicit Non-Scope

The selected slice does not include:

- Home, full Pre-Flight, or full onboarding;
- permanent application navigation, a settings system, a modal framework, or a complete help system;
- multiple scenarios, scenario selection, an editor, or remote scenarios;
- live Android GNSS, Android compass, or real Android barometer integration;
- iOS platform integration;
- live Android permission integration;
- manual Heading control;
- Route, Route waypoints, waypoint carousel, Route progress, Route Navigation, or Active Navigation; the included passive Current Waypoint remains a distinct logical C8 state;
- fuel model, fuel gauge, or user settings;
- map pan, user zoom controls, map-layer selection, or offline maps;
- actual flown track or zero-wind reference path;
- Takeoff Point or Landing Point markers or map presentation of Landing Point;
- C7 distance or bearing calculations to Takeoff Point;
- pilot-facing passive Takeoff Point navigation presentation;
- saved-Flight spatial review;
- final special-point storage representation or manual-completion Landing Point semantics;
- durable persistence, including a database or storage engine, durable schema or migrations, durable Flight identifier, saved Flight list or review, reopening after restart, deletion of durable records, persistence across process termination, or production storage health and recovery;
- a second Flight in the same first-slice development session; the accepted broader capability for multiple independent Flights within one Flight Mode is unchanged;
- Reset during an active Flight or any interpretation of Reset as lifecycle behavior;
- active-Flight exit behavior, manual completion, or false-detection discard;
- real Android process-kill recovery, background-execution implementation, production Android lifecycle mechanisms, checkpointing, active-Flight restoration after process death, continuity guarantees, retry policy, or recovery architecture;
- durable interrupted-Flight recovery or any final retained outcome or Summary classification for an interrupted Flight, including automatic completion, rejection, discard, deletion, or restoration;
- a persistent static in-Flight estimated-wind warning; the required explanation is instead available in a bounded non-flight context;
- in-Flight gust estimation;
- final takeoff or landing algorithm;
- final wind algorithm;
- final Flight Screen design;
- expanded diagnostics overlay;
- a complete application architecture;
- an application-framework or map-provider selection by issue #36, a whole-product framework or code-sharing strategy, or a permanent map-provider standard; issue #37 may prepare only the bounded choices required for this slice, subject to approval, without establishing those broader commitments.

These exclusions are explicit first-slice simplifications, not changes to the broader accepted MVP 0.1 boundary or permanent rejection of future behavior. Excluding special-point markers and navigation presentation does not exclude C3's logical creation of the Takeoff Point and confirmed Landing Point, the C3 → C8 handoff and passive Current Waypoint state, or C9's in-memory retention of the required special-point information. Observable interruption and optional restoration boundaries are included only through the bounded C4/C3/C9 validation path; recovery, continuation guarantees, and final interrupted-Flight product classification remain excluded. The Reset decision does not resolve P1 active-Flight exit or P3 interruption semantics, and this slice does not silently resolve P2, P4, P5, or P6 beyond the explicitly selected experimental and non-scope boundaries.

## Implementation-Ready Decisions and Contracts for Issue #37

Issue #36 fixes the following selection-level semantics and boundaries:

- Candidate C is selected, while Candidates A and B remain valid progressive-recording and retained-result slices rather than Summary-only alternatives;
- Reset availability and meaning, post-landing `Ready on Ground` inside continuing Flight Mode, and the one-Flight-per-development-session harness restriction are fixed as stated above;
- the C3 → C8 Takeoff Point handoff and passive Current Waypoint state are included while Active Navigation remains off;
- the wall-clock, monotonic-time, source-time, and AirLink-observed-time distinctions and invariants are fixed;
- privileged C10 truth remains isolated from product inputs and downstream product logic;
- runtime-value provenance, requested and active handling, and C3-supplied Flight-level classification remain independent;
- C8 degradation remains explicit and cannot redefine or stop the non-map Flight path;
- at least one controlled active-Flight interruption validation case crosses the normal C4 boundary and exposes C3 and C9 state or outcomes without assigning final P3 meaning;
- C9 initializes through the takeoff-side C3 handoff, progressively records in memory, retains the approved effective-boundary-to-confirmation history when that boundary precedes confirmation and the final segment through confirmed landing, and exposes initialization, append or retention, completeness, health, finalization, and outcome;
- a pilot-facing explanation of the estimated nature and accepted limitations of in-Flight wind is required in a bounded non-flight context, while a persistent active-Flight warning is not required;
- Summary is derived from the finalized Flight record and cannot mask an incomplete, degraded, unavailable, or failed recording outcome;
- the selected slice includes no durable persistence, and issue #36 does not authorize product implementation.

Issue #37 must not reselect Candidate C or silently revise these semantics unless the owner explicitly reopens them through a separate authorized decision. It converts the fixed selection into an implementation-ready plan by defining slice contracts, bounded and replaceable mechanisms, acceptance cases and evidence, implementation decomposition, and any approved material technical choices necessary for this slice.

Within those fixed constraints, issue #37 must define or obtain approval for, as applicable:

- the bounded application or framework choice demonstrably required for this slice, without selecting a whole-product framework or code-sharing strategy;
- the bounded map technology or provider choice demonstrably required for this slice, without establishing a permanent provider standard;
- exact scenario asset format;
- scenario values and phase durations;
- the minimum clock contract, including C4-normalized wall-clock, monotonic, source-time, and AirLink-observed-time semantics needed by the slice;
- acceptance coverage for wall-clock adjustment, monotonic discontinuity or invalidity, and tests proving duration, detector windows, and retained ordering do not depend on mutable wall clock;
- the bounded experimental takeoff and landing mechanism, including detector thresholds and windows, recent-history ownership and custody, buffer duration, effective-sample choice, approved retained categories, and acceptance evidence, without claiming final detector or P2 semantics;
- pressure-to-altitude calculation details;
- vertical-speed filtering;
- the first wind-estimation method and its explicitly approved normal-boundary runtime inputs, without granting C7 access to privileged C10 truth or control metadata;
- tests proving the estimator has no privileged truth/control access, deterministic validation reaches it through normal C4/C5 boundaries, and truth remains an oracle rather than an estimator input;
- the bounded non-flight location, minimum pilot-facing wording consistent with the MVP 0.1 Scope, and exact presentation mechanics for the estimated-wind limitation explanation, plus acceptance evidence that it is available while no Flight is active and that active-Flight presentation implies neither gust estimation nor authoritative wind truth;
- slice-specific runtime contracts and handoffs within the accepted Engineering Map concern boundaries, including C3 → C8 Takeoff Point identity, location, Flight association, and passive Current Waypoint state;
- the slice-specific contract and tests that keep runtime-value provenance, C10-requested handling, C4/C5 actually active handling, and C3-supplied Flight-level classification independent without defining a full provenance schema;
- the minimum logical retained-data contract for the slice, including exact retained parameters, sampling or history-reduction rules, representation of semantic status, validity, provenance, separately relevant handling, clock semantics, and retained calculation or version context;
- the C3 → C9 creation and initialization contract, including the authoritative values supplied before the first ordinary retainable active-Flight update and incorporation of only approved retained categories from the effective takeoff boundary through confirmation;
- exact bounded outcomes for successful, degraded, or failed C9 recording initialization, progressive append or retention, and finalization; observable recording health and completeness states; and the C9-to-C3 and pilot-facing handoffs for those outcomes;
- acceptance cases for successful, degraded or incomplete, and failed in-memory recording, including completed-Flight presentation when no complete finalized record exists;
- the exact logical representation of Takeoff Point and confirmed Landing Point within the in-memory record and the C3-to-C9 handoffs required to preserve them;
- progressive in-memory C9 recording and finalization behavior, Flight-record lifetime and Reset behavior, and tests proving that the final segment through confirmed landing is retained;
- compact simulation-panel behavior, including Reset availability only before an active Flight or after a completed outcome, post-landing `Ready on Ground` within continuing Flight Mode, and the no-second-Flight development-session simplification;
- the minimum C8 unavailable or degraded presentation and acceptance cases for basemap, rendering, coverage, orientation-input, and Track degradation, with tests proving that the non-map Flight path continues independently;
- the minimum controlled active-Flight interruption validation case, how it reaches C4 through the normal validation path, and the C4 interruption and optional restoration contract;
- the observable C3 and C9 handoffs, recording-gap, degradation, or incompleteness evidence, and acceptance proof that interruption is not converted into landing, completion, rejection, discard, Flight Mode exit, successful recording continuity, or valid uninterrupted input;
- the explicit stop boundary before P3, without deciding whether the interrupted episode is retained, completed, rejected, discarded, restored, assigned a Summary, or otherwise given final recovery behavior;
- the minimum replaceable validation-observability realization and acceptance evidence for the concern states, handoffs, inputs, outputs, comparisons, degradation, recording outcomes, and Summary derivation selected above;
- the exact Summary contract and tests proving that Summary aggregates come from the finalized Flight record and preserve Flight-level `simulated` classification, authoritative confirmed-landing completion status, and C9 recording status;
- test strategy and acceptance cases;
- implementation decomposition.

This artifact fixes the selection-level constraints above but intentionally does not choose their exact implementation mechanisms or representations. Issue #37 must preserve the Engineering Map's decision classes and stop at any owner-controlled semantic or difficult-to-reverse technical boundary that needs separate approval. In particular, issue #37 must not decide final P3 retention, completion, rejection, discard, restoration, Summary, or recovery meaning for an interrupted episode. It must not select a whole-product framework or code-sharing strategy, permanent map-provider standard, production database, durable schema, migration strategy, complete persistence architecture, final timestamp representation, final discontinuity-recovery behavior, offline-map architecture, provider redundancy, or final map fallback design unless separately authorized.

## Candidate Follow-Up Slices

### Trajectory-context candidate

A possible later candidate group may add:

- actual flown track;
- scenario zero-wind reference path;
- Takeoff Point marker;
- Landing Point marker;
- visual comparison between expected zero-wind movement and actual movement.

This group is a candidate only. It is not selected, committed, or ordered and may be split, reordered, changed, or rejected based on evidence from the first slice. The zero-wind reference path is simulation context, not an AirLink Route.

## Future Flight Screen Direction Informing the Selection

The following owner-provided context is directional only. It is not final design, first-slice scope, or implementation requirements.

1. **Camera-area metric zone:** a compact rotating indication may later show elapsed Flight time, flown distance, or Route completion when a Route exists.
2. **Primary upper information row:** may combine Flight speed, altitude, and current navigation context; on the ground, weather wind may occupy the speed position; without a Route, Takeoff Point may later provide navigation context.
3. **Left-side scale:** vertical speed may be centered at zero, with climb upward, descent downward, and color-supported peripheral reading.
4. **Right-side scale:** fuel may be shown relative to tank capacity, current fuel, configured reserve, and reserve translated into estimated remaining Flight time.
5. **Compass and navigation bugs:** may represent Track, Bearing, and navigation-direction assistance while preserving their distinct meanings.
6. **Central in-Flight wind representation:** may use the aerodrome-windsock analogy so direction and magnitude remain visible in Flight context, with sectioned magnitude and future experiments in geometry and safety indication.
7. **Conditional Route presentation:** may include a waypoint carousel, passed and upcoming waypoints, separate current-navigation values and leg metadata, and Route progress.

The selected first slice intentionally implements only the map and orientation, core Flight-information, and simplified wind foundation of this broader direction.

## Product Direction Alignment

**Outcome:** `Aligned with explicit simplification`.

- **Direction advanced:** the slice models a real pilot-visible Flight process, creates observable end-to-end behavior, advances map-centered Flight awareness, communicates the accepted limitations of estimated wind outside active Flight, and reduces lifecycle, simulation, interruption-boundary, detector, derivation, orientation, and estimated-wind risk.
- **Semantic integrity:** normal C2–C10 responsibilities remain distinct; wall-clock and monotonic time; source time and AirLink-observed time; runtime-value provenance, C10-requested handling, C4/C5 actually active handling, and Flight-level classification; Heading and Track; weather-source wind and estimated wind; estimated wind and gust estimation; unavailable and valid zero; scenario truth and AirLink estimates; interruption and lifecycle or retained-product meaning; and logical passive Current Waypoint state versus its excluded presentation are not collapsed.
- **Explicit simplification:** Home, full Pre-Flight, onboarding, permanent navigation, live sources, durable persistence, saved review, a second Flight in the same development session, interruption recovery and final P3 classification, manual completion, false-detection discard, final algorithms, and final UI are omitted from this first slice under the explicit owner selection recorded here. The normal successful Flight flow remains uninterrupted, while a separate bounded validation case exposes the C4/C3/C9 interruption boundary and stops before recovery or product classification. Flight Mode nevertheless remains active and returns to `Ready on Ground` after the normally completed Flight, preserving the broader multiple-Flight lifecycle rather than redefining it.
- **Boundedness and reversibility:** the slice uses a temporary entry, one read-only scenario, an experimental detector and orientation policy, and a minimal progressive in-memory Flight record. Issue #36 selects no framework, provider, durable schema, complete architecture, or final algorithm; issue #37 may prepare only approved slice-bounded technical choices without turning them into whole-product standards.
- **Long-term direction preserved:** it supports Android-first implementation without redefining AirLink as Android-only and does not deny or collapse Route, wider Flight Support, Pilot Ecosystem, or other future domains.
- **Authority:** the owner decision supplied for issue #36 authorizes this selection record only. Issue #37 owns implementation-ready planning; no implementation is authorized here.

No further Product Vision or Product Direction revision is required by this selection artifact.

## Decision Consequences

The selected slice strongly reduces uncertainty around:

- simulation and input trust;
- clock, provenance, handling, and Flight-classification boundary compatibility;
- lifecycle integration;
- takeoff and landing feasibility;
- map and orientation semantics;
- C8 degraded-state independence from non-map Flight truth;
- early estimated-wind feasibility;
- pilot-facing communication of estimated-wind limitations without expanding the active-Flight UI;
- core Flight-value derivation;
- initial Flight presentation direction.

It partially reduces uncertainty around:

- available, unavailable, stale, and degraded value semantics;
- C4 interruption and optional restoration observability plus C3/C9 boundary handoffs, without resolving P3;
- minimum validation observability across normal responsibility boundaries;
- minimum logical retained-data contract and C3/C4/C7/C9 progressive-recording boundary;
- takeoff-side C3 → C9 initialization and bounded effective-boundary-to-confirmation history handoff;
- recording-health and finalization outcomes;
- C3-to-C9 special-point handoffs;
- preservation of the final segment through confirmed landing;
- completed-Flight Summary classification, completion status, recording status, and Summary-from-record contract;
- cross-platform separation.

It does not materially reduce uncertainty around:

- durable historical-data persistence;
- production storage technology, schema, migration, health, and recovery behavior;
- process-interruption recovery;
- saved-Flight review;
- Android background and lifecycle integration;
- real sensor reliability;
- iOS integration.

## Remaining Work

Issue #36 completes candidate comparison and records the explicit owner selection when this artifact is accepted and merged. Issue #37 must create an implementation-ready plan for only the selected slice, preserving the fixed selection semantics while defining the bounded contracts, mechanisms, approved technical choices, acceptance evidence, and decomposition explicitly handed off above. Issue #38 and the AL-0003 transition remain later work.

No product implementation, AL-0003 implementation issue, complete roadmap, or detailed issue #37 plan begins in this work.
