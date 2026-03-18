---
name: finishing-a-project
description: Use when a project branch is being closed and you need final verification plus closure notes captured in the project index.
---

# Finishing a Project

## Overview

Close implementation work with evidence and a clear handoff.

**Core principle:** Validate project index -> verify baseline state -> write closure handoff in project index.

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

## Example structure

```markdown
## Handoff Note
- Closure: <1-3 line summary of completed project scope>
- Verification: project index closure checks passed; tests `<cmd>` -> <result>; git status -> <summary>
- Follow-ups: <none or concise list>
- User decisions: <none or concise list>
```

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

**Always:**
- Provide evidence-first verification
- Validate all phase and goal closure conditions in the project index
- Write concise closure handoff notes directly in the project index
- Leave merge/push/remote operations to explicit user instruction
