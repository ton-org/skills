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

When you add a new skill at `<group>/<skill-name>/`, you also need to wire it into evals and the repo index. Without this, the skill won't be exercised by the [skill-creator](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator) plugin and won't appear in benchmarks.

Required updates for every new skill:

1. **`tests/evals/evals.json`** — add at least 2–3 evals covering the skill's primary workflows. Use the new skill's name as the `group` value, assign unique sequential `id`s, and follow the format documented in [tests/TESTING.md § Eval format](tests/TESTING.md#eval-format). Write [good expectations](tests/TESTING.md#writing-good-expectations) — verifiable tool calls and response checks, not vague descriptions.
2. **`tests/TESTING.md` § Test coverage** — add a row for the new group (group name, eval count, ID range, one-line description). Update the total eval count in the section heading if it changed.
3. **`README.md` § Available skills** — add the skill to the table under its group with a short description.
4. **Run the evals** — `claude plugin install skill-creator`, then `cd tests` (so the plugin finds `evals/evals.json` relative to cwd) and run `/skill-creator evaluate ton`. Fix the skill or refine expectations until evals pass cleanly with the skill enabled. Do **not** commit the temp files (`grading.json`, `benchmark.json`, `benchmark.md`) — they're already gitignored.
5. **`tests/RESULTS.md`** — append a new dated metrics block from the next full benchmark (`/skill-creator benchmark ton --runs 3`). Per `RESULTS.md` policy, only metrics go there: date+model, per-eval summary, aggregate stats with Wins / Losses / Ties.

If the new skill needs a brand-new group folder (e.g. `defi/`), also create `<group>/metadata.json` if shared metadata is required and follow the layout rules in the previous section.

## Testing

Repository-wide end-to-end test prompts, rules, and metrics live under [tests/TESTING.md](tests/TESTING.md). Aggregated metrics from each benchmark are logged in [tests/RESULTS.md](tests/RESULTS.md). Evals themselves are defined in [`tests/evals/evals.json`](tests/evals/evals.json) and executed by the skill-creator plugin — see TESTING.md for setup.
