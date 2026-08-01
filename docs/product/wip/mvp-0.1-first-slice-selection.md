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

A deterministic simulated movement sequence drives automatic takeoff and landing detection, an active Flight lifecycle, basic Flight metrics, a finalized in-memory Summary, and minimal instrument-style presentation. It provides no meaningful map-centered spatial experience and no early estimated-wind validation.

Its strength is a smaller lifecycle-focused slice. Its weakness is that it under-tests the intended spatial Flight presentation and the central estimated-wind risk.

### Candidate B — Map-centered simulated Flight

Candidate B adds a real basemap, a pilot-centered map, compass and orientation context, and core Flight values to Candidate A. It does not meaningfully calculate or validate estimated wind.

Its strength is establishment of the spatial Flight foundation. Its weakness is that it still postpones an important product and engineering risk.

### Candidate C — Simulation-driven Map Flight Core with early estimated wind

Candidate C provides one complete bounded simulated Flight from ground waiting to Summary. It includes a real map and spatial orientation, automatic takeoff and landing detection, pressure/QNH-derived altitude, derived vertical speed, core Flight metrics, an early estimated-wind calculation with simplified in-Flight presentation, and a finalized in-memory result. It includes no durable persistence.

This is the owner-selected candidate.

### Candidate D — Thin preparation-to-saved-Flight journey

Candidate D spans a broader but shallower journey through preparation, Flight, durable save, and reopening. It reduces depth in map behavior, simulation, detectors, and estimated wind, while requiring early storage and historical-contract decisions.

Its strength is broader lifecycle coverage. Its weakness is that it introduces difficult-to-reverse persistence and workflow decisions before the highest-risk Flight behavior is validated and spreads the first implementation effort too thinly.

## Comparison

| Evaluation dimension | Candidate A | Candidate B | Candidate C | Candidate D |
| --- | --- | --- | --- | --- |
| Pilot-visible value | **Moderate:** coherent lifecycle and metrics, but instrument-like and spatially incomplete | **High:** a recognizable map-centered Flight experience | **High:** a coherent map-centered Flight with meaningful derived information and Summary | **Moderate:** broader journey, but each Flight behavior is shallow |
| Critical risk reduction | **Moderate:** lifecycle and detector learning; little map or wind learning | **High:** lifecycle, detector, map, and orientation learning; wind remains deferred | **Very high:** combines lifecycle, simulation trust, map/orientation, derivation, and early wind evidence | **Moderate:** persistence risk is addressed early, but the highest Flight risks remain weakly exercised |
| Responsibility and handoff coverage | **Moderate:** reaches C2, C3, C4, C6, C7, C10 and pilot presentation | **High:** adds normal C8 spatial responsibility | **Very high:** exercises the bounded lifecycle, C4/C5 input meaning, C6 detection, C7 derivation, C8 spatial context, C10 validation, and pilot-facing flow | **Broad but shallow:** reaches more workflow and C9 retention responsibility without enough depth in core Flight handoffs |
| Simulation and no-real-flight validation | **High:** deterministic lifecycle evidence is straightforward | **High:** deterministic lifecycle and spatial evidence | **Very high:** deterministic lifecycle, spatial, pressure, and truth-wind comparison evidence through normal boundaries | **Moderate:** the breadth is demonstrable, but reduced simulator depth weakens evidence |
| Retained result | **Moderate:** finalized in-memory Summary | **Moderate:** finalized in-memory Summary | **High:** finalized in-memory Flight result used by Summary, without durable storage | **Very high:** durable save and reopening, at the cost of an early historical contract |
| Dependency coverage | **Moderate:** useful lifecycle spine but weak spatial and wind foundation | **High:** useful lifecycle and spatial foundation | **Very high:** covers the most important compatible lifecycle, source, derivation, spatial, and validation boundaries | **Broad:** covers workflow and storage, but defers depth in risk-bearing Flight dependencies |
| Technical-decision burden | **Low to moderate** | **Moderate:** adds map technology and orientation choices | **High but bounded:** adds map, detector, pressure, vertical-speed, and wind decisions needed only for this slice | **Very high:** adds persistence, schema, migration, historical preservation, and reopening decisions |
| Implementation size | **Smallest** | **Medium** | **Largest bounded core** | **Broadest overall journey** |
| Reversibility | **High**, but risks requiring later spatial rework | **High**, provided orientation remains experimental | **High:** no durable schema; algorithms, provider, framework, and detailed UI remain replaceable | **Lower:** early durable contracts and workflow decisions are costly to revise |

Candidate A is smaller, but it does not reduce enough of the map and wind risk. Candidate B establishes the spatial foundation, but still postpones estimated-wind learning. Candidate D covers more of the product journey, but introduces premature persistence and workflow decisions while diluting attention across the core Flight behavior.

Candidate C is preferred because it offers the best balance of coherent pilot-visible value, lifecycle validation, simulation trust, map and orientation learning, early wind-risk reduction, and reversibility. Its selection is not based on containing more features: its additional scope is concentrated on mutually dependent Flight behavior that can be validated together, while durable storage and broader workflow remain deliberately absent.

## Owner Selection

The owner selects Candidate C:

> **Simulation-driven Map Flight Core with early estimated wind.**

The selection is accepted with explicit simplification. It selects the first implementation-slice candidate for detailed planning; it does not authorize implementation. Issue #37 must turn this selection into an implementation-ready plan.

Application framework, map provider, final architecture, exact algorithms, thresholds, contracts, and detailed UI remain unselected.

## Selected Slice Outcome

The selected end-to-end user-observable result is:

1. A development build starts through a temporary entry path.
2. One bundled, read-only simulation scenario is loaded automatically.
3. A simulated Flight Mode context is created outside the Flight Screen.
4. The Flight Screen opens in `Ready on Ground`.
5. No Flight exists yet.
6. The user starts simulation input progression.
7. Normal source-equivalent data changes over time.
8. Takeoff is detected by normal Flight Detection responsibility, not declared by the simulator.
9. The active Flight is created through the normal Flight Mode and Flight lifecycle responsibilities.
10. The map and Flight information update during the simulated Flight.
11. Estimated wind becomes available after sufficient data exists.
12. Landing is detected from normal inputs.
13. The Flight is finalized.
14. A Flight Summary is shown from the finalized in-memory result.
15. The user can reset and repeat the scenario as a fresh development session.

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
- future Home and Pre-Flight flows must be able to enter the same Flight flow without redefining its internal semantics;
- an empty Home, complete navigation graph, coordinator framework, or workflow engine is not required.

## Simulation Scenario

The slice uses exactly one bundled scenario. It is authored outside the application as a declarative, versioned, read-only data asset and is loaded automatically. There is no scenario selector, editor, remote distribution, or scenario-specific branching embedded in simulator code.

The scenario is deterministic and phase-based. It may include ground waiting, acceleration, a takeoff profile, climb, level segments, relative turns, descent, final approach, landing, ground stop, deterministic Heading variation, truth wind, initial position and altitude context, and fixed initial map zoom.

Initial Heading is derived relative to truth wind so takeoff occurs into wind. The relative maneuver profile rotates with the initial Heading, and the final direction may also be into wind. Exact return to the start point is not required. This introduces neither an autopilot nor a navigation controller.

Issue #36 does not select the final scenario file format.

## Source-Equivalent Simulation Boundary

The selected slice preserves the Engineering Map responsibility boundary. C10 — Simulation and Validation Enablement owns scenario state and controlled substitute production, but does not become an alternative lifecycle, detector, calculation system, spatial model, recorder, or product flow.

The simulator may emit normal-source equivalents through the normal C4 — Input Acquisition and Validity or C5 — Weather Context boundaries, including:

- time;
- position;
- Ground Speed source information;
- Track;
- atmospheric pressure;
- source validity, availability, freshness, and provenance;
- simulated weather QNH;
- simulated weather wind.

For the selected pressure path, the simulator supplies atmospheric pressure rather than final pilot-facing altitude. Simulated weather supplies QNH. AirLink calculates barometric altitude and derives vertical speed from altitude history through normal C7 — Flight Information Derivation responsibility.

The simulator must not emit:

- takeoff or landing detected;
- Flight started or completed;
- Flight elapsed time or flown distance;
- calculated altitude as an AirLink result;
- vertical speed as an AirLink result;
- Takeoff Point or Landing Point;
- estimated wind;
- Flight Summary values.

Simulator truth may contain truth wind, truth airspeed, air-relative Heading, phase, and the generated ground vector. Truth wind is never an input to the first-slice wind estimator. It may be used only as a deterministic validation oracle in tests or compact development information, preserving the Engineering Map relationship:

`scenario truth wind → C10-generated ground-relative inputs → normal C4 boundary → independent C7 estimated wind → validation comparison`

Concern identifiers are planning references only. They do not select modules, services, classes, or architecture.

## Flight Lifecycle

The first detector is bounded and experimental, not the final production detector. C6 — Flight Detection infers takeoff and landing from normal simulated inputs; C10 does not send lifecycle events.

- before confirmed takeoff, Flight Mode remains in ground waiting and no Flight exists;
- after confirmed takeoff, C2 — Flight Mode Lifecycle authorizes C3 — Flight Lifecycle and Flight State to begin an active simulated Flight;
- after confirmed landing, the Flight result is finalized;
- each reset creates a fresh development session;
- repeating the scenario does not create a second Flight inside the same Flight Mode in this slice.

Pilot-facing detector state may remain limited to waiting for takeoff, active Flight, an optional brief landing-confirmation state, and completed Summary. Detailed diagnostics, thresholds, final detection semantics, and the final algorithm belong to issue #37 or authorized implementation work.

## Map and Orientation

The selected slice includes:

- a real geographic basemap;
- a centered pilot marker;
- fixed zoom, with the exact zoom and area selected during implementation planning;
- North-up presentation while on the ground before valid movement;
- Track-up presentation after valid Track is available;
- a compass or orientation indication that keeps True North understandable;
- explicit semantic distinction between Heading and Track.

Track-up is a reversible first-slice behavior, not resolution of the final orientation policy. A real Android magnetic compass, pan, user zoom controls, map-layer selection, and offline-map scope are excluded.

The slice also excludes the actual flown-track line, zero-wind reference path, Takeoff Point marker, Landing Point marker, and distance or bearing to Takeoff Point.

## Ground Presentation

Before takeoff, the screen presents only values with current meaning:

- current map position;
- barometric altitude;
- weather-source wind;
- compact simulation controls.

Weather-source wind occupies the primary top-row location that later displays Flight speed. Presentation distinguishes unavailable, valid zero, and—where supported—available but stale, degraded, or uncertain states. Zero must not substitute for unavailable data.

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

## Simplified Wind Presentation

The first slice should attempt a simplified windsock-like presentation inside the compass area:

- use the visual analogy of an aerodrome windsock;
- orient it so the into-wind landing direction is intuitively understandable;
- use visible length or sections to communicate wind speed;
- retain a visible numeric value;
- attempt approximately `0.5 m/s` display granularity.

Final geometry, section rendering, gradients, safety thresholds, blinking, warning policy, placement, and dimensions are not fixed. The purpose is comprehension learning, not approval of the final Flight Screen design.

## Compact Simulation Panel

Only a compact panel is included. It contains:

- Start/Pause;
- Reset;
- `1×` and `2×` speed;
- current simulation phase;
- simulation elapsed time.

Use `Pause`, not product-semantic `Stop`. The panel occupies the lower area beneath the compass that may later serve other Flight or Route presentation. An expanded diagnostics overlay is out of scope because its information architecture and placement require separate design.

## Retained Result and Flight Summary

The retained result is a finalized in-memory Flight result for the current run. The Flight Summary reads from that result rather than from independent UI counters.

The minimum Summary contains:

- Flight duration;
- flown distance;
- average Ground Speed;
- maximum Ground Speed;
- maximum altitude.

No durable storage is included. The selected slice has no database, storage schema, migration, saved Flight list, reopening after restart, deletion, or recovery after process termination.

## Platform Direction

AirLink's intended mobile product supports Android and iOS. Initial implementation and early platform validation are Android-first, while first-slice product semantics and non-platform logic must not be Android-specific. Concrete iOS integration is deferred.

No application framework or code-sharing strategy is selected. This artifact does not select Flutter or two native applications; framework and sharing choices belong to bounded technical planning.

## First-Slice Explicit Non-Scope

The selected slice does not include:

- Home or full Pre-Flight;
- permanent application navigation;
- multiple scenarios, scenario selection, an editor, or remote scenarios;
- live Android GNSS, Android compass, or real Android barometer integration;
- iOS platform integration;
- permissions or background execution;
- manual Heading control;
- Route, waypoints, waypoint carousel, Route progress, or active route navigation;
- fuel model, fuel gauge, or user settings;
- map pan, user zoom controls, map-layer selection, or offline maps;
- actual flown track or zero-wind reference path;
- Takeoff Point or Landing Point markers;
- distance or bearing to Takeoff Point;
- durable persistence or saved Flight review;
- repeated Flights within one Flight Mode;
- interruption or recovery behavior;
- final takeoff or landing algorithm;
- final wind algorithm;
- final Flight Screen design;
- expanded diagnostics overlay;
- complete application architecture;
- provider or framework selection.

These exclusions are explicit first-slice simplifications, not changes to the broader accepted MVP 0.1 boundary or permanent rejection of future behavior.

## Technical Decisions Deferred to Issue #37

Issue #37 must decide only what is necessary to make this selected slice implementation-ready, including:

- the application or framework decision required for the slice;
- the map technology or provider required for the slice;
- exact scenario asset format;
- scenario values and phase durations;
- takeoff and landing detector thresholds;
- pressure-to-altitude calculation details;
- vertical-speed filtering;
- the first wind-estimation method;
- runtime responsibility contracts;
- compact simulation-panel behavior;
- exact Summary contract;
- test strategy and acceptance cases;
- implementation decomposition.

This artifact makes none of those decisions. Issue #37 must also preserve the Engineering Map's decision classes and stop at any owner-controlled semantic or difficult-to-reverse technical boundary that needs separate approval.

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

- **Direction advanced:** the slice models a real pilot-visible Flight process, creates observable end-to-end behavior, advances map-centered Flight awareness, and reduces lifecycle, simulation, detector, derivation, orientation, and estimated-wind risk.
- **Semantic integrity:** normal C2–C10 responsibilities remain distinct; source and derived meanings, Heading and Track, weather-source wind and estimated wind, unavailable and valid zero, and scenario truth and AirLink estimates are not collapsed.
- **Explicit simplification:** Home, Pre-Flight, permanent navigation, live sources, durable persistence, saved review, repeated Flights, interruption recovery, final algorithms, and final UI are omitted from this first slice under the explicit owner selection recorded here.
- **Boundedness and reversibility:** the slice uses a temporary entry, one read-only scenario, an experimental detector and orientation policy, and an in-memory result. It selects no framework, provider, schema, complete architecture, or final algorithm.
- **Long-term direction preserved:** it supports Android-first implementation without redefining AirLink as Android-only and does not deny or collapse Route, wider Flight Support, Pilot Ecosystem, or other future domains.
- **Authority:** the owner decision supplied for issue #36 authorizes this selection record only. Issue #37 owns implementation-ready planning; no implementation is authorized here.

No Product Vision or Product Direction revision is required.

## Decision Consequences

The selected slice strongly reduces uncertainty around:

- simulation and input trust;
- lifecycle integration;
- takeoff and landing feasibility;
- map and orientation semantics;
- early estimated-wind feasibility;
- core Flight-value derivation;
- initial Flight presentation direction.

It partially reduces uncertainty around:

- available, unavailable, stale, and degraded value semantics;
- Summary and in-memory result contract;
- cross-platform separation.

It does not materially reduce uncertainty around:

- durable historical-data preservation;
- Android background and lifecycle integration;
- real sensor reliability;
- interruption recovery;
- storage migration;
- iOS integration.

## Remaining Work

Issue #36 completes candidate comparison and records the explicit owner selection when this artifact is accepted and merged. Issue #37 must create an implementation-ready plan for only the selected slice. Issue #38 and the AL-0003 transition remain later work.

No product implementation, AL-0003 implementation issue, complete roadmap, or detailed issue #37 plan begins in this work.
