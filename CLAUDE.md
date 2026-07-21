# Cursor / Codex / Claude Execution Overlay

@AGENTS.md

## Purpose

`AGENTS.md` contains the shared operating policy. This file adds repository-local execution mechanics for agents working in a local checkout.

## Required Context

Before meaningful work:

1. inspect the working tree and current branch;
2. read `AGENTS.md`;
3. read `ITERATION.md`;
4. read the relevant canonical documents and task artifacts referenced by the issue or prompt;
5. confirm that the requested task fits the active iteration.

`ITERATION.md` supplies the context of the whole active work cycle. A task prompt should define the narrow task, not repeat the entire iteration.

## Local Preflight

Run before switching branches or editing:

```bash
pwd
git status --short
git branch --show-current
```

If the working tree is dirty, stop and report. Do not silently stash, clean, discard, or carry unrelated work.

For a new task, start from an updated clean `main`:

```bash
git fetch origin
git switch main
git pull --ff-only origin main
git status --short
```

Then create a focused task branch.

For an explicit follow-up to an existing unmerged PR, continue that PR branch after verifying it is the correct branch and the tree is clean.

## Planning and Approval

For a non-trivial task, report before implementation:

- your understanding of the goal;
- non-goals;
- files and modules you expect to inspect;
- proposed steps;
- validation plan;
- any decision that requires owner approval.

Apply the Product-Significance Routing rule in `AGENTS.md`. Every non-trivial plan should state whether the task is product-significant. For routine work, a short classification such as `Product significance: Not product-significant — accepted behavior and product semantics are unchanged` is sufficient; do not load the two product WIP documents, perform the alignment questionnaire, or require an expanded final alignment report.

For triggered work, the plan must identify:

- the applicable trigger;
- the Product Governance, Product Direction, and relevant canonical, domain, WIP, issue, and decision artifacts loaded;
- the proposed alignment outcome;
- any explicit simplification and its approval source;
- any owner decision required before implementation.

If product significance is discovered after implementation begins, pause only the affected work boundary, load the triggered context, perform the alignment check, update the plan or report with the reclassification, and request owner direction when required. Do not continue the affected product decision silently.

Do not start broad implementation while unresolved product or architecture decisions remain hidden inside the task.

## Editing Rules

- Inspect existing code and documentation before creating new structures.
- Prefer extending an existing module or contract over adding a parallel one.
- Modify only files required by the task.
- Do not edit `ITERATION.md`, `AGENTS.md`, or `CLAUDE.md` unless the task explicitly authorizes the named file.
- Keep `_working/` local and untracked.
- Never commit secrets, private credentials, raw personal data, or unsanitized local artifacts.

## Branch and PR Workflow

Default discipline:

- one issue or bounded task;
- one branch;
- one PR;
- follow-up changes remain in the same open PR;
- every PR is opened as **Draft**;
- the project owner and ChatGPT perform the initial review;
- only after explicit owner agreement may the PR be marked **Ready for review**;
- Ready for review triggers the integrated GitHub Codex review;
- do not merge unless explicitly instructed.

Before commit and before PR creation, inspect the full task diff:

```bash
git diff --name-only origin/main...HEAD
git diff --check
```

Stop if unrelated or unauthorized files appear.

## Validation

Use validation appropriate to the change. Documentation-only work should at minimum verify:

- links and paths;
- internal consistency;
- no accidental promotion of WIP or legacy material to canon;
- no unrelated files in the branch diff.

Implementation work must run the relevant formatter, static analysis, tests, and focused manual checks defined by the task or repository tooling.

## Final Report

Report:

- branch name and base branch;
- issue and Draft PR link;
- changed files;
- validation performed;
- assumptions and open questions;
- confirmation that governance files were changed only when authorized;
- confirmation that secrets and local `_working/` artifacts were excluded;
- recommended next step.

For product-significant work, also report:

- the actual alignment outcome;
- any accepted explicit simplification and its approval source;
- any unresolved product decision or conflict;
- confirmation that no hidden product behavior was introduced.

Routine work does not require an expanded Product Direction report.

## Stability

Keep this file focused on execution mechanics. Product context belongs in `ITERATION.md` and the relevant product documentation according to its authority status, not in this execution overlay.
