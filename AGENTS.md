# AGENTS.md

## Project
- Name: `ya-superpowers`
- Meaning: `Yet Another superpowers`
- Purpose: A repository of agent skills for building workflows similar to Claude superpowers.

## Working Notes
- Follow skills from this repository and keep instructions concise and actionable.
- Use repository terminology for the workspace (`repository`/`repo`, not `project`).
- Work hierarchy terminology:
  - classify change-causing requests as one of: `project`, `multiple phases` (if already in a project), `single phase/workstream`, `single task`
  - for read-only requests, select skills directly from request type (no forced work-hierarchy classification)
  - hierarchy terms: `project -> phase/workstream -> task -> step`
  - `project`: dedicated git branch; closure typically merges into `main`
  - `phase`: project-contained work requiring design -> planning -> execution
  - `workstream`: phase-equivalent work done outside a project branch
  - `task`: scoped delivery unit; may be independent or within phase/workstream
