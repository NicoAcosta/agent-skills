# Agent Skills by Nico Acosta

A marketplace of reusable agent skills for AI coding assistants.

## Available Skills

| Skill | Description | Standalone Repo |
|-------|-------------|-----------------|
| [llm-seo](skills/llm-seo/) | Optimize websites for AI search visibility, LLM citations, and agent discoverability | [NicoAcosta/llm-seo](https://github.com/NicoAcosta/llm-seo) |
| [claude-md-optimizer](skills/claude-md-optimizer/) | Optimize bloated CLAUDE.md/AGENTS.md files by extracting content to docs/ | [NicoAcosta/claude-md-optimizer](https://github.com/NicoAcosta/claude-md-optimizer) |
| [skill-publisher](skills/skill-publisher/) | Create, organize, and publish agent skills across all major AI coding environments | [NicoAcosta/skill-publisher](https://github.com/NicoAcosta/skill-publisher) |
| [notion-to-docs](skills/notion-to-docs/) | Pull Notion content into local repos — sync pages, download images, export to markdown | [NicoAcosta/notion-to-docs](https://github.com/NicoAcosta/notion-to-docs) |
| [tenderly-devtools](skills/tenderly-devtools/) | Cheat codes for Tenderly forks — fund addresses, manipulate storage, inject bytecode | [NicoAcosta/tenderly-devtools](https://github.com/NicoAcosta/tenderly-devtools) |

## Installation

### Claude Code (all skills at once)

```
/plugin marketplace add NicoAcosta/agent-skills
/plugin install agent-skills@nicoacosta-skills
```

### Individual skills via Skills.sh

```bash
npx skills add NicoAcosta/llm-seo
npx skills add NicoAcosta/claude-md-optimizer
npx skills add NicoAcosta/skill-publisher
npx skills add NicoAcosta/notion-to-docs
npx skills add NicoAcosta/tenderly-devtools
```

### Other environments

Each skill repo includes installation instructions for Cursor, Codex, OpenCode, and Gemini CLI. See the standalone repos linked above.

## How it works

Each skill lives in its own repo as the source of truth. This marketplace repo aggregates them via CI sync for Claude Code's plugin system. When a skill repo tags a new release, a GitHub Action syncs the updated files here automatically.

## License

MIT
