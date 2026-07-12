# Flight Model

## Status

This document is **WIP**, **non-canonical**, and **not an implementation requirement**. It is subject to owner review and later promotion. See the [product WIP policy](README.md) for the governing rules.

## Purpose and Scope

This document defines one Flight as a domain concept: its airborne boundaries, creation and finalization, Takeoff Point, Landing Point, and relationship to summary and replay.

The [Flight Mode model](flight-mode-model.md) owns the operational lifecycle around a Flight. The [limited Navigation model](navigation-model.md) owns the navigation role of the Takeoff Point. This document does not define a Flight Log schema, parameter table, recording frequency, storage design, algorithm, or screen layout.

## Definition of Flight

A `Flight` is one continuous airborne episode between takeoff and landing. It begins only after takeoff is detected or confirmed and ends at confirmed landing or manual completion.

A Flight is distinct from Flight Mode. There is no separate Flight Session domain entity.

## Core Invariants

- A Flight represents one continuous airborne episode.
- A Flight is created only after takeoff is detected or confirmed.
- A saved Flight always has a Takeoff Point based on actual detected takeoff.
- Takeoff Point and Landing Point are distinct special points.
- Everything retained through confirmed landing belongs to the Flight.
- Confirmed landing finalizes the Flight without retrospectively trimming its final segment.
- A Flight contains enough information to support later replay of the physical flight situation, but the required data is deferred to later logging work.

## Relationship to Flight Mode

[Flight Mode](flight-mode-model.md) may exist without a Flight while Ready on Ground. Its takeoff transition creates a Flight, and its landing or manual-completion transition ends the current Flight.

One continuous period in Flight Mode may create multiple independent Flights. Flight Mode is not part of the Flight record, and no Flight Session groups those Flights.

## Flight Lifecycle and Boundaries

The conceptual Flight lifecycle is:

```text
takeoff detected or confirmed
→ Flight created with an estimated actual start boundary
→ continuous airborne episode
→ confirmed landing or manual completion
→ Flight finalized or discarded
```

The start boundary should reflect actual takeoff, not entry into Flight Mode, first ground movement, or the location where the application was opened.

At confirmed landing, the end boundary includes everything recorded up to confirmation. The accepted direction is not to trim the final segment retrospectively.

## Flight Creation

A Flight is created only after takeoff has been detected or confirmed. Creation may use recent buffered measurements to estimate an earlier actual takeoff boundary without treating the earlier ground-waiting period as an existing Flight.

The exact detector, confirmation rule, and retrospective boundary-estimation method are deferred.

## Pre-Takeoff Rolling Buffer

While Flight Mode is Ready on Ground, a short rolling in-memory buffer of recent measurements may be maintained.

After takeoff confirmation, the buffer may be examined retrospectively to estimate:

- the transition from ground movement to stable airborne movement;
- Flight start time;
- Takeoff Point.

If takeoff does not occur, buffered measurements are overwritten and not persisted. The exact duration, measurements, and analysis method are deferred.

## Start Time and Takeoff Point

Flight start time and Takeoff Point should represent the estimated actual takeoff transition. They are not defined by entry into Flight Mode or by the first ground movement.

The Takeoff Point is based on actual detected takeoff. The exact estimation method and any quality or confidence semantics are deferred.

## In-Flight Continuity

Once created, the Flight remains one continuous airborne episode until confirmed landing or manual completion. Everything recorded during that interval belongs to the Flight.

This continuity rule defines the domain boundary only. It does not decide which values are recorded, their frequency, or their storage representation.

## Landing and Finalization

At confirmed landing:

- the Flight ends;
- all information retained through confirmation remains part of it;
- the final segment is not retrospectively trimmed;
- the Flight is finalized;
- its summary becomes available.

Flight Mode immediately resumes Ready on Ground as defined by the [Flight Mode lifecycle](flight-mode-model.md#landing-and-flight-completion).

The exact landing detector and Landing Point determination method are deferred.

## Manual Completion and False Detection

The [Flight Mode model](flight-mode-model.md#manual-flight-completion) owns the pilot's operational choice to save a real Flight or discard a false detection.

Saving manually finalizes the current episode as a real Flight. Discarding removes the false episode entirely: no Flight and no hidden diagnostic Flight record are retained.

Manual completion does not by itself exit Flight Mode. The semantics of a Landing Point for a manually saved Flight remain open.

## Takeoff Point

Takeoff Point:

- is based on actual detected takeoff;
- is not the location where Flight Mode was entered;
- exists for every saved Flight;
- remains a special point throughout the Flight;
- retains its identity when crossed, reached, or revisited;
- may serve a navigation-awareness role.

The [limited Navigation model](navigation-model.md#takeoff-point-as-a-special-waypoint) owns that navigation role. Takeoff Point determination algorithms and data representation are outside this model.

## Landing Point

Landing Point:

- is distinct from Takeoff Point;
- belongs to the completed Flight record;
- supports summary and replay;
- has no navigation role inside the already completed Flight.

This model does not define landing zones, landing assistance, point storage, or determination algorithms.

## Summary and Replay Relationship

A completed Flight makes a summary available. A Flight should also contain enough information to support later replay of the physical flight situation and principal information available to the pilot.

The Flight model establishes that responsibility but does not define summary fields, recorded series, replay behavior, or a persistence schema. Those belong to later, separately reviewed work.

## Excluded and Deferred Concepts

- Flight Log and parameter schemas;
- recorded values, intervals, precision, validity, and quality;
- storage and synchronization;
- takeoff and landing detectors;
- rolling-buffer duration and retrospective analysis;
- exact start-time, Takeoff Point, and Landing Point determination;
- summary content and replay behavior;
- landing-zone logic and landing assistance;
- architecture, APIs, services, frameworks, and UI layout.

## Open Questions

- What exact semantic distinction should be used between detected and confirmed takeoff and landing?
- Must every manually saved Flight have a Landing Point, and how should that point avoid falsely implying a confirmed landing?
- What quality or confidence information, if any, should accompany retrospectively estimated start time and Takeoff Point?
- What minimum information is required for a Flight to support useful summary and faithful replay? This belongs to later logging and parameter work.
- How should an interrupted Flight be classified when neither confirmed landing nor deliberate manual completion is available?
