# Limited Navigation Model

## Status

This document is **WIP**, **non-canonical**, and **not an implementation requirement**. It is subject to owner review and later promotion. See the [product WIP policy](README.md) for the governing rules.

## Purpose and Deliberate Scope Limit

This document defines only the navigation state and directional semantics required by the first solo local paramotor-flight scenario. It covers Waypoint, Current Waypoint, Active Navigation, passive awareness, directional reference, and orientation principles.

It is not a complete navigation specification. Route sequencing, point-reached behavior, detailed guidance, algorithms, parameter definitions, persistence, and screen design remain outside its scope.

## Sources

This WIP is based only on:

- [`ITERATION.md`](../../../ITERATION.md);
- [`docs/product/CurrentState.md`](../CurrentState.md);
- [`docs/product/wip/product-vision-reconstruction.md`](product-vision-reconstruction.md);
- [GitHub issue #6](https://github.com/AlexanderTsarkov/AirLink/issues/6);
- owner-approved decisions supplied for issue #6.

No additional legacy documents were used.

## Assumptions

- The initial free-flight scenario requires only one Current Waypoint and a separate Active Navigation state.
- Detailed route behavior is not required to define the initial navigation semantics.
- Track and estimated Heading can remain distinct product concepts before parameter validity contracts are defined.

## Terminology

- **Waypoint:** a point that may provide navigation target or context.
- **Takeoff Point:** a special Waypoint based on actual detected takeoff.
- **Current Waypoint:** the current navigation target or context.
- **Active Navigation:** a distinct navigation state that adds actual guidance toward the Current Waypoint.
- **Passive awareness:** contextual information about a Current Waypoint without Active Navigation.
- **GPS Track:** actual movement direction over the ground.
- **Estimated Heading:** a derived estimate of aircraft orientation that may be available when valid wind information exists.

Direction terms must remain explicit; Track, Heading, bearing, and device orientation are not interchangeable.

## Directional Reference

All pilot-facing navigation directions use True North.

Magnetic North may be involved in ground device orientation, but a magnetic direction is corrected to True North. Exact declination data, sensors, algorithms, validity rules, and display formatting are deferred.

## Waypoint

A Waypoint may represent:

- a planned route point;
- the Takeoff Point;
- an arbitrary point selected on the map.

This definition does not establish identifiers, storage structures, route collections, editing behavior, or automatic sequencing.

## Takeoff Point as a Special Waypoint

The [Flight model](flight-model.md#takeoff-point) owns the identity and Flight-boundary meaning of Takeoff Point.

Within navigation, Takeoff Point is a special Waypoint that remains available after it is reached, crossed, or revisited. Ordinary route-point completion semantics must not cause it to disappear or lose its identity.

## Current Waypoint

Current Waypoint is the current navigation target or navigation context. It may exist while Active Navigation is off.

Selecting or changing Current Waypoint does not by itself determine route progression or enable guidance.

## Active Navigation

Active Navigation is a separate persistent navigation state that adds actual guidance. It cannot exist without a Current Waypoint.

Here, persistent describes a state that remains active across a Current Waypoint change unless another accepted transition changes it. It does not select a persistence mechanism or storage design.

## State Invariants

- Active Navigation requires a Current Waypoint.
- Current Waypoint may exist without Active Navigation.
- Changing Current Waypoint while Active Navigation is on does not automatically disable Active Navigation.
- Takeoff Point remains available after it is reached, crossed, or revisited.
- Assigning Takeoff Point as Current Waypoint after takeoff does not automatically enable Active Navigation.

The behavior required if a Current Waypoint becomes unavailable while Active Navigation is on remains open.

## Free-Flight Behavior After Takeoff

After takeoff in free flight with no route, Takeoff Point becomes Current Waypoint. Active Navigation remains off.

This gives the Takeoff Point a passive navigation-awareness role without assuming that the pilot requested guidance.

## Passive Awareness

Without Active Navigation, the product may provide passive awareness of Current Waypoint, including:

- point type;
- map visibility;
- distance.

ETA is a **candidate example**, not an accepted mandatory value. Exact content, calculation, update behavior, priority, and presentation are deferred.

## Active Guidance

Active Navigation adds actual guidance toward the Current Waypoint. This is a conceptual distinction from passive awareness, not a definition of guidance outputs or presentation.

No bearings, cues, alerts, path instructions, arrival behavior, or automatic transitions are selected here.

## Changing the Current Waypoint

Changing Current Waypoint while Active Navigation is on does not automatically turn Active Navigation off. The exact transition of guidance to the new Current Waypoint is deferred.

How a change is initiated, validated, represented, or presented is outside this model.

## Ground Orientation

On the ground, device orientation may use the phone magnetometer. Magnetic direction is corrected to True North using position and magnetic declination before it is used as pilot-facing orientation.

This is a semantic product rule. It does not select a sensor-fusion method, declination model, update rate, fallback behavior, or validity threshold.

## In-Flight Track and Estimated Heading

In flight:

- GPS Track represents actual movement over the ground;
- the magnetometer is not the primary movement-direction source;
- estimated Heading may be derived when valid wind information exists.

Track and estimated Heading have different meanings and must not be presented as an unlabeled generic direction. Their detailed availability, validity, quality, calculation, and display contracts belong to later parameter work.

The primary in-flight orientation choice between Track-up and estimated Heading-up remains deferred for experimentation and pilot feedback.

## Pilot-Centered Rotation Principle

For the main preflight and flight presentation:

- the pilot remains centered;
- map and compass graphics rotate around the pilot according to the selected orientation.

This is a behavioral product principle, not a screen-layout specification. It does not select controls, visual hierarchy, dimensions, colors, widgets, or animation.

## Explicitly Deferred Navigation Behavior

- route sequencing and completion;
- waypoint-reached behavior and reach radii;
- automatic transitions between route points;
- landing-start versus landing-end semantics;
- route editing and route collections;
- detailed guidance outputs;
- exact passive-awareness and navigation presentation;
- Track-up versus estimated Heading-up as the final in-flight orientation;
- handling of missing or invalid direction and wind information;
- parameter definitions, storage, APIs, services, architecture, and implementation technology.

## Expected Promotion Target

After owner review and resolution of relevant open questions, accepted durable content may be promoted into an appropriate canonical product-domain document.

The final canonical location has not yet been selected and must not be created by this WIP.

## Open Questions

- What accepted transition disables Active Navigation?
- What should happen if Current Waypoint becomes unavailable while Active Navigation is on?
- Which passive-awareness values are required beyond the accepted possible examples?
- Under what product-level validity conditions may estimated Heading be offered?
- How should the selected in-flight orientation be evaluated with pilots before Track-up or estimated Heading-up is accepted?
- Which parts of planned-route behavior, if any, are needed after the initial free-flight scenario?
