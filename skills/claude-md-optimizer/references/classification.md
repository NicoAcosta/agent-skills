# Section Classification Guide

## Decision Flow

For each `##` section in CLAUDE.md, ask:

1. **Is it empty or placeholder?** ("not yet established", empty profile) → **DELETE**
2. **Is it generic boilerplate?** (workflow enforcement, "use TDD", "write tests") → **DELETE** (belongs in global CLAUDE.md or skills, not per-project)
3. **Is it <15 lines AND directly actionable?** (commands, rules, addresses) → **KEEP**
4. **Is it reference material >15 lines?** (stack, conventions, architecture) → **EXTRACT**

## KEEP Criteria

Content stays in CLAUDE.md when ALL of these are true:
- Referenced in most prompts (not just occasionally)
- Under 15 lines
- Actionable (commands, rules, constraints — not explanations)
- Unique to this project (not generic best practices)

### Always KEEP
| Content | Max Lines | Example |
|---------|-----------|---------|
| Project name + one-line description | 3 | `# Orbofi — Tokenized AI agents platform` |
| Repo/package map (table) | 15 | Table with name, path, stack, entry point |
| Essential commands | 10 | build, test, dev, deploy, lint |
| Critical do's and don'ts | 10 | "Never mock DB in tests", "Always use pnpm" |
| Deployed addresses/endpoints | 15 | Contract addresses, API URLs |
| Environment setup (minimal) | 5 | "Copy .env.example to .env.local" |
| Pointers to docs/ | 5 | "See docs/ for architecture and conventions" |

## EXTRACT Criteria

Content moves to `docs/` when ANY of these are true:
- Section >15 lines
- Contains tables with >5 rows of reference data
- Step-by-step instructions (numbered lists >5 steps)
- Per-language/framework conventions
- Architecture layers, data flows, system diagrams
- Technology rationale ("why we chose X over Y")
- Setup guides (environment, tooling, third-party services)

### Extraction Routing
| Content Pattern | Destination | Trigger Keywords |
|-----------------|-------------|------------------|
| Languages, frameworks, versions, dependencies | `docs/STACK.md` | "Technology Stack", "Dependencies", "Versions" |
| Naming conventions, code style, file organization | `docs/CONVENTIONS.md` | "Conventions", "Style Guide", "Naming" |
| System layers, data flow, component relationships | `docs/ARCHITECTURE.md` | "Architecture", "System Design", "Data Flow" |
| Environment variables, local setup, deployment | `docs/SETUP.md` | "Setup", "Environment", "Installation" |
| Step-by-step processes, deployment workflows | `docs/WORKFLOWS.md` | "Workflow", "How to", "Process" |
| Colors, fonts, design tokens, brand guidelines | `docs/DESIGN-TOKENS.md` | "Brand", "Colors", "Design System" |
| Stack justification, alternatives considered | `docs/STACK.md` (append) | "Why", "Rationale", "Alternatives" |
| Cross-repo integration details | `docs/INTEGRATION.md` | "Integration", "Cross-repo", "ABI Sync" |

## DELETE Criteria

Content is removed (not extracted) when:
- Empty placeholder with no content ("## Conventions — not yet established")
- Generic best practices that apply to all projects ("write tests", "use TypeScript")
- Redundant workflow enforcement that duplicates plugin/skill behavior
- Developer profile sections that are empty or auto-generated
- Verbose explanations of obvious things ("UserService handles users")

## Real Bloat Patterns

These patterns were identified in actual CLAUDE.md files (133-456 lines):

### Pattern 1: Embedded Stack Document (100-150 lines)
```
## Technology Stack
### Languages
- TypeScript 5.x — Primary language...
### Runtimes
- Node.js 20 LTS...
### Frameworks
...20 more subsections with rationale...
```
**Action**: EXTRACT entire section to `docs/STACK.md`. Replace with one-line summary.

### Pattern 2: Per-Language Conventions (50-150 lines)
```
## Conventions
### Frontend Conventions
- Components: PascalCase...
### Backend Conventions
- Routes: kebab-case...
### Smart Contract Conventions
...
```
**Action**: EXTRACT to `docs/CONVENTIONS.md`. Keep only 3-5 universal rules inline.

### Pattern 3: Full Architecture Document (50-100 lines)
```
## Architecture
### Pattern Overview
### Layers
### Data Flow
### Key Abstractions
### Entry Points
### Error Handling
### Cross-Cutting Concerns
```
**Action**: EXTRACT to `docs/ARCHITECTURE.md`. Keep only a 3-line summary.

### Pattern 4: Setup Workflow (20-50 lines)
```
## Tenderly Fork Setup
1. Create a fork...
2. Configure environment...
3. Deploy contracts...
...15 more steps...
```
**Action**: EXTRACT to `docs/SETUP.md` or `docs/WORKFLOWS.md`.

### Pattern 5: GSD Auto-Injected Sections
```
<!-- GSD:stack-start source:STACK.md -->
## Technology Stack
...120 lines of auto-generated content...
<!-- GSD:stack-end -->
```
**Action**: EXTRACT content to `docs/STACK.md`. Keep markers with pointer stub.

## Size Targets

| Repo Type | Target Lines | Hard Max |
|-----------|-------------|----------|
| Single repo | 30-40 | 60 |
| Monorepo | 40-50 | 80 |
| Meta-repo | 40-60 | 100 |
