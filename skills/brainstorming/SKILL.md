---
name: brainstorming
description: Use when starting a phase/workstream or any behavior/design change that requires explicit design before planning or implementation.
---

# Brainstorming

Design is mandatory before implementation for in-scope work.

## Scope Gate

This skill is required for:

- `phase` work in a `project`
- `workstream` work outside a `project`
- Any behavior/design change that needs explicit design decisions

Usually not required for standalone non-behavioral mechanical work (docs/chore/format-only), unless the user asks for design.

<HARD-GATE>
Do not invoke implementation skills, write code, scaffold work, or perform implementation actions until design is approved.
</HARD-GATE>

## Required Workflow

Complete in order:

1. Explore repository context (files, docs, recent commits, active constraints).
2. Clarify requirements one question at a time.
3. Propose 2-3 approaches with trade-offs and a recommendation.
4. Present design sections and iterate with user feedback until approved.
5. Update canonical design doc at `scratch/designs/<component-or-feature>.md`.
6. Run design self-review for coverage, ambiguity, risk, and testability.
7. Revise and re-review until acceptable.
8. Ask user to review the written design doc and approve/revise.
9. Run the canonical `Skill Closeout Workflow` from `using-superpowers`.
10. Route to the next workflow step.

Do not skip steps. Do not route to implementation before step 9 completes.

## Questioning Discipline

During clarification:

- Ask one question per message.
- Prefer multiple-choice where practical.
- Use open-ended questions only when needed to reduce ambiguity.
- Focus on: purpose, constraints, success criteria, and non-goals.
- If a topic needs depth, split it into follow-up single questions.

Avoid turning brainstorming into implementation planning. Keep it design-focused.

## Approach Presentation Discipline

When presenting alternatives:

- Always show 2-3 approaches.
- For each approach, include trade-offs (complexity, risk, testability, migration cost).
- Lead with recommended option and rationale.
- Explicitly call out why non-recommended options were rejected.

## Large-Scope Split Rule

Before deep questioning, check scope size.

If the request spans multiple independent subsystems:

- inside existing `project`: split into `multiple phases`
- outside project: propose starting a new `project`

Then brainstorm only the first `phase/workstream` in this cycle. Each phase/workstream gets its own design -> plan -> execution loop.

## Design Quality Loop

After writing the canonical design doc:

1. Run a structured self-review.
2. If issues found, revise and re-run review.
3. Maximum 5 iterations; if still unresolved, escalate to user for guidance.

Then perform user review gate. If user requests changes, return to the quality loop.

## Canonical Design Doc Requirements

The canonical doc should contain:

- `Overview and Scope`
- `Target Design`
- `Pending Changes`

Rules:

- Keep one canonical path per component/feature.
- `Pending Changes` is an active queue of approved, unimplemented deltas.
- Remove/update entries as implementation lands.
- Keep content concise and actionable.
- Avoid history logs or commit narration in design content.

## User Review Gate

After self-review passes, ask for doc review before routing to implementation:

"Design document written to `<path>`. Please review and let me know if you want any changes before implementation."

If user requests changes, revise and re-run design quality loop.

## Closeout And Next Work

After the design is approved, apply `Skill Closeout Workflow` in `using-superpowers`.

Next work routing rules:

- `phase/workstream` -> `writing-plans`
- `single task` -> execute directly with `test-driven-development`, then run `verification-before-completion` before any completion claim (no planning phase unless user explicitly overrides)

In all cases, make the next-work decision explicit before implementation.

## Red Flags

Stop and correct if you see:

- Implementation pressure before design approval
- Trying to collapse options without trade-off analysis
- Skipping written design doc update
- Skipping user review gate
- Skipping consolidation before handoff
