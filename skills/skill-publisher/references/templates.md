# Templates

## plugin.json

```json
{
  "name": "<skill-name>",
  "description": "<one-line description>",
  "version": "1.0.0",
  "author": { "name": "Nico Acosta" },
  "homepage": "https://github.com/NicoAcosta/<skill-name>",
  "repository": "https://github.com/NicoAcosta/<skill-name>",
  "license": "MIT",
  "keywords": ["<relevant>", "<keywords>"]
}
```

Use same JSON for both `.claude-plugin/` and `.cursor-plugin/`.

## GEMINI.md

```markdown
This repo contains the **<skill-name>** skill for <brief purpose>.

@./skills/<skill-name>/SKILL.md
```

## AGENTS.md

```markdown
# <Skill Name>

<What it does in 2-3 sentences.>

## Usage

Read the full skill at `skills/<skill-name>/SKILL.md`.
```
