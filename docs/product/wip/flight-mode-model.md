# Flight Mode Model

## Status

This document is **WIP**, **non-canonical**, and **not an implementation requirement**. It is subject to owner review and later promotion. See the [product WIP policy](README.md) for the governing rules.

## Purpose and Scope

This document defines the accepted operational lifecycle while a pilot has explicitly indicated an intention to fly. Flight Mode owns that lifecycle; the [Flight model](flight-model.md) owns one airborne episode and its boundaries, and the [limited Navigation model](navigation-model.md) owns navigation state and directional semantics.

This document does not define Flight data, recording schemas, detection algorithms, navigation guidance details, or screen layout.

## Sources

This WIP is based only on:

- [`ITERATION.md`](../../../ITERATION.md);
- [`docs/product/CurrentState.md`](../CurrentState.md);
- [`docs/product/wip/product-vision-reconstruction.md`](product-vision-reconstruction.md);
- [GitHub issue #6](https://github.com/AlexanderTsarkov/AirLink/issues/6);
- owner-approved decisions supplied for issue #6.

No additional legacy documents were used.

## Assumptions

- This model uses the accepted solo local paramotor flight in VMC as its sole scenario context.
- Flight Mode is modeled only at product and domain level, without selecting an implementation representation.
- Detailed detector behavior, timing values, and resource-management mechanisms will be defined separately.

## Terminology

- **Normal application use:** non-flight activity such as weather, logbook, settings, and other ordinary use.
- **Flight Mode:** an explicitly entered and exited operating mode in which automatic takeoff and landing detection is active.
- **Ready on Ground:** Flight Mode is active and waiting for takeoff.
- **In Flight:** a detected or confirmed takeoff has created a current Flight.
- **Flight ended:** the transition in which the detected airborne episode is either finalized as a Flight or discarded as a false detection before Flight Mode resumes Ready on Ground.

`Flight ended` may be a transient transition rather than a long-lived state. No persistence or implementation representation is selected here.

## Normal Application Use

Normal application use may include weather, logbook, settings, and other non-flight activity. It does not detect takeoff.

The boundary is intentional: automatic takeoff and landing detection operates only inside Flight Mode.

## Flight Mode Boundary

The pilot enters Flight Mode explicitly to indicate an intention to fly and exits it explicitly when that intention ends.

Flight Mode is not a persisted domain object. A continuous period in Flight Mode may contain multiple Flights, but those Flights are not grouped in a separate Flight Session entity.

Ending a current Flight and exiting Flight Mode are distinct actions.

## High-Level Lifecycle

The accepted lifecycle is:

```text
Ready on Ground
→ In Flight
→ Flight ended
→ Ready on Ground
```

The final transition occurs immediately after the airborne episode is finalized as a Flight or discarded as a false detection. A summary may remain visible after a saved Flight without suspending the renewed wait for takeoff.

## Ready on Ground

While Ready on Ground:

- the application waits for takeoff;
- takeoff detection is active;
- no current Flight exists;
- a short rolling in-memory measurement buffer may be maintained.

The [Flight model](flight-model.md#pre-takeoff-rolling-buffer) owns the purpose and Flight-boundary implications of the buffer. Its duration and implementation are deferred.

## Takeoff Transition

When takeoff is detected or confirmed, a Flight is created and Flight Mode transitions to In Flight. Flight creation and retrospective estimation of the actual start belong to the [Flight model](flight-model.md#flight-creation).

Takeoff detection must not permanently rely only on speed. Speed, altitude or vertical movement, motion patterns, plausibility relative to wind, and other signals are candidate inputs for later detector design, not an accepted algorithm or mandatory input set.

The exact detector and the distinction between detection and confirmation are deferred.

## In Flight

While In Flight:

- one current Flight is active;
- its continuity ends only at confirmed landing or manual completion;
- landing detection is active;
- the pilot may manually end the current Flight.

Flight Mode owns these operational transitions. The Flight model owns what the transitions mean for the airborne episode.

## Manual Flight Completion

Manual completion must offer two semantically distinct outcomes:

1. save the current episode as a real Flight;
2. discard it as a false detection.

Discarding a false detection retains no Flight and no hidden diagnostic Flight record. Saving or discarding ends the current airborne episode but does not exit Flight Mode; the lifecycle returns to Ready on Ground.

This rule does not select controls, confirmation behavior, or screen layout.

## Landing and Flight Completion

At confirmed landing:

- the current Flight is finalized;
- its summary becomes available;
- Flight Mode immediately returns to Ready on Ground;
- takeoff waiting resumes;
- a new non-persisted rolling buffer may begin.

The exact landing detector is deferred. Flight end boundaries and the decision not to trim the recorded end retrospectively belong to the [Flight model](flight-model.md#landing-and-finalization).

## Completed-Flight Summary

A completed-Flight summary may be shown as an overlay while Flight Mode is already Ready on Ground and waiting for another takeoff.

If a new takeoff is detected while the summary is visible, the summary closes and the new Flight begins. The summary does not block takeoff detection or create a separate lifecycle period.

The summary's content and presentation are outside this model.

## Multiple Flights During One Flight Mode Period

A single continuous period in Flight Mode may contain multiple Flights:

```text
Enter Flight Mode
→ Flight 1
→ land
→ wait on ground
→ Flight 2
→ land
→ exit Flight Mode
```

This sequence does not create a Flight Session or persisted Flight Mode record.

## Ground Inactivity Timeout

Flight Mode must not remain indefinitely in high-resource ground waiting. While Ready on Ground, it uses a sufficiently long inactivity timeout.

Near timeout:

- warn the pilot;
- allow one-tap continuation;
- reset the full waiting period when the pilot continues.

If the pilot does not continue:

- exit Flight Mode;
- reduce or stop high-resource flight tracking;
- return to normal application use.

Exact timeout and warning durations are deferred. This model does not select the warning presentation or define resource-management implementation.

## Relationship to Flight

Flight Mode owns the operational context and transitions. A [Flight](flight-model.md) owns one continuous airborne episode created after takeoff and normally completed at confirmed landing, with manual completion as an explicit exceptional boundary.

Flight Mode can exist without a Flight while Ready on Ground. A Flight created by this lifecycle is distinct from Flight Mode and is not grouped under a Flight Session entity.

## Deferred Decisions

- exact takeoff and landing detectors;
- exact meaning and timing of detection versus confirmation;
- exact inactivity timeout and warning duration;
- exact rolling-buffer duration;
- controls and presentation for entry, exit, warnings, completion, and summary;
- behavior of an explicit Flight Mode exit request while a Flight is active;
- Flight data, logging, parameters, storage, APIs, services, and implementation technology.

## Expected Promotion Target

After owner review and resolution of relevant open questions, accepted durable content may be promoted into an appropriate canonical product-domain document.

The final canonical location has not yet been selected and must not be created by this WIP.

## Open Questions

- Should `Flight ended` remain a named transient lifecycle step or be described only as a transition back to Ready on Ground?
- What pilot action is required if Flight Mode exit is requested while a Flight is active?
- Which conditions, other than pilot continuation, reset the ground inactivity period?
- What minimum information must the completed-Flight summary provide? This question belongs to later summary and logging work, not this lifecycle model.
