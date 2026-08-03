# MVP 0.1 First Slice Implementation Plan

## Status and Authority

Related issue: [#37 — AL-0002-05: Prepare the selected first vertical slice for implementation](https://github.com/AlexanderTsarkov/AirLink/issues/37).

This document is an **owner-approved AL-0002 engineering-planning artifact proposed for repository acceptance through issue #37**.

After merge, its authority is:

- an owner-approved AL-0002 engineering-planning artifact;
- the implementation-ready plan produced by issue #37;
- non-canonical product WIP;
- a reviewed input to the later AL-0003 implementation work;
- not implementation authority by itself.

Implementation may begin only after:

1. AL-0003 is activated through a separate owner-approved transition;
2. an approved bounded implementation issue explicitly references this plan;
3. the owner authorizes execution of that issue.

This document does not promote its supporting WIP sources into canon, redefine the complete MVP 0.1, activate AL-0003, create implementation issues, or authorize product code.

## Purpose

This document prepares the owner-selected first end-to-end implementation slice deeply enough that implementation can begin without an implementation agent inventing unresolved product semantics or material architecture.

It defines:

- the pilot-visible result;
- included behavior and explicit non-goals;
- first-slice responsibility boundaries and conceptual handoffs;
- deterministic simulation requirements;
- takeoff, landing, derivation, map, recording, and Summary semantics;
- observability and validation evidence;
- the bounded technical decisions required to start;
- repository delivery increments;
- implementation-readiness criteria;
- Definition of Done;
- intentionally deferred decisions and stop conditions.

It does not prescribe final modules, packages, classes, APIs, storage schemas, complete application architecture, or whole-product technology strategy.

## Source Basis

This plan uses:

- [`ITERATION.md`](../../../ITERATION.md);
- [`CurrentState.md`](../CurrentState.md);
- canonical [`ProductVision.md`](../vision/ProductVision.md);
- accepted product policy in [`ProductDirection.md`](../policy/ProductDirection.md) and the relevant governance routing;
- [`mvp-0.1-scope.md`](mvp-0.1-scope.md);
- [`mvp-0.1-engineering-map.md`](mvp-0.1-engineering-map.md);
- [`mvp-0.1-first-slice-selection.md`](mvp-0.1-first-slice-selection.md);
- relevant boundaries in [`flight-mode-model.md`](flight-mode-model.md), [`flight-model.md`](flight-model.md), and [`navigation-model.md`](navigation-model.md);
- issue #37;
- explicit owner decisions made during issue #37 planning;
- owner-supplied legacy and newer Flight Screen images used only as non-normative visual references.

The visual references are not copied product specifications. The accepted text in this plan takes precedence over interpretation of those images.

---

# 1. Selected Slice

The owner-selected slice is:

> **Simulation-driven Map Flight Core with early estimated wind**

The slice delivers one bounded simulated Flight from ground waiting to completed Summary. It connects:

- pilot-facing Flight presentation;
- Flight Mode and individual Flight runtime state;
- normal source-equivalent simulation;
- automatic takeoff and landing detection;
- pressure/QNH-derived altitude;
- derived vertical speed;
- early estimated-wind calculation;
- map-centred spatial presentation;
- progressive in-memory recording;
- a finalized in-memory Flight record;
- deterministic validation without a real Flight.

The slice deliberately omits durable persistence and broader preparation, navigation, equipment, history, and ecosystem behavior.

# 2. Purpose and Acceptance Contract

## 2.1 Pilot-visible acceptance

A development build must present one coherent Flight flow:

```text
Waiting for Takeoff
→ simulated ground movement
→ automatic takeoff confirmation
→ active map-centred Flight
→ altitude, vertical speed, and estimated wind
→ automatic landing confirmation
→ finalized in-memory record
→ Flight Summary
```

The result must be understandable as one completed Flight rather than a collection of disconnected technical demonstrations.

## 2.2 Engineering validation acceptance

Structured diagnostics and automated tests must prove that the same execution path:

- preserves C1–C10 responsibility boundaries;
- does not expose simulator truth to normal concerns;
- separates effective transition boundaries from confirmation time;
- applies the accepted lifecycle authority chains;
- uses source monotonic time for calculations and durations;
- progressively records approved observations and domain events;
- produces complete, degraded, and failed retained-result semantics;
- is deterministic and reproducible;
- survives the defined bounded degradation cases.

Pilot-visible acceptance and engineering evidence must not use parallel product implementations.

# 3. Explicit Simplifications Relative to Broader WIP

This first implementation slice intentionally narrows several broader MVP directions.

## 3.1 One Flight per development session

The broader Flight Mode model allows multiple independent Flights in one Flight Mode period. This slice supports only one completed Flight in a development session. After completion, `Reset` discards the current in-memory result and creates a fresh session.

This is a first-slice simplification, not a change to the broader Flight Mode direction.

## 3.2 Takeoff Point without passive waypoint presentation

C3 creates the Takeoff Point and C9 retains it in the Flight record. The first slice does **not** present Takeoff Point as a map marker, distance/bearing target, or passive Current Waypoint.

The broader Navigation WIP behavior in which Takeoff Point becomes Current Waypoint after takeoff is deferred from this slice. No Active Navigation or Route state is created.

## 3.3 Reduced C8 presentation

The broader Engineering Map assigns C8 a compass ring or scale and pilot-controlled map scale as MVP capabilities. This slice provides only the spatial elements required for the selected outcome:

- centred pilot marker;
- orientation behavior;
- north/orientation cue;
- scale indicator;
- fixed target physical viewport width;
- degraded spatial canvas.

A complex compass ring and pilot-controlled scale are deferred.

## 3.4 No durable retained Flight

The broader MVP requires durable local retention and later review. This slice finalizes one in-memory Flight record only. The retained-data semantics are designed to remain compatible with later durable retention without selecting a storage schema or engine.

---

# 4. Included Behavior

The slice includes:

- a greenfield Flutter application foundation;
- a temporary development entry directly into Flight Screen;
- a prepared simulated Flight Mode context supplied to Flight Screen;
- one bundled, read-only deterministic scenario;
- independent asynchronous source-equivalent input streams;
- source validity, freshness, accuracy, provenance, and timing metadata;
- ground waiting and ground movement;
- automatic takeoff detection and Flight creation;
- Takeoff Point creation;
- active Flight lifecycle;
- pressure/QNH-derived altitude MSL;
- height above takeoff;
- derived vertical speed;
- early estimated-wind calculation with quality acceptance;
- map-centred spatial presentation;
- automatic landing detection and Flight completion;
- Landing Point creation;
- progressive in-memory recording;
- finalized complete, degraded, or failed recording outcome;
- Summary derived only from the finalized record;
- product-facing unavailable/degraded states;
- a compact simulation panel and expandable inspector;
- a normal successful scenario;
- a controlled five-second GNSS-outage scenario variant;
- a map-unavailable validation mode;
- Reset into a new development session.

# 5. Explicit Non-Goals

The slice excludes:

- Home;
- permanent application startup or navigation flow;
- full Pre-Flight workflow;
- Route creation, Route progression, or Active Navigation;
- waypoint name, bearing, distance, ETA, or leg presentation;
- Takeoff Point or Landing Point map markers;
- fuel quantity, fuel prediction, or fuel reserve;
- equipment, airspace, checklist, or pilot-profile behavior;
- durable persistence, database, migrations, or saved-Flight reopening;
- logbook, statistics, replay, or media integration;
- server synchronization or authentication;
- social or Pilot Ecosystem behavior;
- live Android sensor integration;
- Android background execution;
- iOS sensor integration or background validation;
- production telemetry;
- user-controlled map pan or zoom;
- offline maps;
- track line or glide-range circle;
- terrain-derived AGL;
- production detector thresholds;
- full aerodynamic or sensor-physics simulation;
- complete application architecture;
- whole-product framework or provider selection.

These omissions are explicit simplifications, not rejection of future product domains.

---

# 6. Terminology and Semantic Distinctions

## 6.1 Horizontal motion

- **AS — Air Speed:** speed of the wing relative to the air mass.
- **GS — Ground Speed:** speed relative to the ground.
- **Air Heading:** orientation and air-relative movement direction of the wing.
- **Device Magnetic Azimuth:** phone orientation relative to Magnetic North as supplied by the orientation source.
- **Device True Azimuth:** the same device orientation after AirLink applies magnetic-declination correction to True North.
- **Track:** direction of the ground-velocity vector, expressed relative to True North.

In the airborne simulation model:

```text
ground velocity vector
=
air-relative velocity vector
+
truth wind velocity vector
```

Requirements and implementation-facing documentation must not use an unlabeled generic `speed` where AS and GS could be confused. They must also keep Air Heading, Device Magnetic Azimuth, Device True Azimuth, and Track distinct rather than using an unlabeled generic `Heading`.

## 6.2 Vertical motion and altitude

- **VS — Vertical Speed:** positive in climb and negative in descent.
- **Altitude MSL:** altitude relative to mean sea level.
- **Height above takeoff:** current barometric altitude minus the altitude at the effective Takeoff Point.

Height above takeoff must not be called AGL because terrain elevation is not modelled.

## 6.3 Wind

- **Weather-source wind:** external weather context available on the ground.
- **Truth wind:** privileged simulator value used only to generate source-equivalent motion and validation comparison.
- **Estimated wind:** C7 result derived from normal movement observations.
- **Estimated AS:** magnitude of `ground velocity − last accepted estimated wind`.

Weather-source wind, truth wind, and estimated wind must remain semantically distinct.

## 6.4 Time

- **Scenario time:** virtual deterministic progression controlled by Start, Pause, and playback speed.
- **Source monotonic time:** time of observation occurrence at the source.
- **Observed monotonic time:** time the observation reaches AirLink.
- **Wall-clock time:** civil time used for displayed takeoff and landing timestamps.

Calculations, windows, ordering, Flight duration, and elapsed Flight time use monotonic time, not wall clock. Monotonic validity is evaluated within a development session and within each source stream; Reset begins a new session and therefore a new monotonic domain.

---

# 7. Slice Inputs and Outputs

## 7.1 Inputs

Normal first-slice inputs are:

- GNSS position;
- GS and Track;
- horizontal, speed, and course accuracy where available;
- Device Magnetic Azimuth and orientation quality where available;
- atmospheric pressure;
- weather-source wind and QNH;
- source monotonic time, observed monotonic time, and wall-clock time;
- availability, validity, freshness, quality, provenance, and applicable handling metadata;
- platform/input interruption state;
- pilot development controls: Start, Pause, playback speed, and Reset;
- bounded validation controls such as GNSS outage and map-unavailable mode.

C10 scenario truth and phase metadata are not normal inputs.

## 7.2 Outputs

The slice produces:

- pilot-visible ground, active-Flight, degraded, completed, and failed-record presentation;
- `TakeoffConfirmed` and `LandingConfirmed` detector outcomes with effective boundaries and diagnostics;
- authoritative Flight identity, lifecycle, Takeoff Point, and Landing Point;
- altitude MSL, height above takeoff, VS, estimated wind, and estimated AS;
- Device True Azimuth derived from Device Magnetic Azimuth, position, civil date/time, and a replaceable magnetic-declination provider;
- pilot-centred map/spatial state and orientation state;
- progressive recording health;
- a finalized complete or degraded in-memory Flight record, or a failed-record outcome;
- Flight Summary derived from that finalized outcome;
- structured diagnostic snapshots and a bounded event timeline;
- deterministic automated validation evidence.

---
# 8. First-Slice Concern Responsibilities

Concern identifiers remain planning references and do not prescribe code modules.

## C1 — Pilot Interaction and Operational Flow

C1 owns:

- pilot-visible Flight Screen presentation;
- temporary development controls as a visually separate development layer;
- pilot-facing complete, degraded, unavailable, and failed outcomes;
- the bounded non-flight explanation of estimated-wind limitations.

C1 does not own lifecycle, detection, calculation, recording, or map semantics.

## C2 — Flight Mode Lifecycle

C2 owns:

- `Ready on Ground` operational state;
- authorization for confirmed takeoff or landing outcomes to affect an individual Flight;
- prevention of Flight creation outside an allowed state;
- return to `Ready on Ground` after completed Flight.

The pilot-facing label for the initial state is `Waiting for Takeoff`.

## C3 — Flight Lifecycle and Flight State

C3 alone owns:

- Flight identity;
- Flight-level `simulated` classification;
- active and completed Flight lifecycle;
- authoritative effective takeoff and landing boundaries;
- Takeoff Point and Landing Point identity and Flight association;
- Flight-scoped elapsed-time semantics and association of runtime aggregate updates with the active Flight;
- completion of the individual Flight.

C3 does not own detection, current-value calculation, map presentation, or recording mechanics.

## C4 — Input Acquisition and Validity

C4 owns the normalized AirLink-facing form of:

- position;
- GS;
- Track;
- raw Device Magnetic Azimuth and orientation quality;
- pressure;
- source and observed time;
- accuracy and quality metadata;
- availability, validity, freshness, and provenance;
- applicable pass-through or controlled-substitute handling state, kept separate from provenance;
- platform/input interruption state.

C4 accepts independent asynchronous streams. Domain logic must not require a fixed sensor frequency. C4 does not calculate magnetic declination or convert magnetic orientation to True North; it preserves the source meaning and supplies the independent position, civil-time, and orientation inputs required by C7.

## C5 — Weather Context

C5 owns:

- weather-source wind and QNH context;
- source time, freshness, validity, provenance, applicable handling state, and degraded state;
- the weather interpretation supplied to C1, C6, and C7 where required.

C5 does not own estimated wind or altitude calculation.

## C6 — Flight Detection

C6 owns:

- takeoff and landing candidates;
- candidate cancellation and expiry;
- one-shot `TakeoffConfirmed` and `LandingConfirmed` outcomes;
- effective-boundary estimation references;
- detector diagnostics and versioning.

C6 does not create or complete a Flight.

## C7 — Flight Information Derivation

C7 owns:

- barometric altitude MSL;
- height above takeoff;
- vertical speed;
- magnetic-declination lookup through a replaceable provider boundary;
- conversion of Device Magnetic Azimuth to Device True Azimuth;
- current, retained, or unavailable estimated-wind state;
- estimated air-velocity used by the landing detector;
- quality and uncertainty semantics for derived values.

## C8 — Spatial Awareness and Map Context

C8 owns:

- pilot-centred map/spatial canvas;
- map orientation presentation;
- scale and north/orientation cues;
- map-unavailable/degraded spatial state.

C8 consumes Device True Azimuth on the ground and Track in Flight. It does not calculate magnetic declination or reinterpret raw magnetic orientation.

C8 does not own Flight lifecycle, Takeoff Point identity, current-value derivation, or Route navigation.

## C9 — Flight Recording and In-Memory Retention

C9 owns:

- recording initialization;
- progressive observation and domain-event retention;
- recording health;
- bounded preconfirmation and final confirmation-tail retention;
- finalization as an in-memory Flight record;
- complete, degraded, or failed outcome;
- Summary source data.

C9 does not infer Flight lifecycle or Flight-level simulation classification.

## C10 — Simulation and Validation Enablement

C10 owns:

- scenario definition and progression;
- privileged truth;
- deterministic substitute production through C4/C5 boundaries, including source-equivalent Device Magnetic Azimuth rather than a ready-made True-North orientation;
- fault variants;
- truth comparison and validation support.

C10 does not own an alternative lifecycle, detector, calculation, map, recording, or product flow.

---

# 9. Product Flow

## 9.1 Development entry

The development build may open directly into Flight Screen. This is not Home, Pre-Flight, or a permanent application root.

Flight Screen receives a prepared simulated Flight Mode context. Flight Screen does not load or own the scenario.

## 9.2 Initial ground state

When Flight Screen loads:

- C4 and C5 source-equivalent streams are already active;
- C2 is internally `Ready on Ground`;
- C1 displays `Waiting for Takeoff`;
- no Flight exists;
- C10 emits valid ground-state source equivalents;
- weather-source wind is available;
- estimated wind is unavailable;
- C4 supplies valid Device Magnetic Azimuth and C7 derives Device True Azimuth for ground map orientation;
- bounded rolling history may exist only in memory.

## 9.3 Start

`Start` begins scenario progression only.

It does not:

- create a Flight;
- start recording directly;
- instruct C6 that takeoff occurred;
- switch map orientation directly;
- command landing or completion.

Ground movement before confirmed takeoff remains visible.

## 9.4 Takeoff

C6 evaluates normal inputs and emits one-shot `TakeoffConfirmed`. C2 authorizes the transition. C3 creates the Flight and Takeoff Point. C9 initializes recording from the authoritative creation context and bounded history.

Product UI remains `Waiting for Takeoff` until confirmation.

## 9.5 Active Flight

During the Flight, C1 presents:

- map/spatial context;
- GS;
- altitude MSL;
- VS;
- Flight elapsed time;
- orientation context;
- compact estimated wind after first acceptance;
- pilot-visible unavailable or degraded states.

Weather-source wind disappears from Product UI after confirmed takeoff.

## 9.6 Landing and finalization

C6 confirms landing from normal inputs. C2 authorizes completion for the current active Flight. C3 completes lifecycle and creates the Landing Point at the effective boundary. C9 retains the final confirmation tail and finalizes the record.

Product UI does not show technical finalization stages. It transitions to Summary only after finalization outcome is available.

Recording failure does not change the fact that the Flight completed.

## 9.7 Completed state and Reset

After completion:

- C2 is again `Ready on Ground`;
- Flight Mode conceptually remains active;
- the completed outcome remains visible;
- no second Flight begins automatically in the same development session;
- `Reset` discards the current in-memory result and creates a new development session.

---

# 10. Flight Screen Guidance

## 10.1 Product role

Flight Screen is:

- map-centred;
- spatial-first;
- pilot-centred;
- glanceable;
- designed for rapid interpretation under Flight cognitive load.

The central spatial region must remain visually clear.

## 10.2 Layer composition

Flight Screen contains three distinct layers.

### Spatial layer

- basemap or degraded spatial canvas;
- centred pilot marker;
- orientation behavior;
- north/orientation cue;
- scale indicator.

### Product overlay layer

- Flight state or elapsed time;
- GS;
- altitude MSL;
- VS;
- weather-source or estimated-wind information;
- source/map warnings.

### Development-only simulation layer

- Start/Pause;
- `1×/2×`;
- Reset;
- scenario phase and time;
- compact concern/source state;
- expandable inspector.

Simulation controls must not appear to be future pilot-facing product controls.

## 10.3 Spatial geometry

The pilot marker remains at the geometric centre of the full map viewport, including the area behind the temporary bottom simulation panel.

Target visible map width is approximately `2 km` on a typical portrait phone. A bounded prototype adjustment toward approximately `1.5 km` is allowed if 2 km materially harms readability.

The first slice uses fixed scale. It does not use automatic zoom, speed-dependent zoom, user pan/zoom, or glide-range-driven zoom.

## 10.4 Primary overlay zones

The conceptual top layout is:

```text
┌────────────────────────────────────┐
│         status / Flight time       │
│                                    │
│   GS          ALT       contextual │
│                                    │
│             map                    │
│         centred pilot              │
│                                    │
├────────────────────────────────────┤
│ temporary simulation panel         │
└────────────────────────────────────┘
```

Exact geometry, typography, spacing, and colors are bounded UI tuning.

### Status / Flight-time zone

- before takeoff: `Waiting for Takeoff`;
- after takeoff: elapsed Flight time from the effective takeoff boundary;
- after landing: no technical lifecycle/finalization messages; transition to completed outcome after C9 finalization.

### GS tile

Displays:

- `GS`;
- Ground Speed;
- unit.

### Altitude tile

Displays:

- `ALT` or `ASL`;
- barometric altitude MSL;
- unit.

### Contextual right tile

Before confirmed takeoff it displays weather-source wind:

- directional arrow;
- direction above the arrow;
- speed below the arrow.

After confirmed takeoff it displays VS:

- `VS`;
- signed value;
- `m/s`.

## 10.5 Estimated-wind presentation

Estimated wind is mandatory but does not replace VS in the contextual right tile.

Before the first accepted estimate:

- no wind value is shown as estimated wind;
- `0` is not shown;
- weather wind is not substituted;
- persistent `Estimating…` is not used as the value.

After acceptance, Product UI shows a compact indicator for the last accepted estimated wind. Exact placement is bounded UI tuning provided that it:

- does not obstruct the central spatial region;
- is visibly different from weather-source wind;
- remains readable at a glance.

Technical state, age, fit window, rejection reasons, and truth comparison remain in diagnostics.

## 10.6 Estimated-wind explanation

The final product must explain that in-Flight wind is an estimated value and may later require explicit pilot acknowledgement in an appropriate non-flight flow. That broader acknowledgement mechanism is outside this slice.

This slice is simulation-only and cannot be used for a real Flight. It therefore provides only a concise ground-state notice, for example:

> `In-flight wind is estimated.`

The notice may be shown directly on the Flight Screen while no Flight is active and may remain available after completion. No acknowledgement checkbox, consent workflow, blocking explanation, or persistent in-Flight disclaimer is required in this slice.

## 10.7 Pilot-visible degraded states

Product UI may show concise states such as:

- `GPS unavailable`;
- `Map unavailable`;
- unavailable-value marker;
- degraded recording status in Summary.

It does not show:

- detector candidates;
- concern identifiers;
- threshold names;
- fit residuals or condition numbers;
- recorder initialization stages;
- privileged simulator phase/truth.

---

# 11. Time and Source Contracts

## 11.1 Scenario time

- Start advances scenario time;
- Pause freezes scenario time;
- `2×` changes wall-duration playback but not source sequence or domain result;
- Pause is not an input outage.

## 11.2 Source and observed time

Each relevant observation carries source monotonic time. AirLink also records observed monotonic time where delivery delay or batching matters.

C4 exposes timestamps used together for domain calculations in one normalized development-session monotonic domain. If an adapter receives a source-native clock with different origin or units, the adapter maps it explicitly and preserves source timing context where required for diagnosis.

Within one source stream and one development session, source monotonic timestamps must be present and must not move backwards. A duplicate timestamp may not advance a time-based window and must be rejected or coalesced deterministically. Independent streams may legitimately contain equal timestamps.

If a required monotonic timestamp is missing, decreases, or otherwise becomes invalid:

- the affected observation is invalid for ordering and time-based calculation;
- detector timers and derivation windows that depend on the affected stream are invalidated rather than continued across the discontinuity;
- no takeoff or landing confirmation, VS result, wind estimate, Flight duration, or retained ordering may be reported as valid across the discontinuity;
- lifecycle must not silently transition to landing, completion, rejection, or another state because of the clock defect;
- C9 records the clock-invalid interval and exposes degraded or incomplete recording state;
- the first-slice validation case stops before assigning a successful completed Summary or general recovery meaning.

Production recovery from monotonic-clock failure remains deferred.

Logic uses time windows and durations, not sample counts.

## 11.3 Wall clock

Wall clock is used only for civil timestamps. A wall-clock jump must not alter detector timing, Flight duration, VS, wind windows, or ordering.

## 11.4 Asynchronous streams

C4 accepts independent streams for:

- GNSS;
- pressure;
- orientation;
- platform/input state.

C5 supplies weather context independently.

Requested cadence is an adapter hint, not a domain requirement. Actual cadence and gaps are observed and retained.

## 11.5 Fixture cadence

The normal visual fixture may use approximately:

- GNSS: `2 Hz`;
- pressure: `10 Hz`;
- orientation: `10 Hz`;
- weather: initial snapshot.

Required tests include:

- GNSS around `1 Hz`;
- jitter;
- missing samples;
- batched delivery preserving source timestamps.

Insufficient cadence or gaps become quality/degraded state rather than silently changing semantics.

---

# 12. Deterministic Simulation Contract

## 12.1 Scenario asset

The slice uses exactly one bundled, versioned, declarative, read-only scenario asset.

There is no scenario selector, editor, remote distribution, or scenario-specific product branching.

The scenario describes phases and deterministic kinematics. It is not a full aerodynamic simulator and is not a giant opaque array of sampled positions.

## 12.2 Origin and surface

Start coordinate:

```text
59°27'16.60"N 24°53'31.96"E
59.4546111, 24.8922111
```

Fixture surface altitude:

```text
35 m MSL
```

Takeoff and landing areas are treated as locally flat at the same surface altitude. Terrain variation is not modelled.

Local geometry is defined in a portable East/North metre coordinate system and converted to latitude/longitude relative to the origin.

## 12.3 Orientation fixture

C10 holds a privileged truth Device True Azimuth for scenario composition but does not provide that value directly to product logic. It generates source-equivalent Device Magnetic Azimuth through the normal C4 boundary:

```text
deviceMagneticAzimuthDeg
=
normalize360(deviceTrueAzimuthTruthDeg − fixtureDeclinationDegEast)
```

The deterministic first fixture uses a synthetic non-zero magnetic declination of `+10.0° east`. This value exists to prove that the product performs the conversion and is not a claim about production geomagnetic truth for the location or date.

C7 obtains declination through a replaceable `MagneticDeclinationProvider` using position and civil date/time, then calculates:

```text
deviceTrueAzimuthDeg
=
normalize360(deviceMagneticAzimuthDeg + declinationDegEast)
```

East declination is positive and west declination is negative. Results are normalized to `[0°, 360°)`.

The first-slice provider deterministically returns the declared fixture value for the fixture context. Selection of a production WMM/IGRF implementation, native platform API, model-update mechanism, altitude treatment, and offline model-data strategy remains deferred.

## 12.4 Truth wind

The first fixture uses constant truth-wind speed:

```text
4 m/s
14.4 km/h
```

Initial Air Heading and final approach are primarily into wind. Absolute wind direction is a bounded fixture parameter selected during numerical composition.

Normal successful weather-source wind may equal truth wind. Weather/truth mismatch testing is deferred.

## 12.5 Airborne and ground motion

Before physical liftoff and after physical touchdown, pilot coordinates follow ground motion; wind does not add free drift.

After physical liftoff and before touchdown:

```text
ground velocity
=
air-relative movement
+
truth wind
```

C10 uses truth wind only to generate normal source equivalents. C8, C6, C7, and C9 never read truth wind.

## 12.6 Target profile

| Parameter | Target |
| --- | ---: |
| Surface altitude | `35 m MSL` |
| Maximum altitude | approximately `135 m MSL` |
| Maximum height above takeoff | approximately `100 m` |
| Nominal physical liftoff AS | `25 km/h` |
| Post-liftoff AS | `35–37 km/h` |
| Cruise AS | approximately `40 km/h` |
| Final-approach AS | approximately `45 km/h` |
| Maximum climb VS | approximately `+2.5 m/s` |
| Ground distance | approximately `1.5–2 km` |
| Flight duration | approximately `3–3.5 min` |
| Landing confirmation interval | `15 s` |

GS is never prescribed by an AS row. It is calculated from AS, Air Heading, and truth wind.

## 12.7 Natural deterministic variation

Even stable phases must not contain perfectly constant values.

### Air Heading

Typical straight-leg variation:

- normal amplitude about `±4–6°`;
- major corrections every roughly `10–15 s`;
- smaller corrections about `±1–2°`;
- occasional correction up to roughly `8–10°`;
- no instantaneous jumps.

### AS

Cruise AS varies smoothly by roughly `±1–2 km/h` without being mechanically synchronized with Air Heading variation.

### Altitude and VS

Nominal level-flight altitude may vary smoothly by roughly `±0.5–1.5 m`, producing small positive and negative VS while preserving a level mean trend.

### Measurement variation

Smaller deterministic sensor-like variation may be applied to GNSS position, source GS, Track, Device Magnetic Azimuth, and pressure. Truth motion variation and measurement variation must remain separately visible in validation diagnostics.

Uncontrolled randomness is prohibited. Explicit deterministic functions are preferred; a fixed seed alone is insufficient if the resulting sequence is opaque or unstable across implementations.

## 12.8 Scenario phases

The scenario contains conceptually:

1. `ground_ready`;
2. `wing_inflation_and_stabilization`;
3. `launch_acceleration`;
4. `liftoff_transition`;
5. `post_liftoff_acceleration`;
6. `initial_climb`;
7. `flight_maneuvers`;
8. `cruise`;
9. `descent`;
10. `final_approach`;
11. `flare`;
12. `float`;
13. `touchdown`;
14. `landing_run`;
15. `landed_confirmation`;
16. `completed_ground`.

Exact phase durations, leg lengths, turn radii, and easing functions are bounded tuning parameters.

## 12.9 Ground and launch phases

### Ground ready

Before Start:

- pilot stands at origin;
- truth Device True Azimuth and Air Heading are into wind with small orientation variation;
- pressure corresponds to `35 m MSL`;
- GNSS position is valid;
- Track may be unavailable at near-zero movement;
- weather-source wind and QNH are valid.

### Wing inflation and stabilization

- low ground movement;
- larger device-orientation and Air Heading corrections than stable running;
- `wingStabilized` exists only in privileged truth.

### Launch acceleration

Still-air baseline for the fixture:

- AS increases to `25 km/h` over roughly `20 m`;
- simplified constant acceleration is approximately `1.21 m/s²`.

With a direct `4 m/s` headwind:

- physical liftoff occurs around `10.6 km/h GS`;
- simplified distance after acceleration begins is about `3.6 m`;
- simplified acceleration time is about `2.4 s`.

Inflation/stabilization time is separate from this acceleration interval.

Physical liftoff occurs only when:

- the wing is stabilized in truth;
- AS reaches `25 km/h`.

C6 receives neither condition directly.

## 12.10 Climb and manoeuvre phases

After liftoff:

- AS rises smoothly from about `25` to `35–37 km/h` over about `6–8 s`;
- VS rises toward about `+2.5 m/s`;
- AS later settles around `40 km/h`;
- climb continues toward about `135 m MSL`;
- Air Heading changes provide upwind, crosswind, downwind, and return-to-upwind directional diversity;
- turns use smooth entry, main turn, exit, and small post-turn correction;
- straight legs retain natural deterministic yaw.

The approximate closed pattern exists to supply broad velocity-vector coverage. It does not create an AirLink Route, autopilot, or navigation controller.

## 12.11 Descent and approach

Descent begins in the second half of the Flight. VS becomes negative smoothly. Final approach is primarily into wind.

Final-approach AS is approximately `45 km/h`. With a full `4 m/s` headwind, corresponding GS is approximately `30.6 km/h` before flare, subject to corrections and exact geometry.

## 12.12 Flare, float, touchdown, and landing run

Flare begins at approximately `1.0–1.5 m` above the known surface, corresponding to approximately `36.0–36.5 m MSL`.

The simplified flare model includes:

- rapid but smooth brake-input increase;
- aggressive AS reduction;
- reduction of negative VS toward zero;
- temporary conversion of forward speed into lift;
- transition to a short nearly horizontal path.

Full wing, pendulum, pitch, or brake aerodynamics are not modelled.

The float segment is approximately `5 m`:

- height decreases toward the surface;
- VS is near zero or weakly negative;
- AS and GS continue to decrease;
- no instantaneous stop occurs.

At physical touchdown:

- truth airborne state becomes false;
- free wind drift no longer moves coordinates;
- C6 receives no touchdown event.

A short ground run or several steps follow, with GS decreasing smoothly into landing-candidate conditions.

## 12.13 Privileged-truth prohibition

Normal concerns must not receive:

- physical liftoff;
- physical touchdown;
- `wingStabilized`;
- truth wind;
- truth AS;
- truth altitude;
- truth Device True Azimuth or fixture declination as a ready-made C8 orientation input;
- scenario phase;
- expected detector result;
- intended route/leg identity as lifecycle or navigation authority.

Privileged truth is available only to C10 and validation diagnostics explicitly labelled as truth comparison.

---

# 13. Takeoff Detection

## 13.1 Candidate

C6 monitors GS continuously.

A candidate begins when:

```text
GS > 7 km/h
```

sustainably for approximately `1 s`.

The candidate retains:

- provisional start monotonic time;
- provisional start position;
- bounded relevant history;
- diagnostics.

If confirmed, the candidate start becomes the effective takeoff boundary. For this experimental slice, that boundary is a detector-defined approximation and may precede privileged physical liftoff slightly; it must not be described as an exact physical-liftoff timestamp.

## 13.2 Confirmation threshold

Base nominal physical liftoff AS:

```text
25 km/h
```

C6 calculates a GS confirmation threshold using usable weather headwind:

```text
confirmation GS threshold
=
25 km/h
−
75% × usable weather headwind component
```

Usable correction requires valid, fresh weather wind and enough valid directional information to calculate a headwind component. Otherwise correction is zero.

For a direct `4 m/s` headwind:

```text
confirmation threshold ≈ 14.2 km/h GS
```

The threshold must be held for approximately `1 s`.

## 13.3 Cancellation

The candidate is cancelled if:

- GS sustainably falls below `7 km/h`; or
- candidate duration reaches approximately `15 s` without confirmation.

## 13.4 Event and authority

`TakeoffConfirmed` is one-shot and includes:

- confirmation monotonic time;
- effective takeoff boundary;
- bounded history reference/range;
- diagnostics;
- detector version.

Authority chain:

```text
C6 detects
→ C2 validates and authorizes
→ C3 creates Flight and Takeoff Point
→ C9 initializes recording
```

Repeated or late confirmations are ignored.

The thresholds are experimental first-slice values, not production or safety thresholds.

---

# 14. Flight Lifecycle

## 14.1 Before confirmation

- C2 is `Ready on Ground`;
- no Flight exists;
- C9 may retain only bounded transient history;
- Product UI remains `Waiting for Takeoff`.

## 14.2 Creation

Only C3 creates:

- Flight identity;
- Flight-level `simulated` classification;
- effective takeoff boundary;
- Takeoff Point;
- active lifecycle.

Flight state must not be owned by Flutter widget state or map-renderer lifetime.

## 14.3 Active Flight

The Flight remains one continuous episode until confirmed landing in this slice. Temporary source outages do not create another Flight or complete the current one.

## 14.4 Completion

Only C3 completes the Flight after C2 authorizes a valid `LandingConfirmed` outcome for the current active Flight.

C3 immediately and unconditionally establishes lifecycle completion and Landing Point at the effective landing boundary. C9 success or failure does not alter lifecycle truth.

---

# 15. Altitude and Vertical Speed

## 15.1 Altitude MSL

C7 calculates barometric altitude MSL from atmospheric pressure and accepted QNH using one deterministic first-slice contract.

Units:

- `pressureHpa`: hectopascals (`hPa`);
- `qnhHpa`: hectopascals (`hPa`);
- `altitudeMslM`: metres (`m`).

The first-slice formula is:

```text
altitudeMslM
=
44330.76923076923
×
(1 − (pressureHpa / qnhHpa)^0.1902632365)
```

Both pressure and QNH must be finite and greater than zero. The ratio is dimensionless. The calculation identifier is `isa-troposphere-pressure-altitude-v1` and is retained with its constants as derivation context.

The normal fixture uses fixed QNH:

```text
1013.25 hPa
```

C10 generates source-equivalent pressure from truth altitude through the inverse of the same contract:

```text
pressureHpa
=
qnhHpa
×
(1 − altitudeMslM / 44330.76923076923)^(1 / 0.1902632365)
```

At the `35 m MSL` fixture surface this yields approximately `1009.052 hPa`. Round-trip tests must prove that C10 pressure generation and C7 derivation use compatible constants without C7 receiving truth altitude.

QNH remains fixed for the duration of the first-slice Flight. The pilot-facing primary altitude is MSL altitude.

## 15.2 Height above takeoff

C7 calculates height above takeoff relative to the barometric altitude at the effective Takeoff Point.

It is retained and used in Summary, but is not required as a second large Flight Screen value.

## 15.3 Vertical speed

C7 derives VS by fitting altitude against source monotonic time over a `3 s` window.

- insufficient history means unavailable, not zero;
- a pressure gap invalidates current VS and breaks the fit window;
- history must accumulate again after recovery.

Altitude and VS are diagnostic/derived information and do not drive first-slice takeoff or landing detection.

---

# 16. Estimated-Wind Calculation

## 16.1 Vector model

Valid GNSS GS and Track are converted into East/North ground-velocity vectors `(vE, vN)`.

The model is:

```text
ground velocity = wind + air-relative velocity
```

At approximately stable AS, ground-velocity samples lie near a circle:

- fitted centre ≈ wind vector;
- fitted radius ≈ AS.

## 16.2 Fit method

Preliminary bounded implementation:

1. Pratt or Taubin circle initialization;
2. geometric radial least-squares refinement.

Kåsa-only fitting is not sufficient for incomplete arcs.

Pratt versus Taubin remains a reversible implementation choice selected through deterministic numerical tests.

## 16.3 Windows

Candidates are recomputed approximately once per second for windows:

```text
30 / 60 / 90 / 120 s
```

The shortest accepted window is used for freshness.

## 16.4 Quality gates

Candidate acceptance requires all of:

- acceptable input validity and quality;
- acceptable radial residual;
- adequate angular coverage;
- acceptable conditioning/geometric observability;
- acceptable centre uncertainty.

Metrics include:

- radial RMSE;
- normalized RMSE/radius;
- maximum or percentile residual;
- robust residual metric such as MAD;
- angular coverage around the fitted centre;
- centre/radius covariance or uncertainty;
- condition number or equivalent observability metric.

Low residual alone is insufficient because a short arc may fit well but yield an unstable centre.

Initial GNSS horizontal-accuracy guidance is better than approximately `10 m`. Speed and course accuracy also matter. Exact thresholds are selected by the first deterministic numerical test suite.

## 16.5 Accepted-state semantics

C7 distinguishes:

- `current` — latest evaluated window produced a new accepted estimate;
- `retained` — a previous accepted estimate remains usable after a newer candidate was rejected or no current window was acceptable;
- `unavailable` — no accepted estimate exists or hard invalidation occurred.

A rejected candidate does not replace or degrade the previous accepted estimate.

Product UI shows the last accepted estimated wind. Diagnostics show state, age, fit window, rejection reasons, and truth comparison.

No time-based expiry is required within the same short Flight. Hard invalidation remains possible, especially after loss of required GNSS integrity.

---

# 17. Landing Detection

## 17.1 Estimated AS

C6 uses:

```text
estimated air velocity
=
ground velocity
−
last accepted estimated wind
```

The vector magnitude is estimated AS.

## 17.2 Availability

Automatic landing detection is available only when:

- a Flight is active;
- a last accepted wind estimate exists;
- the estimate is not hard-invalidated;
- required GNSS/Track data is valid.

There is no GS-only landing fallback.

## 17.3 Candidate and confirmation

A landing candidate begins when both conditions hold continuously:

```text
estimated AS < 25 km/h
GS < 4 km/h
```

The beginning of this interval becomes the provisional/effective landing boundary after confirmation.

Confirmation requires `15 s` continuous satisfaction.

## 17.4 Hysteresis

The candidate is cancelled if, sustainably for approximately `1 s`:

```text
estimated AS > 28 km/h
or
GS > 7 km/h
```

## 17.5 Event and authority

`LandingConfirmed` is one-shot and includes:

- confirmation monotonic time;
- effective landing boundary;
- final confirmation-tail reference;
- diagnostics;
- detector version.

Authority chain:

```text
C6 detects
→ C2 validates current active Flight
→ C3 completes lifecycle and creates Landing Point
→ C9 finalizes record
```

Summary metrics normally end at the effective landing boundary. C9 retains the final segment through confirmation time as evidence for the detector decision.

The thresholds are experimental first-slice values, not production or safety thresholds.

---

# 18. Map and Orientation

## 18.1 Ground orientation

Before confirmed takeoff:

- C4 supplies raw Device Magnetic Azimuth and its source quality/validity;
- C7 obtains magnetic declination from the replaceable provider using current position and civil date/time;
- C7 derives Device True Azimuth using the accepted east-positive convention;
- C8 uses valid Device True Azimuth for Heading-up presentation;
- a valid Track during launch run does not switch orientation;
- invalid magnetic orientation, missing declination context, or unavailable derived Device True Azimuth falls back to North-up.

C4 does not combine location with orientation, C7 does not own sensor acquisition, and C8 does not perform declination correction.

## 18.2 Airborne orientation

After confirmed takeoff:

- valid GNSS Track is the primary source;
- map uses Track-up;
- compass is not an airborne fallback.

## 18.3 Temporary Track loss

For brief Track invalidity:

- hold last valid orientation;
- mark orientation stale/degraded.

For prolonged invalidity:

- use North-up degraded fallback.

Exact grace duration is a bounded implementation parameter.

## 18.4 Map unavailable

If map tiles or provider rendering are unavailable:

- Flight continues;
- the spatial viewport remains;
- pilot marker remains centred;
- orientation cue and scale remain;
- neutral background is shown;
- `Map unavailable` is shown;
- weak grid/rings may be used to make rotation visible.

The slice must not show a fake or misleadingly current cached map.

---

# 19. Progressive In-Memory Recording

C9 builds a two-layer in-memory record.

## 19.1 Normalized observation stream

Retain, when available:

- GNSS position;
- GS;
- Track;
- horizontal accuracy;
- speed accuracy;
- course accuracy;
- pressure;
- raw Device Magnetic Azimuth and orientation quality;
- weather-source wind;
- accepted QNH value and unit, source/update time, validity, freshness, provenance, and applicable handling state;
- validity, freshness, provenance, and applicable handling state;
- source monotonic time;
- observed monotonic time;
- wall-clock time.

## 19.2 Derived/domain timeline

Retain:

- C3-supplied Flight-level `simulated` classification, kept distinct from category-level provenance and handling;
- altitude MSL;
- height above takeoff;
- VS;
- derived Device True Azimuth, declination used, position/date context, and declination-provider identifier/version;
- altitude-calculation identifier/version and constants used with the accepted QNH;
- all accepted wind estimates with quality metadata;
- current/retained/unavailable wind state transitions;
- detector candidates and confirmations;
- effective takeoff and landing boundaries;
- Flight lifecycle events;
- orientation-source transitions;
- degraded intervals;
- recording outcome.

Rejected wind candidates remain diagnostics and are not required in the finalized Flight record.

## 19.3 Boundaries

C9 receives:

- authoritative C3 creation context;
- bounded history beginning at the effective takeoff boundary;
- active Flight observations/events;
- effective landing boundary;
- final confirmation tail through landing confirmation time.

## 19.4 Outcome

C9 finalizes as:

- `complete`;
- `degraded`;
- `failed`.

A complete or degraded outcome produces an immutable in-memory Flight record. A failed outcome preserves completed lifecycle truth but provides no usable Flight record.

---

# 20. Summary

Summary is derived exclusively from the finalized record. C3 may own active-Flight elapsed time and runtime aggregate association during the Flight, but transient C3 state is not the authoritative source for the completed Summary; the finalized record and authoritative C3 boundaries are.

## 20.1 Fields

1. takeoff time;
2. landing time;
3. duration;
4. distance;
5. average GS;
6. maximum GS;
7. maximum altitude MSL;
8. maximum height above takeoff;
9. maximum climb VS;
10. maximum descent VS;
11. last accepted estimated wind;
12. recording status.

## 20.2 Definitions

### Duration

```text
effective landing monotonic time
−
effective takeoff monotonic time
```

Wall clock is used only to display takeoff and landing civil time.

### Distance

Sum distances between consecutive valid GNSS positions. Do not interpolate across gaps.

### Average GS

```text
valid recorded distance
/
valid covered time
```

It is marked degraded when source gaps materially limit coverage.

### Maximum GS

Maximum valid system GS observation.

### Wind

Label as `Estimated wind`. Truth and fit details remain diagnostics.

## 20.3 Failed recording presentation

A failed recording must convey:

```text
Flight completed
Flight record unavailable
```

It must not imply that lifecycle completion failed.

---

# 21. Controlled Degradation Cases

## 21.1 GNSS outage

A separate deterministic variant introduces exactly `5 s` of GNSS unavailability:

- during stable cruise;
- after at least one accepted wind estimate exists;
- before final approach;
- not during a turn or landing sequence.

During the outage:

- Flight remains active;
- Product UI shows `GPS unavailable`;
- position, GS, and Track are unavailable;
- map holds last valid orientation as stale;
- pressure and VS continue if valid;
- C7 retains the last accepted wind;
- no new wind estimate is accepted;
- landing detection is suspended;
- distance is not interpolated;
- C9 records the gap.

After recovery:

- the same Flight continues;
- normal inputs resume;
- final recording outcome is `degraded`;
- Summary remains available.

This behavior is an explicit owner-approved bounded reopening of the previously deferred P3 interruption boundary for this one deterministic five-second GNSS-outage validation case. It does not define general interruption retention, completion, restoration, long-loss, process-recovery, or production recovery semantics.

Long GNSS-loss policy and process-recovery behavior are deferred.

## 21.2 Map unavailable

A development toggle makes C8 map rendering unavailable without changing scenario physics or source data.

The degraded spatial canvas is shown and Flight continues normally.

---

# 22. Observability and Validation

## 22.1 Compact panel

The persistent simulation panel includes:

- Start/Pause;
- `1×/2×`;
- Reset when allowed;
- scenario phase and time;
- C2, C3, C6, C7, and C9 state;
- source health;
- last significant event.

## 22.2 Expandable inspector

The inspector includes:

- detector thresholds, corrections, timers, candidates, and boundaries;
- wind window, residual, coverage, conditioning, uncertainty, and truth comparison;
- source/observed clocks, latency, gaps, and batching;
- recording counts, boundaries, and outcome;
- orientation source, age, and fallback;
- map state and viewport information.

## 22.3 Shared structured diagnostics

UI and tests use the same read-only diagnostic snapshot and bounded event timeline.

Tests must not depend on rendered screen text.

## 22.4 Required validation evidence

- normal end-to-end deterministic run;
- takeoff candidate/confirmation boundary tests;
- landing candidate/confirmation boundary tests;
- circle-fit numerical tests for ideal, noisy, incomplete, poorly conditioned, and outlier cases;
- accepted/retained/unavailable wind-state tests;
- GNSS-outage and recovery test;
- map-unavailable validation;
- `1×/2×` semantic-equivalence test;
- Pause-is-not-outage test;
- wall-clock-jump test;
- missing, duplicate, and backward source-monotonic timestamp tests proving that invalid time cannot produce valid detector windows, duration, VS, wind estimates, retained ordering, or successful Summary;
- Device Magnetic Azimuth to Device True Azimuth tests using the non-zero fixture declination, including normalization and unavailable-declination fallback;
- pressure/QNH forward-and-inverse round-trip tests at the surface and representative Flight altitudes;
- truth-leakage tests;
- recording complete/degraded/failed tests;
- Summary-from-finalized-record tests.

---

# 23. Bounded Technical Decisions

## 23.1 Flutter

Flutter is the provisional, bounded application framework for the first slice. It is not the final whole-product framework decision.

In Dart, the first slice implements:

- presentation;
- application coordination;
- domain/lifecycle logic;
- simulation;
- detection;
- derivation;
- recording;
- Summary;
- tests and diagnostics.

Plugin and platform types must not enter domain/application contracts.

## 23.2 Platform adapter boundaries

Adapters isolate:

- C4 device/platform input acquisition;
- C5 weather input;
- C7 magnetic-declination provider;
- C8 map rendering/provider;
- future native background acquisition and buffering.

Flight state and recording must not depend on Flutter widget lifetime or map-view lifetime.

Custom Kotlin/Swift or federated plugin code remains allowed if later platform qualification requires it.

## 23.3 Map implementation

The first slice may use an OSM-compatible source and a replaceable Flutter map package.

Package selection occurs during the map delivery increment using:

- Android and iOS support;
- centred-pilot behavior;
- programmatic rotation;
- custom overlays;
- degraded-canvas compatibility;
- acceptable performance;
- understandable licensing;
- containment behind C8.

Mapbox remains a long-term direction, not a first-slice dependency.

A package choice must stop for owner review if it introduces material licensing consequences, provider-specific domain coupling, or an irreversible architecture.

---

# 24. Greenfield Development Assumption

This is a new implementation. No application environment or project scaffolding is assumed to exist.

Before or alongside the first repository delivery increment, AL-0003 may require environment-readiness work such as:

- Flutter SDK installation and validation;
- Android Studio and Android SDK setup;
- command-line tools;
- `flutter doctor` resolution;
- Android emulator setup;
- connection of a physical Android device and USB debugging;
- verification of local build/run and IDE integration;
- documentation of material environment blockers.

Such work may be tracked as a task or checklist without a PR when it changes no repository files.

The number of AL-0003 tasks/issues does not have to equal the number of planned repository delivery increments or PRs.

---

# 25. Repository Delivery Increments

The slice is expected to use seven principal repository delivery increments, normally separate Draft PRs. They are not a mandatory one-to-one issue structure.

Every increment must leave the repository buildable and the application or its tested core in a coherent state.

## Increment 1 — Flutter shell, Flight Screen composition, and simulation controls

Includes:

- Flutter project foundation;
- Android development target;
- iOS-compatible project structure without iOS acceptance;
- temporary direct Flight Screen entry;
- spatial placeholder;
- centred pilot marker;
- status/time, GS, altitude, and contextual zones;
- warning placeholders;
- simulation panel;
- Start/Pause, `1×/2×`, Reset;
- virtual/manual clock;
- diagnostic snapshot skeleton;
- formatting, analysis, tests, and Android debug build in CI.

Observable result: recognizable Flight Screen shell and working development session controls.

## Increment 2 — Deterministic simulator and normalized live presentation

Includes:

- declarative scenario;
- privileged truth;
- C4/C5 contracts;
- GNSS, pressure, raw magnetic-orientation, and weather streams;
- C7 Device Magnetic Azimuth to Device True Azimuth conversion through the replaceable declination-provider boundary;
- timing, quality, validity, and provenance;
- fixture cadence and timing tests;
- movement on placeholder canvas;
- live GS and weather-wind presentation;
- source-health diagnostics.

Observable result: Start runs the physical scenario and updates Product UI from source-equivalent inputs, without Flight lifecycle creation.

## Increment 3 — Takeoff detection and active Flight lifecycle

Includes:

- C2 state;
- C3 Flight lifecycle;
- C6 takeoff candidate and confirmation;
- weather headwind correction;
- effective boundary and bounded history;
- Flight identity and Takeoff Point;
- recording initialization seam;
- elapsed Flight time;
- Device True Azimuth-up ground presentation to Track-up airborne transition;
- one-shot and idempotency tests.

Observable result: automatic transition from `Waiting for Takeoff` to active Flight.

## Increment 4 — Altitude, VS, estimated wind, landing, and completion

Includes:

- versioned pressure/QNH altitude contract and round-trip fixture tests;
- height above takeoff;
- VS fit;
- circle-fit estimator and quality gates;
- accepted/retained/unavailable states;
- estimated AS;
- landing candidate and confirmation;
- Landing Point;
- completed lifecycle;
- VS and compact estimated-wind presentation.

Observable result: the scenario automatically completes one Flight from takeoff through landing.

## Increment 5 — Real map adapter and spatial presentation

Includes:

- replaceable C8 adapter;
- selected OSM-compatible implementation;
- fixed physical viewport scale;
- centred pilot;
- Device True Azimuth-up/Track-up behavior and fallbacks;
- north/orientation cue and scale;
- map-unavailable canvas;
- real-device map performance check.

Observable result: real map replaces the placeholder without changing lifecycle or overlay contracts.

## Increment 6 — Progressive in-memory recording and Summary

Includes:

- C9 initialization;
- bounded preconfirmation history;
- two-layer record;
- effective boundaries and final tail;
- complete/degraded/failed outcomes;
- immutable finalized record;
- Summary from record;
- Reset/discard semantics.

Observable result: normal Flight ends in a complete Summary derived from the finalized record.

## Increment 7 — Degradation and final acceptance evidence

Includes:

- five-second GNSS outage and recovery;
- degraded record and Summary;
- no distance interpolation;
- retained wind and suspended landing detection;
- map-unavailable run;
- playback, Pause, wall-clock, monotonic-invalidity, magnetic-declination, truth-leakage, and end-to-end tests;
- concise validation instructions.

Observable result: both pilot-visible success and deterministic boundary/degradation evidence exist.

## 25.8 Task/PR flexibility

AL-0003 may also include:

- environment tasks without PRs;
- bounded research or qualification tasks;
- device-validation tasks;
- review and correction tasks;
- follow-up PRs when an increment is too large;
- more than one PR under an issue when explicitly justified.

Repository increments describe delivery order, not a complete backlog or rigid issue topology.

---

# 26. Scope Control and Stop Conditions

Each implementation issue must state:

- goal;
- included work;
- non-goals;
- referenced plan sections;
- expected tests;
- observable result;
- stop conditions.

Implementation must stop for owner decision when work would:

- change accepted product semantics;
- broaden the slice;
- introduce durable persistence;
- add live sensors or background services before authorized;
- introduce Route, fuel, Home, or Pre-Flight behavior;
- create an alternative lifecycle or simulator-only product path;
- make a material provider/licensing decision;
- couple domain logic to plugin or UI lifetime;
- require a difficult-to-reverse architecture not authorized here;
- contradict Product Vision, Product Direction, Current State, ITERATION, or this plan.

Local tuning may proceed without owner decision only when it preserves accepted semantics, target ranges, deterministic evidence, and scope.

---

# 27. Post-Slice Flutter Qualification Path

These are engineering phases, not pre-created iterations or issues.

## Phase A — Stabilize the simulation-driven slice

- complete the repository increments;
- correct lifecycle, algorithm, and presentation defects;
- obtain reproducible normal and degraded evidence.

## Phase B — Android sensor integration

Validate real Android:

- position;
- GS;
- Track;
- horizontal, speed, and course accuracy;
- source timestamps;
- cadence, gaps, and batching;
- Device Magnetic Azimuth and orientation accuracy;
- pressure;
- recovery behavior.

A custom Android adapter may be introduced if generic plugins do not satisfy the C4 contract.

## Phase C — Android background continuity

Validate:

- app switching;
- incoming call;
- screen lock;
- foreground service;
- extended background;
- UI recreation;
- native buffering;
- process-termination boundaries;
- recovery.

## Phase D — iOS adaptation and validation

Validate:

- Flutter presentation on iOS;
- Core Location semantics;
- Device Magnetic Azimuth and pressure;
- timestamps and accuracy;
- background location;
- suspension/restoration;
- need for native-side buffering.

Flutter becomes a strategic choice only after map, sensor-contract, and continuity risks are sufficiently qualified on both target platforms.

---

# 28. Implementation-Readiness Criteria

The plan is ready for AL-0003 when all criteria below are satisfied.

## 28.1 Product readiness

- pilot-visible outcome is approved;
- included scope and explicit non-goals are approved;
- Flight Screen semantics are approved;
- weather-source and estimated-wind meanings are separated;
- complete, degraded, and failed outcomes are defined.

## 28.2 Domain readiness

- C1–C10 responsibilities are explicit;
- takeoff and landing authority chains are explicit;
- effective and confirmation boundaries are distinct;
- AS/GS/VS, Air Heading/Track, Device Magnetic/True Azimuth, and MSL/relative-height distinctions are explicit;
- record and Summary semantics are explicit.

## 28.3 Simulation readiness

- origin and surface altitude are known;
- flight profile and phases are accepted;
- natural deterministic variation is specified;
- flare/float/touchdown behavior is specified;
- truth isolation is explicit;
- remaining numeric choices are bounded tuning rather than hidden product questions.

## 28.4 Technical readiness

- provisional Flutter decision is accepted;
- plugin/platform boundaries are explicit;
- map implementation is replaceable;
- Flight and recording state are independent from widget/map lifetime;
- repository increments and material stop conditions are explicit;
- greenfield environment work is acknowledged.

## 28.5 Validation readiness

- normal run is defined;
- GNSS-outage and map-unavailable cases are defined;
- diagnostics contract is defined;
- truth-leakage prohibitions are defined;
- tests do not depend on rendered screen text.

## 28.6 Governance readiness

- this plan is owner-approved and merged;
- AL-0003 charter is separately owner-approved;
- AL-0003 is activated only through the transition rule;
- implementation issues are created/activated at the correct time;
- each issue explicitly references this plan.

---

# 29. Definition of Done for the Implemented Slice

## 29.1 Build and execution

- Flutter application runs on the target Android development emulator or device;
- formatting, static analysis, tests, and Android debug build pass;
- normal scenario can be repeated after Reset;
- iOS runtime acceptance is not required in this slice.

## 29.2 Product-visible behavior

- initial state is `Waiting for Takeoff`;
- ground movement is visible;
- takeoff is detected automatically;
- active Flight is created only through C2/C3 authority;
- GS, altitude MSL, VS, and elapsed Flight time are shown;
- weather wind disappears after takeoff;
- estimated wind appears only after acceptance;
- map/orientation behavior follows the contract;
- landing is detected automatically;
- Summary appears only after finalization;
- Reset creates a new development session.

## 29.3 Domain behavior

- no Flight exists before takeoff authorization;
- only C3 creates Flight identity and lifecycle;
- detector events are one-shot;
- recording failure does not alter completed lifecycle truth;
- effective boundaries drive Flight metrics;
- confirmation tails are retained;
- no second Flight begins in the current development session.

## 29.4 Simulation integrity

- normal concerns cannot read truth;
- physical liftoff/touchdown do not command detectors;
- `1×` and `2×` produce equivalent domain outcomes;
- Pause does not appear as source outage;
- fixture is deterministic;
- no uncontrolled randomness is used.

## 29.5 Derivation

- altitude derives from pressure and QNH using the specified units, formula, constants, and versioned calculation context;
- simulator pressure generation round-trips through the same contract without exposing truth altitude to C7;
- Device True Azimuth is derived by C7 from raw Device Magnetic Azimuth plus east-positive declination obtained through the replaceable provider;
- C8 never receives raw magnetic orientation as a ready-made True-North value;
- VS is unavailable with insufficient history;
- wind is accepted only through quality gates;
- rejected candidates do not overwrite accepted wind;
- landing detection does not use truth wind;
- no GS-only landing fallback exists.

## 29.6 Recording and Summary

- C9 creates the approved two-layer record;
- Summary derives only from finalized record;
- complete, degraded, and failed outcomes are distinguishable;
- distance does not interpolate across GNSS gaps;
- duration uses monotonic effective boundaries;
- wall-clock change does not alter duration;
- missing, duplicate, or backward monotonic time cannot be treated as valid ordering or duration;
- accepted QNH and altitude-calculation context are retained with the derived altitude history.

## 29.7 Degradation

- map unavailability does not stop Flight;
- five-second GNSS outage does not complete Flight;
- the same Flight continues after recovery only under the explicit bounded P3 reopening for this validation case;
- accepted wind is retained;
- landing detection is suspended during outage;
- recovery continues the same Flight;
- final record is degraded;
- failed recording shows completed Flight without false successful retention.

## 29.8 Evidence

- automated normal end-to-end test exists;
- controlled GNSS-outage test exists;
- map-unavailable validation exists;
- detector boundary tests exist;
- wind numerical tests exist;
- pressure/QNH round-trip tests exist;
- magnetic-declination conversion and fallback tests exist;
- monotonic-invalidity tests exist;
- truth-leakage tests exist;
- diagnostics explain significant transitions.

---

# 30. Bounded Implementation Tuning

The following do not require a new owner decision when accepted semantics remain unchanged:

- exact scenario phase durations;
- exact leg lengths and turn radii;
- exact deterministic yaw functions;
- exact AS/altitude variation functions;
- exact sensor-like variation functions;
- absolute truth-wind direction;
- exact first accepted-wind time;
- Pratt versus Taubin initialization;
- exact numerical quality thresholds;
- exact Track-loss grace period;
- exact flare curve and touchdown coordinates;
- exact GNSS-outage timestamp;
- exact Flutter map package within the stated stop conditions;
- pixel geometry, typography, spacing, and animation;
- exact compact estimated-wind placement.

Tuning becomes an owner decision when it changes product meaning, authority, scope, accepted outcome, or difficult-to-reverse technical direction.

# 31. Explicitly Deferred Decisions

- production detector thresholds;
- live Android source implementation;
- Android background architecture;
- iOS source and background implementation;
- final map provider and Mapbox integration;
- durable persistence, schema, and migrations;
- saved-Flight reopening and replay;
- multiple Flights in one implemented Flight Mode session;
- passive Takeoff Point navigation awareness;
- Route and Active Navigation;
- fuel;
- terrain AGL;
- airspace;
- production warning policy;
- general interruption/P3 retention and recovery behavior beyond the explicitly bounded five-second GNSS case;
- long GNSS-loss handling;
- monotonic-clock discontinuity recovery;
- production magnetic-declination model/provider, model updates, and offline geomagnetic data;
- process-killed Flight recovery;
- final visual design and accessibility policy;
- settings/units system;
- complete application navigation;
- complete AirLink architecture.

# 32. AL-0003 Boundary

AL-0003 should implement the approved selected slice through bounded work without expanding beyond this plan.

The first repository implementation issue should cover Increment 1 only. An environment-readiness task may precede it and need no PR.

Later issues are prepared or activated incrementally as the active iteration permits. AL-0002 does not need to create a complete backlog or force issue count to match PR count.

# 33. Product Direction Alignment

## Direction advanced

The slice advances the Flight Support pillar through one coherent map-centred Flight that converts controlled source inputs into understandable pilot-facing information, a completed lifecycle, and a retained result.

## Explicit simplifications

The slice omits preparation, durable history, Route, fuel, multiple Flights per session, live platform integration, and broader application flow. It also uses provisional Flutter and a replaceable first map implementation.

## Reversibility

- no durable schema is selected;
- source, map, and platform integrations are adapters;
- domain semantics do not depend on Flutter widgets or plugins;
- simulator truth does not enter normal product paths;
- provider-specific types do not define product meaning;
- deferred domains are not collapsed into the first-slice model.

## Outcome

`Aligned with explicit simplification`.

# 34. Final Owner Decisions

The following issue #37 alignment choices are explicitly owner-approved.

## 34.1 Estimated-wind notice is intentionally minimal

The final product must explain the estimated nature and limitations of in-Flight wind information, potentially through a required acknowledgement in an appropriate non-flight flow. The first slice is simulation-only and implements only a concise ground-state notice such as `In-flight wind is estimated.` It includes no acknowledgement checkbox, consent workflow, blocking explanation, or persistent in-Flight disclaimer.

## 34.2 Passive Takeoff Point awareness is deferred

C3 creates Takeoff Point and C9 retains it, but the first slice does not establish or present Takeoff Point as passive Current Waypoint and does not show its marker, distance, or bearing. This is an explicit first-slice simplification relative to the Navigation WIP and the earlier selection artifact.

The simplification prioritizes broad validation of the fundamental Flight lifecycle, source, derivation, spatial, recording, and completion model over visible but non-foundational navigation context. It does not remove Takeoff Point or passive awareness from the broader product direction.

## 34.3 Five-second GNSS interruption has a bounded P3 reopening

The owner explicitly reopens the previously deferred P3 interruption boundary only for the deterministic five-second GNSS-outage validation case defined by this plan. After the bounded outage, the same Flight continues, C9 records the gap, the finalized recording outcome is `degraded`, and Summary remains available.

This decision does not define general interruption recovery, long-loss behavior, retention after an unresolved interruption, process termination, restoration guarantees, or production P3 semantics. Any extension beyond the exact bounded case requires a separate owner decision.

## 34.4 Magnetic orientation remains source-equivalent

The simulator supplies Device Magnetic Azimuth through C4 rather than supplying a ready-made Device True Azimuth. C4 preserves the raw source meaning; C7 owns magnetic-declination lookup and conversion to True North; C8 consumes the derived Device True Azimuth on the ground. The deterministic fixture uses a synthetic non-zero east declination so validation proves that the product performs the conversion.

Production geomagnetic-model selection and update strategy remain deferred.

All other unresolved values in this document are classified as bounded implementation tuning or explicitly deferred decisions.
