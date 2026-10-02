# Phased Delivery

`phased-delivery` is a small, general agent skill for substantial coding work. It guides an agent to maintain a local execution plan, reconcile partially completed work before resuming, implement one coherent phase at a time, scale testing cost with change risk, preserve durable project knowledge, and create clean local commit boundaries when that workflow is authorized.

The skill is intentionally independent of programming language, framework, test runner, and agent orchestration strategy.

Private execution state stays local; durable project knowledge belongs in Git when repository conventions support it. The skill keeps one execution plan for phase status, agent memory, and validation evidence. That plan is retained for reference but never staged, committed, or pushed. Intentionally tracked roadmaps, design documents, ADRs, migration guides, and user documentation remain normal project artifacts. The skill also avoids proliferating extra memory files by consolidating transient working information into the local execution plan.

Before writing to a local execution plan inside a Git worktree, the skill checks that it is untracked and establishes repository-local exclusion instead of changing the project's tracked `.gitignore` solely for the plan. If a proposed plan is already tracked, it is preserved as durable documentation and a different local execution-plan path is selected. If local exclusion is unavailable, the local plan is kept outside the Git root.

## When to use it

Use it for features, refactors, migrations, or other multi-step changes that benefit from independently verifiable checkpoints. Skip it for small isolated edits where a formal plan would add unnecessary overhead.

## Structure

```text
phased-delivery/
|-- plugin.json                         # Optional plugin-host packaging
`-- skills/
    `-- phased-delivery/
        `-- SKILL.md                    # Canonical, host-neutral skill
```

`skills/phased-delivery/` is the canonical, host-neutral skill. `plugin.json` packages it for compatible plugin hosts such as Codex. Hosts with different packaging requirements should install or adapt the canonical skill directory without changing its instruction body.

## Install by host

The canonical skill is `skills/phased-delivery/SKILL.md`. Preserve its instruction body when installing on another host; adapt only host-specific packaging or metadata.

- **Codex:** Install the repository as a portable plugin, or install the `skills/phased-delivery` directory as a personal or repository-scoped skill. Invoke it as `$phased-delivery`.
- **Claude Code:** Copy or link `skills/phased-delivery` to `~/.claude/skills/phased-delivery` for personal use or `.claude/skills/phased-delivery` for project use. Invoke it as `/phased-delivery`.
- **WorkBuddy:** Import the canonical skill directory. If the selected installation surface requires additional marketplace metadata, create a host-specific package that adds only that metadata and preserves the canonical instruction body.

To delegate installation to an agent, use:

```text
Install the phased-delivery skill from
https://github.com/jiadongdan/phased-delivery-agent-skill.
Use skills/phased-delivery/SKILL.md as the canonical skill. Adapt only the
packaging or metadata required by this host, and do not alter its workflow
instructions.
```

## Example prompts

### Start substantial work

```text
Use phased-delivery to implement this feature. Divide it into coherent phases,
validate and review each phase, and create local commits at verified phase
boundaries. Do not push.
```

### Resume partially completed work

```text
Use phased-delivery to continue this work. Reconcile the existing plans, code,
commits, working-tree changes, and validation evidence before selecting the
next phase.
```

### Use an existing plan

```text
Use phased-delivery with the existing execution plan at <path>. Keep private
execution state local, but preserve and update durable project documentation
when it is in scope.
```

### Control expensive testing

```text
Use phased-delivery for this refactor. The complete test suite is expensive,
so follow the cost-aware validation ladder and reserve the full relevant suite
for final validation.
```

Replace `Use phased-delivery` with `$phased-delivery` in Codex or `/phased-delivery` in Claude Code when explicit invocation syntax is preferred.

Repository-specific instructions should provide the actual test, lint, type-check, and build commands.

## Status

Version `0.5.0` is experimental. The workflow should be refined from observed behavior on real projects rather than expanded with speculative rules.

## License

MIT
