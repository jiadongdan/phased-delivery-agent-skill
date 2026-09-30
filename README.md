# Phased Delivery

`phased-delivery` is a small, general agent skill for substantial coding work. It guides an agent to reuse or create a living local Markdown plan, implement one coherent phase at a time, validate and review each phase, update the plan, and create clean local commit boundaries when that workflow is authorized.

The skill is intentionally independent of programming language, framework, test runner, and agent orchestration strategy.

The plan is local working state: it is retained for reference but never staged, committed, or pushed. When the plan is inside a Git worktree, the skill checks that it is untracked and uses Git's repository-local exclusion mechanism instead of changing the project's tracked `.gitignore` solely for the plan. If local exclusion is unavailable, the plan is kept outside the Git root.

## When to use it

Use it for features, refactors, migrations, or other multi-step changes that benefit from independently verifiable checkpoints. Skip it for small isolated edits where a formal plan would add unnecessary overhead.

## Structure

```text
phased-delivery/
|-- plugin.json
`-- skills/
    `-- phased-delivery/
        `-- SKILL.md
```

## Local use

Make the `skills/phased-delivery` directory available to an Agent Skills-compatible host, or copy that directory into the host's personal or repository-scoped skills directory.

Invoke it explicitly when needed:

```text
Use $phased-delivery to implement this change.
```

Repository-specific instructions should provide the actual test, lint, type-check, and build commands.

## Status

Version `0.2.2` is experimental. The workflow should be refined from observed behavior on real projects rather than expanded with speculative rules.

## License

MIT
