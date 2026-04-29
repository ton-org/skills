# Test Results

## 32

**Model:** claude-opus-4-6 | **Evals:** 32 | **Configurations:** with_skill vs without_skill (baseline)

### Summary

| # | Eval | Group | With Skill | Without Skill | Winner |
|---|------|-------|-----------|--------------|--------|
| 1 | Balance check | ton-balance | 29s, 13K tok | 13s, 10K tok | Tie |
| 2 | All tokens | ton-balance | 28s, 14K tok | 20s, 12K tok | Tie |
| 3 | USDT balance | ton-balance | 31s, 14K tok | 22s, 11K tok | Tie |
| 4 | Last 5 transactions | ton-balance | 31s, 15K tok | 24s, 14K tok | Tie |
| 5 | Transaction status | ton-balance | 27s, 13K tok | 21s, 12K tok | Tie |
| 6 | List wallets | ton-manage-wallets | 25s, 13K tok | 18s, 11K tok | Tie |
| 7 | Active wallet | ton-manage-wallets | 22s, 13K tok | 14s, 11K tok | Tie |
| 8 | Validate wallet | ton-manage-wallets | 34s, 14K tok | 30s, 12K tok | Tie |
| 9 | Swap quote | ton-swap | 31s, 14K tok | 24s, 11K tok | Tie |
| 10 | Execute swap | ton-swap | 49s, 17K tok | 29s, 13K tok | **Skill** |
| 11 | Reverse swap | ton-swap | 53s, 19K tok | 43s, 14K tok | **Skill** |
| 12 | Swap error handling | ton-swap | 38s, 13K tok | 41s, 11K tok | Tie |
| 13 | xStocks price | ton-xstocks | 55s, 22K tok | 68s, 20K tok | Tie |
| 14 | xStocks balance | ton-xstocks | 35s, 13K tok | 15s, 11K tok | Tie |
| 15 | Buy xStock | ton-xstocks | 99s, 26K tok | 95s, 27K tok | Tie |
| 16 | Sell xStock | ton-xstocks | 82s, 32K tok | 57s, 21K tok | Tie |
| 17 | DNS resolve | ton-send | 21s, 12K tok | 18s, 10K tok | Tie |
| 18 | Send TON | ton-send | 37s, 15K tok | 30s, 12K tok | Tie |
| 19 | Send jetton | ton-send | 65s, 16K tok | 84s, 17K tok | **Skill** |
| 20 | List NFTs | ton-nfts | 20s, 12K tok | 14s, 11K tok | Tie |
| 21 | NFT details | ton-nfts | 66s, 15K tok | 29s, 12K tok | Tie |
| 22 | NFT feed | ton-nfts | 27s, 12K tok | 85s, 20K tok | **Baseline** |
| 23 | Jetton transfer docs | ton-docs | 97s, 35K tok | 97s, 33K tok | Tie |
| 24 | Wallet contract docs | ton-docs | 101s, 40K tok | 83s, 26K tok | **Skill** |
| 25 | NFT standard docs | ton-docs | 80s, 34K tok | 79s, 31K tok | Tie |
| 26 | CLI balance | ton-cli | 29s, 15K tok | 19s, 11K tok | Tie |
| 27 | CLI wallets | ton-cli | 35s, 15K tok | 17s, 11K tok | Tie |
| 28 | CLI transactions | ton-cli | 40s, 21K tok | 36s, 20K tok | Tie |
| 29 | Portfolio value | cross-skill | 42s, 17K tok | 37s, 15K tok | Tie |
| 30 | Buy xStock with TON | cross-skill | 116s, 28K tok | 105s, 28K tok | Tie |
| 31 | xStock cost in TON | cross-skill | 72s, 22K tok | 122s, 31K tok | Tie |
| 32 | Balance + NFTs + GetGems | cross-skill | 30s, 15K tok | 36s, 13K tok | Tie |

### Aggregate Stats

|  | With Skill | Without Skill | Delta |
|--|-----------|--------------|-------|
| **Avg time** | 47.7s | 42.1s | +13% |
| **Avg tokens** | 17.6K | 15.6K | +13% |
| **Avg tool calls** | 9.8 | 8.4 | +17% |
| **Wins / Losses / Ties** | 4 / 1 / 27 | — | — |
