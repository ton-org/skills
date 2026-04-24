# Autonomous Testing Plan — TON Skills

## Goal

Run all skill test cases end-to-end autonomously, discovering and recording issues in `ISSUES.md` without waiting for user approval on each step.

`ISSUES.md` is a **temporary** working file for a given run or session: it is not a permanent artifact in the repo. After triage, move findings to your issue tracker or PR and clear or remove the file.

## Setup — reinstall skills before each run

Always reinstall skills from this local repo before running tests. This ensures you are testing the current source, not any globally cached version.

```bash
# From the repo root
npx skills add . --skill '*'
```

## Permissions

All read-only and emulation operations are pre-approved. For write operations:
- **Prefer emulation** — use `emulate_transaction` instead of `send_raw_transaction` wherever possible
- **Only send real transactions** when emulation is not available or the test case explicitly requires on-chain confirmation (e.g. verifying a tx hash via `get_transaction_status`)
- Balance reads, jetton reads, quote fetches, DNS resolution, NFT reads — all pre-approved
- Smart contract reads and validations — pre-approved


## Execution Rules

- **Exhaust all options before marking a case failed.** If a tool call errors, try alternative tools, different parameters, or fallback approaches. Each retry counts toward the actions and errors totals.
- Only mark a case `✗` after genuinely exhausting all reasonable paths. The goal is to complete the task, not to skip it.
- **BLOCKED** is only valid when a case is structurally impossible without a prior result (e.g. B5 needs a tx hash from B4) and no workaround exists.
- Complete all groups regardless of failure count — a failed case never stops the run.

## Issue Recording Format

When a new issue is found, append to `ISSUES.md` under the relevant section:

```
### [CATEGORY-N] Short title
**Affects:** skill name
**Tools:** tool names
**Description:** what went wrong
**Fix:** proposed fix
**Status:** discovered / documented / fixed
**Triggered by:** exact request or tool call that exposed the issue
```

## Test Cases

### GROUP 1: ton-balance
- [ ] B1 — "How much TON do I have?"
- [ ] B2 — "Show me all the tokens in my wallet"
- [ ] B3 — "What's my USDT balance?"
- [ ] B4 — "Show me my last 5 transactions"
- [ ] B5 — "What is the status of my most recent transaction?"

### GROUP 2: ton-manage-wallets
- [ ] W1 — "List all my wallets"
- [ ] W2 — "Which wallet am I currently using?"
- [ ] W3 — "Is my active wallet properly set up and valid?"

### GROUP 3: ton-swap
- [ ] S1 — "How much USDT would I get for 1 TON right now?"
- [ ] S2 — "Swap 0.5 TON to USDT"
- [ ] S3 — "Swap USDT back to TON" *(if balance allows)*
- [ ] S4 — "Get me a quote for swapping an unknown token address" *(expect graceful error)*

### GROUP 4: ton-xstocks
- [ ] X1 — "How much is the NVDAx stock token worth in USDT?"
- [ ] X2 — "Show me my NVDAx balance"
- [ ] X3 — "Buy a small amount of TSLAx"
- [ ] X4 — "Sell my NVDAx" *(if balance allows)*

### GROUP 5: ton-send
- [ ] T1 — "What address does foundation.ton resolve to?"
- [ ] T2 — "Send 0.01 TON to my own address"
- [ ] T3 — "Send a small amount of USDT to myself"

### GROUP 6: ton-nfts
- [ ] N1 — "Do I own any NFTs?"
- [ ] N2 — "Show me details for a specific NFT" *(pick address from N2 results)*
- [ ] N3 — "What's new in the NFT feed right now?"

### GROUP 7: ton-docs
- [ ] D1 — "How does jetton transfer work on TON?"
- [ ] D2 — "Explain the TON wallet contract standard"
- [ ] D3 — "What is the NFT standard on TON and how does ownership transfer?"

### GROUP 8: ton-cli
- [ ] C1 — "Check my TON balance via CLI"
- [ ] C2 — "List my wallets via CLI"
- [ ] C3 — "Show my recent transactions via CLI"

### GROUP 9: cross-skill
*These cases intentionally span multiple skills. Note which skills were invoked in `ISSUES.md` if relevant.*

- [ ] M1 — "What's my total portfolio value in USD?" *(ton-balance + ton-xstocks: balance + current token prices)*
- [ ] M2 — "Buy NVDAx worth 1 USDT, paying with TON" *(ton-balance + ton-swap + ton-xstocks)*
- [ ] M3 — "How much TON do I need to buy 1 TSLAx at current prices?" *(ton-xstocks + ton-swap: quote only, no execution)*
- [ ] M4 — "Check my balance, then show me what NFTs I own and look one up on GetGems" *(ton-balance + ton-nfts)*

## Issue Severity

- **P0** — blocks the skill entirely
- **P1** — wrong result or silent failure
- **P2** — missing documentation / unclear behavior
- **P3** — nice-to-have improvement

## Metrics

Track the following counters for every test run. Update `RESULTS.md` after completing all groups.

| Metric | Definition |
|---|---|
| **Errors** | Total number of tool calls that returned an error (MCP errors, validation failures, unexpected exceptions) |
| **Actions** | Total number of tool calls made across all groups (each MCP tool invocation counts as 1) |
| **Context** | Estimated context window consumed, in tokens (approximate, e.g. `~42 000 tok`) |

### How to count

- **Errors** — increment for every response that contains `MCP error`, `error:`, `failed`, or a non-success status. Count each failed tool call once.
- **Actions** — increment for every tool call dispatched, regardless of outcome (success or error).
- **Context** — after all groups are done, estimate from the conversation length. Use `~N k tok` format.
- **Notes column is removed** — all findings go to `ISSUES.md`, not the results table.

### Δ (delta) vs previous run

After filling in the numbers, compute the percentage change vs the previous run for each metric:

```
Δ = round((current - previous) / previous * 100)  →  "+N %" / "-N %"
```

**Tolerance rule — deviations within ±5 % are normal and expected.**  
Agents are non-deterministic: they may retry a step, pick a slightly different tool order, or produce a longer/shorter response. A swing of a few percent in actions or context does **not** indicate a regression. Only flag deviations that exceed ±5 % as worth investigating.

## Output

After each group:
1. Update `ISSUES.md` with any new findings.
2. Fill in the per-test-case rows for that group in `RESULTS.md` (status, errors, actions — no Notes column).
3. Fill in the group totals row in `RESULTS.md`.

After all groups are done:
4. Compute Δ columns for errors, actions, and context vs the previous run.
5. Add a new row to the **Summary Table** in `RESULTS.md`.
6. If any metric differs from the previous run by more than ±5 %, note it in `ISSUES.md` or a short comment in the PR for further review.

### RESULTS.md format

The **PR** column is the PR title and number, e.g. `#15`. If the current HEAD is not tied to a PR, use the branch name in that column instead.

Then add or update the entry in `RESULTS.md` following the template in that file.
