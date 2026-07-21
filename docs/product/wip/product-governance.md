# AirLink Product Governance

## Status, Purpose, and Authority

This document is **WIP**, **non-canonical**, **under owner review**, **not an active repository rule**, and **not an implementation requirement**. It proposes a future governance mechanism; its use is not required unless its accepted content is later promoted and activated through separately approved changes. See the [product WIP policy](README.md).

Product Governance defines routing and decision discipline for product-significant work: when the [Product Direction](product-direction.md) should be consulted, how alignment should be evaluated and recorded, and when work should stop for an owner decision. It does not define AirLink, approve product decisions, change the repository's source-of-truth order, or replace existing governance and product artifacts.

## Sources and Artifact Responsibilities

This draft is based on the execution scope for GitHub issue [#11](https://github.com/AlexanderTsarkov/AirLink/issues/11), umbrella issue [#8](https://github.com/AlexanderTsarkov/AirLink/issues/8), completed direction-drafting issue [#9](https://github.com/AlexanderTsarkov/AirLink/issues/9), [`ITERATION.md`](../../../ITERATION.md), canonical [`CurrentState.md`](../CurrentState.md), the [product documentation index](../README.md), the [Product Vision WIP](product-vision-reconstruction.md), and the owner-reviewed [Product Direction WIP](product-direction.md). No legacy AirLink material was consulted.

The mechanism preserves these distinct responsibilities:

- **Product Direction** describes AirLink's long-term purpose, shape, principles, two product pillars, full flight lifecycle, fundamental domains, and evolution constraints. The current Product Direction artifact is WIP and does not automatically override canon, an approved iteration, or an explicit owner decision.
- **Product Vision** describes the initial product focus and value proposition. The current Product Vision is also WIP and under review.
- **`CurrentState.md`** records accepted durable project and product state outside an individual iteration.
- **`ITERATION.md`** defines the purpose, boundaries, and constraints of the active work cycle.
- **Canonical domain specifications** define accepted detailed product behavior when such specifications exist.
- **GitHub issues and execution prompts** define bounded task scope, deliverables, and acceptance criteria.
- **`AGENTS.md` and `CLAUDE.md`** define stable agent policy and repository-local execution mechanics.
- **Product Governance** proposes when alignment evaluation is needed, how it is performed, which conclusion is recorded, and when owner resolution is required.

## Product-Significant Work

Work is product-significant when it may materially change, reinterpret, constrain, or contradict one or more of the following:

- AirLink's intended purpose;
- Flight Support, Pilot Ecosystem, or the relationship between them;
- the full flight lifecycle or a major workflow or lifecycle boundary;
- a fundamental product domain;
- user-visible behavior that has not already been accepted;
- a semantic distinction important to safety or decision support;
- a long-term product-evolution path;
- a product simplification that may be difficult to reverse;
- an architecture boundary whose correctness depends on unresolved product semantics;
- the intended role of Home, Feed, Pre-Flight, Flight, Post-Flight, History, or another major product surface.

Material product impact, not the artifact type, determines significance. A documentation, UI, architecture, or code change is not product-significant merely because it belongs to one of those categories.

## Routing and Context Loading

The proposed routing sequence assumes that a later, separately approved active routing artifact contains a compact trigger rule. That rule can perform initial classification without requiring the full Product Governance or Product Direction documents for every routine task:

1. Classify the task using its goal, affected artifacts, accepted issue scope, and expected product impact.
2. If no product-significant trigger applies, continue under normal repository and task context without loading Product Governance or Product Direction.
3. If a trigger applies or is discovered during execution, load Product Governance, Product Direction, and only the relevant domain and product-decision artifacts.
4. Perform the Product Direction Alignment check.
5. Record the outcome in the appropriate product-significant artifact.
6. Stop for an owner decision when the outcome is `Requires product decision` or `Conflicts with Product Direction`.

Product Direction should be referenced rather than copied into iterations, issues, prompts, or plans. A triggered task should load enough context to evaluate the affected product concern, not every future domain. Routine work should normally remain bounded to `ITERATION.md`, the relevant accepted specifications, the issue or prompt, and the implementation context.

### Mandatory triggers

Under the proposed mechanism, Product Direction and Product Governance must be loaded and applied when:

- creating a new iteration or materially changing the purpose or scope of an active iteration;
- creating or materially revising `CurrentState.md`;
- promoting product WIP into canon;
- creating or materially revising canonical Product Vision or Product Direction;
- defining or materially changing a major product domain;
- changing the full flight lifecycle or a major lifecycle boundary;
- defining new user-visible behavior whose product decision is not already accepted;
- changing the intended role of a major product surface;
- making an architecture decision that depends on unresolved product semantics;
- preparing a product-significant issue, plan, or execution prompt;
- detecting a conflict between local work, current canon, active iteration, issue scope, or Product Direction;
- conducting a major product or milestone review;
- the owner explicitly classifies the work as product-significant.

### Routine non-triggers and discovered significance

The full Product Governance and Product Direction documents normally should not be loaded for:

- a local bug fix that restores already accepted behavior;
- behavior-preserving refactoring;
- a dependency or tooling update with no product impact;
- a visual or copy adjustment within accepted UX and semantics;
- implementation of a fully specified and owner-approved issue;
- tests, diagnostics, CI, build, or infrastructure work with no product-semantic effect;
- performance, reliability, or maintainability work that preserves accepted behavior and boundaries;
- a mechanical documentation correction that does not change meaning.

If supposedly routine work reveals unresolved product semantics, a product conflict, or a hidden product decision, it stops being routine. The work should pause at the affected boundary and resume from step 3 of the routing sequence.

## Product Direction Alignment Check

For product-significant work, answer these questions concisely; record `Not applicable` when a question genuinely does not apply:

1. Does the proposal support the intended purpose of AirLink?
2. Does it advance Flight Support, Pilot Ecosystem, or the relationship between them?
3. Does it model a real pilot process rather than only a convenient screen structure?
4. Is every MVP or iteration simplification explicit, bounded, and reversible?
5. Does the local model deny, collapse, or permanently constrain a known future domain?
6. Does the proposal reduce routine through useful reuse and automation, or add avoidable burden?
7. Is any product behavior, requirement, or authority being invented without owner acceptance?
8. Does the change require an explicit revision of Product Direction rather than a silent local exception?

The check is a compact planning and review tool, not a required long-form questionnaire for routine work.

## Alignment Outcomes and Actions

| Outcome | Meaning and permission to proceed | Required record and authority |
| --- | --- | --- |
| `Not product-significant` | No trigger applies. Work may proceed under normal repository and task context. | No standalone alignment record or owner decision is normally required. If classification occurred after a check began, note the reason briefly in the working artifact. |
| `Aligned` | The proposal supports Product Direction and remains within accepted product and task scope. Work may proceed. | Record a compact conclusion in the relevant product-significant iteration, issue, plan, prompt, or review. No new owner decision is required beyond the authority already governing that work. |
| `Aligned with explicit simplification` | The proposal uses a simplification already made explicit and accepted in an owner-approved iteration, issue, product decision, or other appropriate artifact. Work may proceed only within that approval. | Record what is simplified or omitted, why it is bounded, why it is reversible, where approval is recorded, and which long-term concept remains intentionally deferred. If the simplification is new, product-semantic, or lacks identifiable approval, use `Requires product decision` instead. An agent cannot approve its own material exception. |
| `Requires product decision` | Product Direction does not define needed behavior, or the proposal introduces a new product-semantic choice or unapproved simplification. Affected work must stop. | Identify the missing decision and bounded alternatives and consequences when useful. An explicit owner decision is required. Record the resolution in the correct durable artifact and revise iteration or issue scope through separately approved changes when necessary. |
| `Conflicts with Product Direction` | The proposal or another governing source materially disagrees with Product Direction. Affected work must stop; no guessed compromise may be implemented. | Record the exact conflicting statements and classify their authority. An explicit owner decision is required. Product Direction, canon, iteration scope, issue scope, or another durable artifact may then require a separately approved revision. |

## Conflicts, Missing Decisions, and Document Ownership

Under the proposed mechanism, when canon, iteration scope, an issue or prompt, WIP, or a local proposal disagree, the agent or reviewer must:

1. identify the exact conflicting statements;
2. classify each source as canon, WIP, iteration context, issue scope, or local proposal;
3. avoid silently selecting a preferred interpretation;
4. avoid implementing a guessed compromise;
5. describe bounded alternatives and consequences when useful;
6. request an explicit owner decision;
7. record the accepted resolution in the artifact responsible for that decision when appropriate.

Product Direction does not automatically override current canon or an explicit owner-approved iteration, issue, or decision. Two WIP documents do not establish precedence over one another. A local proposal remains a proposal, and overlapping document responsibilities should be resolved using the responsibility boundaries above rather than by duplicating content.

Product Direction may identify a domain or future capability without defining its detailed behavior. A future capability is not automatically a current requirement, and missing behavior remains undefined. An agent must not infer detailed requirements from long-term direction alone. When detail becomes necessary, the appropriate response is a bounded discovery, WIP, or product-decision task. Any temporary assumption affecting product semantics must be explicit, bounded, reversible, and owner-approved.

## Proposed Lightweight Future Use

If this mechanism is later activated, product-significant artifacts may use compact alignment records:

- **Iteration checkpoint:** direction advanced, intentional simplifications, long-term concepts deliberately excluded, direction risks, unresolved owner decisions, and outcome.
- **Issue, prompt, or planning note:** trigger, relevant direction and decisions, any accepted simplification, unresolved decision, and outcome.
- **Draft PR review:** confirmation that the change matches the accepted product decision, preserves declared simplifications, introduces no hidden product behavior, and exposes any remaining conflict.
- **Routine task:** omit these sections unless a trigger is discovered.

These are proposed concepts only. This WIP does not modify iteration, issue, prompt, or PR templates and does not activate a new review requirement.

## Scope, Review, and Future Integration

This WIP does not activate repository rules, change source-of-truth order, modify protected governance files, promote Product Direction or Product Vision into canon, authorize Product Governance or an agent to make product decisions, or define product features, UI, detailed behavior, schemas, APIs, algorithms, architecture, technology, roadmap, or release scope. It does not require Product Direction review for routine technical work.

The routing sequence, trigger grouping, alignment check, and outcome actions are constrained governance synthesis for owner review. No new product decision is proposed. The existing future wording-alignment point between Product Vision and Product Direction remains unresolved and outside this task; this document neither interprets nor resolves it.

After owner and ChatGPT review, accepted content may be rewritten or promoted into canonical `docs/product/ProductGovernance.md`. Activation, agent routing, iteration checkpoints, and template integration require later bounded tasks and explicit owner approval. Until then, this document remains a non-active WIP review artifact.
