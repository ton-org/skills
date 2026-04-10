# TON Agent Skills

A collection of packaged [agent skills](https://agentskills.io/) for the TON blockchain. They give AI coding agents domain-specific instructions and workflows for wallet operations, DeFi, NFTs, and official TON documentation.

## Repository layout

Skills are grouped under **top-level folders** (`wallets/`, `docs/`). Each skill is a **second-level** directory with its own `SKILL.md`:

```text
wallets/
├── ton-balance/
├── ton-cli/
├── …
docs/
└── ton-docs/
```

Do not nest skills inside another skill directory; see [CONTRIBUTE.md](CONTRIBUTE.md). Optional `metadata.json` may live next to a group (e.g. `wallets/metadata.json`).

## Available skills

### `wallets/` — TON MCP & agentic wallets ([@ton/mcp](https://github.com/ton-connect/kit/tree/main/packages/mcp))

| Skill | Description |
| ----- | ----------- |
| [wallets/ton-balance](wallets/ton-balance/) | TON and jetton balances, token lists, transaction history, transaction status |
| [wallets/ton-cli](wallets/ton-cli/) | Run TON MCP tools from the CLI via `npx @ton/mcp@alpha …` |
| [wallets/ton-create-wallet](wallets/ton-create-wallet/) | Create and deploy a TON agentic wallet (operator keys, on-chain deploy) |
| [wallets/ton-manage-wallets](wallets/ton-manage-wallets/) | Multiple wallets, import, switch accounts, rotate operator keys |
| [wallets/ton-nfts](wallets/ton-nfts/) | List, inspect, and transfer NFTs on TON |
| [wallets/ton-send](wallets/ton-send/) | Send TON and jettons to addresses or TON DNS (`.ton`, `.t.me`) |
| [wallets/ton-swap](wallets/ton-swap/) | Swap and trade jettons on TON DEX (e.g. Omniston) |
| [wallets/ton-xstocks](wallets/ton-xstocks/) | Buy and sell Backed xStocks (tokenized equities) on TON |

### `docs/` — documentation & standards

| Skill | Description |
| ----- | ----------- |
| [docs/ton-docs](docs/ton-docs/) | Official TON documentation, TEPs, and SDK topics via the [TON Docs MCP](https://docs.ton.org/mcp) server |

## Installation

### Skills CLI

Install from this repository (pick agents and skills as needed):

```bash
npx skills add ton-org/skills --list

# Examples: install everything, or only selected skills
npx skills add ton-org/skills --skill '*'
npx skills add ton-org/skills --skill ton-docs --skill ton-send
```

For a **local clone**, point at the repo root or a group folder; nested skills are only all discovered if you use full depth (see [CONTRIBUTE.md](CONTRIBUTE.md)):

```bash
npx skills add ./wallets
npx skills add ./docs
```

### Manual

Copy the skill directory you need into your agent’s skills folder (paths shown in the table above), for example:

```bash
cp -r wallets/ton-send ~/.claude/skills/
cp -r docs/ton-docs ~/.claude/skills/
```

## Skill format

Each skill follows the [Agent Skills](https://agentskills.io/) layout. Inside `<group>/<skill-name>/`:

```text
<group>/<skill-name>/
├── SKILL.md          # Required: frontmatter (name, description, …) + instructions
├── metadata.json     # Optional: version, org, references (group-level file is optional)
├── scripts/          # Optional
├── references/       # Optional
└── assets/           # Optional
```

Authoring rules and quality checklist: [AGENTS.md](AGENTS.md).

## Contributing

- Layout and nesting: [CONTRIBUTE.md](CONTRIBUTE.md)
- Writing and maintaining skills: [AGENTS.md](AGENTS.md)

## License

MIT
