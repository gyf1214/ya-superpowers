# ya-superpowers

`ya-superpowers` stands for `Yet Another superpowers`.

This repository stores reusable agent skills for building structured workflows similar to Claude superpowers, with wording and tooling tuned for a Codex-style local editing environment.

## Purpose

The repository defines workflow skills that help an agent:

- classify work before acting
- route design work through explicit brainstorming
- enforce planning when scope requires it
- apply TDD for implementation work
- verify claims before completion
- prepare and receive code review in a consistent way

The goal is not just to store prompts. The repository tries to make agent behavior durable, composable, and auditable across repeated sessions.

## Repository Layout

```text
skills/         Skill definitions, one directory per skill
AGENTS.md       Repository-specific operating instructions
README.md       High-level overview of the repository
```

Each skill lives in `skills/<skill-name>/SKILL.md`.

## Workflow Model

The repository uses a fixed work hierarchy:

```text
project -> phase/workstream -> task -> step
```

Change-causing requests are classified as one of:

- `project`
- `multiple phases`
- `single phase/workstream`
- `single task`

That classification determines the expected workflow:

- `single task` without behavior/design change: execute directly
- `single task` with behavior/design change: design, then execute
- `single phase/workstream` or larger: design, then plan, then execute

## Available Local Skills

| Skill | Purpose |
| --- | --- |
| `brainstorming` | Design-first workflow for phase/workstream or behavior-changing work |
| `executing-plans` | Implementation workflow for an approved written plan |
| `finishing-a-project` | Close out project-level work on a dedicated branch |
| `receiving-code-review` | Triage incoming review feedback before changes |
| `requesting-code-review` | Prepare a review request after an implementation boundary |
| `starting-a-project` | Initialize project-level tracking for new project work |
| `systematic-debugging` | Read-only bug triage and root-cause investigation |
| `test-driven-development` | Enforce test-first implementation for features and bug fixes |
| `using-superpowers` | Entry skill for classification and workflow routing |
| `verification-before-completion` | Require fresh evidence before completion claims |
| `writing-plans` | Produce a written implementation plan |
| `writing-skills` | Create, edit, and review skill definitions |

## Install For Codex

Install the skills into Codex's skill path at `$CODEX_HOME/skills`.

The simplest setup is to clone this repository and symlink its `skills/` directory into `$CODEX_HOME/skills/ya-superpowers`:

```bash
git clone git@github.com:gyf1214/ya-superpowers.git
ln -s /path/to/ya-superpowers/skills "$CODEX_HOME/skills/ya-superpowers"
```

After that, Codex can discover the skills from its normal skill path.

## Authoring Principles

The skills in this repository generally aim for:

- concise, actionable instructions
- explicit workflow gates instead of vague guidance
- small, composable skill boundaries
- durable wording that survives across tasks
- evidence-based completion and review workflows

## Notes

- Some workflows referenced by local instructions may depend on companion skills outside this repository, such as memory-related skills provided by a sibling repository in the local workspace.
