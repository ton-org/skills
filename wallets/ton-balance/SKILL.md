---
name: ton-balance
description: Use this skill when checking TON wallet balances, token holdings, or transaction history. It retrieves how much TON or jettons a wallet holds, lists tokens, looks up token details, checks transaction history, and verifies transaction status. Also use when the user asks about their funds, portfolio, or recent activity on TON.
user-invocable: true
disable-model-invocation: false
---

# TON Balance & Transaction Queries

Read-only queries for wallet balances, token holdings, and transaction history on TON.

## MCP Tools

| Tool | Required | Optional |
| ---- | -------- | -------- |
| `get_wallet` | — | `walletSelector` |
| `get_balance` | — | `walletSelector` |
| `get_balance_by_address` | `address` | `walletSelector` |
| `get_jetton_balance` | `jettonAddress` | `walletSelector` |
| `get_jettons` | — | `walletSelector` |
| `get_jettons_by_address` | `address` | `limit`, `offset`, `walletSelector` |
| `get_jetton_info` | `jettonAddress` | `walletSelector` |
| `get_known_jettons` | — | — |
| `get_transactions` | — | `limit`, `walletSelector` |
| `get_transaction_status` | `normalizedHash` | `walletSelector` |

## Workflows

### Check Balance
1. Call `get_wallet` for address and network info
2. Call `get_balance` for TON balance
3. Call `get_jettons` for all token holdings

### Check Specific Token
1. If user mentions a token by name, call `get_known_jettons` to find its address
2. Call `get_jetton_balance` with the `jettonAddress`

### View Transaction History
1. Call `get_transactions` with an optional `limit` (default varies)

### Verify a Sent Transaction
1. Call `get_transaction_status` with the `normalizedHash` returned by a send/swap operation

## Notes

- All tools are read-only — no confirmation needed
- In registry mode, pass `walletSelector` to query a specific wallet instead of the active one
- All balance responses include both `amountRaw` (raw integer string) and `amount` (human-readable decimal string, e.g. `"1.5"` for 1.5 TON). Use `amount` for display; use `amountRaw` only when a raw integer is required.
