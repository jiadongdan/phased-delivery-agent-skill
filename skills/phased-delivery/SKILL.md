---
name: phased-delivery
description: Use for substantial multi-step coding work, including partially completed work, that should be planned and delivered in independently validated, reviewed, documented, and committed phases with cost-aware testing. Do not use for small isolated changes.
---

# Phased Delivery

Deliver substantial coding work through a living plan and a sequence of coherent, verifiable phases. Adapt the workflow to the repository rather than imposing a language, framework, testing style, or fixed plan filename.

Explicit user instructions and repository instructions take precedence over this skill.

## Plan

Before editing code:

1. Inspect the relevant repository instructions, existing code, validation commands, and working-tree state.
2. Preserve unrelated user changes.
3. Inspect existing plans and related Markdown documents that clearly belong to the task. Classify them by purpose using the rules below rather than assuming every planning-related file is private execution state. Leave unrelated documents untouched; if an applicable document's intended audience or disposition is materially ambiguous, request direction before modifying it.
4. Select one local execution plan for phase status, working decisions, and validation evidence. Reuse a suitable untracked local plan when available; otherwise create one. Do not create additional memory or progress files when the information belongs in this plan.
5. When using Git, determine the Git root explicitly. Evaluate the local execution plan's resolved path against that root rather than guessing from directory names, then establish and verify its local-only disposition as described below.
6. Use applicable tracked project plans and documentation as durable sources of requirements and decisions. Update them only when the task or repository conventions call for it; do not place transient agent memory or detailed execution logs in them.
7. Create or update the local execution plan to record the goal, important constraints, acceptance criteria, and definition of done.
8. Divide the work into phases that are small enough to implement, validate, review, and revert independently.
9. For each phase, record its objective, expected scope, acceptance criteria, validation approach, dependencies, and status.

Keep the local execution plan current as discoveries change the implementation. Record material decisions and deviations with brief reasons, and promote durable knowledge to the appropriate project documentation when it will matter beyond the current delivery effort.

## Classify Planning Artifacts

Private execution state stays local; durable project knowledge belongs in Git when repository conventions support it. Classify artifacts by purpose, intended audience, existing tracked status, and repository conventions rather than by the `.md` extension or the word `plan` in a filename.

- **Local execution state:** phase checklists, progress notes, agent memory, detailed validation logs, and other session-continuity material. Consolidate this information into the selected local execution plan. Retain that plan after completion, but never stage, commit, or push it.
- **Durable project knowledge:** intentionally tracked roadmaps, design documents, architecture decision records, migration guides, user documentation, and other material useful to future contributors. Preserve tracked status and update or commit these documents when they are in scope. Do not move, untrack, or locally exclude them merely because they contain planning information.
- **Temporary artifacts:** debugging notes, generated logs, and investigation output created during the work with no continuing value. Remove them when they are no longer needed; do not misclassify the retained local execution plan as temporary or delete pre-existing user material without authorization.

Apply the following location and Git rules only to the selected local execution plan:

- If the plan is outside the Git root but inside the project workspace, retain it there. Git cannot include it.
- If the proposed local execution plan is inside the Git worktree, first run `git ls-files --error-unmatch -- "<repo-relative-plan-path>"`. If it succeeds, it is tracked and therefore not suitable for private execution state. Preserve the tracked file, classify and use it as durable project documentation when applicable, and select a different local execution-plan path. Do not move, untrack, delete, or rewrite the tracked file without explicit user authorization; ignore rules do not protect tracked files.
- For an untracked in-worktree plan, exclude it through Git's local repository exclusion mechanism. Locate the exclusion file with `git rev-parse --git-path info/exclude`; do not modify tracked `.gitignore` solely for the plan.
- If the local exclusion file cannot be located or safely updated, move the untracked plan outside the Git root but keep it inside the project workspace. If that is not possible, stop and report the conflict.
- Verify the exclusion with `git check-ignore -q -- "<repo-relative-plan-path>"`. If verification fails, use the same outside-the-Git-root fallback or stop and report the conflict.
- Before every commit, confirm that an in-worktree plan remains untracked and ignored, and is absent from `git diff --cached --name-only`.

Delete the retained local execution plan only when the user explicitly requests deletion.

## Resume Existing Work

When applicable planning artifacts or implementation work predate the current session, reconcile them before choosing the next phase:

1. Inspect the local execution plan, applicable durable project documents, current code, relevant commits and diffs, working-tree changes, and existing validation evidence.
2. Treat plan status as navigation, not proof. Confirm completed phases against the implementation and follow the cost-aware validation ladder below when evidence is missing or stale.
3. Preserve existing changes. Do not overwrite, revert, or discard work merely to make the code match the plan.
4. Update the local execution plan to reflect observed state: mark a phase complete only when its implementation and acceptance evidence support that status; mark partial work in progress and record what remains; incorporate in-scope code that the plan omitted. Update durable project documents only with information appropriate to their long-term audience.
5. Resume from the first incomplete or insufficiently verified phase after rechecking its dependencies.

If the planning artifacts and code conflict in a way that materially changes intended behavior, stop and request direction rather than guessing.

## Adjust the Plan When Reality Changes

If an assumption proves wrong, a phase becomes too large, or the current division is no longer coherent:

1. Stop expanding the current phase.
2. Record the discovery and its effect on scope or acceptance criteria.
3. Split, reorder, replace, or abandon affected phases with a brief reason.
4. Recheck dependencies and validation before resuming.

Do not expand scope silently.

## Scale Validation Cost

Use the least expensive check that can provide meaningful evidence, then broaden validation as risk and dependency scope increase:

- During implementation, run the narrowest relevant executable check, such as a specific test, focused lint or type check, or direct behavioral probe.
- At a phase boundary, validate the phase's acceptance criteria, affected modules, and direct integrations.
- Broaden regression coverage when changes cross component boundaries or affect shared APIs, schemas, configuration, dependencies, persistence formats, concurrency, or similarly wide behavior.
- At final completion, run the complete relevant validation suite.

Do not rerun an expensive suite merely because another edit occurred. Reuse recorded evidence only when the tested code, relevant dependencies, configuration, fixtures, and inputs have not changed. Record each material validation command, scope, result, and tested commit or working-tree state in the local execution plan.

Defer expensive GPU, network, end-to-end, or large-data checks to the relevant phase boundary or final validation unless the current change directly affects them. If a full relevant suite is unavailable or impractical, run the strongest feasible subset and report what was omitted and why; do not claim complete validation.

## Deliver Each Phase

For every phase:

1. Re-read the current phase and inspect the code it affects.
2. Implement only the coherent scope of that phase.
3. Add or update tests for new or changed behavior when the repository supports them and the behavior is proportionately testable. Otherwise record why and use another observable check.
4. Map every acceptance criterion to observable evidence and follow the validation ladder above. Do not substitute inspection for an executable check the repository already provides.
5. Repair failures caused by the change before continuing. Distinguish unrelated baseline failures and record them rather than hiding them.
6. Review the diff, affected integrations, edge cases, error handling, compatibility, and test coverage. Broaden regression testing only as required by the validation ladder.
7. Update the local execution plan with completed work, validation evidence, decisions, deviations, and remaining work. Update durable project documentation when the phase changes information that future contributors should retain.
8. When local commits are part of the requested workflow, end each completed phase at a distinct local commit boundary. Stage implementation and durable documentation paths deliberately, verify that the local execution plan and unrelated changes are absent from the staged diff, and use messages that describe intent. Do not combine unrelated phases in one commit; a phase may use multiple commits when repository conventions or safe implementation require them.

Do not weaken an existing test merely to make an implementation pass unless the required behavior has explicitly changed.

## Finish

After all phases:

1. Follow the final-completion rung of the validation ladder: run the complete relevant suite or, when that is unavailable or impractical, run the strongest feasible subset and report omissions and reasons.
2. Check the original acceptance criteria and exercise the main user-visible behavior when possible.
3. Review the complete branch diff and remove temporary or debugging artifacts, excluding the retained local execution plan and any durable project documentation.
4. Update relevant documentation and record known limitations or follow-up work.
5. Confirm that the local execution plan remains local and untracked, then report the delivered behavior, durable documentation updated, validation performed, commits created, and any remaining risks.

Do not push, merge, deploy, publish, rewrite history, or discard user changes unless the user explicitly requests it.

## Keep the Workflow Proportional

Skip the formal multi-phase process for genuinely small, isolated changes. Do not require test-driven development, subagents, worktrees, or workflow artifacts beyond the local execution plan and necessary durable documentation unless the task, repository, or user requires them.
