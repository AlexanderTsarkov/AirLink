# MVP 0.1 Scope and First Meaningful Product Slice

## Status

This document is **owner-reviewed and accepted as the WIP product-level boundary for MVP 0.1**. It remains **non-canonical** and is not a detailed implementation specification.

After merge, this document may be used as an authorized product input for `AL-0002` engineering planning. It does not itself authorize implementation or select architecture, technologies, providers, algorithms, schemas, detailed UI, or implementation sequencing.

## Purpose

The purpose of MVP 0.1 is to establish the smallest coherent and genuinely usable AirLink product experience for the initial target pilot.

MVP 0.1 must support the pilot through a complete local-flight flow:

- understand current conditions;
- perform a minimal explicit Pre-Flight step;
- enter Flight Mode;
- complete one or more Flights;
- receive useful in-flight information;
- have takeoff and landing recognized;
- review the completed Flight;
- retain the Flight locally;
- open and understand the saved Flight later.

The MVP is not only an in-flight instrument prototype. It must deliver a complete preparation–Flight–completion–review outcome.

## Sources

This WIP is based on:

- [`ITERATION.md`](../../../ITERATION.md);
- [`docs/product/CurrentState.md`](../CurrentState.md);
- [`docs/product/vision/ProductVision.md`](../vision/ProductVision.md);
- [`docs/product/wip/flight-mode-model.md`](flight-mode-model.md);
- [`docs/product/wip/flight-model.md`](flight-model.md);
- [`docs/product/wip/navigation-model.md`](navigation-model.md);
- [GitHub issue #20](https://github.com/AlexanderTsarkov/AirLink/issues/20);
- owner decisions recorded in the issue #20 discussion.

No additional legacy documents were used to define this scope.

## Assumptions

- The initial scenario remains one pilot performing a solo local paramotor flight in VMC.
- MVP 0.1 is local-first and does not depend on cloud, social, multi-user, or connected-aircraft capabilities.
- Android remains the accepted initial mobile-client target.
- Detailed engineering choices belong to `AL-0002` or later specification work.
- Replay presentation is outside MVP 0.1, but sufficient time-varying Flight data must be retained now because data omitted from early Flights cannot be reconstructed later.
- Existing WIP domain models remain supporting inputs and do not become canonical through this document.

## Target Pilot and Operating Scenario

The initial target user is one paramotor pilot performing a solo local flight in visual meteorological conditions.

The initial mobile client for MVP 0.1 targets Android.

The operating scenario assumes:

- one pilot;
- one Android mobile device;
- one local flying area;
- no crew coordination;
- no connected aircraft equipment requirement;
- no dependency on another AirLink user;
- no route-based mission requirement;
- one or more sequential airborne Flights during one continuous period in Flight Mode.

The scenario is deliberately narrow. It provides a bounded basis for validating the core product value without defining the full future AirLink audience or product ecosystem.

## First Meaningful Product Slice

The first meaningful product slice is:

> A paramotor pilot can use AirLink to review current local flight conditions, acknowledge a minimal Pre-Flight checklist, explicitly enter Flight Mode, complete and record one or more local Flights, remain oriented and informed during Flight, return with awareness of the Takeoff Point, receive a completed-Flight summary after landing, exit Flight Mode when finished, and later open the saved Flight with its map track and principal summary information.

The slice begins before Flight Mode and ends only when the completed Flight can be accessed and understood after leaving Flight Mode.

Saving a Flight without later access to it is not a complete MVP outcome.

## MVP 0.1 Capability Scope

### Mobile Application Foundation

MVP 0.1 includes the Android mobile-application foundation required to support the accepted product slice.

This includes the application-level permissions and operating access needed for the selected MVP capabilities. Exact Android framework, minimum supported Android version, permission handling, platform behavior, background execution, and resource-management mechanisms are deferred.

### Home and Current-Location Weather Context

The application provides a normal-use surface from which the pilot can understand relevant current conditions and proceed toward Pre-Flight.

The mandatory minimum weather context includes:

- current wind speed;
- current gusts;
- current wind direction;
- atmospheric pressure or QNH required by the initial altitude use case;
- observation or update time sufficient to understand data freshness.

A near-term wind forecast should also be available when the selected provider supplies suitable data without disproportionate implementation complexity. The intended horizon is approximately the next two hours.

The exact provider, refresh policy, forecast intervals, fallback behavior, and representation are deferred.

Temperature, precipitation, cloud cover, and broader weather forecasting are not mandatory for MVP 0.1.

### Minimal Pre-Flight

Before entering Flight Mode, the pilot is presented with a minimal explicit Pre-Flight step.

It includes:

- current wind and the near-term forecast when available;
- acknowledgement that weather for the expected Flight period is known and suitable for safe Flight and completion;
- acknowledgement that the equipment preflight inspection has been completed;
- acknowledgement that the intended Flight area is open;
- an explicit action to enter Flight Mode.

For MVP 0.1, these acknowledgements establish the intended product habit but do not require automated validation, persistence, enforcement, recommendation logic, or blocking behavior.

### Explicit Flight Mode

Flight Mode is entered explicitly when the pilot intends to fly.

Automatic takeoff and landing detection operates only while Flight Mode is active.

Flight Mode may exist while no Flight is active. While the pilot remains on the ground, the application waits for takeoff.

Exiting Flight Mode is distinct from ending an individual Flight.

### Multiple Flights Within One Flight Mode Period

One continuous period in Flight Mode may contain multiple independent Flights.

The accepted lifecycle is:

- the pilot explicitly enters Flight Mode;
- confirmed takeoff begins a Flight;
- confirmed landing completes that Flight;
- the application resumes waiting for another takeoff;
- a later confirmed takeoff begins another Flight;
- the pilot exits Flight Mode separately when the intention to continue flying has ended.

No separate persisted Flight Session is required.

### Takeoff and Landing Detection

MVP 0.1 includes automatic takeoff and landing detection sufficient to support the accepted Flight lifecycle.

A Flight begins only after takeoff is detected or confirmed. Its start should represent the estimated actual takeoff rather than entry into Flight Mode or initial ground movement.

Confirmed landing finalizes the current Flight and makes its summary available.

Detection algorithms, signals, thresholds, filters, confirmation timing, false-detection recovery, retrospective boundary estimation, and interruption handling are deferred.

### Flight Screen and Pilot-Centered Map

During Flight, the application provides a pilot-centered map presentation.

The pilot remains centered while the map and directional graphics rotate according to the active orientation model.

The pilot can control the map scale during Flight. Map zoom is a required functional capability of the Flight presentation, not optional visual refinement.

A separate numeric GPS Track field is not required.

A compass ring or scale around the pilot is required so that the pilot can understand orientation at a glance. A digital course value may be incorporated into that presentation but is not required as a separate standalone field.

On the ground, orientation may use device orientation corrected to True North. In Flight, orientation may use movement direction or a later accepted derived orientation model.

Exact orientation-source selection, source switching, validity, smoothing, fallbacks, map-scale interaction, and presentation behavior are deferred.

### Core In-Flight Information

The Flight experience includes the minimum information necessary for the initial local-flight use case:

- Ground Speed;
- altitude;
- vertical speed;
- Flight time;
- estimated wind speed and direction;
- Takeoff Point awareness;
- distance and direction or bearing to the Takeoff Point.

The exact visual hierarchy, grouping, formatting, units, update rates, validity rules, and fallback behavior are deferred.

### Estimated Wind

MVP 0.1 includes an in-flight estimate of wind speed and direction.

The product does not claim that short-term changes in calculated wind can be reliably separated from:

- pilot input;
- climb or descent;
- changes in wing behavior or configuration;
- turbulence;
- actual wind variation or gusts.

In-flight gust estimation is not part of MVP 0.1.

The estimated nature and limitations of the wind value should be explained in an appropriate non-flight context, such as onboarding, setup, help, or Pre-Flight. A persistent static warning is not required on the already information-dense Flight presentation.

A calculation-quality or stability indicator may be retained as a research and debugging parameter for development and validation of the wind-estimation mechanism. It is not required on the normal pilot-facing Flight screen.

The exact calculation model, time window, smoothing, quality metric, confidence semantics, thresholds, and diagnostic presentation are deferred.

### Takeoff Point

A Takeoff Point is created automatically after takeoff is confirmed.

It:

- represents the estimated actual takeoff location;
- remains visible throughout the Flight;
- uses a distinct recognizable map representation;
- retains its identity if crossed, reached, or revisited;
- provides the navigation context needed for the initial return-to-start use case.

The Flight presentation must provide a minimal Takeoff Point indication containing:

- distance to the Takeoff Point;
- direction or bearing toward the Takeoff Point.

MVP 0.1 does not provide active route guidance to the Takeoff Point.

### Completed-Flight Summary

Flight Summary is not a separate application screen.

After landing is confirmed and the current Flight is finalized, a Summary card or window appears within the Flight screen.

It:

- confirms that the individual Flight has ended;
- shows the principal information needed to understand the completed Flight;
- represents the same underlying Flight information later used by saved-Flight review;
- does not stop Flight Mode from waiting for another takeoff.

The Summary provides an explicit action to end Flight Mode.

If the pilot selects that action, the application exits Flight Mode and no longer expects another automatic takeoff.

If the pilot takes no action, the application remains in Flight Mode and waits for another takeoff. If another takeoff is detected while the Summary is visible, the Summary closes and a new Flight begins.

### Ground Waiting and Automatic Flight Mode Exit

Flight Mode must not remain indefinitely in its ground-waiting state.

While waiting for another takeoff after landing or before the first takeoff:

- a sufficiently long inactivity period is allowed;
- before automatic exit, the application warns the pilot;
- the warning provides a direct action to continue waiting;
- continuing resets the full ground-waiting period;
- if the pilot does not continue, the application exits Flight Mode and returns to normal application use.

Exact timeout duration, warning timing, warning and notification presentation, and continuation-control implementation are deferred.

### Local Flight Persistence

Each completed and retained Flight is stored locally.

The minimum pilot-facing saved Flight information required by MVP 0.1 is:

- recorded map track;
- date;
- start time;
- duration;
- distance;
- average speed;
- maximum speed;
- maximum altitude.

In addition to these pilot-facing fields, the Flight record must retain sufficient time-varying physical-flight and pilot-visible information to support historically faithful future replay. MVP 0.1 does not include replay presentation or playback behavior.

The exact retained parameters, sampling and recording intervals, precision, validity, historical-value preservation rules, data representation, retention policy, and storage design are deferred to `AL-0002` and later specification work.

### Saved Flight Access and Review

After leaving Flight Mode, the pilot can access and open a previously saved local Flight.

Opening the Flight shows:

- its recorded track on a map;
- its principal summary information.

When reviewing a saved Flight track, the pilot can control the map scale.

The saved Flight must be understandable both spatially and through its main summary values.

Summary-only access without the map track is not sufficient.

The post-landing Summary and later saved-Flight view should use the same core information model, while their detailed layouts may differ.

## Mandatory Flight Simulation Framework

A minimum integrated Flight Simulation Framework is mandatory for MVP 0.1.

It is a cross-cutting product capability required to execute and validate the accepted MVP behavior without requiring a real Flight for every development or test cycle.

The framework must be sufficient to exercise the meaningful product slice, including relevant transitions and pilot-visible results such as:

- ground waiting;
- takeoff;
- in-flight movement;
- changes in core flight parameters;
- estimated wind behavior;
- Takeoff Point behavior;
- landing;
- completed-Flight Summary;
- local Flight persistence;
- later saved-Flight review;
- multiple sequential Flights within one Flight Mode period where required for validation.

This scope does not define simulation architecture, scenario format, control interface, synthetic-data model, fidelity, automation, tooling, or test strategy.

The framework is mandatory for MVP 0.1, but its detailed design belongs to engineering planning.

## MVP 0.1 Principles

### Coherent Outcome Over Feature Count

Every included capability must contribute to the complete preparation–Flight–completion–review outcome.

A collection of isolated instrument values is not sufficient.

### Solo Local Flight First

The MVP remains centered on one paramotor pilot performing a solo local Flight in VMC.

Capabilities that require teams, shared operations, complex route missions, or wider ecosystem behavior are deferred.

### Explicit Pilot Intent

The pilot explicitly enters and exits Flight Mode.

Automatic detection operates inside that declared operating context rather than continuously during ordinary application use.

### Flight and Flight Mode Remain Distinct

A Flight is one airborne episode.

Flight Mode is the operating context in which one or more Flights may occur.

Ending a Flight does not automatically mean that the pilot has finished flying.

### Useful Without Pretending to Be Exact

Derived values, especially estimated wind, must be useful without being represented as direct or exact measurements.

Product presentation should communicate limitations appropriately without overloading the in-flight interface.

### Minimum Navigation Awareness, Not Route Navigation

MVP 0.1 does not expand this into a general route-navigation system.

### Local Completeness Before Connected Features

The pilot must be able to complete, save, and later review a Flight locally.

Cloud services, sharing, social functions, and multi-user behavior are not required for the initial meaningful outcome.

### Preserve Future Replayability

Replay presentation is not part of MVP 0.1, but the data needed to support historically faithful future replay must be retained from the beginning.

Later algorithm changes must not silently replace historical values that were available or used during the original Flight.

### Simulation Is Part of the MVP Capability

The Flight Simulation Framework is not optional internal convenience work. It is required to make the MVP behavior executable, testable, and reviewable without dependence on repeated real Flights.

### Product Scope Does Not Preselect Implementation

This document defines required pilot outcomes and capabilities.

It does not select technical architecture or implementation decomposition.

## Explicit Non-Scope

The following are not part of MVP 0.1:

- active route navigation;
- route lines, turn-by-turn instructions, or automatic route progression;
- multi-waypoint route planning or editing;
- landing-zone logic or landing assistance;
- Flight replay or animated playback;
- time-synchronized playback of recorded parameters;
- advanced charts, analytics, or comparative analysis;
- editing completed Flights;
- sharing Flights;
- cloud synchronization;
- multi-device synchronization;
- social Feed behavior beyond the minimum normal-use surface needed by the initial slice;
- pilot-to-pilot communication or coordination;
- group Flights or crew operations;
- school, instructor, student, or fleet-management workflows;
- marketplace or commercial ecosystem behavior;
- configurable or equipment-specific Pre-Flight checklists;
- automatic airspace verification;
- automated safety approval or weather suitability decisions;
- in-flight gust calculation;
- broad weather forecasting beyond the accepted minimum;
- persistent user-facing wind-confidence diagnostics;
- complete navigation semantics;
- a Flight Session domain object;
- detailed Flight Log or parameter schemas;
- final architecture or technology selection;
- iOS and other additional client platforms;
- production cloud or server implementation unless later required by a separately accepted capability.

These exclusions are boundaries of MVP 0.1, not permanent rejection of future AirLink concepts.

## Decisions Deferred to AL-0002 and Later Specification

The following decisions are deliberately deferred:

### Android Application Engineering

- Android application architecture;
- Android application framework;
- minimum supported Android version;
- permission implementation;
- foreground and background execution behavior;
- resource-management strategy;
- local-storage technology;
- platform-specific APIs and implementation choices.

### Weather

- weather provider;
- data-source hierarchy;
- refresh and cache policy;
- forecast intervals;
- failure and fallback behavior;
- QNH acquisition and application.

### Flight Detection and Boundaries

- takeoff and landing algorithms;
- detection versus confirmation semantics;
- thresholds, filters, debouncing, and timing;
- rolling-buffer duration and contents;
- retrospective Flight-start estimation;
- false-detection handling;
- interrupted-Flight behavior;
- manual Flight completion behavior where not already defined at product level.

### Flight Parameters and Replay-Supporting Data

- exact parameter definitions;
- which time-varying physical-flight and pilot-visible values must be retained;
- units and formatting;
- sampling and recording intervals;
- filtering and smoothing;
- precision and validity;
- stale or unavailable value behavior;
- data quality semantics;
- historical-value preservation rules;
- persistence schema and storage representation.

### Wind Estimation

- estimation algorithm;
- required inputs;
- time-window behavior;
- smoothing;
- stabilization;
- quality and confidence metrics;
- diagnostic data;
- validation methodology.

### Orientation, Navigation, and Map Interaction

- final choice between Track-up, estimated Heading-up, or another accepted orientation behavior;
- orientation-source switching;
- compass and map rotation behavior;
- magnetic-declination handling;
- bearing semantics;
- invalid-direction fallbacks;
- exact Takeoff Point tile behavior;
- exact zoom interaction method;
- zoom limits;
- automatic versus manual scale behavior;
- recentering after manual map interaction;
- persistence of the selected scale;
- platform-specific gestures and controls.

### Flight Mode Lifecycle Details

- inactivity timeout duration;
- warning timing;
- warning and notification presentation;
- exact continuation control;
- conditions other than pilot continuation that reset the waiting period;
- explicit exit behavior while a Flight is active.

### Summary and Saved Flight Review

- exact summary field semantics;
- detailed card and screen layouts;
- information hierarchy;
- navigation to saved Flights;
- retention and deletion behavior;
- later expansion toward replay or analysis.

### Flight Simulation Framework

- architecture;
- simulation scenario representation;
- controls and operator workflow;
- synthetic input generation;
- simulation fidelity;
- test automation;
- integration with application layers;
- implementation sequence.

Deferral means that these decisions must be made deliberately during engineering planning or later product work. It does not authorize an implementation agent to choose them silently.

## Open Questions

None block acceptance of the MVP 0.1 product boundary.

The detailed decisions listed in the deferred section belong to `AL-0002` or later specification work. They are not unresolved product-scope decisions in this document and must not be selected silently during implementation.

## Expected Promotion Target

After relevant engineering-planning decisions and later owner review, durable accepted content may be promoted into appropriate canonical product-scope or product-domain documentation.

The final canonical location has not yet been selected and must not be created or assumed by this WIP.

## Accepted Product Boundary

The owner has accepted this MVP 0.1 scope on the following boundary:

- it defines one coherent end-to-end outcome for the initial paramotor pilot;
- it targets Android as the initial mobile client;
- it distinguishes the product slice from the supporting capability list;
- it includes current weather context and minimal Pre-Flight;
- it includes explicit Flight Mode;
- it supports multiple sequential Flights within one Flight Mode period;
- it includes takeoff and landing detection at product level;
- it includes the required core in-flight information;
- it includes pilot-centered orientation, controllable map scale, and Takeoff Point awareness;
- it includes estimated wind without claiming unsupported precision;
- it includes completed-Flight Summary behavior;
- it includes finite ground waiting, mandatory warning and continuation before automatic exit, and eventual Flight Mode exit;
- it includes local Flight persistence;
- it requires retaining sufficient time-varying information for historically faithful future replay while deferring replay presentation;
- it includes later access to and review of the saved Flight with its map track and controllable map scale;
- it explicitly makes the minimum Flight Simulation Framework mandatory;
- it distinguishes included, excluded, and deferred areas;
- it avoids selecting architecture, technologies, providers, algorithms, schemas, thresholds, update rates, or detailed UI;
- it remains limited to the accepted solo local paramotor-flight scenario.

This accepted WIP establishes the product-level boundary for later engineering planning after merge.

It does not itself authorize implementation, promote the document to canon, or resolve the decisions explicitly deferred to `AL-0002` and later work.
