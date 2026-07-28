# AL-0002: MVP 0.1 Engineering Planning

## Purpose

Produce and approve the minimum engineering plan required to decompose MVP 0.1 into bounded implementation iterations, including the system boundary, responsibility boundaries, live/simulation substitution expectations, dependency and risk order, deferred decisions, and implementation-readiness criteria.

The iteration must:

- use the existing MVP 0.1 Scope as an owner-reviewed WIP planning baseline;
- create an engineering map of the entire MVP 0.1;
- prepare the first end-to-end vertical slice deeply enough for implementation in AL-0003;
- avoid beginning product implementation or selecting unnecessary technologies prematurely;
- avoid prematurely designing the complete system.

## Starting Point

AL-0001 is substantively complete and becomes formally complete when the transition that installs this charter is merged.

The accepted artifact roles at the start of AL-0002 are:

- the Product Vision is canonical;
- Product Direction is accepted, owner-controlled living product policy;
- Product Governance is accepted conditional product policy invoked through repository governance routing;
- [`docs/product/wip/mvp-0.1-scope.md`](docs/product/wip/mvp-0.1-scope.md) is an owner-reviewed, non-canonical WIP planning baseline;
- the MVP 0.1 Scope is not a detailed specification and does not itself authorize implementation;
- Flight Mode, Flight, Navigation, and other controlled WIP documents remain non-canonical supporting inputs;
- legacy materials remain sources rather than automatic product truth.

Promotion of the MVP 0.1 Scope to canon is not required by this iteration. AL-0002 may perform a bounded planning-sufficiency review and make only corrections necessary to remove an actual blocker.

## Iteration Goal

Produce two levels of engineering planning:

1. a broad engineering map of the whole MVP 0.1 at the minimum depth needed to understand boundaries, responsibilities, handoffs, dependencies, risk and decision order, live versus simulated input boundaries, deferred decisions, and a high-level sequence of future implementation iterations;
2. an implementation-ready plan for only the first bounded end-to-end vertical slice, to be implemented in AL-0003.

AL-0002 is an engineering-planning and documentation iteration. It is not an implementation iteration and does not redefine or fully formalize the product scope of MVP 0.1.

## Primary Outcomes

### 1. MVP 0.1 Engineering Map

Create and obtain owner approval for an engineering-planning artifact that describes, at the minimum necessary abstraction level:

- the engineering boundary of MVP 0.1;
- major engineering concerns;
- responsibility boundaries for MVP 0.1 concerns;
- ownership of important state and information;
- conceptual handoffs;
- external dependencies;
- live versus simulated input boundaries;
- key dependencies and risk order;
- deferred decisions;
- a high-level candidate sequence of implementation iterations.

The Engineering Map is not the final component architecture. It must not require final modules, classes, services, exact APIs, complete schemas, detailed storage design, provider selections, full algorithms, complete UI design, or a complete MVP backlog.

### 2. First Vertical Slice Plan

Compare reasonable candidate slices and obtain explicit owner selection of one first bounded end-to-end implementation slice.

The selected slice must connect a meaningful subset of:

- pilot-facing flow;
- runtime state;
- simulated input;
- observable behavior;
- minimal persistence or a retained result;
- validation without a real Flight.

The plan must define:

- purpose;
- included behavior;
- explicit non-goals;
- inputs and outputs;
- runtime responsibilities and handoffs;
- simulated input requirements;
- minimal persistence requirements;
- validation strategy;
- required technical decisions;
- deferred technical decisions;
- implementation-readiness criteria;
- Definition of Done.

The selected slice must not be only project scaffolding, storage infrastructure, a navigation shell, dependency-injection setup, a standalone simulator, or another technical foundation without observable end-to-end behavior.

### 3. AL-0003 Charter

Prepare and obtain owner approval for the charter of AL-0003, the first bounded implementation iteration. AL-0003 is expected to implement the selected first end-to-end vertical slice.

AL-0003 must not be activated during AL-0002. The approved first implementation issue must be prepared as reviewed draft content during AL-0002 and created only after the AL-0003 transition is merged.

## Governing and Supporting Artifacts

Work in this iteration follows the source-of-truth order and product-significance routing in [`AGENTS.md`](AGENTS.md).

Primary inputs are:

- this active iteration charter;
- [`docs/product/CurrentState.md`](docs/product/CurrentState.md);
- the canonical [`docs/product/vision/ProductVision.md`](docs/product/vision/ProductVision.md);
- the accepted product-policy roles and alignment context in [`docs/product/policy/`](docs/product/policy/);
- the owner-reviewed [`docs/product/wip/mvp-0.1-scope.md`](docs/product/wip/mvp-0.1-scope.md);
- relevant controlled WIP only when the bounded planning task identifies why it is needed;
- the relevant approved GitHub issue and task artifacts.

Planning outputs do not gain product or implementation authority merely by being created.

## In Scope

- a bounded sufficiency review of the MVP 0.1 Scope;
- identification and resolution of only genuinely blocking product contradictions or omissions;
- the MVP 0.1 engineering boundary;
- major engineering concerns and responsibilities;
- conceptual handoffs and ownership boundaries;
- external dependencies;
- the live/simulation substitution boundary;
- the minimum Flight Simulation Framework responsibility;
- major risks and dependency order;
- deferred decisions;
- a high-level implementation sequence;
- comparison and owner selection of the first vertical slice;
- detailed planning of that selected slice;
- preparation and owner approval of the AL-0003 charter;
- preparation and owner approval of the first implementation issue as reviewed draft content for creation after AL-0003 becomes active.

## Out of Scope

- product implementation or production code;
- executable product prototypes;
- technical spikes unless separately owner-authorized as bounded evidence gathering that produces no product implementation;
- complete architecture of AirLink or MVP 0.1;
- detailed design of all future slices;
- a complete MVP backlog;
- mandatory promotion of the MVP 0.1 Scope to canon;
- broad revision of Product Vision or Product Direction;
- technology selection for the complete product;
- detailed APIs, schemas, storage models, providers, or algorithms outside the first slice;
- full UX or UI design;
- Pilot Ecosystem implementation;
- mobile, web, server, firmware, or infrastructure implementation.

## Planning Depth Rule

For the full MVP 0.1, detail only what affects engineering boundaries, responsibility ownership, dependencies, implementation order, risk, or difficult-to-reverse decisions. Defer lower-level details to the relevant implementation iteration.

For the first vertical slice, plan deeply enough that implementation can begin without the implementation issue inventing unresolved product semantics or material architecture.

## Technical Decision Rule

AL-0002 may make only the technical decisions required to define and safely begin the first vertical slice.

Material or difficult-to-reverse technical choices must be handled through a separate bounded issue or an explicitly authorized decision section of the active issue. Minor local choices do not require standalone decision issues.

## Artifact Authority

- Reviewed AL-0002 engineering-planning artifacts may be accepted by the owner for use as planning inputs.
- Acceptance does not automatically make them canonical product definition.
- Acceptance does not make them implementation authority outside an active implementation iteration and approved bounded issue.
- WIP, hypotheses, planning outputs, and accepted decisions must remain distinguishable.

## Working Method

- Research before recommendation.
- Inspect existing repository material before proposing new structure.
- Preserve modularity and reuse.
- Do not create speculative documentation hierarchies.
- Create only artifacts that the current iteration demonstrably needs.
- Require owner approval for material product, architecture, scope, and sequencing decisions.
- Decompose work into small, reviewable issues and Draft PRs.
- Do not create parallel or alternative implementations.
- Do not silently broaden MVP 0.1.
- Do not reopen accepted product decisions unless a contradiction is discovered.

## Product Direction Alignment

- **Direction advanced:** AL-0002 advances the first meaningful Flight Support outcome by exposing the minimum engineering structure needed to implement the accepted MVP 0.1 product boundary safely and incrementally.
- **Intentional simplifications:** The whole MVP receives only boundary-, dependency-, risk-, and sequence-level planning, while implementation-ready depth is limited to the first selected vertical slice. No product implementation begins, no complete architecture is designed, and no complete-product technology selection is made.
- **Why the simplifications are bounded and reversible:** The owner-approved scope of this iteration explicitly defers lower-level decisions to the implementation iteration that needs them. The Engineering Map must preserve responsibility and future-domain boundaries without prematurely fixing their realization.
- **Long-term concepts intentionally deferred:** Detailed future Flight Support domains, the wider Pilot Ecosystem, complete architecture, later slices, providers, algorithms, schemas, and detailed UI remain outside AL-0002 unless a bounded first-slice decision demonstrably requires them.
- **Direction risks:** Planning could be mistaken for implementation authority, the MVP 0.1 WIP baseline could be treated as canon, the Engineering Map could be mistaken for final architecture, or first-slice decisions could silently constrain future domains.
- **Unresolved owner decisions:** Candidate-slice comparison and selection, the necessary first-slice technical decisions, the AL-0003 charter, and the first implementation issue remain owner-controlled outcomes of this iteration.
- **Outcome:** `Aligned with explicit simplification`. The iteration supports Product Vision and Product Direction, models a real end-to-end pilot outcome, keeps omissions explicit and reversible, invents no product behavior or authority, and requires no revision of Product Vision or Product Direction.

## Completion Criteria

AL-0002 is complete only when:

1. the MVP 0.1 Scope has been reviewed for planning sufficiency;
2. any product contradiction that genuinely blocks engineering planning or the first slice has been explicitly resolved;
3. the MVP 0.1 Scope is confirmed sufficient as the WIP planning baseline or receives only necessary bounded corrections;
4. the MVP 0.1 Engineering Map is reviewed and owner-approved;
5. the Engineering Map identifies the MVP 0.1 engineering boundary;
6. major engineering responsibilities and conceptual handoffs are identified at the minimum useful abstraction level;
7. key dependencies, risks, and decision order are explicit;
8. the live/simulation substitution boundary is approved;
9. the minimum responsibility of the Flight Simulation Framework is approved;
10. deferred decisions are explicitly recorded;
11. a high-level candidate sequence of future implementation iterations exists without becoming a full backlog;
12. reasonable candidates for the first end-to-end vertical slice have been compared;
13. the owner has explicitly selected one first vertical slice;
14. the selected slice has an approved implementation-ready plan covering scope, non-goals, inputs, outputs, responsibilities, persistence, simulation, validation, necessary technical decisions, deferred decisions, and Definition of Done;
15. the charter for AL-0003 is prepared and owner-approved;
16. the first bounded implementation issue is prepared as reviewed draft content for creation after AL-0003 becomes active;
17. no product implementation has begun under AL-0002.

## Transition Rule

AL-0002 ends only through a separate owner-approved transition that:

- verifies every completion criterion;
- replaces `ITERATION.md` with the approved AL-0003 charter;
- creates or activates the first bounded implementation issue at the correct time;
- does not automatically mark a PR Ready for review or merge it.

## Authority and Stop Conditions

This file is maintained by the project owner and the owner-led planning process. Agents may edit it only when explicitly instructed.

Stop and request an owner decision when work:

- requires a product decision not already accepted;
- conflicts with Product Vision, Product Direction, Current State, this charter, or approved issue scope;
- would broaden MVP 0.1 or AL-0002;
- would begin implementation;
- would make a material or difficult-to-reverse technical choice without the required bounded authority;
- would treat WIP or a planning output as canonical or implementation authority;
- would activate AL-0003 before the separate approved transition.
