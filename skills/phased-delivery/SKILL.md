---
name: phased-delivery
description: Use for substantial multi-step coding work that should be planned and delivered in independently validated, reviewed, documented, and committed phases. Do not use for small isolated changes.
---

# Phased Delivery

Deliver substantial coding work through a living plan and a sequence of coherent, verifiable phases. Adapt the workflow to the repository rather than imposing a language, framework, testing style, or fixed plan filename.

Explicit user instructions and repository instructions take precedence over this skill.

## Plan

Before editing code:

1. Inspect the relevant repository instructions, existing code, validation commands, and working-tree state.
2. Preserve unrelated user changes.
3. Check whether the user or project already provides a Markdown plan that clearly belongs to the task. Reuse it instead of creating a duplicate. Leave ambiguous or unrelated plans untouched.
4. When using Git, determine the Git root explicitly. Evaluate the selected plan's resolved path against that root rather than guessing from directory names.
5. Before writing to an existing or new plan, apply the local-plan rules below. For a path inside the Git worktree, stop if it is tracked; otherwise establish and verify repository-local exclusion. For a path outside the Git root, keep it inside the project workspace.
6. If no suitable plan exists, create one at the selected location. Create or update the plan to record the goal, important constraints, acceptance criteria, and definition of done.
7. Divide the work into phases that are small enough to implement, validate, review, and revert independently.
8. For each phase, record its objective, expected scope, acceptance criteria, validation approach, dependencies, and status.

Keep the plan current as discoveries change the implementation. Record material decisions and deviations with brief reasons.

## Keep the Plan Local

The plan is local working state. Retain it after completion, but never stage, commit, or push it.

- If the plan is outside the Git root but inside the project workspace, retain it there. Git cannot include it.
- If the plan is inside the Git worktree, first run `git ls-files --error-unmatch -- "<repo-relative-plan-path>"`. If it succeeds, the plan is tracked: stop and report the conflict. Do not move, untrack, delete, or rewrite it without explicit user authorization; ignore rules do not protect tracked files.
- For an untracked in-worktree plan, exclude it through Git's local repository exclusion mechanism. Locate the exclusion file with `git rev-parse --git-path info/exclude`; do not modify tracked `.gitignore` solely for the plan.
- If the local exclusion file cannot be located or safely updated, move the untracked plan outside the Git root but keep it inside the project workspace. If that is not possible, stop and report the conflict.
- Verify the exclusion with `git check-ignore -q -- "<repo-relative-plan-path>"`. If verification fails, use the same outside-the-Git-root fallback or stop and report the conflict.
- Before every commit, confirm that an in-worktree plan remains untracked and ignored, and is absent from `git diff --cached --name-only`.

Never treat the plan as a temporary or debugging artifact. Delete it only when the user explicitly requests deletion.

## Adjust the Plan When Reality Changes

If an assumption proves wrong, a phase becomes too large, or the current division is no longer coherent:

1. Stop expanding the current phase.
2. Record the discovery and its effect on scope or acceptance criteria.
3. Split, reorder, replace, or abandon affected phases with a brief reason.
4. Recheck dependencies and validation before resuming.

Do not expand scope silently.

## Deliver Each Phase

For every phase:

1. Re-read the current phase and inspect the code it affects.
2. Implement only the coherent scope of that phase.
3. Add or update tests for new or changed behavior when the repository supports them and the behavior is proportionately testable. Otherwise record why and use another observable check.
4. Map every acceptance criterion to observable evidence and run the strongest proportional checks available, such as targeted tests, linting, type checking, builds, smoke tests, or direct behavioral checks. Do not substitute inspection for an executable check the repository already provides.
5. Repair failures caused by the change before continuing. Distinguish unrelated baseline failures and record them rather than hiding them.
6. Review the diff, affected integrations, edge cases, error handling, compatibility, and test coverage. Use cumulative regression tests where practical instead of repeatedly reviewing the entire repository.
7. Update the plan with completed work, validation evidence, decisions, deviations, and remaining work.
8. When local commits are part of the requested workflow, end each completed phase at a distinct local commit boundary. Stage implementation and documentation paths deliberately, verify that the plan and unrelated changes are absent from the staged diff, and use messages that describe intent. Do not combine unrelated phases in one commit; a phase may use multiple commits when repository conventions or safe implementation require them.

Do not weaken an existing test merely to make an implementation pass unless the required behavior has explicitly changed.

## Finish

After all phases:

1. Run the complete relevant validation suite.
2. Check the original acceptance criteria and exercise the main user-visible behavior when possible.
3. Review the complete branch diff and remove temporary or debugging artifacts, excluding the retained local plan.
4. Update relevant documentation and record known limitations or follow-up work.
5. Confirm that the plan remains local and untracked, then report the delivered behavior, validation performed, commits created, and any remaining risks.

Do not push, merge, deploy, publish, rewrite history, or discard user changes unless the user explicitly requests it.

## Keep the Workflow Proportional

Skip the formal multi-phase process for genuinely small, isolated changes. Do not require test-driven development, subagents, worktrees, or workflow artifacts beyond the local plan unless the task, repository, or user requires them.
