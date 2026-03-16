---
name: writing-plans
description: Use when executing a phase/workstream or any multi-task work that needs an explicit implementation plan before execution
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** This should run in a clean workspace or branch dedicated to the plan execution.

**Save plans to:** `scratch/plans/YYYY-MM-DD-<feature-name>.md`
- (User preferences for plan location override this default)

**Design input source:** Read the canonical feature/component spec at `scratch/designs/<component-or-feature>.md` and use its `Migration / Pending Changes` section as the primary implementation-gap input.

## Work Hierarchy Fit

Use this skill when:
- A `phase` or `workstream` is being implemented.
- A standalone `task` expands into multiple tasks/steps requiring coordination.

Do not require this skill for a truly standalone single-task request with no planning value.

## Scope Check

If the spec covers multiple independent subsystems, it should have been broken into sub-project specs during brainstorming. If it wasn't, suggest breaking this into separate plans — one per subsystem. Each plan should produce working, testable software on its own.

If the canonical spec has no `Migration / Pending Changes` section, add one before planning and capture approved unimplemented deltas there.

## File Structure

Before defining tasks, map out which files will be created or modified and what each one is responsible for. This is where decomposition decisions get locked in.

- Design units with clear boundaries and well-defined interfaces. Each file should have one clear responsibility.
- You reason best about code you can hold in context at once, and your edits are more reliable when files are focused. Prefer smaller, focused files over large ones that do too much.
- Files that change together should live together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns. If the codebase uses large files, don't unilaterally restructure - but if a file you're modifying has grown unwieldy, including a split in the plan is reasonable.

This structure informs the task decomposition. Each task should produce self-contained changes that make sense independently.

Include the canonical spec file in the plan's file map whenever implementation tasks will change migration status.

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED: Use `executing-plans` to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

**Work Level:** [phase | workstream | task]

**Parent Context:** [project branch name or `independent`]

**Design Reference:** [`scratch/designs/<component-or-feature>.md` or `none` with reason]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`
- Spec: `scratch/designs/<component-or-feature>.md` (update `Migration / Pending Changes` as items are implemented)

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## Remember
- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits
- Plan must include explicit steps to reconcile `Migration / Pending Changes` with completed implementation work

## Plan Review Loop

After completing each chunk of the plan:

1. Run a structured review pass on the chunk (requirements coverage, file paths, test strategy, and dependency order).
   - Confirm tasks implement items listed in `Migration / Pending Changes`
   - Confirm completion criteria includes updating/removing implemented migration entries in the canonical spec
   - Provide to the review pass: chunk content, path to spec document
2. If ❌ Issues Found:
   - Fix the issues in the chunk
   - Re-run the review pass for that chunk
   - Repeat until ✅ Approved
3. If ✅ Approved: proceed to next chunk (or execution handoff if last chunk)

**Chunk boundaries:** Use `## Chunk N: <name>` headings to delimit chunks. Each chunk should be ≤1000 lines and logically self-contained.

**Review loop guidance:**
- Same agent that wrote the plan fixes it (preserves context)
- If loop exceeds 5 iterations, surface to human for guidance
- Reviewers are advisory - explain disagreements if you believe feedback is incorrect

## Execution Handoff

After saving the plan:

**"Plan complete and saved to `scratch/plans/<filename>.md`. Ready to execute?"**

**Execution path:**
- Execute plan in current session using executing-plans
- Batch execution with checkpoints for review
