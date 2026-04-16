# Test Results

## Summary

| Run | PR / Branch | Date | Errors | ΔErrors | Actions | ΔActions | Context | ΔContext |
|-----|-------------|------|--------|---------|---------|----------|---------|----------|
| general: add tests (#31) | 30-general-add-tests | 2026-04-16 | 14 | — | 62 | — | ~55 k tok | — |


## general: add tests (#31)

### GROUP 1: ton-balance

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| B1 | How much TON do I have? | ✓ | 0 | 2 |
| B2 | Show me all the tokens in my wallet | ✓ | 0 | 1 |
| B3 | What's my USDT balance? | ✓ | 0 | 2 |
| B4 | Show me my last 5 transactions | ✓ | 1 | 2 |
| B5 | What is the status of my most recent transaction? | ✓ | 0 | 1 |
| **G1 total** | | | 1 | 8 |

### GROUP 2: ton-manage-wallets

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| W1 | List all my wallets | ✓ | 0 | 1 |
| W2 | Which wallet am I currently using? | ✓ | 0 | 1 |
| W3 | Is my active wallet properly set up and valid? | ✓ | 0 | 2 |
| **G2 total** | | | 0 | 4 |

### GROUP 3: ton-swap

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| S1 | How much USDT would I get for 1 TON right now? | ✓ | 0 | 1 |
| S2 | Swap 0.5 TON to USDT | ✓ | 1 | 3 |
| S3 | Swap USDT back to TON | ✓ | 0 | 1 |
| S4 | Quote for unknown token address | ✓ | 1 | 1 |
| **G3 total** | | | 2 | 6 |

### GROUP 4: ton-xstocks

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| X1 | How much is the NVDAx stock token worth in USDT? | ✓ | 0 | 2 |
| X2 | Show me my NVDAx balance | ✓ | 0 | 1 |
| X3 | Buy a small amount of TSLAx | ✓ | 0 | 3 |
| X4 | Sell my NVDAx | ✓ | 0 | 1 |
| **G4 total** | | | 0 | 7 |

### GROUP 5: ton-send

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| T1 | What address does foundation.ton resolve to? | ✓ | 0 | 1 |
| T2 | Send 0.01 TON to my own address | ✓ | 0 | 2 |
| T3 | Send a small amount of USDT to myself | ✓ | 1 | 3 |
| **G5 total** | | | 1 | 6 |

### GROUP 6: ton-nfts

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| N1 | Do I own any NFTs? | ✓ | 0 | 1 |
| N2 | Show me details for a specific NFT | BLOCKED | 3 | 4 |
| N3 | What's new in the NFT feed right now? | ✗ | 3 | 3 |
| **G6 total** | | | 6 | 8 |

### GROUP 7: ton-docs

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| D1 | How does jetton transfer work on TON? | ✓ | 0 | 2 |
| D2 | Explain the TON wallet contract standard | ✓ | 0 | 4 |
| D3 | What is the NFT standard on TON and how does ownership transfer? | ✓ | 0 | 2 |
| **G7 total** | | | 0 | 8 |

### GROUP 8: ton-cli

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| C1 | Check my TON balance via CLI | ✓ | 0 | 1 |
| C2 | List my wallets via CLI | ✓ | 0 | 1 |
| C3 | Show my recent transactions via CLI | ✓ | 1 | 2 |
| **G8 total** | | | 1 | 4 |

### GROUP 9: cross-skill

| Case | Description | Status | Errors | Actions |
|------|-------------|--------|--------|---------|
| M1 | What's my total portfolio value in USD? | ✓ | 0 | 3 |
| M2 | Buy NVDAx worth 1 USDT, paying with TON | ✓ | 0 | 2 |
| M3 | How much TON do I need to buy 1 TSLAx? | ✓ | 0 | 2 |
| M4 | Balance + NFTs + GetGems lookup | ✓ | 2 | 4 |
| **G9 total** | | | 2 | 11 |

