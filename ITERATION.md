# AL-0001: Project Foundation and Product Recovery

## Purpose

This file defines the active AirLink work iteration. It gives executing agents enough shared context to understand why a narrow task exists, how it relates to the current work cycle, which artifacts may contain supporting information, and where the iteration must stop.

It is not a permanent product specification, a detailed task backlog, or a progress dashboard. Concrete work remains governed by the relevant GitHub issue, task prompt, and referenced artifacts.

## Project Context

AirLink is being restarted as a new project rather than continued from the previous implementation.

Existing Google Drive documents, older ParaFlight materials, and a local historical project folder contain potentially valuable product knowledge. They are source material only. Their requirements, architecture, technology choices, and implementation details are not automatically valid for the restarted project.

The new GitHub repository is the canonical workspace for accepted documentation and future implementation.

## Iteration Goal

Create a controlled project foundation and reconstruct the minimum accepted product definition needed to choose the first meaningful product slice.

The iteration should establish:

- clear repository and AI-agent governance;
- a minimal documentation structure;
- explicit separation between canon, WIP, and legacy sources;
- an initial accepted current state;
- a carefully reconstructed Product Vision based on selected evidence rather than bulk migration;
- enough product context to decide the next iteration without choosing implementation architecture prematurely.

## Iteration Work Model

Tasks in this iteration should be narrow and linked to a specific issue or approved artifact.

An executing agent must:

1. read this file for iteration-wide context;
2. read the concrete issue or prompt for task scope;
3. inspect the referenced iteration artifacts when additional context is needed;
4. consult canonical documents before WIP;
5. consult legacy sources only when the task explicitly requires them;
6. stop when work requires a decision outside the active task or iteration.

The whole iteration context should not be recopied into every task prompt. Prompts should identify the narrow goal, non-goals, relevant artifacts, allowed files, and validation.

## Current Anchor Artifacts

During the foundation step:

- `AGENTS.md` — stable shared AI-agent policy;
- `CLAUDE.md` — local Cursor / Codex / Claude execution mechanics;
- `docs/README.md` — documentation map;
- `docs/product/README.md` — product canon and WIP index;
- `docs/product/CurrentState.md` — accepted current project state;
- `docs/product/wip/` — active product drafts;
- `docs/chatgpt/README.md` — minimal ChatGPT operating context;
- GitHub issues — bounded task definitions and acceptance criteria.

Additional artifacts may be added only when a concrete need appears.

## In Scope

- repository governance and documentation discipline;
- minimal documentation indexes;
- definition of canon, WIP, legacy-source, and local-working roles;
- initial Current State;
- selective review of the minimum legacy sources needed for Product Vision;
- creation and review of a Product Vision WIP draft;
- promotion of an approved Product Vision into canon;
- identification of unresolved product questions;
- definition of the next iteration candidate.

## Out of Scope

- mobile, web, or server implementation;
- framework, language, database, cloud, map-provider, or service selection;
- final system architecture;
- bulk import of Google Drive or local legacy documentation;
- migration or continuation of old source code;
- full feature backlog;
- detailed subsystem specifications;
- social network, marketplace, school-management, or commercial implementation design;
- treating previous AirLink documents as approved requirements;
- CI/CD and production infrastructure, unless separately approved as repository-process work.

## Working Principles

- Start from current user needs and product intent, not old implementation structure.
- Use the minimum legacy material required for the current question.
- Preserve useful domain knowledge without preserving obsolete decisions.
- Mark facts, assumptions, hypotheses, decisions, and open questions distinctly.
- Keep WIP non-canonical until explicit owner approval.
- Prefer one bounded issue, one branch, and one Draft PR.
- Owner + ChatGPT perform initial review before Ready for review and integrated Codex review.

## Iteration Completion Criteria

This iteration is complete when:

- governance and documentation roles are accepted;
- the repository has a minimal usable documentation map;
- `CurrentState.md` reflects the accepted restarted-project state;
- an approved canonical Product Vision exists;
- legacy sources remain classified as sources rather than silently becoming canon;
- the principal open product questions are recorded;
- the next iteration has a clearly stated goal and non-goals.

## Authority

This file is maintained by the project owner and the owner-led planning process. Agents may read it and report mismatches, but may edit it only when explicitly instructed.