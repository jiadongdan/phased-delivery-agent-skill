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
3. Record the goal, important constraints, acceptance criteria, and definition of done in a Markdown plan.
4. Divide the work into phases that are small enough to implement, validate, review, and revert independently.
5. For each phase, record its objective, expected scope, acceptance criteria, validation approach, dependencies, and status.

Keep the plan current as discoveries change the implementation. Record material decisions and deviations with brief reasons.

## Deliver Each Phase

For every phase:

1. Re-read the current phase and inspect the code it affects.
2. Implement only the coherent scope of that phase.
3. Add or update tests when they are appropriate for the changed behavior.
4. Run the most relevant available validation, such as targeted tests, linting, type checking, builds, smoke tests, or direct behavioral checks.
5. Repair failures caused by the change before continuing.
6. Review the diff, affected integrations, edge cases, error handling, compatibility, and test coverage. Use cumulative regression tests where practical instead of repeatedly reviewing the entire repository.
7. Update the plan with completed work, validation evidence, decisions, deviations, and remaining work.
8. When working in a Git repository and local commits are part of the requested workflow, commit the verified phase as one coherent change. Exclude unrelated changes and use a message that describes intent.

Do not weaken an existing test merely to make an implementation pass unless the required behavior has explicitly changed.

## Finish

After all phases:

1. Run the complete relevant validation suite.
2. Check the original acceptance criteria and exercise the main user-visible behavior when possible.
3. Review the complete branch diff and remove temporary or debugging artifacts.
4. Update relevant documentation and record known limitations or follow-up work.
5. Report the delivered behavior, validation performed, commits created, and any remaining risks.

Do not push, merge, deploy, publish, rewrite history, or discard user changes unless the user explicitly requests it.

## Keep the Workflow Proportional

Skip the formal multi-phase process for genuinely small, isolated changes. Do not require test-driven development, subagents, worktrees, or additional artifacts unless the task, repository, or user requires them.
