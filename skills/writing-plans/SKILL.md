---
name: writing-plans
description: Use when executing a phase/workstream, or when a user asks for planning on a standalone task, before execution
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** This should run in a clean workspace or branch dedicated to the plan execution.

**Save plans to:** `scratch/plans/YYYY-MM-DD-<feature-name>.md`
- (User preferences for plan location override this default)

**Design input source:** Read the relevant approved design doc (typically `scratch/designs/<component-or-feature>.md`). If the plan includes design changes, use its `Pending Changes` section as the primary implementation-gap input.

## Work Hierarchy Fit

Use this skill when:
- A `phase` or `workstream` is being implemented.
- A standalone `task` explicitly requires a written plan.

Do not require this skill for a truly standalone single-task request with no planning value.

## Scope Check

If the design doc covers multiple independent subsystems, it should have been broken into separate phase/workstream design docs during brainstorming. If it wasn't, suggest breaking this into separate plans — one per subsystem. Each plan should produce working, testable software on its own.

If this phase/workstream requires design changes but the design doc is not updated and user-approved yet, stop planning and route back to `brainstorming`.
If the design doc is already approved and unchanged for this phase/workstream, do not add design-doc update tasks for migration reconciliation.

## File Structure

Before defining tasks, map out which files will be created or modified and what each one is responsible for. This is where decomposition decisions get locked in.

- Design units with clear boundaries and well-defined interfaces. Each file should have one clear responsibility.
- You reason best about code you can hold in context at once, and your edits are more reliable when files are focused. Prefer smaller, focused files over large ones that do too much.
- Files that change together should live together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns. If the codebase uses large files, don't unilaterally restructure - but if a file you're modifying has grown unwieldy, including a split in the plan is reasonable.

This structure informs the task decomposition. Each task should produce self-contained changes that make sense independently.

Always include the relevant design doc in the plan's file map.
- If implementation changes migration status, include explicit steps to update `Pending Changes`.
- If implementation does not change design, mark the design doc as reference-only for this plan.

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

**Execution Requirement:** Use `executing-plans` to implement this plan.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

**Work Level:** [phase | workstream | task]

**Parent Context:** [project branch name or `independent`]

**Design Reference:** [`scratch/designs/<component-or-feature>.md` or equivalent approved design doc path]

**Design Doc Status In This Plan:** [`unchanged` | `updated`]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`
- Design Doc: `scratch/designs/<component-or-feature>.md` (`reference-only` or `update Pending Changes` as items are implemented)

**Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature

Co-authored-by: Codex <codex@openai.com>"
```
````

## Remember
- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- Reference relevant skills by skill name (for example: `test-driven-development`, `executing-plans`, `verification-before-completion`)
- DRY, YAGNI, TDD, frequent commits
- Plan must include explicit steps to reconcile `Pending Changes` with completed implementation work

## Plan Review Loop

After drafting the full plan:

1. Run a structured self-review on the plan and verify all checks below:
   - Scope coverage: every task maps to the goal and design reference.
   - Path clarity: create/modify/test paths are concrete and valid for the repository.
   - Test flow: behavior changes include failing-test -> implementation -> passing-test steps.
   - Task order: prerequisites come before dependent tasks.
   - Design-doc alignment:
     - `updated`: tasks map to `Pending Changes` and include reconciliation steps.
     - `unchanged`: no design-doc mutation steps are included.
2. If issues are found:
   - Fix the issues in the plan
   - Re-run the self-review
   - Repeat until the plan passes
3. When the plan passes, proceed to next-work confirmation.
4. If the loop exceeds 5 iterations, stop and ask the user for guidance.

## Post-Review User Feedback

After the plan review loop passes:

1. Go through unresolved planning open questions one by one with the user.
2. For each question:
   - If the user decides, update the plan accordingly.
   - If no decision is made, keep it explicit in the plan as an open item.
3. Repeat until all currently known open questions are addressed or explicitly tracked.

## End-of-Boundary Consolidation And Next-Work Confirmation

After the post-review user feedback step:

1. Record next work: execute this plan using `executing-plans`.
2. Run `memory-consolidation`.
3. Request user confirmation before starting the recorded next work.

## Next-Work Confirmation

After saving the plan:

**"Plan complete and saved to `scratch/plans/<filename>.md`. Open questions have been reviewed, next work is to execute this plan with `executing-plans`, and memory has been consolidated. Confirm and I will proceed."**
