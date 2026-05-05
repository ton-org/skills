# Contributing to this skills repository

## Skill layout

All skills in this repository must live **two levels below the repo root**, following the same pattern as today:

```text
<group>/<skill-name>/SKILL.md
```

Examples:

- `wallets/ton-send/SKILL.md`
- `docs/ton-docs/SKILL.md`

**`<group>`** is a logical folder (e.g. `wallets`, `docs`), not a skill by itself. **`<skill-name>`** is a single skill directory; it must contain a `SKILL.md` with valid frontmatter (`name`, `description`, etc. per [AGENTS.md](AGENTS.md)).

## What to avoid

- **Do not nest a skill inside another skill** (e.g. `docs/ton-docs/xd/SKILL.md`). The `npx skills add` CLI does not pick up those paths by default: once the parent folder is treated as a skill, nested skills are not indexed unless you pass `--full-depth`.
- **Do not rely on a single root-level `SKILL.md`** as the only way to ship multiple skills. For multi-skill packages, use separate second-level directories under the appropriate group.

Keeping skills at the second level makes discovery predictable for install tooling and keeps navigation in the repo consistent.

## Adding a new skill

When you add a new skill at `<group>/<skill-name>/`, also:

1. **`README.md` § Available skills** — add a row for the skill under its group.
2. **Evals & benchmark** — wire the skill into evals, run them, and log aggregated metrics. See [tests/TESTING.md](tests/TESTING.md).

If the new skill needs a brand-new group folder (e.g. `defi/`), also create `<group>/metadata.json` if shared metadata is required and follow the layout rules in the previous section.

## Testing

See [tests/TESTING.md](tests/TESTING.md). Aggregated metrics: [tests/RESULTS.md](tests/RESULTS.md).
