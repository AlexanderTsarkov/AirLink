# AirLink Product Direction

## Status

This document is **WIP**, **non-canonical**, and **not an implementation requirement**. It records owner-approved long-term product direction for review, but it does not become product canon until its content is explicitly accepted and promoted. See the [product WIP policy](README.md) for the governing rules.

## Purpose and Authority

This document describes what AirLink is intended to become, why the product is being shaped in this way, and which stable principles should guide future product-significant decisions. It keeps important long-term product domains visible without turning them into requirements for the current iteration, MVP 0.1, or any specific release.

This document has a different responsibility from other project artifacts:

- [`CurrentState.md`](../CurrentState.md) records accepted project state;
- [`product-vision-reconstruction.md`](product-vision-reconstruction.md) defines the initial focus and first product slice under owner review;
- [`ITERATION.md`](../../../ITERATION.md) defines the active work cycle and its boundaries;
- roadmaps and issues, when created or approved, define bounded work and release intent;
- domain specifications define detailed accepted behavior;
- implementation documentation defines technical realization.

Product processes and domain meaning are primary here. Screens and implementation structures are secondary representations and are not defined by this document.

The owner-approved concepts identified below are authoritative inputs to this draft. The draft itself remains non-canonical and subject to deliberate owner review.

## Source Basis

This WIP uses only:

1. the owner-approved long-term product context supplied for GitHub issue [#9](https://github.com/AlexanderTsarkov/AirLink/issues/9);
2. [`ITERATION.md`](../../../ITERATION.md);
3. canonical repository context in [`CurrentState.md`](../CurrentState.md) and the [product documentation index](../README.md);
4. GitHub issue [#9](https://github.com/AlexanderTsarkov/AirLink/issues/9) and its parent, issue [#8](https://github.com/AlexanderTsarkov/AirLink/issues/8);
5. the existing [Product Vision](product-vision-reconstruction.md), [Flight Mode](flight-mode-model.md), [Flight](flight-model.md), and [Navigation](navigation-model.md) WIP documents listed in the product documentation index.

No additional legacy documents were consulted. Historical feature lists, architecture, technology choices, and commercial proposals are not sources of current product truth for this draft.

## Direction and Constrained Synthesis

The long-term direction, product pillars, lifecycle, fundamental domains, and product principles in this document come from owner-approved context. Explanatory connections between those concepts are constrained synthesis for owner review. They clarify the direction without adding requirements, release commitments, or detailed behavior.

The main scope distinction is intentional:

- the existing Product Vision WIP describes the accepted initial focus and first product slice;
- this Product Direction WIP describes the owner-approved long-term direction of AirLink as a service.

Pilot Ecosystem is therefore accepted here as long-term direction, while social, marketplace, school, service, maintenance, and wider ecosystem functionality remain outside MVP 0.1 and every other release unless separately approved. This document does not modify, supersede, or retroactively reinterpret the existing Product Vision WIP. Alignment of wording between Product Vision and Product Direction must be handled deliberately during later owner review and canonical promotion.

## AirLink as a Service

AirLink is intended to become a service that supports the pilot throughout the lifecycle of real flying activity and forms an ecosystem around that activity.

It is not adequately defined as only a flight tracker, logbook, navigation application, weather screen, social feed, or collection of mobile screens. Each of those may represent part of the service, but none expresses the whole product direction.

The service should connect preparation, the Flight itself, completion, history, accumulated experience, and the wider flying context. Information produced through one part of normal use should, where appropriate, improve other parts. This continuity is important both to practical Flight Support and to the credibility of the Pilot Ecosystem.

## Two Interdependent Product Pillars

AirLink has two major, mutually dependent product pillars.

### Flight Support

Flight Support is the primary practical value and the initial reason for a pilot to use AirLink. At a long-term conceptual level, it may include:

- situational awareness before flight;
- weather and wind context;
- route preparation and navigation;
- airspace awareness;
- pilot readiness and experience context;
- equipment readiness, inspections, maintenance, and operating history;
- customizable Pre-Flight preparation;
- support during Flight;
- Post-Flight completion, history, replay, and analysis.

Flight Support should not merely display raw measurements. It should help transform available measurements, history, and context into information that is understandable and useful for pilot decisions.

This direction does not define which capabilities belong in an early release or how they are presented, calculated, sourced, or implemented.

### Pilot Ecosystem

Pilot Ecosystem is the long-term social, discovery, retention, growth, and sustainability layer around real flying activity. It may connect:

- pilots and their actual Flights;
- routes and flying places;
- nearby activity and local or wider communities;
- shared Flights and events;
- schools and instructors;
- services and maintenance providers;
- manufacturers and equipment;
- new places, opportunities, and relevant activity beyond a pilot's immediate circle.

These are domains and relationships that may become relevant over time, not an approved feature catalogue, commercial model, or release commitment.

### Why Both Pillars Matter

Flight Support gives the Pilot Ecosystem credible entities, useful context, and real activity. The Pilot Ecosystem gives Flight Support long-term continuity, discovery, community value, reach, and a possible basis for sustainability.

Flight Support alone could remain an isolated utility. Pilot Ecosystem alone could become a generic engagement product disconnected from the real needs of pilots. AirLink's long-term direction depends on their relationship, even when product delivery begins narrowly and advances one pillar before the other.

## Continuing Flight Lifecycle

AirLink should support a continuing conceptual lifecycle:

```text
Home / situational context
    ↓
Pre-Flight preparation and readiness
    ↓
Flight
    ↓
Post-Flight completion and analysis
    ↓
History and accumulated experience
    ↓
Preparation for the next flight
```

The stages should inform one another. For example, a completed Flight can contribute to pilot experience and equipment history, while that accumulated context can inform later preparation.

This lifecycle describes product continuity, not a mandatory screen sequence, fixed application navigation model, workflow gate, or release order. An early product may support only a narrow part of it.

## Fundamental Product Domains

AirLink's long-term direction includes independent but related product domains:

- Pilot;
- Flight;
- Weather;
- Route;
- Equipment;
- Airspace;
- Pre-Flight readiness and customizable checklists;
- Feed and Pilot Ecosystem.

Their inclusion does not make them requirements for MVP 0.1 or any other specific release. An early product may omit, combine, or minimally represent them as an explicit simplification. Early decisions should nevertheless avoid denying their possible future existence or collapsing them into a misleading permanent model.

Flight is an important context in which many domains meet, but it should not become a single container that owns all other domains.

### Pilot

Pilot is more than an account or profile. Over time, relevant pilot context may include physical parameters, declared previous experience, AirLink-recorded experience, recent activity, experience with particular equipment or configurations, and information derived from Flight history.

The product should be capable of distinguishing declared, recorded, and derived information where their meaning matters. Exact fields, formulas, thresholds, and schemas are deferred.

### Flight

Flight represents real flying activity and is a central context for Flight Support, history, and the Pilot Ecosystem. It may connect the pilot, equipment configuration, route context, weather, places, and later analysis without owning those domains.

The existing [Flight model](flight-model.md) defines the narrower Flight boundaries currently under review. This document does not change those boundaries or define a broader Flight schema.

### Weather

Weather and wind context contribute to situational awareness, preparation, in-flight understanding, and later analysis. AirLink should make relevant information understandable rather than present undifferentiated measurements.

Providers, forecast models, parameter sets, refresh behavior, recommendations, and thresholds are outside this direction document.

### Route

Route is a meaningful independent domain, not merely coordinates embedded inside a Flight. Routes may matter for unfamiliar terrain, difficult geography, forest or water crossings, planned cross-country Flights, coordination points, later reuse, and comparison.

This direction does not define route formats, editors, storage, navigation algorithms, or UI. The existing [limited Navigation model](navigation-model.md) remains deliberately narrower and defers planned-route behavior.

### Equipment

Equipment is an independent domain that may be associated with Flights. Relevant long-term context may include wing operating time and inspections, line or fabric condition, engine maintenance cycles, propeller or component history, reserve-parachute presence and repack date, and the equipment configuration used for a Flight.

The objective is useful readiness context, not digital bureaucracy. Normal AirLink use should allow relevant operating history and reminders to be derived or updated with minimal repeated input. Exact hierarchies, maintenance rules, mandatory intervals, and schemas are deferred.

### Airspace

Airspace awareness is a fundamental future concern. Paramotor pilots commonly fly VFR in uncontrolled airspace but may operate near, beneath, or within controlled-airspace structures, near aerodromes or other aviation zones, over areas with minimum-height or environmental restrictions, or in areas affected by temporary restrictions.

AirLink may eventually provide airspace context during planning, Pre-Flight preparation, Flight, and Post-Flight analysis. This document defines no legal interpretation, provider, regulatory rule, warning threshold, or algorithm.

### Pre-Flight Readiness

Pre-Flight is a workflow rather than merely one screen. It may combine:

1. AirLink-known context such as weather, Route, Airspace, Equipment, fuel, and relevant warnings;
2. configuration-dependent checks;
3. pilot-defined personal checklist items.

Personal items might include a helmet, gloves, radio, frequency, batteries, water, cameras, or other pilot-specific preparation. These examples illustrate customizability; they are not mandatory checklist content.

The purpose is reliable preparation without excessive manual work. Exact workflow, mandatory items, blocking behavior, and UI are deferred.

### Feed and Pilot Ecosystem

Feed is a product surface through which the pilot's visible flying world may gradually expand. The Pilot Ecosystem is the wider network of pilots, Flights, places, routes, events, equipment, organizations, and relevant services connected by real flying activity.

Neither concept defines a generic social network, content schema, ranking system, or committed commercial feature set.

## Safety and Decision Support

AirLink should support better-informed pilot decisions through useful context and reduced cognitive load. It should not claim to guarantee safety, replace pilot judgment, or silently assume legal, operational, or decision-making authority that has not been defined.

Stable principles include:

- make important information quickly understandable;
- distinguish semantically different values;
- distinguish measured, estimated, declared, recorded, and derived information where relevant;
- make recommendations and warnings contextual rather than generic;
- reuse information already known by the service;
- avoid repeatedly asking the pilot for the same information;
- combine weather, wind, airspace, equipment state, pilot experience, Route, and Flight context where that combination provides legitimate value.

Expectations may become stricter in higher-responsibility contexts, such as tandem flying or demanding conditions, but no policy, threshold, enforcement mechanism, or requirement is established here.

During Flight, the product should conceptually prioritize glanceability, low cognitive load, semantic correctness, and immediate visibility of safety-relevant information. It should preserve distinctions such as Heading versus GPS Track and estimated versus directly measured information. This principle does not design the Flight screen or define parameter behavior.

## Automation and Reduction of Routine

A central AirLink principle is:

> Make disciplined and safe behavior the easiest and most natural way to use the service.

AirLink should not reproduce aviation paperwork electronically as additional burden. Information entered or generated through normal use should provide value in multiple appropriate contexts.

Conceptually:

- recorded Flights may contribute to experience history;
- Equipment associated with a Flight may accumulate operating time;
- inspection and maintenance dates may produce relevant reminders;
- Pre-Flight readiness may use known Pilot, Route, Weather, Equipment, and Airspace context;
- one entered fact should be reused where its meaning and provenance remain valid.

Automation should follow a coherent product model and preserve semantic correctness. This direction does not define triggers, formulas, workflows, notifications, or technical mechanisms.

## Feed and Relevance Expansion

Feed is not intended to be a generic engagement feed, an endless stream optimized primarily for attention capture, a replacement for Facebook, or a collection of arbitrary social posts.

Its relevance should expand conceptually from:

1. immediate personal and situational relevance;
2. friends and explicitly selected pilots;
3. pilots, Flights, places, and activity nearby;
4. interesting pilots and Flights farther away;
5. events, services, new places, and new opportunities.

Real flying activity—especially Flight and the entities connected to it—is the common language of this ecosystem. The order expresses increasing contextual distance, not a release sequence, ranking algorithm, mandatory content mix, or UI structure.

## Product-Evolution Principles

Future product work should preserve these principles:

- model real pilot processes before screen structures;
- separate domain concepts even when an early UI combines them;
- treat MVP omissions as explicit simplifications, not proof that a future domain does not exist;
- prefer early decisions that are reversible and naturally extensible;
- avoid overengineering future capabilities that are not yet used;
- avoid prematurely selecting architecture, schemas, providers, algorithms, or detailed workflows;
- do not transform hypotheses, examples, or possible future capabilities into current requirements;
- do not invent missing owner requirements;
- identify product-significant ambiguity rather than silently resolving it;
- require an explicit owner decision when a local solution would materially alter product direction.

Compatibility with future evolution does not require building future domains early. It requires making current simplifications visible, bounded, and honest.

## Intentionally Deferred

This Product Direction does not define:

- MVP 0.1 scope or the scope of any other release;
- a release sequence, roadmap, or backlog;
- UI layout, application navigation, or mandatory screen flow;
- detailed domain models, entity relationships, or schemas;
- APIs, algorithms, application architecture, or technology choices;
- data providers or detailed parameter definitions;
- legal or regulatory interpretation;
- warning thresholds, readiness enforcement, or mandatory checklist behavior;
- a mandatory business model;
- final social, marketplace, school, instructor, service, maintenance, manufacturer, or commercial functionality;
- Product Governance, agent routing, context-loading rules, or iteration alignment checkpoints.

These matters require later bounded discovery and explicit approval when they become relevant. Their deferral is intentional and should not be mistaken for rejection of the long-term domain.

## Assumptions and Material Review Point

- **Assumption:** Long-term product direction can remain stable while individual early releases intentionally cover only a narrow part of it.
- **Assumption:** Keeping domains conceptually distinct is sufficient at this stage; no future architecture or data model needs to be anticipated.
- **Constrained synthesis:** The two pillars reinforce one another through the real entities, activity, continuity, and context created by Flight Support and made discoverable through the Pilot Ecosystem.

The material future review point is wording alignment between the initial Product Vision and long-term Product Direction. That alignment should be addressed deliberately during owner review and canonical promotion without assuming that either current WIP document automatically supersedes the other.

No other product question needs resolution for this direction-level draft. Detailed decisions remain deferred to the bounded work that requires them.

## Expected Promotion Target

After owner and ChatGPT review, accepted durable content is expected to be rewritten or promoted into canonical `docs/product/ProductDirection.md`.

Promotion requires explicit owner acceptance, deliberate handling of the Product Vision alignment point, and an update to the product documentation index. Until then, this WIP remains a review artifact and not product canon or an implementation requirement.
