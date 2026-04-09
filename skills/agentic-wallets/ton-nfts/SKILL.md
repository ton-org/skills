---
name: ton-nfts
description: Use this skill when viewing or transferring NFTs on the TON blockchain. It lists collectibles, checks NFT details, and sends NFTs to other addresses. Also use when the user mentions collectibles, digital assets, NFT collections, or wants to send an NFT to someone on TON.
user-invocable: true
disable-model-invocation: false
---

# TON NFT Operations

View and transfer NFTs on the TON blockchain.

## MCP Tools

| Tool | Required | Optional |
| ---- | -------- | -------- |
| `get_nfts` | — | `limit`, `offset`, `walletSelector` |
| `get_nfts_by_address` | `address` | `limit`, `offset` |
| `get_nft` | `nftAddress` | — |
| `send_nft` | `nftAddress`, `toAddress` | `comment`, `walletSelector` |
| `emulate_transaction` | `messages` | `validUntil` |

## Workflows

### List My NFTs
1. Call `get_nfts` to list NFTs in the active wallet
2. Use `limit` and `offset` for pagination

### View NFT Details
1. Call `get_nft` with the `nftAddress`

### Send an NFT
1. Call `get_nfts` to find the NFT address if the user doesn't have it
2. Ask one short yes/no confirmation before transferring the NFT
3. Call `send_nft` with `nftAddress` and `toAddress`
4. Poll `get_transaction_status` with the returned `normalizedHash` until status is `completed` or `failed` (see `ton-balance` skill)

## Notes

- Use `emulate_transaction` to dry-run any transaction before sending — it returns expected balance changes, fees, and high-level actions so you can verify the outcome
- Always confirm with the user before transferring an NFT; prefer the host client's structured confirmation UI when available, otherwise accept natural-language yes/no and do not require a fixed confirmation phrase
- If no wallet is configured, use the `ton-create-wallet` skill first
