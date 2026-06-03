# Finance Skills Agent Contract

## Project Overview

This repository is a collection of agent skills for financial analysis and trading, following the Agent Skills open standard. Skills are installable into Claude Code, Claude.ai, Codex, Gemini CLI, GitHub Copilot, and other supported agents.

## Repository Structure

This repo is three things at once:

1. A Claude Code plugin marketplace: `.claude-plugin/marketplace.json` plus `plugins/`
2. An Agent Skills repository: `SKILL.md` files inside `plugins/<group>/skills/`
3. An opencli plugin monorepo: `opencli-plugin.json` at root plus `opencli-plugins/`

```text
.claude-plugin/
  marketplace.json
plugins/
  market-analysis/
  social-readers/
  data-providers/
  startup-tools/
  ui-tools/
  skill-creator/
opencli-plugin.json
opencli-plugins/
  tradingview/
workspaces/
.agents/
.github/workflows/
  release-skills.yml
  skill-lint.yml
```

`.agents/` is auto-generated for agent distribution. Do not edit it directly.

## Skill Format

Each skill is a self-contained directory under `plugins/<group>/skills/`. The `SKILL.md` file tells the model when to activate, what steps to follow, and where to find reference details.

Required frontmatter:

```markdown
---
name: skill-name
description: >
  Multi-line trigger-oriented description.
---
```

The `description` field controls when the skill activates. Write it as a comprehensive trigger list, not a short summary.

Use reference files under `references/` for detailed API references, code templates, formulas, or schema docs.

## Creating A New Skill

1. Choose the appropriate plugin group: `market-analysis`, `social-readers`, `data-providers`, `startup-tools`, `ui-tools`, or `skill-creator`.
2. Create `plugins/<group>/skills/<skill-name>/`.
3. Write `SKILL.md` with YAML frontmatter and step-by-step instructions.
4. Add reference files under `references/` when details would bloat the main instructions.
5. Add a `README.md` for the skill's GitHub page.
6. Update the root `README.md` to list the new skill in the appropriate plugin group table.
7. The skill will be auto-zipped and released on tag push via GitHub Actions.

## Platform Considerations

Skills requiring shell access, network calls, or external binaries only work on CLI-based agents such as Claude Code.

Skills that only use built-in tools can work on Claude.ai.

## Dynamic Content

Skills can embed shell commands that Claude Code executes at invocation time using fenced command syntax. Use this for runtime environment checks, authentication checks, and live data.

Guidelines:

- Keep commands fast, ideally under two seconds.
- Always include fallback output so the skill degrades gracefully.
- Use dynamic commands only for CLI-based agents.

## Instruction Style

- Organize instructions as numbered steps.
- Use tables to map user intents to actions.
- Include defaults for missing parameters.
- Put lengthy code templates and API references in `references/`.
- End with a response step describing how to present results.

## Plugin System

This repo ships as a Claude Code plugin marketplace containing six plugins:

| Plugin | Description |
|---|---|
| `finance-market-analysis` | Stock analysis, earnings, correlations, options via yfinance |
| `finance-social-readers` | Social media research feeds |
| `finance-data-providers` | External API data |
| `finance-startup-tools` | Startup analysis frameworks |
| `finance-ui-tools` | Generative UI design system for Claude widgets |
| `finance-skill-creator` | Skill authoring, evaluation, and improvement |

Users install all plugins with:

```bash
npx plugins add himself65/finance-skills
```

Individual plugins and skills can be selected with `--plugin` or `--skill`.

## CI/CD

- `release-skills.yml`: zips each skill and publishes GitHub releases on `v*` tags.
- `skill-lint.yml`: lints all `SKILL.md` files.
- `opencli-plugin-test.yml`: runs pure-JS unit tests for opencli plugins with test files.

## opencli Plugins

Custom opencli adapters live under `opencli-plugins/` as a Node monorepo. Each sub-plugin has its own `opencli-plugin.json`, `package.json`, command files, shared helpers, and tests.

Install path:

```bash
opencli plugin install github:himself65/finance-skills/<sub-plugin-name>
```

For desktop-app adapters, use `Strategy.UI` with `browser: true`. For pure HTTP, use `Strategy.PUBLIC` with `browser: false`.

## Important Constraints

- No trade execution. Brokerage-related skills must be read-only.
- Most of the repo is documentation and `SKILL.md` files.
- `opencli-plugins/` contains real Node code and should be verified with tests plus practical PoC checks when relevant.
