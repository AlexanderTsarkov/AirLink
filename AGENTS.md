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

## Core Rules

- Documentation and explicit intent precede implementation.
- Inspect before editing.
- Reuse and extend existing modules before creating new ones.
- Keep changes small, bounded, and reviewable.
- Do not mix unrelated documentation, refactoring, and behavior changes.
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