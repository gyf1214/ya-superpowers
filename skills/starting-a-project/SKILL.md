---
name: starting-a-project
description: Use when new project-level work starts on a dedicated branch and you need to initialize a project index in scratch.
---

# Starting a Project

Initialize project-level tracking before phase execution begins.

Announce at start: "I'm using the starting-a-project skill to initialize this project."

## Step 1: Confirm Scope

Use this skill only for `project`-level work (dedicated branch). If scope is smaller, use normal task/phase workflows without a project index.

## Step 2: Create Project Branch

Pick a short branch name in this form:

`<label>/<what-it-does>`

Examples: `feat/project-index`, `chore/workflow-tuning`

Then:

1. Confirm current branch is `main` or `master`.
2. Create and switch to the project branch.

```bash
git branch --show-current
git checkout -b <label>/<what-it-does>
```

If not on `main` or `master`, stop and ask the user how to proceed before creating the project branch.

## Step 3: Create Project Index

Create one non-committed index file:

`scratch/project-index/<project-slug>.md`

Do not create per-phase index docs. Keep phase implementation details in existing docs (`scratch/designs`, `scratch/plans`, `scratch/review_requests`, etc.).

Use this template:

```markdown
# Project Index: <Project Name>

## Project Branch
- Branch: <label>/<what-it-does>

## Purpose
- Why this repository work exists:
- In-scope outcomes:
- Out-of-scope boundaries:

## Success Criteria
- Criterion 1
- Criterion 2
- Criterion 3

## Phases
- [ ] Phase 1: <name>
  - status: `not started | designed | planned | in progress | blocked | done`
  - design doc: <path>
  - plan doc: <path>
  - review doc: <path>

- [ ] Phase 2: <name>
  - status: `not started | designed | planned | in progress | blocked | done`
  - design doc: <path>
  - plan doc: <path>
  - review doc: <path>

## Handoff Note
- This section will be created during closing.
```

## Step 4: Verify Hygiene

- Ensure index is under `scratch/project-index/`.
- Ensure it is not committed.
- Ensure phase status vocabulary matches workflow: `design -> plan -> execute`.
- Ensure branch recorded in index matches the active project branch.

## Red Flags

Stop and correct if you see:
- index outside `scratch/project-index/`
- per-phase index documents
- phase statuses that do not match workflow terms
