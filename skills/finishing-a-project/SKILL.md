---
name: finishing-a-project
description: Use when implementation work is complete and you need to close the project with final verification and a project-closing document
---

# Finishing a Project

## Overview

Close implementation work with evidence and a clear handoff.

**Core principle:** Verify baseline state -> write project-closing document.

**Announce at start:** "I'm using the finishing-a-project skill to close this work."

## The Process

### Step 1: Verify baseline (tests + git status)

Run project tests and capture exact result:

```bash
npm test / cargo test / pytest / go test ./...
```

Then verify repository state:

```bash
git status --short --branch
```

Record in the closing doc:
- test command used + pass/fail result
- current branch
- whether working tree is clean or has pending changes

If tests fail, stop and report blockers before closing.

### Step 2: Write project-closing document

Create `docs/project-closure/YYYY-MM-DD-<topic>-closure.md` with these sections:

1. `Summary`
- what was completed in this project cycle

2. `Verification`
- test command(s) and outcomes
- git status summary from Step 1

3. `Follow-ups`
- concrete next tasks needed for continuation
- unresolved risks/debt/issues

4. `Future Directions`
- 2-4 plausible directions for next iteration
- brief trade-off notes for each direction

5. `Handoff Notes`
- what the user should decide next (merge/push/release timing, if applicable)

## Example structure

```markdown
# <Topic> Project Closure

## Summary
- ...

## Verification
- Tests: `pytest` -> 42 passed, 0 failed
- Git status: `## feature/x...` + clean working tree

## Follow-ups
- [ ] ...

## Future Directions
- Direction A: ...
- Direction B: ...

## Handoff Notes
- User-managed actions: merge/push/release.
```

## Common Mistakes

**Closing without evidence**
- Problem: completion claims are not verifiable
- Fix: include exact test command + git status snapshot

**No actionable follow-ups**
- Problem: next cycle loses momentum
- Fix: list concrete tasks and risks, not vague ideas

**Mixing remote git operations into closure**
- Problem: conflicts with user-managed integration workflow
- Fix: keep closure focused on verification and handoff

## Red Flags

**Never:**
- Claim closure without running tests in this session
- Skip git status in closure baseline
- Omit follow-ups/future directions

**Always:**
- Provide evidence-first verification
- Write a closure doc with explicit next steps
- Leave merge/push/remote operations to explicit user instruction

## Integration

**Called by:**
- **executing-plans** - After all tasks complete

**Pairs with:**
- **verification-before-completion** - Ensure claims are backed by fresh command output
