---
name: claude-md-optimizer
description: >
  Use when CLAUDE.md or AGENTS.md is bloated, slow, or needs restructuring.
  Use when user says "optimize claude md", "claude md too long", "slim down claude md",
  "move to docs", "extract from claude md", "claude md best practices",
  "reduce context", "too much context in claude md".
---

# CLAUDE.md Optimizer

## Overview

Analyze CLAUDE.md/AGENTS.md files and restructure them: keep only critical content inline, extract valuable detail to `docs/`, delete filler.

**Goal**: CLAUDE.md under 50 lines (hard max: 100). Every line must earn its token cost.

## Workflow

### Phase 1: Discovery

1. Find all CLAUDE.md and AGENTS.md files in workspace (recursive, max 3 levels)
2. Count lines per file. Flag any file >50 lines as a candidate
3. Detect repo type:
   - **Single repo**: One CLAUDE.md at root
   - **Monorepo**: Root CLAUDE.md + package-level CLAUDE.md files
   - **Meta-repo**: Root CLAUDE.md referencing sub-repos with their own CLAUDE.md
4. Check for existing `docs/` directory
5. Detect GSD markers (`<!-- GSD:*-start -->`)

### Phase 2: Analysis

Parse each CLAUDE.md by `##` headers into sections. Classify each using `references/classification.md`:

- **KEEP**: Content that directly helps every prompt (identity, commands, critical rules)
- **EXTRACT**: Valuable but reference-only content → move to `docs/`
- **DELETE**: Empty placeholders, generic boilerplate, redundant enforcement blocks

**Key signals for EXTRACT**: section >15 lines, contains tables with >5 rows, step-by-step instructions, per-language conventions, architecture layers, technology rationale.

**Key signals for DELETE**: "not yet established" placeholders, empty developer profile sections, generic workflow enforcement that duplicates plugin behavior.

Calculate: current lines → projected lines after optimization.

### Phase 3: Proposal

Present to user:

```
## CLAUDE.md Optimization Proposal

**File**: path/to/CLAUDE.md
**Current**: 456 lines → **Target**: ~45 lines (90% reduction)
**Repo type**: meta-repo

### Section Plan
| Section | Lines | Action | Destination |
|---------|-------|--------|-------------|
| Project Overview | 12 | KEEP | — |
| Technology Stack | 141 | EXTRACT | docs/STACK.md |
| ...

### New docs/ files
- docs/STACK.md (from Technology Stack section)
- docs/CONVENTIONS.md (from Conventions section)
- ...

### AGENTS.md
- [ ] Create symlink → CLAUDE.md (no existing AGENTS.md)
```

**Stop and wait for user approval before Phase 4.**

### Phase 4: Execution

1. Read current CLAUDE.md fully
2. Create `docs/` files with extracted content (preserve GSD markers as stubs)
3. Write minimal CLAUDE.md using template from `references/templates.md`
4. If approved: `ln -s CLAUDE.md AGENTS.md`
5. Report before/after line counts and files created

## GSD Marker Handling

Preserve markers but replace content with a pointer. See `references/templates.md` for stub format.

## Reference Routing

- **Classification criteria** → Read `references/classification.md`
- **Output templates** → Read `references/templates.md`

## Common Mistakes

- Moving deployed addresses to docs (referenced constantly — KEEP)
- Deleting instead of extracting (if written, someone needed it — EXTRACT)
- Making CLAUDE.md just a table of contents (must still contain critical rules)
- Forgetting to preserve GSD markers (breaks auto-sync)
