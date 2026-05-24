---
name: finishing-a-project
description: Use when closing project-level work on a dedicated branch.
---

# Finishing a Project

## Overview

Close implementation work with evidence and a clear handoff.

**Core principle:** Validate project index -> verify baseline state -> write closure handoff -> consolidate memory for post-project reuse.

**Scope:** This skill closes `project`-level work (dedicated branch). For independent workstreams/tasks, use verification-before-completion unless the user asks for a closure note.

**Announce at start:** "I'm using the finishing-a-project skill to close this work."

## The Process

### Step 1: Validate project index closure

Read the project index:

`scratch/project-index/<project-slug>.md`

Before closing, confirm all of the following:

- every phase checkbox is complete (`[x]`)
- every phase status is `done`
- no unresolved gap against `Purpose`
- no unresolved gap against `Success Criteria`

If any gap remains, stop and report blockers before closing.

### Step 2: Verify baseline (tests + git status)

Run project tests and capture exact result:

```bash
npm test / cargo test / pytest / go test ./...
```

Then verify repository state:

```bash
git status --short --branch
```

Record for closure handoff:
- test command used + pass/fail result
- current branch
- whether working tree is clean or has pending changes

If tests fail, stop and report blockers before closing.

### Step 3: Write closure handoff in the project index

Update `## Handoff Note` inside:

`scratch/project-index/<project-slug>.md`

Keep this section short (no extra subsections). Include:
closure summary, verification summary, follow-ups, and user decisions.

Example handoff structure:

```markdown
## Handoff Note
- Closure: <1-3 line summary of completed project scope>
- Verification: project index closure checks passed; tests `<cmd>` -> <result>; git status -> <summary>
- Follow-ups: <none or concise list>
- User decisions: <none or concise list>
```

### Step 4: Apply project-close memory consolidation

When running closeout `memory-consolidation`, treat the project index and linked plan/review docs as closure evidence only, not persistent memory content. Keep memory to durable post-project signal:

- remove all project-specific timeline, phase history, implementation narration, completed-work summaries, and settled decision trails
- remove `Reference` pointers that only preserve project history, implementation detail, plan docs, review docs, or the completed project index
- keep only reusable workflow lessons, repository-wide constraints, unresolved follow-ups, and links to canonical design docs that remain useful after the project is closed
- if completed project detail matters later, capture it in the relevant canonical design doc and keep only that design reference in memory
- promote reusable cross-repository lessons to global memory only when they are durable outside this repository
- leave `Journal` empty and remove completed project `Work` items

## Common Mistakes

**Closing without evidence**
- Problem: completion claims are not verifiable
- Fix: include index validation result, exact test command, and git status snapshot in project index handoff

**Closing with unresolved phase or goal gaps**
- Problem: project close does not match declared scope/purpose
- Fix: stop closure, list blockers, and resolve or explicitly defer before closing

**Verbose closure notes**
- Problem: handoff becomes hard to scan and maintain
- Fix: keep handoff notes short and evidence-focused

**Mixing remote git operations into closure**
- Problem: conflicts with user-managed integration workflow
- Fix: keep closure focused on verification and handoff

## Red Flags

**Never:**
- Claim closure without running tests in this session
- Claim closure without validating the project index
- Skip git status in closure baseline
- Skip updating `## Handoff Note` in the project index
- Leave completed project timeline or implementation history in memory
- Preserve completed project index/plan/review links as memory references unless they are canonical design references

**Always:**
- Provide evidence-first verification
- Validate all phase and goal closure conditions in the project index
- Write concise closure handoff notes directly in the project index
- Apply project-close rules during closeout memory consolidation
- Leave merge/push/remote operations to explicit user instruction
