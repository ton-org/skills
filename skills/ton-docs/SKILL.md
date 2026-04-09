---
name: ton-docs
description: >
  Searches and references official TON blockchain documentation, standards (TEPs), and SDK guides
  via the TON Docs MCP server. Use this skill when answering questions about TON architecture,
  smart contracts, FunC, Tact, Tolk, TL-B schemas, validators, sharding, message routing, wallet
  contracts, jetton standard (TEP-74), NFT standard (TEP-62), TON addresses, BOC encoding, or any
  TON protocol details.
license: MIT
compatibility: Requires the TON Docs MCP server (https://docs.ton.org/mcp)
metadata:
  author: ton-tech
  version: "0.2.0"
---

# TON Documentation

Search and reference the official TON blockchain documentation via the TON Docs MCP server.

## MCP Server Setup

The TON Docs MCP server uses a remote HTTP transport — no local process to install.

Add to your MCP settings:

```json
{
  "mcpServers": {
    "ton-docs": {
      "type": "http",
      "url": "https://docs.ton.org/mcp"
    }
  }
}
```

## MCP Tools

| Tool | Required Params | Description |
| ---- | --------------- | ----------- |
| `search_ton_docs` | `query` | Search across the TON Docs knowledge base for relevant content, code examples, API references, and guides |
| `get_page_ton_docs` | `page` | Retrieve the full content of a specific documentation page by its path (e.g., from search results) |

## When to Use

- User asks about TON blockchain concepts (workchains, sharding, validators, masterchain)
- User asks about smart contract development (FunC, Tact, Tolk, Fift, TL-B schemas)
- User asks about TON standards (TEP, jetton standard TEP-74, NFT standard TEP-62)
- User asks about wallet contracts, message formats, or internal/external messages
- User asks about TON addresses, BOC encoding, cell serialization
- User asks about DeFi protocols, DEX, or token mechanics on TON
- User needs code examples for TON smart contracts or SDK usage
- User asks about TON APIs (TonCenter v2/v3), node operations, or infrastructure

## Workflow

1. Identify the topic and its sub-aspects from the user's question
2. Run multiple `search_ton_docs` calls with different targeted queries to cover all angles (e.g. one broad query + one SDK/tooling-specific query)
3. From the combined search results, identify the 2–4 most relevant page paths
4. Call `get_page_ton_docs` on those pages **in parallel** to retrieve full content efficiently
5. If the pages reference other relevant pages (e.g. a "See also" section), fetch those too
6. Synthesize the answer from all fetched pages, include code examples, and link to sources (prefix paths with `https://docs.ton.org/`)

## Gotchas

- Search returns short snippets — always follow up with `get_page_ton_docs` to get full page content including code examples
- Page paths from search results can be passed directly to `get_page_ton_docs` without modification
- A good answer typically requires fetching 3–5 pages; a single page rarely covers the full picture
- For smart contract questions, determine whether the user is working with FunC, Tact, or Tolk before providing examples
- For SDK questions, determine the language (TypeScript/JavaScript via `@ton/ton`, Python via `pytoniq`, etc.)
- TON docs are regularly updated — always search for fresh content rather than relying on cached knowledge

## TEPs (TON Enhancement Proposals)

For protocol standards not covered by the docs site, TEPs are on GitHub:

| TEP | Topic | URL |
| --- | ----- | --- |
| TEP-62 | NFT Standard | `https://github.com/ton-blockchain/TEPs/blob/master/text/0062-nft-standard.md` |
| TEP-64 | Token Data Standard | `https://github.com/ton-blockchain/TEPs/blob/master/text/0064-token-data-standard.md` |
| TEP-74 | Jetton Standard | `https://github.com/ton-blockchain/TEPs/blob/master/text/0074-jettons-standard.md` |
| TEP-81 | DNS Standard | `https://github.com/ton-blockchain/TEPs/blob/master/text/0081-dns-standard.md` |
| All TEPs | Index | `https://github.com/ton-blockchain/TEPs/blob/master/README.md` |
