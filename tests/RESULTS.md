# Test Results

## Summary

| PR | Date | Errors | ΔErrors | Actions | ΔActions | Context | ΔContext |
|----|------|--------|---------|---------|----------|---------|----------|
| #31 | 2026-04-16 | 13 | — | 62 | — | ~47 k tok | — |


## general: add tests (#31)

### GROUP 1: ton-balance

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| B1 | How much TON do I have? | ✓ | 0 | 2 | ~800 |
| B2 | Show me all the tokens in my wallet | ✓ | 0 | 1 | ~700 |
| B3 | What's my USDT balance? | ✓ | 0 | 2 | ~800 |
| B4 | Show me my last 5 transactions | ✓ | 1 | 2 | ~4.2k |
| B5 | What is the status of my most recent transaction? | ✓ | 0 | 1 | ~600 |
| **G1 total** | | | 1 | 8 | ~7.1k |

### GROUP 2: ton-manage-wallets

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| W1 | List all my wallets | ✓ | 0 | 1 | ~800 |
| W2 | Which wallet am I currently using? | ✓ | 0 | 1 | ~500 |
| W3 | Is my active wallet properly set up and valid? | ✓ | 0 | 2 | ~700 |
| **G2 total** | | | 0 | 4 | ~2.0k |

### GROUP 3: ton-swap

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| S1 | How much USDT would I get for 1 TON right now? | ✓ | 0 | 1 | ~900 |
| S2 | Swap 0.5 TON to USDT | ✓ | 1 | 3 | ~3.0k |
| S3 | Swap USDT back to TON | ✓ | 0 | 1 | ~900 |
| S4 | Quote for unknown token address | ✓ | 1 | 1 | ~1.8k |
| **G3 total** | | | 2 | 6 | ~6.6k |

### GROUP 4: ton-xstocks

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| X1 | How much is the NVDAx stock token worth in USDT? | ✓ | 0 | 2 | ~1.1k |
| X2 | Show me my NVDAx balance | ✓ | 0 | 1 | ~500 |
| X3 | Buy a small amount of TSLAx | ✓ | 0 | 3 | ~2.3k |
| X4 | Sell my NVDAx | ✓ | 0 | 1 | ~1.8k |
| **G4 total** | | | 0 | 7 | ~5.7k |

### GROUP 5: ton-send

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| T1 | What address does foundation.ton resolve to? | ✓ | 0 | 1 | ~500 |
| T2 | Send 0.01 TON to my own address | ✓ | 0 | 2 | ~700 |
| T3 | Send a small amount of USDT to myself | ✓ | 1 | 3 | ~1.2k |
| **G5 total** | | | 1 | 6 | ~2.4k |

### GROUP 6: ton-nfts

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| N1 | Do I own any NFTs? | ✓ | 0 | 1 | ~500 |
| N2 | Show me details for a specific NFT | BLOCKED | 3 | 4 | ~1.4k |
| N3 | What's new in the NFT feed right now? | ✗ | 3 | 3 | ~1.0k |
| **G6 total** | | | 6 | 8 | ~2.9k |

### GROUP 7: ton-docs

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| D1 | How does jetton transfer work on TON? | ✓ | 0 | 2 | ~2.5k |
| D2 | Explain the TON wallet contract standard | ✓ | 0 | 4 | ~4.0k |
| D3 | What is the NFT standard on TON and how does ownership transfer? | ✓ | 0 | 2 | ~2.0k |
| **G7 total** | | | 0 | 8 | ~8.5k |

### GROUP 8: ton-cli

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| C1 | Check my TON balance via CLI | ✓ | 0 | 1 | ~500 |
| C2 | List my wallets via CLI | ✓ | 0 | 1 | ~600 |
| C3 | Show my recent transactions via CLI | ✓ | 1 | 2 | ~4.0k |
| **G8 total** | | | 1 | 4 | ~5.1k |

### GROUP 9: cross-skill

| Case | Description | Status | Errors | Actions | Tokens |
|------|-------------|--------|--------|---------|--------|
| M1 | What's my total portfolio value in USD? | ✓ | 0 | 3 | ~2.0k |
| M2 | Buy NVDAx worth 1 USDT, paying with TON | ✓ | 0 | 2 | ~2.0k |
| M3 | How much TON do I need to buy 1 TSLAx? | ✓ | 0 | 2 | ~1.5k |
| M4 | Balance + NFTs + GetGems lookup | ✓ | 2 | 4 | ~1.5k |
| **G9 total** | | | 2 | 11 | ~7.0k |
