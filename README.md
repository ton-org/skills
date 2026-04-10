# TON Agent Skills

A collection of packaged [agent skills](https://agentskills.io/) for the TON blockchain. Gives AI coding agents domain-specific knowledge and tools for wallet operations, token management, and TON documentation.

## Available Skills

| Skill | Description |
| ----- | ----------- |
| [agentic-wallets](agentic-wallets/) | TON wallet operations — balances, transfers, swaps, NFTs, agentic wallet creation and management via [@ton/mcp](https://github.com/ton-connect/kit/tree/main/packages/mcp) |
| [ton-docs](ton-docs/) | Search and reference official TON blockchain documentation, standards (TEPs), and SDK guides |

## Installation

### Claude Code

```bash
npx skills add ton-org/skills
```

### Manual

Copy the desired skill directory into your agent's skills folder:

```bash
cp -r agentic-wallets ~/.claude/skills/
cp -r ton-docs ~/.claude/skills/
```

Or add a SKILL.md to your project knowledge on claude.ai.

## Skill Structure

Each skill follows the [Agent Skills](https://agentskills.io/) format:

```
{skill-name}/
├── SKILL.md          # Agent instructions (frontmatter + markdown)
├── metadata.json     # Skill metadata (version, org, references)
└── {sub-skill}/      # Optional sub-skills
    └── SKILL.md
```

## Contributing

See [AGENTS.md](AGENTS.md) for guidelines on creating and modifying skills.

## License

MIT
