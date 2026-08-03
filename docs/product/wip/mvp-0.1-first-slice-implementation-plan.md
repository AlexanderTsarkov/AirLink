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

A complex graduated compass ring and pilot-controlled scale are deferred. The slice retains one simple, thin orientation circle centred on the pilot, approximately `80%` of screen width, with no degree scale, dense ticks, cardinal labels, or additional rings. It provides a bounded visual frame for orientation and the estimated-wind comprehension experiment.

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
- a compact simulation panel and replaceable structured developer observability;
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
- **Virtual civil UTC / AirLink-facing wall-clock time:** scenario-derived civil time normalized by C4 and used for displayed and retained takeoff/landing timestamps; host wall time is not authoritative.

Calculations, windows, ordering, Flight duration, and elapsed Flight time use monotonic time, not wall clock. Monotonic validity is evaluated within a development session and within each source stream; Reset begins a new session and therefore a new monotonic domain.

# 7. Slice Inputs and Outputs

## 7.1 Inputs

Normal first-slice inputs are:

- GNSS position;
- GS and Track;
- horizontal, speed, and course accuracy where available;
- Device Magnetic Azimuth and orientation quality where available;
- atmospheric pressure;
- weather-source wind and QNH;
- source monotonic time, observed monotonic time, and source-equivalent virtual civil UTC normalized by C4;
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

C4 accepts independent asynchronous streams, including a source-equivalent `gnssAvailability` stream that is distinct from GNSS observations. C4 normalizes explicit availability transitions immediately and derives position, GS, and Track availability from them; downstream concerns observe those effects only through C4-derived state. For simulation, C4 also normalizes the source-equivalent virtual civil timestamp defined in section 11.3 into the AirLink-facing wall-clock value used for civil display and retention. Domain logic must not require a fixed sensor frequency. C4 does not calculate magnetic declination or convert magnetic orientation to True North; it preserves the source meaning and supplies the independent position, civil-time, and orientation inputs required by C7.

For `scenario-v1`, C4 preserves the source Track availability emitted by C10. An exact-zero truth ground-velocity sample emits Track as unavailable even if source GS error produces a positive source GS. C4 receives no truth-vector shortcut and does not synthesize Track from that positive source GS.

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
- screen-oriented visible map attribution and its accessible licence/source action;
- package-specific viewport calculation required to preserve the fixed target visible ground width;
- map-unavailable/degraded spatial state.

C8 consumes Device True Azimuth on the ground and Track in Flight. It does not calculate magnetic declination or reinterpret raw magnetic orientation.

C8 is the complete replacement boundary for the bounded `flutter_map` and OpenStreetMap Standard raster implementation defined in section 23.3. No `flutter_map`, URL-launcher, tile-provider, attribution-widget, URL, HTTP-client, or package-lifecycle type or detail may escape C8. C1–C7, C9, and C10 use only neutral AirLink/C8 presentation contracts such as geographic centre, viewport orientation, target visible ground width, centred pilot marker, orientation circle, estimated-wind overlay state, spatial availability/degradation state, and any already-approved neutral special-point state. The OSM copyright URL and external-link realization remain internal implementation details rather than neutral-contract values.

C8 does not own Flight lifecycle, Takeoff Point identity, current-value derivation, Route navigation, or scenario-controlled map configuration.

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
- weather-source wind is available in the primary left tile while GS and Flight VS are absent as primary values;
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
- flown distance;
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
- one simple orientation circle approximately `80%` of screen width;
- orientation behavior;
- north/orientation cue;
- scale indicator.

### Product overlay layer

- Flight state or elapsed time;
- transient flown-distance presentation;
- a primary left tile that shows weather-source wind on the ground and switches to GS only after confirmed takeoff;
- altitude MSL;
- VS only during active Flight;
- estimated-wind information only in the separate orientation-circle/windsock context after estimator acceptance;
- source/map warnings.

### Development-only simulation layer

- Start/Pause;
- `1×/2×`;
- Reset;
- scenario phase and time;
- compact concern/source state.

Simulation controls must not appear to be future pilot-facing product controls.

## 10.3 Spatial geometry

The pilot marker remains at the geometric centre of the full map viewport, including the area behind the temporary bottom simulation panel.

The first slice uses one fixed spatial scale:

```text
targetVisibleGroundWidthM = 2000.0
accepted tolerance = ±2%
```

Visible ground width is the ground distance between the geographic positions under the left and right edges of the full logical map viewport along its horizontal centreline, with the pilot at the viewport centre. The full logical viewport includes the area behind the temporary bottom simulation panel.

C8 computes the package-specific fractional zoom required to maintain the target for the current full logical viewport width, the current pilot/centre latitude, and the selected Web Mercator raster renderer. A layout-size change may cause C8 to recalculate that fractional zoom only to preserve the same target. Track-up rotation changes orientation, not the accepted spatial scale.

The same `2000 m ±2%` target applies on the ground, during active Flight, after GNSS recovery, regardless of live or simulated provenance, and independently of scenario metadata. The first slice does not use automatic or speed-dependent zoom, a readability fallback, user pan/zoom, glide-range-driven zoom, scenario-controlled zoom, or C10-controlled map scale.

## 10.4 Primary overlay zones

The conceptual top layout is:

```text
Ground:
[ WEATHER WIND ] [ ALT ] [ no Flight VS ]

Active Flight:
[ GS ]           [ ALT ] [ VS ]
```

Exact geometry, typography, spacing, and colors are bounded UI tuning.

### Camera-adjacent status / Flight-progress zone

For the initial portrait prototype, the compact top-centre status zone uses the oval area around the camera cutout where the device layout permits it. A device without that geometry uses the same logical top-centre zone.

- before takeoff: `Waiting for Takeoff`;
- after takeoff, the default presentation is `FLT mm:ss`, using elapsed Flight time from the effective takeoff boundary;
- whenever cumulative flown distance crosses the next `500 m` threshold, the zone temporarily shows `DST` with the crossed distance for `3 s`, then returns to `FLT`;
- the normal approximately `1.92 km` fixture therefore produces bounded notifications at approximately `0.5 km`, `1.0 km`, and `1.5 km`;
- `ELT` is not used as the elapsed-time label because it is an established aviation abbreviation for Emergency Locator Transmitter;
- after landing: no technical lifecycle/finalization messages; transition to completed outcome after C9 finalization.

### GNSS distance-continuity segments

For both active flown distance and finalized Summary distance, `consecutive valid GNSS positions` means adjacent accepted positions inside the same uninterrupted GNSS distance-continuity segment. A segment ends when C4 reports GNSS unavailable, a required GNSS position is invalid, C4 records an explicit GNSS source interruption or continuity gap, or the stream enters the source-monotonic invalid/discontinuous state defined by this plan. An identical same-timestamp redelivery that is ignored idempotently does not end the segment.

After a segment ends, the first valid recovered GNSS position opens a new segment and becomes its distance anchor. No distance is added from the final valid position of the previous segment to that recovered position; accumulation resumes only when a subsequent valid position is accepted in the new segment. Conceptually, `P1 → P2 → P3 | GNSS gap | P4 → P5` includes `distance(P1, P2) + distance(P2, P3) + distance(P4, P5)` and excludes `distance(P3, P4)`. This slice does not interpolate, dead-reckon, integrate GS, reconstruct a route, or estimate missing distance. The resulting distance can therefore be lower than the unknown real flown distance, and the degraded recording/Summary status communicates the incomplete GNSS coverage.

Observed delivery timing alone does not end a segment. Late or batched observations remain continuous when their source-monotonic timestamps are valid and ordered, C4 records no unavailable, invalid, interruption, or continuity-gap interval, and the observations preserve continuous source semantics. Nominal cadence, `1 Hz` test transforms, deterministic jitter, and batching do not by themselves define a gap; continuity follows normalized source availability, validity, interruption, and source-time semantics rather than wall-clock arrival spacing.

The active flown-distance aggregate starts at the effective takeoff boundary and applies that segment rule only to accepted position pairs. It holds its current value and is marked stale/degraded during GNSS unavailability, opens a new anchor after recovery, and never joins the final pre-gap position to the first post-gap position. The existing transient `DST` threshold behavior is unchanged. This runtime presentation aggregate is not the authoritative Summary source; completed distance is derived independently from the finalized C9 record using the same segment semantics.

For every eligible accepted latitude/longitude pair `p1` and `p2`, both derivations use the same fixed first-slice haversine algorithm. With the exact mean Earth radius `distanceEarthRadiusM R = 6371008.8`, convert both coordinates to radians and calculate:

```text
deltaPhi    = phi2 − phi1
deltaLambda = shortest signed longitude difference in radians

h
=
sin(deltaPhi / 2)^2
+
cos(phi1) × cos(phi2) × sin(deltaLambda / 2)^2

h = clamp(h, 0, 1)

centralAngle
=
2 × atan2(sqrt(h), sqrt(max(0, 1 − h)))

distanceM
=
R × centralAngle
```

This pairwise formula is applied only after the segment rule has established pair eligibility. Active distance and finalized Summary distance derive independently, but use this same formula and eligibility rule. Neither derivation may use simulator truth distance, integrated truth East/North distance, GS integration, route reconstruction, or interpolation. The calculation identifier is `haversineMeanEarthR6371008_8V1`; C9 retains it with the distance-calculation context, and validation diagnostics expose it for the active aggregate.

### Primary left tile

Before confirmed takeoff, the primary left tile displays weather-source wind and Ground Speed is absent as a primary value. It shows the weather wind's direction and speed with its source-state semantics. Unavailable, valid zero, stale/degraded, and valid weather states remain distinguishable; estimated wind is never substituted for weather wind.

After confirmed takeoff, the same tile switches to:

- `GS`;
- Ground Speed;
- unit.

The switch occurs only from the confirmed C2/C3 Flight transition. Product UI does not show both primary GS and weather wind before takeoff, and weather-source wind disappears after takeoff.

### Altitude tile

Displays:

- `ALT` or `ASL`;
- barometric altitude MSL;
- unit.

### Contextual right tile

Before confirmed takeoff it does not display Flight VS. Estimated wind is not moved into this tile.

After confirmed takeoff it displays VS:

- `VS`;
- signed value;
- `m/s`.

## 10.5 Estimated-wind presentation

Estimated wind is mandatory but does not replace VS in the contextual right tile.

The first slice retains one simple, thin orientation circle centred on the pilot and approximately `80%` of screen width. It is not a full graduated compass ring: it has no degree scale, dense ticks, cardinal labels, or additional rings. The existing north/orientation cue remains sufficient.

Before the first accepted estimate:

- no windsock-like glyph or numeric value is shown as estimated wind;
- `0` is not shown;
- weather wind is not substituted;
- persistent `Estimating…` is not used as the value.

After acceptance, Product UI shows a bounded windsock-like comprehension experiment inside the orientation context:

- the wide mouth/head of the windsock is centred on the pilot/map centre;
- the pilot marker is rendered above it or retains clear central separation;
- the windsock body extends downwind from the centre, so the opposite direction is the intuitive into-wind direction;
- its screen-relative angle is calculated from the accepted estimated-wind vector and the current C8 orientation frame;
- visible body length or simple sections encode wind speed over a display range of `0–8 m/s`;
- maximum visual length remains inside the simple orientation circle;
- a visible numeric value in `m/s` remains present and is displayed at approximately `0.5 m/s` granularity;
- display rounding does not quantize the estimator's retained value;
- above `8 m/s`, visual length is capped while the numeric value continues to show the rounded estimated result;
- warning colour, blinking, operational thresholds, and safety policy above the display range are deferred.

An accepted valid zero-wind estimate is distinguishable from unavailable state: it shows `0.0 m/s` with a minimal/collapsed glyph, while unavailable state shows neither a false zero nor a windsock.

Exact stroke, section count, dimensions, colours, animation, and placement within the fixed orientation context are bounded UI tuning. Weather-source wind and estimated wind remain visibly distinct. Technical state, age, fit window, rejection reasons, and truth comparison remain in replaceable developer diagnostics.

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

- Start advances scenario time and virtual civil time together;
- Pause freezes scenario time and virtual civil time;
- `2×` changes wall-duration playback but not source sequence or domain result;
- `2×` changes only how quickly scenario time advances relative to host elapsed time and does not alter the mapping from scenario/source time to virtual civil UTC;
- Reset returns scenario time to `0.0 s`, restores `wallClockOffsetS = 0`, and restores virtual civil time to `civilStartUtc`;
- Pause is not an input outage.

## 11.2 Source and observed time

Each relevant observation carries source monotonic time. AirLink also records observed monotonic time where delivery delay or batching matters.

C4 exposes timestamps used together for domain calculations in one normalized development-session monotonic domain. If an adapter receives a source-native clock with different origin or units, the adapter maps it explicitly and preserves source timing context where required for diagnosis.

Within one source stream and one development session, source monotonic timestamps must be present and must not move backwards. Independent streams may legitimately contain equal timestamps.

Two same-stream observations with the same source monotonic timestamp are handled as follows:

- an **identical redelivery** — the same normalized value and metadata delivered again — is ignored idempotently, does not advance a window or timer, is not retained twice, and does not degrade the Flight;
- a **timestamp collision** — different normalized value or metadata at the same timestamp — is invalid and fails closed for the affected stream.

If a required monotonic timestamp is missing, decreases, or forms a timestamp collision:

- the affected observation is invalid for ordering and time-based calculation;
- detector timers and derivation windows that depend on the affected stream are invalidated rather than continued across the discontinuity;
- no takeoff or landing confirmation, VS result, wind estimate, Flight duration, or retained ordering may be reported as valid across the discontinuity;
- lifecycle must not silently transition to landing, completion, rejection, or another state because of the clock defect;
- C9 records the clock-invalid interval and exposes degraded or incomplete recording state;
- the first-slice validation case stops before assigning a successful completed Summary or general recovery meaning.

Production recovery from monotonic-clock failure remains deferred.

Logic uses time windows and durations, not sample counts.

## 11.3 Wall clock

The deterministic first-slice virtual civil-time contract is independent from host wall time:

```text
virtualCivilUtc(t)
=
civilStartUtc
+ scenarioTimeS(t)
+ wallClockOffsetS(t)
```

For the normal scenario, `wallClockOffsetS(t) = 0`, so `virtualCivilUtc(t) = civilStartUtc + scenarioTimeS(t)`. The host computer or device wall clock is not a source of scenario civil timestamps.

Each source-equivalent observation retains the virtual civil timestamp corresponding to its source scenario time. Batched or delayed delivery does not rewrite that timestamp. C4 normalizes the source-equivalent virtual timestamp into the AirLink-facing wall-clock value used for civil display and retention. Observed monotonic delivery time remains separate and does not redefine civil time.

Any optional diagnostic host-arrival timestamp is non-authoritative and must not be used for detector timing, Flight duration, VS, wind windows, ordering, effective boundaries, or retained Flight civil timestamps.

The exact wall-clock-jump validation transform is:

```text
jumpSourceMonotonicTimeS = 80.0
jumpAmountS = +3600.0

for t < 80.0:
    wallClockOffsetS(t) = 0

for t >= 80.0:
    wallClockOffsetS(t) = 3600.0
```

The jump applies prospectively. Civil timestamps already emitted or retained before `80.0 s` are not rewritten. The Takeoff Point remains pre-jump; the Landing Point and its retained civil timestamp are post-jump and include the `+3600 s` offset. Summary displays those retained boundary timestamps. Monotonic Flight duration, source ordering, detector holds, VS, and wind windows remain unchanged, and `1×` and `2×` produce identical civil timestamps for identical source times.

The civil timestamp for a Takeoff Point or Landing Point is the C4-normalized virtual civil timestamp associated with the accepted source observation at its effective boundary, not host arrival time or confirmation wall time. This transform does not alter source monotonic time and does not introduce general timezone, locale, DST, NTP, or production clock-recovery architecture.

## 11.4 Asynchronous streams

C4 accepts independent streams for:

- GNSS;
- pressure;
- orientation;
- platform/input state.

C5 supplies weather context independently.

Requested cadence is an adapter hint, not a domain requirement. Actual cadence and gaps are observed and retained.

## 11.5 Fixture cadence

The normal fixture uses exactly:

- GNSS: `2 Hz`;
- pressure: `10 Hz`;
- orientation: `10 Hz`;
- weather and QNH: one initial snapshot at scenario time `0.0 s`.

Required tests additionally include:

- GNSS at `1 Hz`;
- deterministic jitter;
- missing samples;
- batched delivery preserving source timestamps.

Insufficient cadence or gaps become quality/degraded state rather than silently changing semantics.

## 11.6 Source-time predicate holds

All detector holds use one reusable primitive evaluated from accepted normalized source observations. For predicate `P` and duration `D`:

1. the hold begins at source monotonic time `t0` of the first accepted relevant evaluation for which `P` is true;
2. it completes at the first later accepted evaluation at source monotonic time `t` for which `P` remains true and `t - t0 >= D`;
3. every accepted relevant evaluation from `t0` through `t` must satisfy `P`;
4. an accepted relevant evaluation for which `P` is false immediately resets that hold;
5. an explicit required-input unavailable, invalid, continuity-gap, or source-monotonic-invalid/discontinuous boundary resets every dependent hold;
6. identical idempotent redelivery does not advance or reset a hold;
7. observed delivery time, batching, host wall time, and playback speed do not affect a hold;
8. missing delivery alone does not reset a hold unless C4 records unavailable, invalid, gap, or discontinuity state;
9. elapsed source monotonic time, never sample count, defines completion;
10. the endpoint comparison is inclusive: `t - t0 >= D`.

Takeoff and landing evaluate this primitive on accepted normalized GNSS evaluations using the current valid synchronized state required by the relevant detector.

One accepted evaluation may complete a hold that activates a new detector state and then be evaluated once as the first possible observation of the next state's hold when the detector-specific processing order says so. It cannot contribute elapsed duration before its own source timestamp, and identical redelivery cannot repeat either transition or advance either hold.

# 12. Deterministic Simulation Contract

## 12.1 Exact scenario asset

The slice uses exactly one bundled, versioned, declarative, read-only JSON asset:

```text
assets/scenarios/mvp_0_1_first_slice_v1.json
```

The asset has:

```text
schemaVersion: 1
scenarioId: "mvp-0.1-first-slice-v1"
civilStartUtc: "2026-08-03T09:00:00Z"
truthStepS: 0.1
```

There is no scenario selector, editor, remote distribution, or scenario-specific product branching.

The JSON top level must contain exactly these logical groups:

- `schemaVersion`;
- `scenarioId`;
- `civilStartUtc`;
- `units`;
- `truthStepS`;
- `origin`;
- `environment`;
- `sourceCadenceHz`;
- `variationProfiles`;
- `phases`;
- `faultVariants`.

The logical field names define the required asset contract. Code may use typed model names internally, but it must parse this format without inventing additional required scenario semantics.

Each `phases` item contains exactly:

```text
id
startS
endS: number | null
motionMode: ground | airborne | airborneUntilEnd
headingSegments[]
speedSegments[]
altitudeSegments[]
variationProfileIds[]
```

Every non-terminal phase has finite numeric `startS` and `endS`, with `endS > startS`. Exactly one phase may use `endS: null`: it must be the final phase, have `id: completed_ground`, and represent the half-open interval `[startS, +∞)`. No phase may follow it. Reset, not an artificial terminal time, ends this held state. The parser must reject `null` on a non-final phase, more than one open-ended phase, any phase after the open-ended phase, string or sentinel encodings such as `"212.0+"`, and a terminal phase with incompatible non-hold values.

Each segment contains:

```text
startS
endS: number | null
profile: hold | linear | smoothstep | turnSmoothstep | flareV1
startValue
endValue
```

Only terminal hold segments belonging to `completed_ground` may use `endS: null`, subject to the same final/open-ended rules.

A heading segment additionally carries `reference: true` and, for `turnSmoothstep`, an explicit signed `turnDeltaDeg`. A speed segment carries `quantity: AS | GS` and `unit: kmh`. An altitude segment carries `reference: MSL` and `unit: m`. The exact values and segment boundaries are defined by the normative phase table below.

`faultVariants.gnssOutage` contains this exact logical representation:

```text
startS: 112.0
endExclusiveS: 117.0
availabilityTransitions:
  - atS: 112.0
    state: unavailable
    reason: controlledInterruption
  - atS: 117.0
    state: available
    reason: controlledRestoration
```

The fields, values, and transition order are normative. This is sufficient to generate the separate source-equivalent `gnssAvailability` stream deterministically and does not create a generic configuration subsystem. `sourceCadenceHz` contains `gnss`, `pressure`, and `orientation`; weather/QNH is represented by the initial snapshot in `environment`.

## 12.2 Units and deterministic evaluation

The asset uses:

```text
time: seconds
horizontal distance: metres
altitude: metres MSL
AS and GS: kilometres per hour in the asset
wind: metres per second
angles: degrees clockwise from True North unless explicitly magnetic
pressure and QNH: hPa
```

Truth is evaluated at fixed `0.1 s` steps. Airborne East/North position is integrated with the trapezoidal rule from the truth ground-velocity vector. Ground phases use the declared ground-speed and non-zero movement-direction profiles without adding wind drift; source Track availability follows the exact rule below. Source streams sample this truth at their exact declared cadences.

All truth-level phase variations are applied first. For each source field that is available at a sample, its declared source-error function is evaluated at that sample's scenario/source monotonic time `t`, after the corresponding ideal truth/source-equivalent quantity is calculated and before the value enters C4. No intermediate value is rounded, no declared error profile may be unused across the scenario's applicable samples, and no source error is evaluated to create a value when the corresponding phase/source field is unavailable.

At each GNSS sample time, C10 must generate the source-equivalent position in this exact order:

1. evaluate and integrate truth ground displacement in local East/North metres;
2. evaluate `gnssEastErrorM(t)` and `gnssNorthErrorM(t)`;
3. calculate `sourceEastM = truthEastM + gnssEastErrorM(t)` and `sourceNorthM = truthNorthM + gnssNorthErrorM(t)`;
4. convert `sourceEastM` and `sourceNorthM` to latitude/longitude using section 12.3;
5. emit that latitude/longitude observation through the normal C10 to C4 GNSS boundary.

GNSS GS is generated from the truth ground-velocity vector, not by differentiating noisy positions:

```text
truthGsKmh
=
hypot(truthVelocityEastMps, truthVelocityNorthMps) × 3.6

sourceGsKmh
=
max(0, truthGsKmh + sourceGsErrorKmh(t))
```

Track availability is determined before Track source error is applied, using the phase contract and the ideal truth ground-velocity magnitude:

```text
if the phase explicitly declares Track unavailable:
    source Track is unavailable

else if truthGsKmh == 0.0:
    source Track is unavailable

else:
    truthTrackDeg
    =
    normalize360(
      degrees(
        atan2(
          truthVelocityEastMps,
          truthVelocityNorthMps
        )
      )
    )

    sourceTrackDeg
    =
    normalize360(
      truthTrackDeg + sourceTrackErrorDeg(t)
    )
```

`atan2(0, 0)` is never used to create source Track. Availability is based on `truthGsKmh` before `sourceGsErrorKmh(t)`: a positive source GS created by measurement error at an exact-zero truth vector does not make Track available, and `sourceTrackErrorDeg(t)` is not evaluated to synthesize the absent field. Once ideal truth GS is greater than zero in a phase whose Track is otherwise available, Track is derived normally from the truth vector and its source error is applied. No epsilon, speed hysteresis, production GNSS course rule, or generic platform Track threshold is introduced.

C10 must not convert truth coordinates first and add degree-space errors, round intermediate East/North values, radians, radii, latitude, or longitude, use a library whose geodesic algorithm or version can vary, or expose truth East/North directly to product logic. The implementation language's normal IEEE-754 binary64 arithmetic is sufficient for the fixture.

The following interpolation identifiers are normative:

- `hold` — start value is held;
- `linear` — linear interpolation over phase-local time;
- `smoothstep` — `3u² − 2u³`, where `u` is phase-local progress in `[0,1]`;
- `turnSmoothstep` — explicit signed turn delta multiplied by `smoothstep(u)`;
- `flareV1` — the exact flare/float sub-boundaries defined below.

## 12.3 Origin and environment

The exact fixture context is:

```text
origin.latitudeDeg: 59.4546111
origin.longitudeDeg: 24.8922111
surfaceAltitudeMslM: 35.0
qnhHpa: 1013.25
fixtureDeclinationDegEast: +10.0
truthWind.speedMps: 4.0
truthWind.fromDegTrue: 270.0
weatherWind.speedMps: 4.0
weatherWind.fromDegTrue: 270.0
```

The wind therefore blows toward `090° True`. Initial Air Heading and final approach are `270° True`, directly into wind.

Takeoff and landing areas are locally flat at `35.0 m MSL`. Terrain variation is not modelled.

### Fixed local-to-geographic conversion

`scenario-v1` uses the fixed first-slice WGS84 local-tangent approximation below relative to the declared fixture origin. The exact constants are:

```text
semiMajorAxisM a = 6378137.0
inverseFlattening = 298.257223563
flattening f = 1 / inverseFlattening
eccentricitySquared e2 = f × (2 − f)

originLatitudeDeg  = 59.4546111
originLongitudeDeg = 24.8922111

phi0    = radians(originLatitudeDeg)
lambda0 = radians(originLongitudeDeg)

M0
=
a × (1 − e2)
/
(1 − e2 × sin(phi0)^2)^(3/2)

N0
=
a
/
sqrt(1 − e2 × sin(phi0)^2)
```

For a source-like local position `eastM`, `northM`, calculate:

```text
latitudeRad
=
phi0 + northM / M0

longitudeRad
=
lambda0 + eastM / (N0 × cos(phi0))

latitudeDeg  = degrees(latitudeRad)
longitudeDeg = degrees(longitudeRad)
```

This formula is normative for the local approximately `2 km` fixture and has the calculation identifier `scenarioV1Wgs84LocalTangentV1`, which is retained or exposed in the existing scenario validation/calculation-version context. It does not select production geodesy. Production geodesic libraries, ECEF/ENU conversion, map-provider projection policy, long-distance accuracy, altitude-dependent ellipsoid treatment, and whole-product geospatial architecture remain deferred.

## 12.4 Orientation fixture

C10 holds privileged truth Device True Azimuth for scenario composition but does not provide that value directly to product logic. It generates source-equivalent Device Magnetic Azimuth through C4:

```text
sourceDeviceMagneticAzimuthDeg
=
normalize360(
  deviceTrueAzimuthTruthDeg
  − fixtureDeclinationDegEast
  + magneticAzimuthErrorDeg(t)
)
```

C4 receives this source-equivalent magnetic value. The error profile does not bypass C4 and does not provide ready-made True orientation.

C7 obtains declination through a replaceable `MagneticDeclinationProvider` using position and civil date/time, then calculates:

```text
deviceTrueAzimuthDeg
=
normalize360(deviceMagneticAzimuthDeg + declinationDegEast)
```

East declination is positive and west declination is negative. Results are normalized to `[0°, 360°)`.

The first-slice provider deterministically returns `+10.0°` for the fixture context. This synthetic value exists to prove conversion and is not a claim about production geomagnetic truth. Production WMM/IGRF selection, native APIs, model updates, altitude treatment, and offline model-data strategy remain deferred.

For scenario truth, Device True Azimuth equals the declared ground Track in ground phases and the declared Air Heading in airborne phases before source-like magnetic error is added. This does not imply that a real phone is mechanically aligned with the wing; it is a bounded first-slice fixture assumption.

## 12.5 Motion model

Before physical liftoff and after physical touchdown, pilot coordinates follow declared ground motion and wind does not add free drift.

After physical liftoff and before touchdown:

```text
ground velocity
=
air-relative movement
+
truth wind
```

C10 uses truth wind only to generate normal source equivalents. C8, C6, C7, and C9 never read truth wind.

## 12.6 Exact phase schedule

`ground_ready` is the pre-Start state and is held indefinitely while source streams remain active. Scenario time starts at `0.0 s` when Start is pressed.

| Phase id | Time, s | Motion | Air Heading / Track | AS or GS profile | Altitude MSL profile |
| --- | ---: | --- | --- | --- | --- |
| `wing_inflation_and_stabilization` | `0.0–5.0` | ground | non-zero movement Track `270°` | GS `0.0→1.5 km/h`, smoothstep | `35.0 m`, hold |
| `launch_acceleration` | `5.0–7.4` | ground | Track `270°` | GS `1.5→10.6 km/h`, smoothstep | `35.0 m`, hold |
| `liftoff_transition` | `7.4–11.0` | airborne | Air Heading `270°` | AS `25.0→31.0 km/h`, smoothstep | `35.0→38.0 m`, linear |
| `post_liftoff_acceleration` | `11.0–18.0` | airborne | Air Heading `270°` | AS `31.0→36.0 km/h`, smoothstep | `38.0→50.0 m`, linear |
| `initial_climb_upwind` | `18.0–48.0` | airborne | Air Heading `270°` | AS `36.0→40.0 km/h`, smoothstep | `50.0→122.0 m`, linear |
| `turn_north` | `48.0–63.0` | airborne | signed turn `+90°`, `270→000°` | AS `40.0 km/h`, hold | `122.0→135.0 m` during `48.0–58.0`, then hold |
| `north_crosswind_level` | `63.0–88.0` | airborne | Air Heading `000°` | AS `40.0 km/h`, hold + variation | `135.0 m`, hold + variation |
| `turn_east` | `88.0–100.0` | airborne | signed turn `+90°`, `000→090°` | AS `40.0 km/h`, hold | `135.0 m`, hold |
| `east_downwind` | `100.0–120.0` | airborne | Air Heading `090°` | AS `40.0 km/h`, hold + variation | `135.0→130.0 m`, linear |
| `turn_south` | `120.0–132.0` | airborne | signed turn `+90°`, `090→180°` | AS `40.0→41.0 km/h`, smoothstep | `130.0→113.0 m`, linear |
| `south_crosswind_descent` | `132.0–157.0` | airborne | Air Heading `180°` | AS `41.0 km/h`, hold + variation | `113.0→76.0 m`, linear |
| `turn_west_final_alignment` | `157.0–172.0` | airborne | signed turn `+90°`, `180→270°` | AS `41.0→44.0 km/h`, smoothstep | `76.0→54.0 m`, linear |
| `final_approach_upwind` | `172.0–187.0` | airborne | Air Heading `270°` | AS `44.0→45.0 km/h`, smoothstep | `54.0→36.3 m`, linear |
| `flare` | `187.0–190.0` | airborne | Air Heading `270°` | AS `45.0→22.0 km/h`, `flareV1` | `36.3→35.2 m`, `flareV1` |
| `float_and_touchdown` | `190.0–192.0` | airborne until `192.0` | Air Heading `270°` | AS `22.0→18.0 km/h`, smoothstep | `35.2→35.0 m`, smoothstep |
| `landing_run` | `192.0–197.0` | ground | Track `270°` | GS `3.6→0.0 km/h`, smoothstep | `35.0 m`, hold |
| `landed_confirmation` | `197.0–212.0` | ground | Track unavailable at zero speed | GS `0.0 km/h`, hold | `35.0 m`, hold |
| `completed_ground` | `[212.0, +∞)` | ground | Track unavailable at zero speed | GS `0.0 km/h`, hold | `35.0 m`, hold |

The `completed_ground` JSON item has `startS: 212.0` and `endS: null`; `null`, not the string `"212.0+"`, encodes the open-ended terminal interval. A declared ground direction such as `270°` describes the direction of non-zero ground movement. At exact-zero truth ground velocity, source Track is unavailable under section 12.2. This applies to `ground_ready`, the exact `0.0 s` start of `wing_inflation_and_stabilization`, the zero-speed portion of `landed_confirmation`, and all of `completed_ground`. The `turn_north` altitude profile is two deterministic subsegments encoded in that phase: linear climb to `135.0 m` at `58.0 s`, then hold. Physical liftoff occurs exactly at `7.4 s`; physical touchdown occurs exactly at `192.0 s`. Neither truth event is passed to C6.

This profile reaches the accepted approximately `100 m` height above takeoff by `58.0 s`, provides approximately `42 s` of level flight before descent begins, and leaves approximately `92 s` for a progressive descent, final alignment, approach, flare, float, touchdown, and confirmation. It is an engineering fixture derived from the accepted speed, climb, altitude, route-diversity, and total-duration inputs; it is not an aerodynamic performance prediction.

## 12.7 Exact variation profiles

All variation uses scenario time `t` in seconds and radians inside trigonometric functions.

Straight-leg Air Heading variation:

```text
headingVariationDeg(t)
=
4.5 × sin(2πt / 12.0 + 0.30)
+
1.5 × sin(2πt / 4.5 + 1.10)
```

Cruise/descent AS variation, applied only where the phase table says `+ variation`:

```text
asVariationKmh(t)
=
1.2 × sin(2πt / 17.0 + 0.70)
+
0.6 × sin(2πt / 6.5 + 2.00)
```

Level-altitude variation, applied only to `north_crosswind_level`:

```text
altitudeVariationM(t)
=
0.8 × sin(2πt / 18.0 + 0.20)
+
0.4 × sin(2πt / 7.0 + 1.40)
```

Deterministic source-like variation is:

```text
gnssEastErrorM(t)  = 1.5 × sin(2πt / 11.0 + 0.40) + 0.5 × sin(2πt / 3.7 + 1.20)
gnssNorthErrorM(t) = 1.3 × sin(2πt / 13.0 + 1.00) + 0.4 × sin(2πt / 4.1 + 2.10)
sourceGsErrorKmh(t) = 0.35 × sin(2πt / 8.0 + 0.60)
sourceTrackErrorDeg(t) = 0.8 × sin(2πt / 7.5 + 1.30)
magneticAzimuthErrorDeg(t) = 0.9 × sin(2πt / 5.5 + 0.90)
pressureErrorHpa(t) = 0.015 × sin(2πt / 9.0 + 1.70)
```

No uncontrolled randomness or unspecified fixed-seed generator is permitted. Turn phases use only their explicit turn profile plus the magnetic source error; straight-leg Heading variation is not added inside turns. `sourceTrackErrorDeg(t)` is applied only after section 12.2 has established that Track is available; it never synthesizes Track for an exact-zero truth vector or a phase-declared unavailable field.

The normative source-generation order for every emitted field is:

1. evaluate phase profiles and truth state;
2. calculate truth kinematics, altitude, and orientation;
3. calculate the ideal source-equivalent field and determine its availability from the phase/truth contract before error application;
4. when the field is available, evaluate the declared field-specific source-error function at source time `t`;
5. add and normalize or clamp exactly as defined;
6. attach source, observed, and virtual civil timestamps plus metadata;
7. emit through the normal C10 to C4 boundary.

## 12.8 Source cadence and normal delivery

The exact normal source cadence is:

```text
GNSS: every 0.5 s, first sample at 0.0 s
pressure: every 0.1 s, first sample at 0.0 s
orientation: every 0.1 s, first sample at 0.0 s
weather/QNH: one snapshot at 0.0 s
```

Normal observed monotonic time equals source monotonic time. Jitter, batching, missing samples, `1 Hz` GNSS, and timestamp-invalidity cases are deterministic test transforms and do not change `scenario-v1`.

The `gnssAvailability` transitions in section 12.9 are separate logical-stream events emitted at their exact scenario times; they are not inferred from or delayed by the GNSS observation cadence.

## 12.9 Exact fault variants and estimator timing

The controlled GNSS outage uses the asset contract in section 12.1. At exactly `112.0 s`, before any GNSS observation at that scenario time, C10 emits:

```text
stream: gnssAvailability
state: unavailable
reason: controlledInterruption
sourceMonotonicTimeS: 112.0
observedMonotonicTimeS: 112.0
```

C4 normalizes that transition immediately at `112.0 s`. GNSS source availability and normalized position, GS, and Track become unavailable, and the active GNSS distance-continuity segment ends at that boundary. No GNSS position, GS, or Track observation is emitted in `[112.0, 117.0)`. Pressure and orientation continue normally.

At exactly `117.0 s`, C10 emits:

```text
stream: gnssAvailability
state: available
reason: controlledRestoration
sourceMonotonicTimeS: 117.0
observedMonotonicTimeS: 117.0
```

The normative equal-time processing order is:

1. normalize the availability-restoration transition;
2. emit and normalize the first recovered GNSS observation with `sourceMonotonicTimeS: 117.0`.

The availability transition and recovered observation are separate logical streams, so their equal source timestamps do not violate the same-stream duplicate/collision rule. Restoration means only that the source can supply data again; it does not synthesize position, GS, or Track. Those values become valid only when C4 accepts the recovered observation. That observation opens the new distance-continuity segment as an anchor and contributes no pre-gap chord; the normally scheduled `117.5 s` observation may form the first post-restoration pair.

For the controlled first-slice fixture, GNSS silence alone is not an outage signal. The outage and restoration boundaries are defined by explicit source-equivalent availability transitions normalized by C4. No freshness timeout, cadence inference, or wall-clock delay defines this controlled outage.

The outage occurs on the stable east-downwind leg, after directional diversity has been generated and before the next turn. C10 does not signal C1, C6, C7, C8, or C9 directly: `GPS unavailable`, suspended GNSS-dependent detection, blocked wind acceptance, degraded map behavior, and recorded gap evidence all arise through C4 and the existing concern boundaries.

The accepted estimator configuration must produce the first accepted estimated-wind result no later than `108.0 s` in the normal fixture, so the outage always begins after an accepted estimate exists. The exact earlier acceptance time remains an algorithm result, not privileged simulator input.

Map-unavailable mode is an independent C8 development toggle and is not encoded as altered scenario physics.

Unexpected silent-source handling, operating-system callback loss, provider-specific timeouts, long GNSS loss, process suspension, restart recovery, and automatic source switching remain deferred production concerns.

## 12.10 Launch interpretation

The direct `4 m/s` headwind produces approximately `14.4 km/h` air-relative flow before ground acceleration is considered. The declared launch phase therefore reaches physical liftoff at `10.6 km/h GS` and `25.0 km/h AS` at `7.4 s`.

C6 receives neither truth AS, `wingStabilized`, nor the physical-liftoff event. Its effective takeoff boundary remains detector-defined and may precede `7.4 s`.

## 12.11 Climb, level flight, and descent interpretation

The exact altitude profile is chosen so that:

- climb reaches `122 m MSL` by `48.0 s`;
- the final climb reaches `135 m MSL` at `58.0 s`;
- level flight continues through the north leg and east turn;
- descent starts gently on the east-downwind leg at `100.0 s`;
- the main descent continues through the south leg and final alignment;
- final approach begins at `172.0 s` from `54.0 m MSL`;
- flare begins at `187.0 s` from `36.3 m MSL`;
- touchdown occurs at `192.0 s`.

The maximum nominal climb rate is `2.4 m/s` during `initial_climb_upwind`, within the accepted approximately `+2.5 m/s` target. Descent rates remain approximately `1.2–1.5 m/s` before flare. These vertical phases do not gate estimated-wind calculation: a steady climb or descent may retain sufficiently stable AS for the current wing, power, and brake configuration, and vertical speed or altitude trend alone is not a rejection condition.

## 12.12 Flare, float, touchdown, and landing run

`flareV1` is the bounded first-slice approximation:

- `187.0–190.0 s`: AS and altitude follow smoothstep from the declared start/end values, representing active braking and rapid reduction of descent;
- `190.0–192.0 s`: short near-horizontal float with weaker altitude reduction and continued AS loss;
- at `192.0 s`: airborne truth becomes false and wind drift stops affecting coordinates;
- `192.0–197.0 s`: ground run decelerates smoothly to zero;
- `197.0–212.0 s`: zero-speed confirmation interval.

Full wing, pendulum, pitch, or brake aerodynamics are not modelled.

## 12.13 Expected physical scale

With the declared profiles and no source-like measurement offsets, the full `0.0–212.0 s` truth path is approximately `1.92 km`. The effective detected Flight duration is expected to be approximately `3.0–3.2 min`, depending only on the accepted detector boundaries. Automated fixture tests must calculate and freeze the resulting reference values with explicit tolerances; they must not retune the phase schedule silently.

## 12.14 Privileged-truth prohibition

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

# 13. Takeoff Detection

## 13.1 Candidate

C6 evaluates accepted normalized GNSS observations using the source-time hold primitive in section 11.6.

Candidate qualification uses:

```text
predicate: GS > 7.0 km/h
duration: 1.0 s
```

The qualification hold begins at the first accepted observation satisfying the predicate. When the hold completes, a takeoff candidate becomes active; the first qualifying observation time `t0` becomes the provisional effective takeoff boundary, its accepted position becomes the provisional Takeoff Point source anchor, and bounded retained history begins no later than that observation. An accepted observation with `GS <= 7.0 km/h` resets an uncompleted qualification hold.

When no takeoff candidate is active at the start of one accepted normalized GNSS evaluation, the exact processing order is:

1. validate required normalized inputs and source-time continuity;
2. update the `GS > 7.0 km/h` candidate-qualification hold;
3. if qualification completes on this observation, activate the candidate while retaining the first qualification observation and its position as the provisional effective boundary and Takeoff Point anchor;
4. evaluate this same accepted observation once as the first possible confirmation-hold observation;
5. if it satisfies `GS >= current confirmationThresholdKmh`, establish the confirmation-hold start at this observation's source monotonic time;
6. otherwise, do not start confirmation on this observation;
7. continue the existing candidate semantics on later accepted observations.

The qualification-completing observation can start the confirmation hold but cannot complete a new `1.0 s` confirmation hold at the same timestamp. The effective takeoff boundary remains the first observation of the completed qualification interval, not the qualification-completing observation, confirmation-hold start, or confirmation event time; those observations are distinct under the current non-zero `1.0 s` qualification rule.

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

Weather wind uses meteorological `from` direction relative to True North. The authoritative launch direction for the correction is the current valid GNSS Track from the same normalized observation whose GS is being evaluated. Device True Azimuth, phone orientation, candidate displacement, simulator Air Heading, and privileged truth are not used.

For each evaluated observation:

```text
deltaDeg
=
shortest angular difference between
weatherWindFromDegTrue and trackDegTrue

usableHeadwindMps
=
max(0, weatherWindSpeedMps × cos(deltaDeg))

confirmationThresholdKmh
=
25
−
0.75 × usableHeadwindMps × 3.6
```

The cosine uses radians internally. Usable correction requires valid and fresh weather-wind speed and direction plus valid and fresh GNSS Track with acceptable course accuracy. If any required value is missing, stale, invalid, or insufficiently accurate, correction is zero. A crosswind contributes zero; a tailwind never lowers the threshold. The threshold is recalculated for each observation during the confirmation hold.

For a direct `4 m/s` headwind:

```text
confirmation threshold ≈ 14.2 km/h GS
```

After the candidate is active—including during the remainder of the qualification-completing evaluation—confirmation is evaluated on accepted normalized GNSS observations using:

```text
predicate: GS >= current confirmationThresholdKmh
duration: 1.0 s
```

The threshold is recalculated for each evaluated observation using that observation's current valid synchronized Track and weather-headwind context. Every accepted observation in the hold must satisfy its recalculated threshold. The qualification-completing observation starts the confirmation hold when it satisfies the predicate; otherwise a later satisfying observation starts it. In either case, confirmation requires a later accepted satisfying observation at least `1.0 s` after the confirmation-hold start. Completion emits one `TakeoffConfirmed`; the effective takeoff boundary remains the candidate's first qualification observation, not the confirmation-hold start or confirmation time.

## 13.3 Cancellation

While a candidate is active, low-speed cancellation uses:

```text
predicate: GS <= 7.0 km/h
duration: 1.0 s
```

If this hold completes before confirmation, C6 cancels the candidate and discards its provisional boundary. If GS rises above `7.0 km/h` before completion, only the cancellation hold resets.

The exact candidate timeout is `15.0 s`, measured in source monotonic time from the provisional effective candidate boundary `t0`. At the first accepted evaluation for which `currentSourceTime - t0 >= 15.0 s`, the candidate times out unless confirmation completes on that same evaluation. Tie priority is normative:

```text
confirmation first
timeout second
```

Required-input unavailable/invalid state, an explicit continuity gap, or source-monotonic invalidity/discontinuity clears the takeoff candidate, its provisional boundary, and all associated holds. No takeoff boundary may span such a discontinuity.

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
idealPressureHpa
=
qnhHpa
×
(1 − truthAltitudeMslM / 44330.76923076923)^(1 / 0.1902632365)

sourcePressureHpa
=
idealPressureHpa + pressureErrorHpa(t)
```

The pressure error is applied after the inverse calculation. The resulting `sourcePressureHpa` must still be finite and positive before C4 accepts it. C7 receives only source-equivalent pressure and QNH, never truth altitude.

At the `35 m MSL` fixture surface the ideal inverse calculation, before `pressureErrorHpa(t)`, yields approximately `1009.052 hPa`. Round-trip and error-order tests must prove that C10 pressure generation and C7 derivation use compatible constants without C7 receiving truth altitude.

QNH remains fixed for the duration of the first-slice Flight. The pilot-facing primary altitude is MSL altitude.

## 15.2 Height above takeoff

C7 calculates height above takeoff relative to the barometric altitude at the effective Takeoff Point.

It is retained and used in Summary, but is not required as a second large Flight Screen value.

## 15.3 Vertical speed

C7 evaluates VS on every newly accepted valid pressure/altitude sample using unweighted ordinary least squares over all distinct accepted valid C7-derived altitude MSL samples whose source monotonic time lies in the inclusive window `[t - 3.0 s, t]`.

For samples `(ti, hi)`:

```text
meanT = arithmetic mean of ti
meanH = arithmetic mean of hi

numerator
=
Σ((ti - meanT) × (hi - meanH))

denominator
=
Σ((ti - meanT)^2)

verticalSpeedMps
=
numerator / denominator
```

The output is available only with at least `20` distinct samples, a source-time span of at least `2.0 s`, and a finite denominator greater than zero. There is no interpolation, endpoint slope, weighting, robust regression, host/observed-time input, or phase-label input. An identical same-timestamp redelivery is ignored and does not increase the sample count.

A pressure unavailable/invalid/gap boundary or pressure source-monotonic discontinuity makes current VS unavailable and clears the complete fit window. After recovery, VS remains unavailable until at least `20` fresh distinct accepted samples span at least `2.0 s`. Missing samples without an explicit gap remain usable only when both minimum rules still hold. Batched delivery preserving valid ordered source timestamps produces the same VS series as normal delivery.

The calculation identifier is `olsAltitudeSlope3sMin20Span2sV1` and is retained or exposed in the required calculation context. Finalized maximum climb and descent values derive only from retained valid outputs of this algorithm; unavailable intervals do not create zero VS samples.

Altitude and VS are diagnostic/derived information and do not drive first-slice takeoff or landing detection.

# 16. Estimated-Wind Calculation

## 16.1 Vector model

During every active Flight interval with valid GNSS GS and Track, C7 continuously converts observations into East/North ground-velocity vectors `(vE, vN)` and evaluates the available candidate windows. Estimator execution is not started, stopped, or reset by simulator phase, route leg, altitude trend, climb, level flight, or descent.

The model is:

```text
ground velocity = wind + air-relative velocity
```

At approximately stable horizontal AS magnitude, ground-velocity samples lie near a circle:

- fitted centre ≈ wind vector;
- fitted radius ≈ AS.

Approximately stable AS is a property of the observations inside the candidate window, not a synonym for level flight. A steady climb or descent can remain usable when the paramotor configuration and AS are sufficiently stable. Aggressive manoeuvres such as tight spirals may produce poor fit or observability and therefore fail the normal quality gates, but they are not rejected by a separate phase or manoeuvre label.

## 16.2 Fit method

Preliminary bounded implementation:

1. Pratt or Taubin circle initialization;
2. geometric radial least-squares refinement.

Kåsa-only fitting is not sufficient for incomplete arcs.

Pratt versus Taubin remains a reversible implementation choice selected through deterministic numerical tests.

## 16.3 Windows

While a Flight is active and required inputs are valid, candidates are recomputed approximately once per second for windows:

```text
30 / 60 / 90 / 120 s
```

Candidate windows may span climb, level flight, descent, turns, or multiple scenario phases. Phase boundaries do not clear otherwise valid history. The shortest accepted window is used for freshness.

## 16.4 Quality gates

Candidate acceptance is determined by observation and fit quality, not by a declared Flight phase. There is no explicit eligibility gate for climb, level flight, descent, route leg, vertical speed, or simulator phase.

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

## 17.1 Estimated AS and stationary ground vector

C6 uses:

```text
estimated air velocity
=
ground velocity
−
last accepted estimated wind
```

The vector magnitude is estimated AS.

For landing detection only, C6 constructs the ground-velocity vector with this exact stationary rule:

```text
stationaryGsThresholdKmh = 1.0
```

- when valid GS is greater than `1.0 km/h`, valid Track is required and the normal GS/Track vector is used;
- when valid GS is less than or equal to `1.0 km/h`, ground velocity is defined as `(0, 0)` and Track is not required;
- missing or invalid GS is never treated as stationary;
- unavailable Track at stationary GS is not converted into a synthetic Track and does not become valid input for C8 or C9.

This is not a GS-only landing fallback: a valid last accepted estimated wind remains mandatory, and estimated AS is still derived from the vector difference.

## 17.2 Availability

Automatic landing detection is available only when:

- a Flight is active;
- a last accepted wind estimate exists;
- the estimate is not hard-invalidated;
- required GNSS GS is valid;
- Track is valid whenever GS is above the stationary threshold.

The normal fixture may therefore keep Track unavailable during the zero-speed `197.0–212.0 s` interval while still confirming the candidate that began near touchdown. With the accepted schedule, the exact `15.0 s` hold completes at approximately `207.0 s`.

There is no landing fallback that omits the accepted estimated-wind requirement.

## 17.3 Candidate and confirmation

Landing entry uses this predicate on accepted evaluations:

```text
estimated AS < 25 km/h
GS < 4 km/h
```

The first accepted evaluation satisfying both conditions creates the landing candidate, begins the confirmation hold, and establishes the provisional effective landing boundary. Confirmation duration is exactly `15.0 s` and completes at the first accepted evaluation that still satisfies both conditions and for which:

```text
currentSourceTime - provisionalBoundaryTime >= 15.0 s
```

## 17.4 Hysteresis

Whenever an established landing candidate leaves the entry predicate, C6 resets the confirmation hold and clears the provisional effective boundary. If the hard-cancellation predicate is not satisfied, candidate identity is retained and no confirmation time accumulates in the hysteresis band. On later re-entry into both entry conditions, a new `15.0 s` confirmation hold begins and the re-entry observation becomes the new provisional boundary. No elapsed time from an earlier incomplete hold is reused.

Hard cancellation uses:

```text
predicate:
estimated AS > 28.0 km/h
or
GS > 7.0 km/h

duration: 1.0 s
```

If the predicate remains true for the full source-time hold, C6 cancels the candidate entirely. If it becomes false before `1.0 s`, only the cancellation hold resets while candidate identity is preserved subject to the hysteresis rules above. Values exactly equal to `28.0 km/h` or `7.0 km/h` do not satisfy the strict hard-cancellation predicate.

Unavailable or invalid required GNSS input, a GNSS continuity gap, hard-invalidated wind, or source-monotonic discontinuity resets all landing holds, clears the candidate and provisional boundary, and does not complete the Flight. The accepted controlled outage occurs before the normal landing candidate and therefore does not change the bounded P3 outcome.

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

If provider, network, tiles, or renderer are unavailable:

- Flight continues;
- the spatial viewport remains;
- pilot marker remains centred;
- orientation cue and scale remain;
- neutral background is shown;
- `Map unavailable` is shown;
- weak grid/rings may be used to make rotation visible.

The degraded spatial state has no product or lifecycle authority and must not change Flight detection, derivation, recording, completion, or Summary. The slice must not show a fake or misleadingly current cached map and must not silently switch to another provider.

## 18.5 Map attribution and licence/source action

The visible `© OpenStreetMap contributors` attribution remains present on the map and is not obscured by Product overlays or the simulation panel. It is rendered in the screen-oriented presentation layer: Track-up or Device-True-Azimuth-up rotation affects map content but does not rotate the attribution text or action.

The attribution exposes an accessibility- and keyboard-discoverable action where supported by the target Flutter platform. Activating it uses an appropriate external-link mechanism to open exactly:

```text
https://www.openstreetmap.org/copyright
```

Failure to launch the external link remains a bounded presentation failure and does not alter map state, Flight lifecycle, detection, derivation, recording, or Summary. The URL, link-launch mechanism, provider details, and any package-specific attribution realization remain wholly inside C8.

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
- explicit GNSS interruption/restoration transitions or equivalent normalized gap-boundary evidence;
- validity, freshness, provenance, and applicable handling state;
- source monotonic time;
- observed monotonic time;
- C4-normalized virtual civil UTC used as AirLink-facing wall-clock time.

## 19.2 Derived/domain timeline

Retain:

- C3-supplied Flight-level `simulated` classification, kept distinct from category-level provenance and handling;
- altitude MSL;
- height above takeoff;
- VS;
- derived Device True Azimuth, declination used, position/date context, and declination-provider identifier/version;
- altitude-calculation identifier/version and constants used with the accepted QNH;
- VS calculation identifier `olsAltitudeSlope3sMin20Span2sV1` with the retained valid VS outputs used for finalized extrema;
- `scenarioV1Wgs84LocalTangentV1` projection context for fixture-generated observations and `haversineMeanEarthR6371008_8V1` distance-calculation context;
- all accepted wind estimates with quality metadata;
- current/retained/unavailable wind state transitions;
- detector candidates and confirmations, including qualification start/completion, candidate activation, same-observation confirmation-hold start when applicable, later confirmation time, resets, provisional-boundary changes, and timeout/tie evidence needed to reproduce effective boundaries;
- effective takeoff and landing boundaries;
- Flight lifecycle events;
- orientation-source transitions;
- degraded intervals;
- recording outcome.

Rejected wind candidates remain diagnostics and are not required in the finalized Flight record.

## 19.3 Retained special-point representation

C9 retains Takeoff Point and Landing Point using this logical representation:

```text
FlightSpecialPoint
- pointId
- flightId
- kind: takeoff | landing
- classification: detectedTakeoff | confirmedLanding
- effectiveBoundaryMonotonicTime
- confirmationMonotonicTime
- wallClockTime
- latitudeDeg
- longitudeDeg
- horizontalAccuracyM
- sourceObservationRef
- detectorVersion
- locationStatus: observedAtEffectiveBoundary
```

The field names are logical retained-data requirements rather than a prescribed Dart class or storage schema.

The effective boundary must be anchored to the accepted GNSS observation that begins the confirmed detector interval. `sourceObservationRef` identifies that observation. No hidden interpolation is permitted. Point identity, Flight association, kind, classification, both monotonic times, and detector version are mandatory for any complete or degraded record. In the normal fixture, point location and accuracy are also mandatory.

`wallClockTime` is the C4-normalized virtual civil timestamp associated with that accepted effective-boundary source observation. It is not host-arrival time or confirmation wall time.

## 19.4 C3 to C9 creation handoff

After C2 authorizes takeoff, C3 creates the Flight and Takeoff Point, then supplies C9 before recording initialization with:

```text
- flightId
- Flight-level simulated classification
- active lifecycle state
- effectiveTakeoffBoundary
- takeoffConfirmationTime
- complete Takeoff Point representation
- detectorVersion
- boundedHistoryStart/end reference
```

C9 initializes the record from that authoritative context before accepting the first ordinary active-Flight update. If C9 cannot retain the Flight identity, boundary, or Takeoff Point context, initialization is `failed`; it must not create an apparently valid record missing those fields.

## 19.5 C3 to C9 completion handoff

After C2 authorizes landing, C3 completes lifecycle and creates the Landing Point, then supplies C9 before finalization with:

```text
- flightId
- completed lifecycle state
- effectiveLandingBoundary
- landingConfirmationTime
- complete Landing Point representation
- detectorVersion
- finalConfirmationTailStart/end reference
```

C9 first retains the completion context and final confirmation tail, then finalizes. A complete or degraded record must contain both special points and their Flight association. Failure to retain required completion context yields a `failed` record outcome without changing C3 lifecycle truth.

## 19.6 Observation and finalization boundaries

C9 receives:

- bounded history beginning at the effective takeoff boundary;
- active Flight observations/events;
- final confirmation tail through landing confirmation time.

C9 finalizes as:

- `complete`;
- `degraded`;
- `failed`.

A complete or degraded outcome produces an immutable in-memory Flight record. A failed outcome preserves completed lifecycle truth but provides no usable Flight record.

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

Takeoff and landing civil times come from the retained C4-normalized virtual civil timestamps attached to the effective-boundary observations. The normal mapping and exact `80.0 s / +3600 s` transform in section 11.3 do not change monotonic duration.

### Distance

Derive distance independently from the retained C9 record by summing only accepted position-pair distances within the uninterrupted GNSS distance-continuity segments defined in section 10.4. Every eligible pair uses the section 10.4 haversine calculation with `R = 6371008.8 m`; pair eligibility is determined before applying the formula. Do not connect two retained valid positions when a recorded unavailable, invalid, interruption, continuity-gap, or source-monotonic-invalid/discontinuous interval lies between them. The first valid position after each recorded gap is an anchor only. The active runtime aggregate must not be copied into Summary.

### Average GS

```text
valid recorded distance
/
valid covered time
```

Valid covered time is the sum of source-monotonic time intervals for the same accepted within-segment position pairs that contribute valid recorded distance. A source-time interval crossing a GNSS gap contributes neither distance nor valid covered time. Overall Flight duration remains defined by the effective lifecycle boundaries and includes the outage. Average GS is marked degraded when source gaps materially limit coverage.

### Maximum GS

Maximum valid system GS observation.

### Maximum climb and descent VS

Maximum climb and descent derive only from retained valid VS outputs produced by `olsAltitudeSlope3sMin20Span2sV1`. Unavailable intervals contribute no synthetic zero values.

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

A separate deterministic variant introduces exactly `5 s` of GNSS unavailability from scenario time `112.0 s` inclusive to `117.0 s` exclusive:

- during the stable east-downwind leg;
- after at least one accepted wind estimate exists;
- before the next turn and final approach.

The source-equivalent interruption transition defined in section 12.9 reaches C4 exactly at `112.0 s`, before any GNSS observation at that time. C4 immediately normalizes GNSS source, position, GS, and Track as unavailable and ends the current distance-continuity segment. C1, C6, C7, C8, and C9 receive no fault or phase metadata from C10; every downstream effect below follows only from normal C4-derived state and normal concern boundaries.

During the outage:

- Flight remains active;
- Product UI shows `GPS unavailable`;
- position, GS, and Track are unavailable;
- map holds last valid orientation as stale;
- pressure and VS continue if valid;
- C7 retains the last accepted wind;
- no new wind estimate is accepted;
- landing detection is suspended;
- active distance holds its current value;
- C9 records the gap.

After recovery:

- the same Flight continues;
- the last normal pre-gap GNSS sample is expected at approximately `111.5 s` under the normative cadence;
- no GNSS sample exists in `[112.0, 117.0)`;
- the source-equivalent restoration transition reaches C4 exactly at `117.0 s` and is normalized before the recovered GNSS observation at the same source time;
- the restoration transition alone does not synthesize valid position, GS, or Track;
- the valid sample at `117.0 s` opens the recovered distance-continuity segment as an anchor only and adds no distance from the pre-gap anchor;
- the next valid sample, normally at `117.5 s`, may produce the first post-gap distance contribution;
- the unavailable interval contributes neither GNSS-derived distance nor valid GNSS covered time, but remains part of elapsed Flight time and total Flight duration;
- normal inputs resume;
- final recording outcome is `degraded`;
- Summary remains available.

No interpolation, dead reckoning, GS integration, route reconstruction, or estimated missing distance is permitted for the outage.

Silence alone does not establish the controlled fixture outage. The explicit unavailable/restoration transitions define its normative boundaries; this section introduces no general production freshness or timeout policy.

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

## 22.2 Replaceable detailed developer output

An expanded interactive diagnostics overlay is out of scope. Detailed observability is exposed through tests, structured logs, a bounded developer dump, or another replaceable non-product mechanism using the shared diagnostic snapshot and event timeline.

The replaceable output must make available when required:

- detector thresholds, corrections, qualification start/completion, candidate activation, same-observation confirmation-hold start when applicable, hold elapsed/reset reasons, confirmation/timeout priority, and effective/confirmation boundaries;
- wind window, residual, coverage, conditioning, uncertainty, and truth comparison;
- source/observed clocks, latency, gaps, and batching;
- recording counts, boundaries, and outcome;
- orientation source, age, and fallback;
- map state and viewport information.

No separate inspector information architecture, interaction model, or placement work is required in the first slice.

## 22.3 Shared structured diagnostics

UI and tests use the same read-only diagnostic snapshot and bounded event timeline.

Tests must not depend on rendered screen text.

## 22.4 Required validation evidence

- normal end-to-end deterministic run;
- exact `scenario-v1` asset parsing and phase-boundary tests proving the asset is valid JSON, `completed_ground` begins at `212.0 s` with `endS: null`, remains active for arbitrary later scenario times until Reset, preserves normal completion and Summary behavior, and rejects non-final/multiple/followed open ends, string sentinels such as `"212.0+"`, and incompatible terminal values;
- local-projection tests proving zero East/North maps back to the declared origin within a tight binary64 tolerance, a known positive East offset changes longitude by the section 12.3 formula without changing latitude beyond that tolerance, and a known positive North offset changes latitude without changing longitude beyond that tolerance;
- GNSS generation-order test proving East/North source errors are added before geographic conversion;
- deterministic source-error reference tests at multiple fixed source times proving every declared error affects its intended emitted field with the correct zero, sign, and phase; GS is clamped at zero; Track and magnetic azimuth are normalized to `[0°, 360°)`; Track error does not synthesize Track in zero-speed Track-unavailable phases; pressure error follows the inverse pressure calculation; C7 receives no truth-altitude or truth-orientation shortcut; and `1×`/`2×` produce the same frozen all-errors source sequence;
- exact-zero truth-vector Track tests proving `atan2(0,0)` is not evaluated as source Track; Track is unavailable at scenario time `0.0 s`; positive `sourceGsErrorKmh(t)` at exact-zero truth velocity does not create Track; the first later sample with `truthGsKmh > 0.0` derives Track and applies `sourceTrackErrorDeg(t)`; zero-speed `landed_confirmation` and `completed_ground` remain Track-unavailable; Track error never synthesizes the field; the frozen source reference sequence includes all of these availability outcomes; and C6's separate accepted-GS-at-or-below-`1.0 km/h` stationary landing vector remains unchanged without synthesizing source Track;
- independently generated reference latitude/longitude observations match the frozen `scenario-v1` reference sequence at `1×` and `2×` without requiring platform-specific decimal text formatting;
- truth-isolation tests proving no truth East/North or truth-distance shortcut reaches C4, C7, C8, C9, or Summary;
- calculation-context tests proving `scenarioV1Wgs84LocalTangentV1` and `haversineMeanEarthR6371008_8V1` are exposed or retained where required;
- takeoff candidate/confirmation boundary tests, including direct/partial headwind, crosswind, tailwind, invalid Track, stale weather, and deliberate Track-versus-Device-True-Azimuth disagreement;
- detector-hold tests at normal `2 Hz`, `1 Hz`, deterministic jitter, missing samples without a declared outage, and batched delivery proving elapsed source-time rather than sample-count completion; inclusive `>= D` endpoints; independence from observed delay and `2×`; idempotent redelivery behavior; reset on explicit gaps/unavailability; the first takeoff qualification observation as effective boundary; separate exact `1.0 s` qualification, confirmation, and low-speed-cancellation holds; exact `15.0 s` timeout from the provisional boundary; and confirmation priority on an exact timeout tie;
- takeoff same-observation ordering tests proving at normal `2 Hz` and `1 Hz` that the qualification-completing observation is evaluated as the first possible confirmation-hold observation; it starts but cannot instantly complete the `1.0 s` confirmation hold when the current threshold is satisfied; a later satisfying observation starts the hold when it is not; the effective boundary remains the first qualification observation and differs from confirmation time; batching/observed delay do not alter ordering; identical redelivery cannot repeat candidate activation or start/advance the hold twice; confirmation-first timeout tie priority remains unchanged; and retained diagnostics record qualification start/completion, candidate activation, same-observation confirmation start where applicable, and final confirmation time;
- landing candidate/confirmation boundary tests, including stationary GS with unavailable Track, moving GS with invalid Track, invalid GS, exact `15.0 s` confirmation, confirmation/provisional-boundary reset when entry conditions fail, candidate identity retained without accumulated time in the hysteresis band, exact `1.0 s` strict hard cancellation, invalidity preventing confirmation across the invalid interval, and equivalent effective boundaries/Summary metrics under source-equivalent delivery transforms;
- circle-fit numerical tests for ideal, noisy, incomplete, poorly conditioned, and outlier cases;
- accepted/retained/unavailable wind-state tests;
- first accepted wind no later than `108.0 s` in the normal fixture;
- estimator-continuity test proving calculation runs across climb, level-flight, and descent observations without phase gating or phase-boundary resets;
- quality-gate test proving acceptance/rejection depends on input, residual, coverage, conditioning, and uncertainty rather than vertical phase labels;
- exact `112.0–117.0 s` GNSS-outage and recovery test proving C4 receives and immediately normalizes `unavailable/controlledInterruption` at `112.0 s` without timeout, cadence inference, or wall-clock delay;
- outage-state test proving position, GS, and Track remain unavailable from interruption until valid recovered data is accepted and no GNSS observation is emitted in `[112.0, 117.0)`;
- restoration-order test proving C4 receives `available/controlledRestoration` exactly at `117.0 s`, processes it before the recovered `117.0 s` observation, and accepts their equal timestamps because the transition and observation are separate streams;
- C4-boundary test proving C6, C7, C8, and C9 observe the outage only through normal C4-derived state and never through C10 phase or fault metadata;
- controlled-silence test proving silence without an explicit availability transition does not establish the normative fixture outage boundary;
- gap-evidence test proving C9 retains explicit interruption/restoration or equivalent normalized boundaries sufficient to reproduce degraded Summary semantics;
- map-unavailable validation;
- fixed-map tests proving C8 targets `2000 m` visible ground width within `±2%` at at least two representative portrait viewport widths; Track-up rotation and GNSS recovery preserve the scale; no `1500 m` fallback exists; scenario/C10 metadata cannot control zoom or provider; provider failure produces only the degraded spatial presentation; C8 neutral contracts contain no `flutter_map` types; and no offline, bulk-download, or prefetch feature exists;
- OSM attribution tests using semantic presentation/accessibility state rather than a specific package widget class, proving visible `© OpenStreetMap contributors` credit is not obscured by overlays or the simulation panel; map rotation does not rotate its text/action; an accessible action targets exactly `https://www.openstreetmap.org/copyright`; provider, attribution, URL-launch, URL, and HTTP details remain internal to C8 without package-specific attribution types entering neutral contracts; launch failure has no effect outside presentation; and existing User-Agent, caching, current-view-only, no-prefetch/offline/bulk-download, and degraded-map requirements remain unchanged;
- active and finalized distance tests proving independently applied common segment semantics and the common haversine pair calculation with exact `R = 6371008.8 m`: a normal uninterrupted pair contributes distance; the final pre-gap position to first post-gap position contributes exactly zero; the first recovered `117.0 s` position only establishes a new anchor; and the normally scheduled `117.5 s` position can resume accumulation;
- distance-coverage tests proving the outage interval contributes neither distance nor valid covered time while elapsed Flight time and total duration still include it;
- degraded-distance evidence proving Summary distance is lower than the corresponding uninterrupted fixture by the omitted GNSS-covered segment;
- continuity tests proving late or batched observations with continuous valid ordered source timestamps do not create a false segment break, while invalid or source-monotonic-discontinuous GNSS input does break the segment;
- active flown-distance tests proving effective-boundary start, value hold during unavailability, `500 m` threshold notifications, and `3 s` return to `FLT`;
- windsock-presentation tests proving downwind body orientation, simple-circle containment, numeric `0.5 m/s` display granularity, valid-zero versus unavailable distinction, and visual capping above `8 m/s` without estimator quantization;
- semantic presentation-state tests proving GS is absent as a primary value before confirmed takeoff, weather wind occupies the primary left tile on the ground with unavailable/zero/stale/valid distinctions, that tile switches to GS only after confirmation, VS exists only during active Flight, weather wind disappears from Product UI after takeoff, and estimated wind remains in the separate orientation context; these tests do not depend on rendered text when semantic widget/state assertions are available;
- Pause-is-not-outage test;
- virtual-civil-time tests proving the normal `civilStartUtc + scenarioTime` mapping, Pause freezes both clocks, `2×` changes host playback duration but not observation civil timestamps, batching preserves source civil timestamps, host wall time is not an input, and Reset restores scenario/offset/civil start state;
- exact wall-clock-jump tests proving the offset changes from `0` to `+3600 s` at `80.0 s`, pre-jump observations and Takeoff Point remain unchanged, observations at/after the boundary and Landing Point include the offset, Summary uses the retained boundary timestamps, and duration, detector holds, ordering, VS, wind, and `1×`/`2×` source-time equivalence remain unchanged;
- identical same-timestamp redelivery is ignored idempotently without duplicate retention or degradation;
- conflicting same-timestamp observation fails closed;
- missing and backward source-monotonic timestamp tests prove invalid time cannot produce valid detector windows, duration, VS, wind estimates, retained ordering, or successful Summary;
- Device Magnetic Azimuth to Device True Azimuth tests using the non-zero fixture declination, including normalization and unavailable-declination fallback;
- pressure/QNH forward-and-inverse round-trip tests at the surface and representative Flight altitudes;
- VS reference tests proving exact synthetic linear-ramp recovery, climb/descent sign, unavailable output until both `20` distinct samples and `2.0 s` span exist, inclusive `[t - 3.0, t]` membership and deterministic eviction, redelivery idempotency, batching equivalence, missing-sample eligibility only when both minimum rules remain met, pressure-gap reset, fresh-history recovery, wall-clock/playback independence, finalized maximum climb/descent from retained valid outputs, and retained/exposed `olsAltitudeSlope3sMin20Span2sV1` context;
- Takeoff Point creation-handoff retention test;
- Landing Point completion-handoff retention test;
- finalized-record test proving both special points retain identity, location, effective/confirmation times, detector version, and Flight association;
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

The bounded first-slice map implementation is:

```text
Flutter renderer package: flutter_map
map technology: raster tiles
tile source/provider: OpenStreetMap Standard raster tiles
tile template: https://tile.openstreetmap.org/{z}/{x}/{y}.png
ownership boundary: C8 map adapter
```

This is a bounded development choice, not a permanent whole-product provider, Flutter package, final Mapbox integration strategy, offline-map architecture, vector-map architecture, or general provider-selection subsystem. A later Mapbox implementation may replace the complete internal C8 renderer rather than merely changing a tile URL.

No `flutter_map`, URL-launcher, or provider-specific type crosses C8. In particular, C8 does not expose `MapController`, `TileLayer`, package-specific `LatLng`, package layer objects, tile-provider types, attribution-widget types, link-launcher types, package lifecycle objects, the OSM tile or copyright URL, attribution implementation details, or HTTP/link implementation details. C1–C7, C9, and C10 do not depend on `flutter_map` or a link-launch package.

The scenario asset contains no map package, provider, tile URL, zoom, attribution, API key, or package-specific configuration. C8 alone converts the neutral `targetVisibleGroundWidthM = 2000.0` contract into the package-specific fractional zoom needed for the current viewport width and centre latitude.

Use of OpenStreetMap Standard for this bounded slice requires:

- visible `© OpenStreetMap contributors` attribution on the map, not hidden behind Product UI or the simulation panel;
- screen-oriented attribution text and action that remain readable and do not rotate with Track-up or Device-True-Azimuth-up map content;
- an accessibility- and keyboard-discoverable attribution action where supported, targeting exactly `https://www.openstreetmap.org/copyright` through an appropriate external-link mechanism internal to C8;
- external-link launch failure contained within presentation, with no effect on map state, Flight lifecycle, detection, derivation, recording, or Summary;
- an application-identifying HTTP `User-Agent`, not a generic library default;
- honouring server HTTP caching headers and never forcing no-cache behavior;
- requesting only tiles required for the currently viewed map;
- no bulk download, scraping, pre-seeding, prefetching, offline download, or tile archive;
- best-effort tile availability with no product or lifecycle authority;
- the section 18.4 degraded spatial canvas on provider, network, tile, or renderer failure;
- a replaceable endpoint and HTTP implementation wholly internal to C8;
- owner review rather than silent provider switching if the selected service becomes unsuitable.

A compatible stable `flutter_map` version is selected during implementation and locked through normal repository dependency management. The plan selects the package and provider, not a permanent package version.

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
- simple orientation circle;
- status/time plus state-specific primary-left, altitude, and contextual-right zones;
- warning placeholders;
- simulation panel;
- Start/Pause, `1×/2×`, Reset;
- deterministic virtual civil clock with normal and exact wall-clock-jump transforms;
- diagnostic snapshot skeleton;
- formatting, analysis, tests, and Android debug build in CI.

Observable result: recognizable Flight Screen shell and working development session controls.

## Increment 2 — Deterministic simulator and normalized live presentation

Includes:

- exact JSON `scenario-v1` asset and parser;
- nullable final-phase and terminal-hold parsing with invalid-arrangement rejection;
- fixed phase schedule, profiles, units, cadences, variation formulas, and truth-step integration;
- fixed WGS84 local-tangent East/North-to-geographic conversion with source errors applied before conversion;
- deterministic GS, Track, magnetic-azimuth, and pressure source-error application in the normative generation order;
- exact-zero truth-vector Track-unavailable semantics before source errors, including positive source-GS-error and frozen-reference availability cases;
- privileged truth;
- C4/C5 contracts;
- GNSS position, explicit GNSS availability, pressure, raw magnetic-orientation, and weather streams;
- C7 Device Magnetic Azimuth to Device True Azimuth conversion through the replaceable declination-provider boundary;
- timing, quality, validity, and provenance;
- fixture geographic reference-sequence, projection, generation-order, and cadence tests;
- movement on placeholder canvas;
- live weather-wind primary-tile and altitude presentation, with normalized GS available to detection/diagnostics but absent as a pre-takeoff primary value;
- source-health diagnostics.

Observable result: Start runs the exact physical fixture and updates Product UI from source-equivalent inputs, without Flight lifecycle creation.

## Increment 3 — Takeoff detection and active Flight lifecycle

Includes:

- C2 state;
- C3 Flight lifecycle;
- C6 takeoff candidate and confirmation;
- reusable source-time predicate holds with exact qualification, confirmation, cancellation, timeout, and tie semantics;
- qualification-completing-observation reuse as the first possible confirmation-hold observation, with exact event ordering and retained diagnostics;
- weather headwind correction;
- effective boundary and bounded history;
- Flight identity and complete Takeoff Point representation;
- explicit C3 to C9 creation handoff and recording initialization seam;
- elapsed Flight time and camera-adjacent `FLT`/transient `DST` presentation;
- active flown-distance aggregate with `500 m` notifications, the fixed haversine pairwise formula, within-segment position-pair accumulation, value hold during GNSS unavailability, anchor-only recovery, and no pre-gap to post-gap chord;
- Device True Azimuth-up ground presentation to Track-up airborne transition;
- one-shot, idempotency, and Takeoff Point handoff tests.

Observable result: automatic transition from `Waiting for Takeoff` to active Flight with an authoritative retained creation context.

## Increment 4 — Altitude, VS, estimated wind, landing, and completion

Includes:

- versioned pressure/QNH altitude contract and round-trip fixture tests;
- height above takeoff;
- exact `olsAltitudeSlope3sMin20Span2sV1` VS fit and gap/recovery semantics;
- circle-fit estimator and quality gates;
- accepted/retained/unavailable states;
- estimated AS;
- landing candidate and confirmation;
- exact landing confirmation, hysteresis, hard-cancellation, and invalidity holds;
- complete Landing Point representation;
- explicit C3 to C9 completion handoff seam;
- completed lifecycle;
- VS and simple-circle windsock-like estimated-wind presentation.

Observable result: the scenario automatically completes one Flight from takeoff through landing with authoritative completion context ready for C9 finalization.

## Increment 5 — Real map adapter and spatial presentation

Includes:

- replaceable C8 adapter;
- `flutter_map` Web Mercator raster renderer contained wholly inside C8;
- OpenStreetMap Standard tiles from `https://tile.openstreetmap.org/{z}/{x}/{y}.png` with visible screen-oriented attribution, an accessible action to `https://www.openstreetmap.org/copyright`, identifying User-Agent, honoured caching, current-view-only requests, and no prefetch/offline/bulk-download behavior;
- fixed `2000 m ±2%` full-logical-viewport ground width preserved by C8-computed fractional zoom;
- centred pilot;
- Device True Azimuth-up/Track-up behavior and fallbacks;
- north/orientation cue and scale;
- map-unavailable canvas;
- real-device map performance check.

Observable result: real map replaces the placeholder without changing lifecycle or overlay contracts.

## Increment 6 — Progressive in-memory recording and Summary

Includes:

- C9 initialization from the complete C3 creation handoff;
- bounded preconfirmation history;
- two-layer record;
- retained Takeoff Point and Landing Point logical representations;
- C3 completion handoff and final tail;
- complete/degraded/failed outcomes;
- immutable finalized record;
- Summary from record;
- Reset/discard semantics.

Observable result: normal Flight ends in a complete Summary derived from a finalized record containing both authoritative special points.

## Increment 7 — Degradation and final acceptance evidence

Includes:

- exact five-second GNSS outage and recovery through explicit source-equivalent availability transitions normalized by C4;
- degraded record and Summary;
- no pre-gap to post-gap distance chord, anchor-only first recovery position, resumed accumulation from the next within-segment position, and no outage contribution to valid covered time;
- independent active and finalized-record distance derivations using the same segment semantics and fixed haversine pairwise formula;
- continuous valid source-time batching that does not create a false segment break, plus invalid and source-monotonic-discontinuous inputs that do;
- retained wind and suspended landing detection;
- map-unavailable run;
- playback, Pause, exact virtual-civil/wall-clock-jump, monotonic-invalidity, duplicate/collision, exact detector-hold and same-observation takeoff ordering, exact OLS VS, all-source-error and exact-zero Track availability, terminal-phase JSON, fixed-map-scale/C8-isolation/OSM-policy and accessible attribution action, primary-tile semantic-state, magnetic-declination, exact-scenario, headwind-projection, stationary-landing, active-distance, windsock-presentation, special-point-handoff, truth-leakage, and end-to-end tests;
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
- cadence, gaps, redelivery, collisions, and batching;
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
- ground weather-wind to active-Flight GS primary-tile switching and active-only VS are explicit;
- weather-source and estimated-wind meanings are separated;
- complete, degraded, and failed outcomes are defined.

## 28.2 Domain readiness

- C1–C10 responsibilities are explicit;
- takeoff and landing authority chains are explicit;
- effective and confirmation boundaries are distinct;
- the qualification-completing takeoff observation's same-evaluation confirmation-hold ordering is explicit;
- AS/GS/VS, Air Heading/Track, Device Magnetic/True Azimuth, and MSL/relative-height distinctions are explicit;
- Takeoff Point and Landing Point representations and both C3 to C9 handoffs are explicit;
- record and Summary semantics are explicit.

## 28.3 Simulation readiness

- exact asset path and JSON contract are defined;
- the terminal phase uses the validated `endS: number | null` contract and no string sentinel;
- origin, civil time, environment, wind, QNH, and declination are fixed;
- fixed WGS84 constants, local-tangent conversion, GNSS generation order, and geographic reference sequence are defined;
- phase start/end times and kinematic endpoints are fixed;
- deterministic interpolation, integration, variation, and cadence contracts are fixed;
- every declared source-error profile has a mandatory generation order and emitted-field formula;
- exact-zero truth-vector Track availability is fixed before source errors without an epsilon or generic low-speed Track policy;
- virtual civil time, Pause/`2×`/Reset behavior, and the exact `80.0 s / +3600 s` jump transform are fixed independently of host wall time;
- exact GNSS-outage interval, availability transitions, and equal-time restoration ordering are fixed;
- flare/float/touchdown behavior is fixed for the fixture;
- truth isolation is explicit;
- remaining choices are implementation mechanics or explicitly bounded algorithm/UI tuning rather than hidden scenario questions.

## 28.4 Technical readiness

- provisional Flutter decision is accepted;
- plugin/platform boundaries are explicit;
- `flutter_map` plus OpenStreetMap Standard raster tiles are selected as a bounded replaceable C8-only implementation;
- the fixed `2000 m ±2%` full-logical-viewport target and C8 fractional-zoom responsibility are explicit;
- OSM visible screen-oriented attribution, exact accessible copyright action, C8-contained link handling, User-Agent, cache, request, no-prefetch/offline, and degraded-state requirements are explicit;
- Flight and recording state are independent from widget/map lifetime;
- repository increments and material stop conditions are explicit;
- greenfield environment work is acknowledged.

## 28.5 Validation readiness

- normal run is defined;
- GNSS-outage and map-unavailable cases are defined, with the controlled outage crossing C4 through explicit source-equivalent availability transitions rather than silence or a production timeout policy;
- diagnostics contract is defined;
- truth-leakage prohibitions are defined;
- exact detector holds, same-observation takeoff ordering, exact-zero Track availability, accessible OSM attribution, and exact OLS VS evidence are defined across relevant cadence/delivery/rotation/failure transforms and invalidity boundaries;
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
- before takeoff the primary left tile shows weather-source wind, GS is absent as a primary value, altitude remains central, and Flight VS is absent;
- after takeoff the primary left tile shows GS, altitude remains central, and the right tile shows VS;
- active flown distance is accumulated from the effective boundary and appears as transient `DST` notifications at each `500 m` threshold;
- weather wind disappears after takeoff;
- estimated wind appears only after acceptance;
- estimated wind remains in the orientation-circle/windsock context and never replaces weather wind or VS;
- the estimated-wind indicator uses the simple orientation circle and bounded centre-origin windsock experiment;
- map/orientation behavior follows the contract;
- visible screen-oriented OSM attribution exposes the accessible licence/source action;
- landing is detected automatically;
- Summary appears only after finalization;
- Reset creates a new development session.

## 29.3 Domain behavior

- no Flight exists before takeoff authorization;
- only C3 creates Flight identity and lifecycle;
- detector events are one-shot;
- the qualification-completing observation is evaluated once as the first possible confirmation-hold observation, can start but cannot instantly complete that `1.0 s` hold, and does not replace the first qualification observation as effective boundary;
- recording failure does not alter completed lifecycle truth;
- effective boundaries drive Flight metrics;
- confirmation tails are retained;
- no second Flight begins in the current development session.

## 29.4 Simulation integrity

- normal concerns cannot read truth;
- physical liftoff/touchdown do not command detectors;
- `1×` and `2×` produce equivalent domain outcomes;
- Pause does not appear as source outage;
- deterministic observation civil timestamps derive from `civilStartUtc + scenarioTime + wallClockOffsetS`, never host wall time;
- the exact jump applies prospectively at `80.0 s` without changing monotonic calculations;
- source-like GNSS East/North errors are added before the fixed local-to-geographic conversion, and truth coordinates do not reach normal concerns;
- every declared source error is applied to its intended emitted field in the normative order without synthesizing unavailable Track or other absent values;
- exact-zero truth velocity makes source Track unavailable before errors even when source GS error is positive; the first later non-zero truth sample may derive Track normally, and zero-speed landing/terminal phases remain Track-unavailable;
- the final phase is valid JSON with `completed_ground.startS = 212.0` and `endS: null`;
- fixture is deterministic;
- no uncontrolled randomness is used.

## 29.5 Derivation

- altitude derives from pressure and QNH using the specified units, formula, constants, and versioned calculation context;
- simulator pressure generation round-trips through the same contract without exposing truth altitude to C7;
- Device True Azimuth is derived by C7 from raw Device Magnetic Azimuth plus east-positive declination obtained through the replaceable provider;
- C8 never receives raw magnetic orientation as a ready-made True-North value;
- VS uses unweighted OLS over the inclusive `3.0 s` source-time window, requires `20` distinct samples spanning at least `2.0 s`, exposes `olsAltitudeSlope3sMin20Span2sV1`, and clears/rebuilds its window across pressure invalidity or gaps;
- wind candidates are evaluated continuously during active Flight whenever required observations are valid, without climb/level/descent or scenario-phase gating;
- wind is accepted only through quality gates and no later than `108.0 s` in the normal fixture;
- rejected candidates do not overwrite accepted wind;
- takeoff weather correction uses current valid GNSS Track and the meteorological weather-wind `from` direction, with zero correction when required direction/quality context is unavailable;
- landing detection does not use truth wind;
- stationary GS at or below `1.0 km/h` uses a zero ground vector without synthesizing Track;
- moving landing evaluation still requires valid Track;
- no landing fallback without accepted estimated wind exists;
- takeoff and landing holds use exact elapsed-source-time predicates, inclusive endpoints, defined reset behavior, and the accepted confirmation/timeout tie priority rather than sample counts or observed time;

## 29.6 Recording and Summary

- C9 creates the approved two-layer record;
- C9 initialization consumes the complete C3 creation handoff;
- C9 finalization consumes the complete C3 completion handoff;
- finalized complete/degraded records contain both special points with required identity, location, boundary, confirmation, detector, and Flight-association fields;
- Summary derives only from finalized record;
- complete, degraded, and failed outcomes are distinguishable;
- active and finalized distance independently sum only accepted position pairs within uninterrupted GNSS distance-continuity segments;
- each eligible active and finalized pair independently uses the fixed haversine formula with `R = 6371008.8 m`;
- unavailable, invalid, explicit interruption/continuity-gap, and source-monotonic-invalid/discontinuous intervals end a distance segment, while an idempotently ignored identical redelivery and delivery delay alone do not;
- the first valid position after a segment break is anchor-only, so the final pre-gap to first post-gap chord contributes zero and only a subsequent valid within-segment pair resumes accumulation;
- valid covered time sums only the source-monotonic intervals belonging to the same accepted position pairs as distance, excluding every gap-crossing interval;
- Flight duration still includes GNSS outages;
- the transient active-distance display is not the authoritative Summary source;
- duration uses monotonic effective boundaries;
- wall-clock change does not alter duration, detector holds, VS, wind, or ordering, while retained boundary civil timestamps follow the exact prospective offset mapping;
- identical same-timestamp redelivery is idempotently ignored;
- missing, backward, or conflicting same-timestamp monotonic input cannot be treated as valid ordering or duration;
- accepted QNH and altitude-calculation context are retained with the derived altitude history;
- projection and distance calculation identifiers/versions are present in the required retained or validation context.

## 29.7 Degradation

- map unavailability does not stop Flight or alter lifecycle, detection, derivation, recording, or Summary;
- map scale remains `2000 m ±2%` across ground, active Flight, rotation, layout-size recalculation, and GNSS recovery, with no `1500 m` fallback or scenario control;
- `flutter_map`, attribution, copyright URL, and external-link details remain inside C8; attribution is visible, screen-oriented, unobscured, and exposes the accessible action to `https://www.openstreetmap.org/copyright` without making launch success authoritative;
- attribution-link launch failure does not affect map state, Flight lifecycle, detection, derivation, recording, or Summary;
- no offline, prefetch, or bulk-download feature exists;
- the exact `112.0–117.0 s` GNSS outage does not complete Flight;
- C4 normalizes the explicit unavailable transition at `112.0 s` before any same-time GNSS observation and no timeout or silence inference is required;
- C4 normalizes restoration at `117.0 s` before accepting the recovered observation at the same source time;
- position, GS, and Track remain unavailable until that recovered observation is accepted;
- all downstream outage behavior crosses C4 rather than using C10 fault or phase metadata;
- the same Flight continues after recovery only under the explicit bounded P3 reopening for this validation case;
- accepted wind is retained;
- landing detection is suspended during outage;
- recovery continues the same Flight;
- final record is degraded;
- failed recording shows completed Flight without false successful retention.

## 29.8 Evidence

- automated normal end-to-end test exists;
- exact scenario parser, phase-boundary, and reference-sequence tests exist;
- fixed-origin, known-East, known-North, error-before-conversion, and geographic reference-sequence tests exist with tight binary64 tolerances;
- frozen-reference tests cover exact-zero truth Track unavailability, positive source GS error without Track synthesis, later non-zero Track derivation/error, and zero-speed landing/terminal availability while preserving the separate stationary-landing rule;
- controlled GNSS-outage tests cover exact explicit interruption/restoration timing, restoration-before-recovered-observation ordering, separate-stream equal timestamps, unavailable derived GNSS state, absence of observations in the half-open gap, C4-only downstream propagation, retained gap evidence, and silence-without-transition behavior;
- map-unavailable validation exists;
- primary top-row semantic-state validation and fixed-scale/C8/OSM validation exist without relying on rendered labels or package widget classes where semantic/accessibility assertions are available;
- OSM tests cover visible/unobscured and screen-oriented attribution, the exact accessible copyright target, C8-only implementation details, and launch-failure isolation while preserving existing tile-service rules;
- detector boundary tests include GNSS-Track headwind projection and stationary zero-vector landing behavior;
- wind numerical and windsock-presentation tests exist;
- active flown-distance threshold tests exist;
- deterministic distance-segment tests cover an uninterrupted contribution, zero pre-gap to post-gap contribution, anchor-only recovery, resumed within-segment accumulation, excluded outage distance and covered time, and unchanged elapsed/total Flight duration;
- tests prove the fixed haversine formula uses `R = 6371008.8 m`, active and finalized Summary distance apply that formula and the segment rule independently, and the degraded Summary is lower than the corresponding uninterrupted fixture by the omitted GNSS-covered segment;
- truth-leakage tests prove no truth-coordinate or truth-distance shortcut reaches C4, C7, C8, C9, or Summary, and calculation-version tests cover the projection and distance identifiers;
- tests prove continuous valid source-time batching creates no false segment break and invalid or source-monotonic-discontinuous GNSS input does;
- pressure/QNH round-trip tests exist;
- every declared source-error profile has deterministic reference-sequence coverage;
- exact virtual civil-time and `80.0 s / +3600 s` jump tests exist;
- exact nullable-terminal-phase parser tests exist;
- exact detector-hold tests cover `2 Hz`, `1 Hz`, jitter, missing, batched, idempotent, invalid, hysteresis, cancellation, timeout, and tie cases;
- takeoff-order tests and retained diagnostics cover qualification start/completion, candidate activation, same-observation confirmation-hold start when applicable, later confirmation, effective-boundary custody, batching/delay equivalence, and redelivery idempotency;
- exact OLS VS tests cover window membership, minimum data, gaps, recovery, batching, redelivery, sign, linear recovery, extrema, and calculation context;
- magnetic-declination conversion and fallback tests exist;
- identical-redelivery and conflicting-collision tests exist;
- monotonic-invalidity tests exist;
- Takeoff Point and Landing Point handoff/retention tests exist;
- truth-leakage tests exist;
- diagnostics explain significant transitions.

---

# 30. Bounded Implementation Tuning

The following do not require a new owner decision when accepted semantics and the exact `scenario-v1` reference sequence remain unchanged:

- internal parser/model organization for the fixed JSON asset;
- mathematically equivalent implementation of the specified interpolation and integration formulas within accepted numeric tolerance;
- Pratt versus Taubin circle initialization;
- exact numerical wind-quality thresholds, provided the normal fixture accepts by `108.0 s` and all quality tests pass;
- exact Track-loss grace period;
- compatible stable `flutter_map` version selected and locked through normal dependency management without changing the C8 boundary or accepted behavior;
- pixel geometry, typography, spacing, and animation;
- exact compact estimated-wind placement within the fixed simple-circle/windsock semantics;
- exact visual stroke, section count, colour, animation, and pilot-marker layering;
- bounded typography and animation of the fixed `FLT`/`DST` status-zone behavior.

The following are no longer implementation tuning for `scenario-v1` and require an explicit plan update:

- phase start/end times;
- phase kinematic endpoints;
- truth-wind direction or speed;
- QNH, origin, civil start time, or fixture declination;
- variation formulas;
- source cadences;
- truth integration step/method;
- physical liftoff/touchdown times;
- GNSS-outage interval.

The fixed primary-tile state switch, `2000 m ±2%` map target, `flutter_map`/OSM Standard bounded implementation, virtual civil mapping and jump transform, nullable terminal phase, source-error order, detector holds, and OLS VS algorithm are not implementation tuning.

Tuning becomes an owner decision when it changes product meaning, authority, scope, accepted outcome, reference fixture, or difficult-to-reverse technical direction.

# 31. Explicitly Deferred Decisions

- production detector thresholds;
- live Android source implementation;
- Android background architecture;
- iOS source and background implementation;
- post-slice whole-product map-provider standard, final Mapbox replacement/integration strategy, vector-map architecture, and offline-map architecture;
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
- a full graduated compass ring;
- expanded interactive diagnostics overlay;
- operational wind-warning thresholds, colour/blink policy, and safety behavior above the `0–8 m/s` visual scale;
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

The slice omits preparation, durable history, Route, fuel, multiple Flights per session, live platform integration, and broader application flow. It also uses provisional Flutter and the owner-approved bounded `flutter_map` plus OpenStreetMap Standard raster implementation behind a replaceable C8 boundary.

## Reversibility

- no durable schema is selected;
- source, map, and platform integrations are adapters;
- domain semantics do not depend on Flutter widgets or plugins;
- the OSM tile/copyright endpoints, HTTP behavior, attribution/link-launch realization, and all `flutter_map` or launcher types remain internal to C8, so a later renderer may replace that implementation as a whole;
- simulator truth does not enter normal product paths;
- provider-specific types do not define product meaning;
- deferred domains are not collapsed into the first-slice model.

## Outcome

`Aligned with explicit simplification`.

The owner-approved presentation, time, detector, derivation, and bounded map decisions support a real pilot-visible Flight process, preserve semantic distinctions and lifecycle authority, and do not require Product Vision or Product Direction revision. The map choice is explicitly limited to the first slice and leaves permanent provider, vector/offline, and Mapbox strategy deferred.

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

## 34.5 Scenario-v1 timing and format are fixed

The owner accepts the exact `212.0 s` scenario schedule, JSON asset contract, altitude profile, route headings, source cadences, deterministic variation formulas, and `112.0–117.0 s` GNSS outage defined in section 12 as the normative first-slice fixture.

The schedule is an engineering approximation derived from the previously accepted launch speed, climb rate, target height, route diversity, approach, flare, and total-duration inputs. It intentionally determines when climb reaches approximately `100 m` above takeoff, when the nominal altitude plateau occurs, and when descent begins so implementation agents do not invent incompatible fixtures. Those altitude phases do not control estimated-wind eligibility. It is not a claim about exact real-world paramotor performance.

## 34.6 Special-point retention and handoffs are explicit

The owner accepts the logical Takeoff Point and Landing Point representation and the explicit C3 to C9 creation/completion handoffs defined in section 19. Visible passive Takeoff Point navigation remains deferred, but both points remain mandatory authoritative retained Flight context.

## 34.7 Estimated-wind calculation is phase-independent

C7 evaluates estimated-wind candidates continuously throughout the active Flight whenever required GNSS observations are valid and a candidate window exists. Climb, nominal level flight, descent, route-leg identity, vertical speed, and simulator phase are not estimator eligibility gates and do not reset valid history. Acceptance or rejection is determined by the defined input-quality, residual, angular-coverage, conditioning, and uncertainty gates. Flight conditions that violate the circle-model assumptions are rejected through those measured quality characteristics rather than through privileged phase labels.

## 34.8 Active Flight progress uses `FLT` with transient `DST`

The top-centre camera-adjacent zone shows `Waiting for Takeoff` on the ground, defaults to elapsed Flight time as `FLT` while active, and temporarily shows `DST` for `3 s` whenever cumulative flown distance crosses another `500 m` threshold. Distance starts at the effective takeoff boundary and sums only accepted position pairs within an uninterrupted GNSS distance-continuity segment. Any recorded unavailable, invalid, interruption/continuity-gap, or source-monotonic-invalid/discontinuous interval ends the segment; the first recovered position is a new anchor and the pre-gap to post-gap chord is excluded. Identical idempotent redelivery and observed batching or delay with continuous valid source-time semantics do not create a break. Active distance holds during an outage and is not the completed Summary's authoritative source; Summary derives independently from the retained C9 record using the same segment and valid-covered-time rule.

## 34.9 The first wind UI keeps a simple circle and bounded windsock experiment

The first slice includes one simple orientation circle approximately `80%` of screen width rather than a complex graduated compass ring. After the first accepted estimate, a windsock-like glyph begins at the pilot centre and extends downwind. Length/sections encode `0–8 m/s`, the numeric value is shown at approximately `0.5 m/s` display granularity, and visual length is capped above `8 m/s` without quantizing the estimator. Warning and safety policy remain deferred.

## 34.10 Expanded diagnostics overlay remains out of scope

The earlier considered expandable inspector is not implemented. The compact simulation panel remains, while detailed observability uses replaceable tests, structured logs, bounded developer output, the shared diagnostic snapshot, and the event timeline.

## 34.11 Stationary landing evaluation does not require Track

For landing detection, valid GS at or below `1.0 km/h` defines a zero ground-velocity vector and does not require Track. Above that threshold, Track remains mandatory. This rule does not synthesize Track for map presentation or retention and does not remove the requirement for a valid accepted estimated wind.

## 34.12 Takeoff headwind correction uses current GNSS Track

The bounded experimental takeoff detector projects valid fresh meteorological weather wind onto the current valid GNSS Track from the same GS observation. Device orientation, candidate displacement, simulator heading, and privileged truth are excluded. Missing, stale, invalid, or insufficiently accurate direction context produces zero correction, and tailwind never lowers the threshold.

## 34.13 Scenario geography and pairwise distance are deterministic

The owner accepts the exact section 12.3 WGS84 constants and fixed local-tangent East/North-to-latitude/longitude approximation for `scenario-v1`. Deterministic GNSS East/North source errors are added before conversion. Active and finalized distance derive independently from accepted geographic observations using the section 10.4 haversine formula with `R = 6371008.8 m`, after the existing distance-continuity segment rule has established pair eligibility. These bounded fixture choices do not select production geodesy or whole-product geospatial architecture.

## 34.14 Controlled GNSS outage crosses the normal C4 boundary

The controlled fixture emits an explicit source-equivalent unavailable transition at `112.0 s` and restoration transition at `117.0 s` through C10 to C4. At restoration, C4 processes the transition before the recovered GNSS observation at the same source time. Silence alone does not define the fixture outage, and no production timeout or freshness policy is introduced. All downstream outage and recovery behavior follows from C4-derived state through the existing concern boundaries.

## 34.15 Ground primary tile switches from weather wind to GS

Before confirmed takeoff, weather-source wind occupies the primary left tile, GS is absent as a primary value, altitude remains central, and Flight VS is absent. After confirmed takeoff, that same left tile switches to GS, altitude remains central, and the right tile shows VS. Weather wind disappears from Product UI after takeoff; estimated wind appears only in its separate orientation-circle/windsock context after acceptance. Unavailable, valid-zero, stale/degraded, and valid weather states remain distinct.

## 34.16 Map scale is fixed at `2000 m ±2%`

The target is the horizontal-centreline ground distance between the geographic positions under the left and right edges of the full logical viewport, including the area behind the temporary simulation panel. C8 computes only the fractional Web Mercator zoom needed to preserve that target for viewport width and centre latitude. Rotation, ground/Flight state, GNSS recovery, provenance, scenario metadata, and C10 do not select another scale; there is no `1500 m` fallback.

## 34.17 The bounded renderer is `flutter_map` with OSM Standard raster tiles

The first slice uses `flutter_map` and OpenStreetMap Standard raster tiles from `https://tile.openstreetmap.org/{z}/{x}/{y}.png`, wholly behind C8. Visible, unobscured `© OpenStreetMap contributors` attribution remains screen-oriented during map rotation and exposes an accessible action to exactly `https://www.openstreetmap.org/copyright`. Attribution, URL-launch, provider, and package types/details do not escape C8, and link-launch failure has no map, lifecycle, detector, derivation, recording, or Summary authority. Identifying User-Agent, normal HTTP caching, current-view-only requests, no prefetch/offline/bulk download, and degraded spatial behavior remain mandatory. This does not establish a permanent provider or package; a later Mapbox implementation may replace the complete C8 renderer.

## 34.18 Virtual civil time is scenario-derived

Virtual civil UTC is `civilStartUtc + scenarioTimeS + wallClockOffsetS`, independent from host wall time. Pause freezes it, `2×` changes only host playback duration, Reset restores the zero offset and civil start, and batching preserves source civil timestamps. The exact validation jump begins at source time `80.0 s` and adds `+3600 s` prospectively, leaving the Takeoff Point pre-jump and the Landing Point post-jump without altering monotonic duration, holds, ordering, VS, or wind.

## 34.19 The terminal scenario phase uses nullable JSON end time

`endS` is `number | null`. The sole open-ended phase is the final `completed_ground` item with `startS: 212.0` and `endS: null`, meaning `[212.0, +∞)` until Reset. String/sentinel encodings and invalid nullable-end arrangements are rejected.

## 34.20 Every declared source error is applied in a fixed order

Truth profiles and ideal source-equivalent values are evaluated before field-specific source errors. GNSS position errors precede geographic conversion; GS is derived from truth velocity then error-added and clamped; magnetic error is added to the source-equivalent magnetic azimuth; and pressure error follows inverse pressure calculation. Track availability is decided before errors: a phase-declared unavailable field or exact-zero truth ground-velocity vector emits no Track, does not evaluate `atan2(0,0)`, and remains unavailable even if GS source error is positive. Only an otherwise available, non-zero truth vector is converted to Track and then receives normalized source error. Errors never synthesize unavailable fields, no declared profile is unused, and the landing-specific stationary vector remains separate and unchanged.

## 34.21 Detector holds use exact source-time semantics

All takeoff and landing holds use accepted normalized source observations, elapsed source monotonic time, inclusive endpoints, explicit reset boundaries, and no sample-count, observed-time, batching, playback-speed, or host-wall-time interpretation. Takeoff uses exact `1.0 s` qualification/confirmation/cancellation holds and a `15.0 s` timeout from its provisional boundary with confirmation-first tie priority. The accepted observation that completes takeoff qualification activates the candidate and is then evaluated once as the first possible confirmation-hold observation; it may start but cannot instantly complete that `1.0 s` hold. The effective boundary remains the first qualification observation, and diagnostics retain the complete ordering. Landing uses exact `15.0 s` confirmation, hysteresis that preserves identity but clears accumulated confirmation and boundary, and strict `1.0 s` hard cancellation.

## 34.22 Vertical speed uses versioned unweighted OLS

C7 uses unweighted ordinary least squares over the inclusive `[t - 3.0 s, t]` source-time window, requiring at least `20` distinct accepted altitude samples spanning at least `2.0 s`. Invalidity or a pressure gap clears the window; recovery requires fresh minimum history. The calculation identifier is `olsAltitudeSlope3sMin20Span2sV1`, and finalized climb/descent extrema come only from retained valid outputs.

All other unresolved values in this document are classified as bounded implementation tuning or explicitly deferred decisions.
