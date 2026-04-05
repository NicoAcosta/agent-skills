# Output Templates

## Minimal CLAUDE.md — Single Repo

```markdown
# Project Name
One-line description of what this project does.

## Commands
| Command | Description |
|---------|-------------|
| `pnpm dev` | Start dev server |
| `pnpm build` | Production build |
| `pnpm test` | Run tests |

## Rules
- Rule 1 (critical do)
- Rule 2 (critical don't)
- Rule 3

## Docs
- [Architecture](docs/ARCHITECTURE.md)
- [Stack & conventions](docs/STACK.md)
```

Target: ~20-30 lines.

## Minimal CLAUDE.md — Monorepo

```markdown
# Project Name
One-line description.

## Packages
| Package | Path | Purpose |
|---------|------|---------|
| `app` | `apps/app` | Main application |
| `api` | `apps/api` | API server |
| `ui` | `packages/ui` | Shared components |

## Commands
| Command | Description |
|---------|-------------|
| `pnpm dev` | Start all apps |
| `pnpm build` | Build all |
| `pnpm test` | Run all tests |
| `pnpm dev --filter=app` | Dev single app |

## Rules
- Always use workspace protocol for internal deps (`workspace:*`)
- Rule 2
- Rule 3

## Docs
- [Architecture](docs/ARCHITECTURE.md)
- [Stack](docs/STACK.md)
- [Conventions](docs/CONVENTIONS.md)

Each package may have its own CLAUDE.md with package-specific context.
```

Target: ~30-40 lines.

## Minimal CLAUDE.md — Meta-Repo

```markdown
# Project Name
One-line description of the workspace.

## Repositories
| Repo | Stack | Purpose |
|------|-------|---------|
| `frontend` | Next.js, TypeScript | Web application |
| `backend` | Node.js, Express | API server |
| `contracts` | Solidity, Foundry | Smart contracts |

## Commands
| Command | Description |
|---------|-------------|
| `cd frontend && pnpm dev` | Start frontend |
| `cd backend && pnpm dev` | Start backend |

## Deployed
| Resource | Address/URL |
|----------|-------------|
| Frontend | https://app.example.com |
| API | https://api.example.com |
| Contract | `0x1234...` (Mainnet) |

## Rules
- Always sync ABIs after contract changes
- Rule 2

## Docs
- [Architecture](docs/ARCHITECTURE.md)
- [Integration workflows](docs/WORKFLOWS.md)
- [Stack](docs/STACK.md)

Each repo has its own CLAUDE.md — read those for repo-specific patterns.
```

Target: ~40-50 lines.

## GSD Marker Stub

When a GSD-managed section is extracted, preserve markers with a pointer:

```markdown
<!-- GSD:stack-start source:STACK.md -->
## Technology Stack
See [docs/STACK.md](docs/STACK.md) for full details.
<!-- GSD:stack-end -->
```

This keeps GSD's section detection working while eliminating inline bloat.

## docs/ File Format

Each extracted file should follow this format:

```markdown
# Section Title

> Extracted from CLAUDE.md — this is the detailed reference.
> CLAUDE.md contains a summary with a pointer here.

[Original content preserved as-is]
```

Keep the original content intact. Don't reorganize or rewrite — just move it. The user can improve it later.

## AGENTS.md Symlink

When creating AGENTS.md as a symlink:

```bash
ln -s CLAUDE.md AGENTS.md
```

Only do this when:
- No AGENTS.md exists, OR
- Existing AGENTS.md is identical to CLAUDE.md

Do NOT symlink when AGENTS.md has distinct content (e.g., cross-repo context that differs from the repo's own CLAUDE.md).

## Proposal Output Format

```
## CLAUDE.md Optimization Proposal

**File**: `path/to/CLAUDE.md`
**Current**: {N} lines → **Target**: ~{M} lines ({P}% reduction)
**Repo type**: {single|monorepo|meta-repo}

### Section Plan
| # | Section | Lines | Action | Destination |
|---|---------|-------|--------|-------------|
| 1 | Project Overview | 8 | KEEP | — |
| 2 | Technology Stack | 141 | EXTRACT | docs/STACK.md |
| 3 | Conventions | 153 | EXTRACT | docs/CONVENTIONS.md |
| 4 | Architecture | 99 | EXTRACT | docs/ARCHITECTURE.md |
| 5 | GSD Workflow | 12 | DELETE | — |
| 6 | Developer Profile | 6 | DELETE | — |

### Files to Create
| File | Source Section | Lines |
|------|---------------|-------|
| docs/STACK.md | Technology Stack | ~141 |
| docs/CONVENTIONS.md | Conventions | ~153 |
| docs/ARCHITECTURE.md | Architecture | ~99 |

### AGENTS.md
- {recommendation}

Approve to proceed with restructuring.
```
