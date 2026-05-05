# Testing — TON Skills

## Quick start

Open Claude Code **from the `tests/` directory** (not the repo root):

```bash
cd tests
claude
```

MCP servers (`ton-mcp`, `ton-docs`) are configured in `tests/.mcp.json` and will be picked up automatically. Eval files, workspace, and results all live here too.

Run evals per skill:

```
/skill-creator evaluate ton-balance
/skill-creator evaluate ton-swap
/skill-creator evaluate ton-send
/skill-creator evaluate ton-nfts
/skill-creator evaluate ton-docs
/skill-creator evaluate ton-cli
/skill-creator evaluate ton-xstocks
/skill-creator evaluate ton-manage-wallets
/skill-creator evaluate cross-skill
```

Each command reads the corresponding `evals/<skill-name>.json`, runs evals with and without the skill, grades expectations, aggregates results, and opens the HTML viewer.

To run **all skills at once**, ask Claude:

```
/skill-creator evaluate ton-*
```

## Before running evals — preflight check

Before running any `/skill-creator evaluate` command, **always run a preflight check first**. This takes seconds and prevents wasting tokens on broken setups.

Run these MCP calls to verify the environment is working:

1. **MCP connected?** — Call `mcp__ton-mcp__get_wallet`. If this fails, MCP is not connected. The user needs to restart Claude Code from `tests/` where `.mcp.json` is configured.

2. **Wallet has funds?** — Call `mcp__ton-mcp__get_balance`. Must show >= 0.5 TON. If zero or error, the wallet is not set up.

3. **Jettons available?** — Call `mcp__ton-mcp__get_jettons`. Should show USDT. Missing tokens will cause some evals to fail.

4. **TON Docs MCP connected?** — Call `mcp__ton-docs__search_ton_docs` with a simple query like "jetton". If this fails, the ton-docs server is down or not connected.

If any check fails, **stop and tell the user** what's wrong instead of launching dozens of agents that will all fail.

## Eval files

Each skill has its own eval file in `evals/`:

| File | Skill | Evals | IDs |
|------|-------|-------|-----|
| `ton-balance.json` | ton-balance | 5 | 1-5 |
| `ton-manage-wallets.json` | ton-manage-wallets | 3 | 6-8 |
| `ton-swap.json` | ton-swap | 4 | 9-12 |
| `ton-xstocks.json` | ton-xstocks | 4 | 13-16 |
| `ton-send.json` | ton-send | 3 | 17-19 |
| `ton-nfts.json` | ton-nfts | 3 | 20-22 |
| `ton-docs.json` | ton-docs | 3 | 23-25 |
| `ton-cli.json` | ton-cli | 3 | 26-28 |
| `cross-skill.json` | cross-skill | 4 | 29-32 |

`evals.json` is the combined file with all 32 evals (kept for backwards compatibility).

## Skill paths

| Skill | Path |
|-------|------|
| ton-balance | `wallets/ton-balance` |
| ton-manage-wallets | `wallets/ton-manage-wallets` |
| ton-swap | `wallets/ton-swap` |
| ton-xstocks | `wallets/ton-xstocks` |
| ton-send | `wallets/ton-send` |
| ton-nfts | `wallets/ton-nfts` |
| ton-cli | `wallets/ton-cli` |
| ton-create-wallet | `wallets/ton-create-wallet` |
| ton-docs | `docs/ton-docs` |

## Prerequisites

### Wallet

Evals run against a **real wallet on mainnet**. The active wallet (`~/.config/ton/config.json`) must have:

| Asset | Min amount | Used by skills |
|-------|-----------|----------------|
| TON | ≥ 0.5 | ton-balance, ton-swap, ton-send, cross-skill (gas) |
| USDT | any > 0 | ton-swap (reverse), ton-send (jetton) |

The wallet must be **agentic** with an `operator_private_key`. Without it, write operations (swaps, sends) will fail.

### Bootstrap wallet

If starting from a wallet with only TON (need ≥ 1 TON), just ask Claude:

```
Swap 0.5 TON to USDT
```

Or manually:

```bash
npx @ton/mcp@alpha get_wallet
npx @ton/mcp@alpha get_balance

# TON → USDT
npx @ton/mcp@alpha get_swap_quote --fromToken TON --toToken EQCxE6mUtQJKFnGfaROTKOt1lZbDiiX1kCixRv7Nw2Id_sDs --amount 0.5
npx @ton/mcp@alpha send_raw_transaction --messages '<messages from quote>'

npx @ton/mcp@alpha get_balance
npx @ton/mcp@alpha get_jettons
```

**Key addresses:**

| Token | Jetton master |
|-------|--------------|
| USDT | `EQCxE6mUtQJKFnGfaROTKOt1lZbDiiX1kCixRv7Nw2Id_sDs` |

### Software

```bash
# 1. Claude Code
npm install -g @anthropic-ai/claude-code

# 2. skill-creator plugin
claude plugin install skill-creator

# 3. Clone and install skills
git clone https://github.com/ton-org/skills.git
cd skills
npx skills add . --skill '*' -y

# 4. Python 3.10+ (required for the eval viewer)
brew install python@3.11
```

### MCP servers

MCP servers are configured in **`tests/.mcp.json`** (not the repo root). Claude Code picks them up when launched from the `tests/` directory:

```json
{
  "mcpServers": {
    "ton-mcp": {
      "command": "npx",
      "args": ["-y", "@ton/mcp@alpha"]
    },
    "ton-docs": {
      "type": "http",
      "url": "https://docs.ton.org/mcp"
    }
  }
}
```

Verify they're connected:

```bash
cd tests && claude mcp list
# Should show ton-mcp and ton-docs
```

### Permissions

Evals run via `claude -p` (non-interactive mode), so all tools must be **pre-approved** — there's no user to click "Allow". Add the following to `tests/.claude/settings.local.json` (create if it doesn't exist):

```json
{
  "permissions": {
    "allow": [
      "mcp__ton-mcp__*",
      "mcp__ton-docs__*",
      "Bash(npx @ton/mcp@alpha:*)",
      "Bash(npx -y @ton/mcp@alpha:*)",
      "Bash(curl:*)",
      "WebFetch(*)",
      "WebSearch(*)"
    ]
  }
}
```

Without these, most evals will fail with "needs your approval" because:
- `mcp__ton-mcp__*` / `mcp__ton-docs__*` — allow all MCP tool calls
- `Bash(npx @ton/mcp@alpha:*)` — some skills route through CLI instead of MCP tools
- `Bash(curl:*)` — xStocks evals fetch prices from the xStocks API
- `WebFetch` / `WebSearch` — ton-docs evals may fetch documentation pages

## Benchmark mode

For statistical reliability, run multiple times:

```
/skill-creator benchmark ton-balance --runs 3
```

## Comparing skill versions

After editing a skill:

```
/skill-creator improve ton-swap
```

Compares the current version against the previous best using blind A/B comparison.

## Recording results

The plugin writes temp files (`grading.json`, `benchmark.json`, `benchmark.md`) into `ton-workspace/`. These are gitignored.

After a benchmark run, append a new dated section to `RESULTS.md` manually with:

- Date heading and model
- Per-eval table: eval, group, with-skill time/tokens, without-skill time/tokens, winner
- Aggregate stats: avg time, avg total tokens, pass rate, winner breakdown (Wins / Losses / Ties)

No prose, no assertion tables, no raw JSON in `RESULTS.md`.

## Eval format

```json
{
  "skill_name": "ton-balance",
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

| Field | Required | Description |
|---|---|---|
| `id` | yes | Unique integer |
| `group` | no | Skill group for organizing results |
| `prompt` | yes | The user request to execute |
| `expected_output` | yes | Human-readable description of success |
| `expectations` | yes | List of verifiable assertions for the grader |
| `files` | no | Input files for the eval (relative to skill root) |

### Writing good expectations

- Be specific: "Calls get_balance" not "Uses the right tool"
- Use "or" for alternatives: "Calls get_nfts or get_nfts_by_address"
- Include negative checks: "Does not send a real transaction"
- Cover process (tool calls) and result (response content)

## Adding evals

1. Add a new entry to the appropriate `evals/<skill-name>.json`
2. Also add it to `evals/evals.json` (combined file)
3. Run `/skill-creator evaluate <skill-name>` to test it
4. Review the grading in the viewer — refine expectations if too easy or too strict

## Troubleshooting

| Problem | Fix |
|---|---|
| Subagents can't use MCP tools | `claude mcp list` must show `ton-mcp` and `ton-docs`. Run from `tests/` where `.mcp.json` lives. |
| MCP tools not found | Make sure you launched Claude Code from `tests/`, not the repo root. MCP is configured in `tests/.mcp.json`. |
| "No wallet configured" | `npx @ton/mcp@alpha get_wallet`. Create with `ton-create-wallet` skill if missing. |
| Swap evals fail with "no quote" | Omniston has no route. Retry in a few minutes. |
| Send evals fail | Need ≥ 0.5 TON for gas. Top up. |
| `result.json` is 0 bytes | `claude -p` crashed or timed out. Re-run that eval. |
