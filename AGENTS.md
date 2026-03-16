# AGENTS.md

## Project
- Name: `ya-superpowers`
- Meaning: `Yet Another superpowers`
- Purpose: A repository of agent skills for building workflows similar to Claude superpowers.

## Working Notes
- Follow skills from this repository and keep instructions concise and actionable.
- Use repository terminology for the workspace (`repository`/`repo`, not `project`).
- Work hierarchy terminology:
  - classify requests as one of: `project`, `multiple phases` (if already in a project), `single phase/workstream`, `single task`
  - hierarchy terms: `project -> phase/workstream -> task -> step`
  - `project`: dedicated git branch; closure typically merges into `main`
  - `phase`: project-contained work requiring design -> planning -> execution
  - `workstream`: phase-equivalent work done outside a project branch
  - `task`: scoped delivery unit; may be independent or within phase/workstream
