# Agent Guidelines for TON Agent Skills

Guidelines for creating and maintaining skills in this repository. Based on the [Agent Skills specification](https://agentskills.io/specification) and [best practices](https://agentskills.io/skill-creation/best-practices).

For a map of published skills and install commands, see [README.md](README.md). For **where** skills may live in this repo (group folders, second level, no nesting) and what to update when adding a new skill, see [CONTRIBUTE.md](CONTRIBUTE.md). For **end-to-end evaluations** (setup, commands, eval format, results), see [tests/TESTING.md](tests/TESTING.md).

## Repository layout

In this repository, every installable skill lives **two levels below the root**:

```text
<group>/<skill-name>/
```

- **`<group>`** — a top-level folder that groups related skills (e.g. `wallets` for [@ton/mcp](https://github.com/ton-connect/kit/tree/main/packages/mcp) wallet workflows, `docs` for documentation-oriented skills). It is not a skill by itself.
- **`<skill-name>`** — the skill directory; must contain `SKILL.md`. The directory name must match the `name` field in frontmatter (`kebab-case`).

Optional **`metadata.json`** may exist at the **group** level (e.g. `wallets/metadata.json`) for shared metadata; per-skill extras stay inside `<skill-name>/`.

**Do not** place one skill inside another skill’s directory (e.g. `wallets/ton-send/other-skill/SKILL.md`). Tools such as `npx skills add` will not discover nested skills by default. If you need another skill, add a **sibling** directory at `<group>/<new-skill-name>/`.

## Skill Structure

A skill is a directory containing, at minimum, a `SKILL.md` file:

```
<group>/<skill-name>/
├── SKILL.md          # Required: metadata + instructions
├── metadata.json     # Optional: version, organization, references (per-skill)
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
└── assets/           # Optional: templates, resources
```

Use **`references/`**, **`scripts/`**, and **`assets/`** for progressive disclosure. Do not add a second full skill (another `SKILL.md`) nested under the same `<skill-name>` folder.

## SKILL.md Format

### Frontmatter

```yaml
---
name: {kebab-case-name}
description: {What the skill does and when to use it. Imperative phrasing, keyword-rich.}
license: MIT
compatibility: Requires @ton/mcp MCP server
metadata:
  author: ton-org
  version: "x.y.z"
---
```

On disk the skill lives at paths like `wallets/ton-balance/` or `docs/ton-docs/`; the `name` field must match the **skill** directory (`ton-balance`, `ton-docs`), not the group folder.

| Field | Required | Constraints |
| ----- | -------- | ----------- |
| `name` | Yes | Max 64 chars. Lowercase letters, numbers, hyphens only. Must match the **skill** directory name (`<skill-name>` in `<group>/<skill-name>/`). No leading/trailing/consecutive hyphens. |
| `description` | Yes | Max 1024 chars. Non-empty. Describes what the skill does and when to use it. |
| `license` | No | License name or reference to bundled file. |
| `compatibility` | No | Max 500 chars. Environment requirements (intended product, system packages, network access, etc.). |
| `metadata` | No | Arbitrary key-value mapping for additional properties. |
| `allowed-tools` | No | Space-delimited list of pre-approved tools (experimental). |

### Body Content

The Markdown body after frontmatter contains the skill instructions. There are no format restrictions — write whatever helps agents perform the task effectively. Recommended sections:

1. Brief description of the skill's purpose
2. "When to Use" — specific trigger conditions
3. Tool/workflow documentation with tables
4. Step-by-step workflows
5. Gotchas / notes and caveats

---

## Starting from Real Expertise

A common pitfall is asking an LLM to generate a skill without providing domain-specific context. The result is vague, generic procedures rather than the specific API patterns, edge cases, and project conventions that make a skill valuable.

### Extract from a Hands-On Task

Complete a real task in conversation with an agent, providing context, corrections, and preferences along the way. Then extract the reusable pattern into a skill. Pay attention to:

- **Steps that worked** — the sequence of actions that led to success
- **Corrections you made** — places where you steered the agent's approach
- **Input/output formats** — what the data looked like going in and coming out
- **Context you provided** — project-specific facts, conventions, or constraints the agent didn't already know

### Synthesize from Existing Artifacts

When you have a body of existing knowledge, feed it into an LLM and ask it to synthesize a skill. Good source material includes:

- Internal documentation, runbooks, and style guides
- API specifications, schemas, and configuration files
- Code review comments and issue trackers
- Version control history (reveals patterns through what actually changed)
- Real-world failure cases and their resolutions

### Refine with Real Execution

The first draft usually needs refinement. Run the skill against real tasks, then feed the results back into the creation process. Ask: what triggered false positives? What was missed? What could be cut?

Read agent execution traces, not just final outputs. If the agent wastes time on unproductive steps, common causes include instructions that are too vague, instructions that don't apply to the current task, or too many options presented without a clear default.

---

## Writing Effective Descriptions

The `description` field carries the entire burden of triggering. If the description doesn't convey when the skill is useful, the agent won't know to reach for it. Agents use it to decide which skill to activate from potentially 100+ available skills.

### Description Principles

- **Use imperative phrasing.** Frame as an instruction to the agent: "Use this skill when..." rather than "This skill does..."
- **Focus on user intent, not implementation.** Describe what the user is trying to achieve, not the skill's internal mechanics.
- **Err on the side of being pushy.** Explicitly list contexts where the skill applies, including cases where the user doesn't name the domain directly.
- **Keep it concise.** A few sentences to a short paragraph. Hard limit of 1024 characters.
- **Be specific and include key terms.** Include both what the skill does and when to use it.

### Good Description Examples

```yaml
description: >
  Checks TON wallet balances, token holdings, and transaction history.
  Use this skill when querying balances, listing tokens, viewing jettons,
  checking transaction history, or verifying transaction status on the TON
  blockchain, even if the user doesn't explicitly mention "TON" or "jetton."
```

```yaml
description: >
  Sends TON and jettons (tokens) to addresses or TON DNS domains (.ton, .t.me).
  Use this skill for transfers, payments, or sending assets on the TON blockchain.
```

### Bad Description Examples

```yaml
description: Helps with TON transfers.
```

```yaml
description: I can help you check balances.
```

### Optimizing Descriptions

To test triggering systematically:

1. Create ~20 eval queries (half should-trigger, half should-not-trigger)
2. Vary phrasing, explicitness, detail, and complexity in should-trigger queries
3. Use **near-misses** for should-not-trigger queries (share keywords but need something different)
4. Split into train (~60%) and validation (~40%) sets to avoid overfitting
5. Run queries through your agent and measure trigger rate
6. Iterate: broaden if should-trigger queries fail, add specificity if false-triggering

---

## Naming Conventions

- Skill directories (the `<skill-name>` segment): `kebab-case` (e.g., `ton-balance`, `ton-send`)
- Prefix TON-specific skills with `ton-`
- Name must match the parent directory name exactly
- No uppercase, no consecutive hyphens, no leading/trailing hyphens
- Prefer gerund form (`processing-pdfs`) or noun phrases (`pdf-processing`)
- Avoid vague names: `helper`, `utils`, `tools`
- Avoid reserved words: `anthropic`, `claude`

---

## Spending Context Wisely

Once a skill activates, its full `SKILL.md` body loads into the agent's context window alongside conversation history, system context, and other active skills. Every token competes for attention.

### Add What the Agent Lacks, Omit What It Knows

Focus on what the agent *wouldn't* know without your skill: project-specific conventions, domain-specific procedures, non-obvious edge cases, and the particular tools or APIs to use. Don't explain what blockchain is, how HTTP works, or what tokens are.

Ask yourself about each piece of content: "Would the agent get this wrong without this instruction?" If the answer is no, cut it. If you're unsure, test it.

### Design Coherent Units

Deciding what a skill should cover is like deciding what a function should do: encapsulate a coherent unit of work that composes well with other skills. Skills scoped too narrowly force multiple skills to load for a single task. Skills scoped too broadly become hard to activate precisely.

### Aim for Moderate Detail

Overly comprehensive skills can hurt more than they help — the agent struggles to extract what's relevant. Concise, stepwise guidance with a working example tends to outperform exhaustive documentation. When you find yourself covering every edge case, consider whether most are better handled by the agent's own judgment.

### Progressive Disclosure

Skills use three layers of context loading:

1. **Metadata (~100 tokens):** `name` and `description` are loaded at startup for all skills
2. **Instructions (< 5000 tokens recommended):** Full `SKILL.md` body is loaded when the skill is activated
3. **Resources (as needed):** Files in `scripts/`, `references/`, and `assets/` are loaded only when required

Keep `SKILL.md` under **500 lines**. Move detailed reference material to separate files.

#### Progressive Disclosure Patterns

**Pattern 1: High-level guide with references**

````markdown
# TON Balance Queries

## Quick start
Call `get_balance` to check TON balance.

## Advanced features
**Token lookups**: See [known-jettons.md](references/known-jettons.md)
**Transaction history**: See [transactions.md](references/transactions.md)
````

**Pattern 2: Domain-specific organization (this repo)**

```
wallets/
├── ton-balance/SKILL.md
├── ton-send/SKILL.md
├── ton-swap/SKILL.md
└── …
docs/
└── ton-docs/SKILL.md
```

Each row is a separate skill at `<group>/<skill-name>/`. There is no umbrella `wallets/SKILL.md` that wraps the others.

**Pattern 3: Conditional details**

```markdown
## Sending assets

For simple TON transfers → use `send_ton`

**For jetton transfers**: See the ton-send skill (`wallets/ton-send/` in this repo)
**For swaps**: See the ton-swap skill (`wallets/ton-swap/`)

From inside one skill’s `SKILL.md`, link to sibling reference files with paths relative to that skill root, e.g. [swap-notes.md](references/swap-notes.md). Cross-skill pointers are prose or repo paths — avoid deep chains of skill-to-skill file links.
```

### File References

When referencing other files, use relative paths from the skill root:

```markdown
See [the reference guide](references/REFERENCE.md) for details.
```

**Keep file references one level deep from SKILL.md.** Avoid deeply nested reference chains where file A references file B which references file C. All referenced files should link directly from SKILL.md.

Tell the agent *when* to load each file. "Read `references/api-errors.md` if the API returns a non-200 status code" is more useful than a generic "see references/ for details."

### Structure Longer Files

For reference files longer than 100 lines, include a table of contents at the top so agents can see the full scope even when previewing with partial reads.

---

## Calibrating Control

Not every part of a skill needs the same level of prescriptiveness. Match the specificity of your instructions to the fragility of the task.

### Match Specificity to Fragility

**Give the agent freedom** when multiple approaches are valid. Explaining *why* can be more effective than rigid directives — an agent that understands the purpose makes better context-dependent decisions:

```markdown
## Code review process

1. Check all database queries for SQL injection (use parameterized queries)
2. Verify authentication checks on every endpoint
3. Look for race conditions in concurrent code paths
```

**Be prescriptive** when operations are fragile, consistency matters, or a specific sequence must be followed:

````markdown
## Database migration

Run exactly this sequence:

```bash
python scripts/migrate.py --verify --backup
```

Do not modify the command or add additional flags.
````

Most skills have a mix. Calibrate each part independently.

### Provide Defaults, Not Menus

When multiple tools or approaches could work, pick a default and mention alternatives briefly:

````markdown
<!-- Clear default with escape hatch -->
Use pdfplumber for text extraction:

```python
import pdfplumber
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
````

### Favor Procedures Over Declarations

A skill should teach the agent *how to approach* a class of problems, not *what to produce* for a specific instance:

```markdown
<!-- Reusable method -->
1. Call `get_known_jettons` to resolve the token's jetton address
2. Call `get_jetton_balance` to verify sufficient balance
3. Ask one short yes/no confirmation
4. Call `send_jetton` with the jettonAddress, toAddress, and amount
5. Poll `get_transaction_status` until completed or failed
```

---

## Patterns for Effective Instructions

Use the patterns that fit your task — not every skill needs all of them.

### Gotchas Sections

The highest-value content in many skills is a list of gotchas — environment-specific facts that defy reasonable assumptions. Not general advice but concrete corrections to mistakes the agent will make without being told:

```markdown
## Gotchas

- Omniston consistently returns no quote for TON → xStock and xStock → TON
  routes. All xStock buys and sells must go through USDT.
- The `amount` field in balance responses is already human-readable decimal
  (e.g., "1.5" for 1.5 TON). Use `amountRaw` only when a raw integer is needed.
- Quotes expire. If the user waited before confirming, fetch a fresh quote.
```

When an agent makes a mistake you have to correct, add the correction to the gotchas section.

### Templates for Output Format

When you need the agent to produce output in a specific format, provide a template. This is more reliable than describing the format in prose:

````markdown
## Report structure

```markdown
# [Analysis Title]

## Executive summary
[One-paragraph overview]

## Key findings
- Finding 1 with supporting data
- Finding 2 with supporting data
```
````

### Checklists for Multi-Step Workflows

An explicit checklist helps the agent track progress and avoid skipping steps:

```markdown
## Swap workflow

Progress:
- [ ] Step 1: Resolve token addresses
- [ ] Step 2: Get swap quote
- [ ] Step 3: Emulate transaction
- [ ] Step 4: Confirm with user
- [ ] Step 5: Execute swap
- [ ] Step 6: Verify transaction status
```

### Validation Loops

Instruct the agent to validate its own work before moving on:

```markdown
## Editing workflow

1. Make your edits
2. Run validation: `python scripts/validate.py output/`
3. If validation fails → fix issues → run validation again
4. Only proceed when validation passes
```

### Plan-Validate-Execute

For batch or destructive operations, have the agent create an intermediate plan, validate it, then execute:

```markdown
1. Analyze the form fields
2. Create a plan file (field_values.json)
3. Validate the plan against the form schema
4. If validation fails, revise and re-validate
5. Execute the plan
```

### Examples Pattern

Provide input/output pairs for output-quality-sensitive tasks:

````markdown
**Example:**
Input: Send 1.5 TON to foundation.ton
Steps:
1. resolve_dns → UQA...
2. Confirm: "Send 1.5 TON to foundation.ton (UQA...)?"
3. send_ton → normalizedHash
4. Poll get_transaction_status → completed
````

### Conditional Workflow Pattern

Guide agents through decision points:

```markdown
1. Determine the task type:
   **Checking balance?** → Use ton-balance skill
   **Sending assets?** → Use ton-send skill
   **No wallet configured?** → Use ton-create-wallet skill first
```

---

## Content Guidelines

### Avoid Time-Sensitive Information

Don't include information that will become outdated. Use versioned references instead of dates. If you must document deprecated patterns, use a collapsible "Old patterns" section.

### Use Consistent Terminology

Choose one term and use it throughout:

- **Good:** Always "jetton" (not mixing "jetton", "token", "coin", "Jetton")
- **Good:** Always "agentic wallet" (not mixing "agent wallet", "agentic wallet", "smart wallet")

### Avoid Offering Too Many Options

Provide a default approach with an escape hatch for edge cases, rather than listing every alternative.

---

## Using Scripts in Skills

Skills can instruct agents to run shell commands and bundle reusable scripts in a `scripts/` directory.

### One-Off Commands

When an existing package already does what you need, reference it directly in SKILL.md. Pin versions for reproducibility:

```bash
npx @ton/mcp@alpha get_balance
```

State prerequisites in your SKILL.md (e.g., "Requires Node.js 18+") rather than assuming the agent's environment has them. Use the `compatibility` frontmatter field for runtime-level requirements.

### Bundled Scripts

When you notice the agent independently reinventing the same logic across runs, that's a signal to write a tested script and bundle it in `scripts/`.

Make clear whether the agent should **execute** or **read** the script:
- "Run `scripts/validate.sh` to check the output" (execute — most common)
- "See `scripts/validate.sh` for the validation algorithm" (read as reference)

### Designing Scripts for Agentic Use

**Avoid interactive prompts.** Agents operate in non-interactive shells — they cannot respond to TTY prompts. Accept all input via command-line flags, environment variables, or stdin.

**Document usage with `--help`.** Include a brief description, available flags, and usage examples. Keep it concise — the output enters the agent's context window.

**Write helpful error messages.** Say what went wrong, what was expected, and what to try:

```
Error: --format must be one of: json, csv, table.
       Received: "xml"
```

**Use structured output.** Prefer JSON/CSV over free-form text. Send data to stdout and diagnostics to stderr.

**Additional considerations:**
- **Idempotency.** Agents may retry commands. "Create if not exists" is safer than "create and fail on duplicate."
- **Dry-run support.** For destructive operations, a `--dry-run` flag lets the agent preview what will happen.
- **Meaningful exit codes.** Use distinct codes for different failure types and document them in `--help`.
- **Safe defaults.** Destructive operations should require explicit confirmation flags (`--confirm`, `--force`).
- **Predictable output size.** If output might be large, default to a summary or support `--offset` for pagination.
- **No "voodoo constants."** All magic numbers should be justified and documented.

---

## Evaluating and Iterating on Skills

### Repository-wide E2E runs

For setup, commands, eval format, and the per-skill eval files, see [tests/TESTING.md](tests/TESTING.md); aggregated metrics are committed to [tests/RESULTS.md](tests/RESULTS.md). The methodology below complements that flow — use it when designing or tightening eval cases for a single skill.

### Build Evaluations First

Create evaluations BEFORE writing extensive documentation. This ensures your skill solves real problems rather than documenting imagined ones.

1. **Identify gaps:** Run the agent on representative tasks without a skill. Document specific failures.
2. **Create test cases:** Build 2-3 scenarios that test these gaps. Each has a prompt, expected output, optional input files, and expectations.
3. **Establish baseline:** Measure performance without the skill.
4. **Write minimal instructions:** Create just enough content to address the gaps.
5. **Iterate:** Execute evaluations, compare against baseline, and refine.

### Test Case Structure

For where eval files live and the JSON schema, see [tests/TESTING.md § Eval format](tests/TESTING.md#eval-format).

### Writing Good Expectations

- **Good:** "The output file is valid JSON" — programmatically verifiable
- **Good:** "Calls get_swap_quote before send_raw_transaction" — observable
- **Weak:** "The output is good" — too vague to grade
- **Weak:** "Uses exactly the phrase 'Balance: X TON'" — too brittle

### The Iteration Loop

1. Give eval signals and current SKILL.md to an LLM and ask it to propose improvements
2. Review and apply changes
3. Rerun all test cases in a new `iteration-N/` directory
4. Grade and aggregate results
5. Review with a human. Repeat.

**When iterating:**
- **Generalize from feedback.** Fixes should address underlying issues broadly, not add narrow patches for specific test cases.
- **Keep the skill lean.** Fewer, better instructions often outperform exhaustive rules.
- **Explain the why.** Reasoning-based instructions ("Do X because Y tends to cause Z") work better than rigid directives ("ALWAYS do X, NEVER do Y").
- **Bundle repeated work.** If every test run independently wrote a similar helper script, bundle it in `scripts/`.

Stop when feedback is consistently empty or you're no longer seeing meaningful improvement.

---

## Anti-Patterns to Avoid

- **Nested skill directories:** Do not put `<group>/<skill-a>/<skill-b>/SKILL.md` — use `<group>/<skill-b>/SKILL.md` instead (see [CONTRIBUTE.md](CONTRIBUTE.md))
- **Windows-style paths:** Always use forward slashes (`references/guide.md`, not `references\guide.md`)
- **Vague descriptions:** "Helps with TON" tells the agent nothing
- **Over-explaining basics:** Don't explain what blockchain is or how wallets work
- **Too many options:** Provide one recommended approach, not five alternatives
- **Deeply nested references:** Keep file references one level deep from SKILL.md
- **Magic constants:** Document why specific values are chosen (slippage, timeouts, etc.)
- **Assuming tools are installed:** List required packages and MCP servers explicitly
- **Generic instructions without domain context:** "Handle errors appropriately" adds no value — say what specific errors occur and how to handle them
- **Punting to the agent:** Scripts should handle error conditions, not fail and let the agent figure it out

---

## Checklist for New Skills

### Repository layout
- [ ] Skill path is `<group>/<skill-name>/SKILL.md` (second level under repo root; see [CONTRIBUTE.md](CONTRIBUTE.md))
- [ ] `name` in frontmatter matches the **skill** folder name, not the group folder
- [ ] No extra `SKILL.md` nested under another skill’s directory

### Core Quality
- [ ] `name` matches the **skill** directory (`<skill-name>`), is kebab-case, max 64 chars
- [ ] `description` uses imperative phrasing, is specific, includes trigger keywords, max 1024 chars
- [ ] SKILL.md body is under 500 lines / ~5000 tokens
- [ ] Additional details are in separate files (if needed)
- [ ] No time-sensitive information
- [ ] Consistent terminology throughout
- [ ] Only includes context the agent wouldn't already know
- [ ] Gotchas section captures non-obvious edge cases
- [ ] File references are one level deep
- [ ] Progressive disclosure used appropriately
- [ ] Workflows have clear steps

### MCP Tool References
- [ ] All MCP tool names are accurate
- [ ] Required and optional parameters are documented
- [ ] Workflows reference the correct tools in the correct order

### Scripts (if applicable)
- [ ] Scripts are self-contained or clearly document dependencies
- [ ] No interactive prompts — all input via flags, env vars, or stdin
- [ ] Helpful error messages (what went wrong, what to try)
- [ ] Structured output (JSON/CSV) to stdout, diagnostics to stderr
- [ ] `--help` output documents flags and usage
- [ ] No magic numbers — all values justified

### Testing
- [ ] SKILL.md parses correctly (valid YAML frontmatter)
- [ ] All referenced tools/commands exist
- [ ] Workflows are accurate and complete
- [ ] Tested with real usage scenarios
- [ ] At least 2-3 eval test cases created
- [ ] Description triggers reliably on relevant prompts
