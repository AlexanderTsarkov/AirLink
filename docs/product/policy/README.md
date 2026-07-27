# Product Policy

This directory contains AirLink's owner-controlled product-policy artifacts.

Product policy provides long-term direction and decision discipline for product-significant work. It is distinct from canonical product truth, product specifications, release scope, architecture, and implementation documentation. Placement in this directory does not create product requirements or implementation authority.

## Policy Artifacts

- [`ProductDirection.md`](ProductDirection.md) is an accepted product-policy artifact describing AirLink's broader product domains, relationships, evolution principles, constraints, and alignment context. It cannot create release scope, approved detailed behavior, architecture, technology, a roadmap, or a committed feature catalogue.
- [`ProductGovernance.md`](ProductGovernance.md) is an accepted conditional product-policy artifact defining product-significance classification, context loading, alignment, outcomes, and stop conditions. It cannot define the product or approve product decisions.

## Conditional Loading

The repository-wide routing gate in [`AGENTS.md`](../../../AGENTS.md) determines when Product Governance must be consulted.

When that gate triggers, or product significance is discovered during work:

1. load Product Governance;
2. load Product Direction only as relevant alignment context;
3. load only the other canonical, domain, issue, and decision artifacts needed for the affected concern.

Routine work loads neither policy artifact by default. This index describes their roles and does not itself activate Product Governance.

## Authority Boundaries

- [`ProductVision.md`](../vision/ProductVision.md) is canonical product truth for AirLink's enduring purpose, mission, product pillars, initial focus, principles, and Vision boundaries.
- [`CurrentState.md`](../CurrentState.md) records accepted durable project and product state.
- [`ITERATION.md`](../../../ITERATION.md) defines the active iteration and its boundaries.
- Bounded issues, prompts, and approved task artifacts define task scope and acceptance criteria.
- Canonical specifications define approved detailed product behavior when they exist.
- Product Direction does not create release scope or detailed behavior.
- Product Governance does not approve product decisions.
- Neither policy artifact changes the repository's source-of-truth precedence or overrides Product Vision, CurrentState, the active iteration, an explicit owner decision, or bounded issue scope.

## Change Control

Product Direction and Product Governance are owner-controlled. Changes require a bounded task, explicit owner approval, and normal repository review.

These accepted product-policy artifacts are living policy, not ordinary WIP awaiting promotion into a final static specification. Their content may evolve through approved repository changes while remaining within their defined authority boundaries.
