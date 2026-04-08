---
name: agentic-wallets
description: >
  Manages TON blockchain agentic wallet operations — balances, transfers, token swaps, NFTs, wallet creation and lifecycle management.
  Use this skill when interacting with TON wallets, sending TON or jettons, swapping tokens on DEX, managing NFTs, creating or importing
  agentic wallets, or resolving TON DNS domains, even if the user doesn't explicitly mention "TON" or "agentic wallet."
license: MIT
metadata:
  author: ton-connect
  version: "0.1.0"
---

# TON Agentic Wallets

Self-custody wallets for autonomous agents on the TON blockchain. Provides MCP tools for balance queries, transfers, token swaps, NFT operations, and wallet lifecycle management.

## When to Use

- User wants to check TON wallet balances or transaction history
- User wants to send TON, jettons (tokens), or NFTs
- User wants to swap/trade tokens on TON DEX
- User wants to create, import, or manage agentic wallets
- User wants to interact with TON DNS domains
- User wants to buy/sell xStocks (tokenized equities) on TON

## Sub-Skills

| Skill | Description |
| ----- | ----------- |
| [ton-balance](ton-balance/SKILL.md) | Balance queries, token holdings, transaction history |
| [ton-send](ton-send/SKILL.md) | Send TON & jettons to addresses or DNS domains |
| [ton-swap](ton-swap/SKILL.md) | Token swaps via DEX aggregator |
| [ton-nfts](ton-nfts/SKILL.md) | View and transfer NFTs |
| [ton-create-wallet](ton-create-wallet/SKILL.md) | Create & deploy agentic wallets |
| [ton-manage-wallets](ton-manage-wallets/SKILL.md) | Switch wallets, import, rotate keys |
| [ton-cli](ton-cli/SKILL.md) | Raw CLI mode for direct tool invocation |
| [ton-xstocks](ton-xstocks/SKILL.md) | Buy/sell Backed xStocks (tokenized equities) |

## Setup

### Install Skills

```bash
npx skills add ton-connect/ton-agent-skills
```

### MCP Server Configuration

```json
{
  "mcpServers": {
    "ton": {
      "command": "npx",
      "args": ["-y", "@ton/mcp@alpha"]
    }
  }
}
```

See the [@ton/mcp README](https://github.com/ton-connect/kit/tree/main/packages/mcp#readme) for full configuration options including single-wallet mode, HTTP mode, and serverless deployment.
