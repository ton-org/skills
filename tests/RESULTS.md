# Test Results

## 2026-05-05 — cross-skill, ton-balance, ton-cli, ton-docs, ton-manage-wallets, ton-nfts, ton-send, ton-swap, ton-xstocks

**Model:** claude-sonnet-4-6 | **Evals:** 32 | **Configurations:** with_skill vs without_skill (baseline)

### Summary

| # | Eval | Group | With Skill | Without Skill | Winner |
|---|------|-------|-----------|--------------|--------|
| 1 | Balance check | ton-balance | 21.2s, 13K tok | 14.7s, 11K tok | **Baseline** |
| 2 | All tokens | ton-balance | 26.2s, 13K tok | 21.7s, 12K tok | Tie |
| 3 | USDT balance | ton-balance | 24.9s, 13K tok | 22.1s, 12K tok | Tie |
| 4 | Last 5 transactions | ton-balance | 37.3s, 15K tok | 44.7s, 14K tok | Tie |
| 5 | Transaction status | ton-balance | 29.9s, 13K tok | 25.8s, 12K tok | Tie |
| 6 | List wallets | ton-manage-wallets | 20.5s, 13K tok | 16.9s, 11K tok | Tie |
| 7 | Active wallet | ton-manage-wallets | 17.0s, 13K tok | 18.0s, 11K tok | Tie |
| 8 | Validate wallet | ton-manage-wallets | 30.8s, 14K tok | 29.6s, 12K tok | Tie |
| 9 | Swap quote | ton-swap | 26.0s, 13K tok | 19.6s, 12K tok | **Baseline** |
| 10 | Execute swap | ton-swap | 101.7s, 21K tok | 41.1s, 15K tok | **Skill** |
| 11 | Reverse swap | ton-swap | 122.6s, 24K tok | 49.7s, 15K tok | **Skill** |
| 12 | Swap error handling | ton-swap | 31.5s, 12K tok | 19.0s, 11K tok | **Baseline** |
| 13 | xStocks price | ton-xstocks | 43.1s, 18K tok | 92.0s, 19K tok | Tie |
| 14 | xStocks balance | ton-xstocks | 29.5s, 15K tok | 29.4s, 13K tok | Tie |
| 15 | Buy xStock | ton-xstocks | 149.0s, 28K tok | 279.8s, 37K tok | **Skill** |
| 16 | Sell xStock | ton-xstocks | 33.7s, 17K tok | 37.8s, 16K tok | Tie |
| 17 | DNS resolve | ton-send | 23.1s, 12K tok | 15.8s, 11K tok | Tie |
| 18 | Send TON | ton-send | 36.1s, 14K tok | 67.0s, 15K tok | **Skill** |
| 19 | Send jetton | ton-send | 39.0s, 15K tok | 36.6s, 14K tok | Tie |
| 20 | List NFTs | ton-nfts | 20.1s, 12K tok | 16.5s, 11K tok | Tie |
| 21 | NFT details | ton-nfts | 20.8s, 12K tok | 18.0s, 12K tok | Tie |
| 22 | NFT feed | ton-nfts | 20.5s, 12K tok | 25.3s, 12K tok | Tie |
| 23 | Jetton transfer docs | ton-docs | 87.2s, 30K tok | 110.8s, 30K tok | Tie |
| 24 | Wallet contract docs | ton-docs | 204.0s, 34K tok | 105.6s, 27K tok | **Baseline** |
| 25 | NFT standard docs | ton-docs | 59.7s, 28K tok | 66.2s, 29K tok | Tie |
| 26 | CLI balance | ton-cli | 23.8s, 14K tok | 15.6s, 12K tok | **Baseline** |
| 27 | CLI wallets | ton-cli | 26.8s, 14K tok | 16.1s, 11K tok | **Baseline** |
| 28 | CLI transactions | ton-cli | 77.9s, 21K tok | 94.5s, 24K tok | Tie |
| 29 | Portfolio value | cross-skill | 37.6s, 17K tok | 39.2s, 16K tok | Tie |
| 30 | Buy xStock with TON | cross-skill | 189.5s, 34K tok | 189.2s, 29K tok | Tie |
| 31 | xStock cost in TON | cross-skill | 61.5s, 21K tok | 137.8s, 19K tok | **Skill** |
| 32 | Balance + NFTs + GetGems | cross-skill | 24.0s, 13K tok | 25.3s, 12K tok | Tie |

### Aggregate Stats

|  | With Skill | Without Skill | Delta |
|--|-----------|--------------|-------|
| **Avg time** | 53.0s | 54.4s | -3% |
| **Avg total tokens** | 17K | 16K | +8% |
| **Pass rate** | 98% | 95% | +3% |
| **Wins / Losses / Ties** | 5 / 6 / 21 | — | — |
