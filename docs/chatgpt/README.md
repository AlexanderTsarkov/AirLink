# ChatGPT Project Context

## Purpose

This directory contains only the minimum stable context needed for ChatGPT to support AirLink consistently.

Repository rules, product state, iteration scope, and specifications should remain in their canonical GitHub locations rather than being duplicated here.

## Role of ChatGPT

ChatGPT supports the project owner by:

- helping reconstruct and clarify the product;
- reviewing selected legacy sources;
- separating accepted decisions from hypotheses and WIP;
- preparing bounded specifications and prompts for Cursor / Codex;
- reviewing Draft PRs with the owner before integrated Codex review;
- checking consistency between iteration context, current state, documentation, issues, and implementation.

ChatGPT must not silently promote legacy material or WIP into canon, invent architecture before it is needed, or expand a task beyond the active iteration.

## Context Order

For repository-specific work, consult:

1. `ITERATION.md`;
2. `docs/product/CurrentState.md`;
3. the relevant documentation index and canonical documents;
4. the relevant GitHub issue or PR;
5. WIP documents only when the task concerns them;
6. legacy sources only when required.

## Working Model

- Project owner + ChatGPT: product framing, iteration control, decisions, task shaping, and initial review.
- Cursor / Codex: bounded repository execution under explicit scope.
- Integrated GitHub Codex review: triggered after owner-approved transition from Draft to Ready for review.

## Maintenance Rule

Keep this file small and stable. Changeable workflow details belong in `AGENTS.md`, `CLAUDE.md`, `ITERATION.md`, or the relevant canonical documentation so they can be updated through normal GitHub review.