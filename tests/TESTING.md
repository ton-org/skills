# Testing — TON Skills

Evals run via the [skill-creator](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator) plugin. It handles execution, grading, benchmarking, and an HTML viewer — we only maintain the eval definitions in [`tests/evals/evals.json`](evals/evals.json).

## Setup (one-time)

1. Install [Claude Code](https://docs.anthropic.com/en/docs/claude-code):

```bash
npm install -g @anthropic-ai/claude-code
```

2. Install the skill-creator plugin:

```bash
claude plugin install skill-creator
```

3. Clone this repo and install skills from the local source:

```bash
git clone https://github.com/ton-org/skills.git
cd skills
npx skills add . --skill '*'
```

## Running evals

The skill-creator plugin reads evals from `./evals/evals.json` relative to its working directory. Run it from the `tests/` folder so that path resolves to `tests/evals/evals.json`:

```bash
cd tests
```

Then in Claude Code:

```
/skill-creator evaluate ton
```

This will:
1. Read `tests/evals/evals.json`
2. Run each eval **with** and **without** the skill (parallel subagents)
3. Grade expectations → `grading.json` (pass/fail with evidence per assertion)
4. Aggregate results → `benchmark.json` + `benchmark.md` (mean, stddev, delta)
5. Open the HTML eval viewer for review

### Benchmark mode (multiple runs)

For statistical reliability, ask skill-creator to run a benchmark with multiple runs:

```
/skill-creator benchmark ton --runs 3
```

This runs each eval 3 times per configuration and produces summary stats with stddev.

### Comparing skill versions

After editing a skill, run:

```
/skill-creator improve ton
```

Skill-creator compares the new version against the previous best using blind A/B comparison and tracks version history.

## Recording results

After a run, the plugin writes several temp files into `tests/` (its working directory): `grading.json`, `benchmark.json`, `benchmark.md`. These are gitignored — only the final metrics go into `RESULTS.md` so git history tracks skill progress over time.

`RESULTS.md` is a metrics-only file. It contains, per run, exactly:

- Date heading and model
- Per-eval summary table (eval, group, with-skill time/tokens, without-skill time/tokens, winner)
- Aggregate stats table (avg time, avg tokens, avg tool calls — with delta — and winner breakdown: Skill / Baseline / Tie)

Do **not** add prose, findings, assertion tables, raw grading JSON, or subagent traces to `RESULTS.md`. Anything narrative goes in the PR description or a separate notes file.

The goal: a compact metrics ledger where `git diff tests/RESULTS.md` makes regressions and improvements visible at a glance.

## Eval format

Evals live in `tests/evals/evals.json`:

```json
{
  "skill_name": "ton",
  "evals": [
    {
      "id": 1,
      "group": "ton-balance",
      "prompt": "How much TON do I have?",
      "expected_output": "Shows the user's TON balance as a numeric amount",
      "expectations": [
        "Calls get_balance or get_balance_by_address",
        "Response contains a numeric TON amount",
        "Does not hallucinate a wallet address"
      ]
    }
  ]
}
```

### Fields

| Field | Required | Description |
|---|---|---|
| `id` | yes | Unique integer |
| `group` | no | Skill group for organizing results (e.g. `ton-balance`) |
| `prompt` | yes | The user request to execute |
| `expected_output` | yes | Human-readable description of success |
| `expectations` | yes | List of verifiable assertions for the grader |
| `files` | no | Input files for the eval (relative to skill root) |

### Writing good expectations

- Be specific and verifiable: "Calls get_balance" not "Uses the right tool"
- Use "or" for acceptable alternatives: "Calls get_nfts or get_nfts_by_address"
- Include negative checks where relevant: "Does not send a real transaction"
- Cover both the process (tool calls) and the result (response content)

## Adding evals

1. Add a new entry to `tests/evals/evals.json` with a unique `id`
2. Run `/skill-creator evaluate ton` to test it
3. Review the grading in the viewer — refine expectations if too easy or too strict

## Test coverage

The 32 evals cover 9 skill groups:

| Group | Evals | IDs | Description |
|---|---|---|---|
| ton-balance | 5 | 1–5 | Balances, tokens, transactions |
| ton-manage-wallets | 3 | 6–8 | Wallet list, active wallet, validation |
| ton-swap | 4 | 9–12 | Quotes, swaps, error handling |
| ton-xstocks | 4 | 13–16 | xStock prices, buy, sell |
| ton-send | 3 | 17–19 | DNS, TON transfers, jetton transfers |
| ton-nfts | 3 | 20–22 | NFT list, details, feed |
| ton-docs | 3 | 23–25 | Jettons, wallets, NFT standards |
| ton-cli | 3 | 26–28 | CLI balance, wallets, transactions |
| cross-skill | 4 | 29–32 | Portfolio value, multi-step operations |
