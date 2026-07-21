# AI Agent Operating Policy

## Purpose

This file defines stable rules for AI-assisted work in the AirLink repository. It applies to Cursor, Codex, Claude, and other coding or documentation agents.

It is not a product specification, roadmap, iteration plan, or task description.

## Source-of-Truth Order

Before meaningful work, use this order:

1. the explicit user task;
2. `ITERATION.md` for active iteration context and boundaries;
3. canonical documentation under `docs/`;
4. the relevant GitHub issue and approved task artifacts;
5. WIP documents only when the task explicitly concerns them;
6. legacy material only as source material, never as current truth by default.

WIP is not loaded by default. In addition to work that explicitly concerns WIP, the Product-Significance Routing rule below requires consultation of `docs/product/wip/product-governance.md` and `docs/product/wip/product-direction.md` when a trigger applies. Any other WIP may be consulted only when it is directly relevant to the affected product decision and the task, issue, or triggered alignment process identifies why it is needed. Consultation does not make WIP canonical or an implementation requirement.

If sources conflict, stop and report the conflict. Do not silently choose a preferred interpretation.

## Start-of-Task Discipline

Before editing:

1. read `ITERATION.md`;
2. identify the task goal and non-goals;
3. inspect relevant existing files and modules;
4. identify the iteration artifacts needed for context;
5. check whether the task fits the active iteration;
6. propose a short execution plan for any non-trivial change;
7. confirm that no secrets, private data, or transient artifacts will be committed.

`ITERATION.md` provides shared iteration context so it does not need to be repeated in every prompt. The agent must still read the concrete issue, specification, decision, or WIP artifact referenced by the task.

If the task does not fit the active iteration, stop and report the mismatch. Do not expand or redefine the iteration without explicit approval.

## Product-Significance Routing

Before loading long-term product context, classify a task using its goal, affected artifacts, approved issue scope, and expected product impact. Work is product-significant when it may materially change, reinterpret, constrain, or contradict AirLink's intended purpose; Flight Support, Pilot Ecosystem, or their relationship; a major lifecycle, workflow boundary, or product domain; unaccepted user-visible behavior; safety-relevant or decision-support semantics; a long-term evolution path; a difficult-to-reverse product simplification; an architecture boundary dependent on unresolved product semantics; the intended role of a major product surface; product canon, Product Vision, Product Direction, `CurrentState.md`, or active iteration intent.

The alignment path is mandatory when work creates or materially changes an iteration, `CurrentState.md`, canonical Product Vision or Product Direction, a major product domain, lifecycle or workflow boundary, or the role of a major product surface; promotes product WIP into canon; introduces user-visible behavior not already accepted; makes an architecture decision dependent on unresolved product semantics; prepares a product-significant issue, plan, or execution prompt; detects a material conflict among local work, canon, iteration scope, issue scope, or Product Direction; or is explicitly classified as product-significant by the owner.

When a trigger applies or is discovered:

1. consult `docs/product/wip/product-governance.md` as the detailed alignment procedure and `docs/product/wip/product-direction.md` as long-term direction context;
2. load only the canonical, domain, product-decision, issue, and directly relevant WIP artifacts needed for the affected concern;
3. perform the Product Direction Alignment check defined in Product Governance and record a compact outcome in the relevant product-significant issue, iteration artifact, plan, prompt, or review;
4. stop and request an explicit owner decision for `Requires product decision` or `Conflicts with Product Direction`.

The repository-wide obligation comes from this active rule. Product Governance and Product Direction remain WIP, non-canonical, not independently active repository policy, and not implementation requirements. Mandatory consultation does not let them override canon, an approved iteration, approved issue scope, or an explicit owner decision. Identify exact conflicting statements and their authority; do not invent missing product behavior or implement a guessed compromise.

The full Product Governance and Product Direction documents normally are not loaded for routine work that preserves accepted product semantics and boundaries. Typical examples include fixes restoring accepted behavior, behavior-preserving refactoring, product-neutral dependency or tooling changes, implementation of a fully specified owner-approved issue, tests and diagnostics, CI or infrastructure changes without product-semantic impact, adjustments within accepted UX and copy semantics, behavior-preserving performance or reliability work, and mechanical documentation corrections.

If routine work reveals unresolved product semantics, a hidden product decision, a material conflict, or a difficult-to-reverse simplification, pause only the affected work boundary and enter the triggered alignment path before continuing it.

## Core Rules

- Documentation and explicit intent precede implementation.
- Inspect before editing.
- Reuse and extend existing modules before creating new ones.
- Keep changes small, bounded, and reviewable.
- Do not mix unrelated documentation, refactoring, and behavior changes.
- Each AI task should operate at one primary abstraction level. Do not mix product discovery, domain modeling, architecture, implementation, refactoring, and review unless the task explicitly requires a controlled transition between them.
- Do not design a screen before the domain model represented by that screen is sufficiently defined.
- Do not introduce frameworks, dependencies, services, schemas, or architectural patterns without a demonstrated need and explicit scope.
- Do not convert an idea into a requirement or implementation without approval.
- Keep facts, accepted decisions, assumptions, hypotheses, and open questions clearly separated.
- Update durable documentation when accepted behavior or project state changes.
- Do not duplicate canonical information across multiple files without a specific reason.

## Canon and WIP

Canonical documents describe accepted project truth.

Documents under `docs/product/wip/` are exploratory and are not implementation requirements. Agents must not implement WIP material unless the task explicitly authorizes that work and identifies the approved scope.

Legacy Google Drive and local-project materials are historical sources. They may inform new documents, but they do not become canonical through copying.

## Scope Control

Implement only the requested task.

Stop and report before proceeding when work requires:

- adjacent product decisions;
- changes to active iteration scope;
- a new architectural boundary;
- a competing module or implementation;
- a broad refactor;
- destructive data or filesystem changes;
- handling of secrets or sensitive data;
- changes to protected governance files not explicitly requested.

Protected governance files are:

- `ITERATION.md`;
- `AGENTS.md`;
- `CLAUDE.md`.

## Local Workspace

`_working/` is an ignored local workspace for disposable drafts, exports, comparisons, temporary prompts, and iteration artifacts.

Its contents are not canonical and must not be committed unless a task explicitly promotes a sanitized artifact into an approved repository location.

A file that must survive deletion of `_working/` belongs elsewhere.

## GitHub Review Policy

- One bounded issue or task should normally use one branch and one PR.
- Every PR must be created as **Draft**.
- Initial review is performed by the project owner together with ChatGPT.
- A PR may be marked **Ready for review** only after explicit owner agreement.
- Marking it Ready is the trigger for the integrated GitHub Codex review.
- Do not merge, force-push, rewrite shared history, close issues, or delete branches without explicit instruction.

## Final Report

At the end of a task, report:

- what changed;
- files changed;
- validation performed;
- assumptions or unresolved questions;
- scope intentionally not changed;
- branch and Draft PR, when applicable;
- recommended next step.

## Stability

Keep this file compact and stable. Current iteration details, implementation plans, and temporary project state belong elsewhere.
