---
name: notion-to-docs
description: >
  Use when pulling content from Notion into a local repo — syncing pages,
  downloading images, exporting to markdown. Triggers on "notion to docs",
  "pull from notion", "sync notion", "export notion", "download notion page",
  "grab notion content", "notion markdown", "notion images".
---

# Notion to Docs

## Overview

Fetch Notion pages, download embedded images, convert to local markdown, and save image descriptions to avoid re-reading in future sessions.

## Prerequisites

Load tools via `ToolSearch query: "notion search"` — this loads both `notion-search` and `notion-fetch` from the `plugin:Notion:notion` OAuth server. If no results, ask the user to run `/mcp` to re-auth.

Do NOT use `Notion:search` or `Notion:find` skills — wrong MCP server.

## Workflow

```
1. Find page(s)     → search by keyword, or parse ID from URL
2. Fetch content     → get page body + S3 image URLs
3. Download images   → curl with full signed URLs, verify each
4. Convert markdown  → replace S3 URLs with local paths
5. Describe images   → read each image, write text descriptions
```

### Step 1: Find Pages

**By keyword:** `notion-search` with `query` + `query_type: "internal"`. Returns `title`, `url`, `id`.

**By URL:** Extract hex ID from `notion.so/HEXID` or `notion.so/Page-Title-HEXID`.

**By ID:** With or without dashes both work.

### Step 2: Fetch Page Content

`mcp__plugin_Notion_notion__notion-fetch` with `id` (UUID). Returns XML-like format (not markdown) with `<image source="...">` tags for images. See [notion-mcp.md](references/notion-mcp.md#fetch-format) for format details.

**Save raw content immediately** — S3 signed URLs expire in ~1 hour.

### Step 3: Download Images

```bash
curl -sL -o /final/path/image.png 'FULL_S3_URL_WITH_ALL_QUERY_PARAMS'
```

Critical: Use full URL (including query params), single-quote it, download to final destination (not `/tmp/`), and verify each with `file`. See [notion-mcp.md](references/notion-mcp.md#image-download-rules) for all rules.

### Step 4: Convert to Markdown

Parse XML-like content into markdown. Replace `<image source="...">` with `![alt](./images/category/name.png)`. Replace `<empty-block/>` with blank lines.

### Step 5: Create Image Descriptions

Read each image with the Read tool (supports images). Write `image-descriptions.md` with a `## filename` heading and 1-2 line description per image. This avoids re-reading images in future sessions.

## Directory Structure

```
docs/flows/                     # or user-specified location
  page-name.md                  # converted markdown
  images/
    category/                   # group by page/flow
      01-screen-name.png
      02-screen-name.png
  image-descriptions.md         # all image descriptions
```

## Quick Reference

| Tool | Purpose | Key detail |
|------|---------|------------|
| `notion-search` | Find pages by keyword | `query_type: "internal"` |
| `notion-fetch` | Get page content by UUID | Returns XML-like format, not markdown |
| `curl -sL` | Download S3 images | Must use full signed URL with query params |
| `file <path>` | Verify image download | Valid: `PNG image data`. Invalid: `XML document` |

## Troubleshooting

| Problem | Fix |
|---------|-----|
| No Notion tools from ToolSearch | User runs `/mcp` to re-auth OAuth |
| 243-byte XML instead of image | Use full S3 URL with all query params |
| AccessDenied after ~1 hour | Re-fetch page for fresh signed URLs |
| Files disappear between commands | Download to final destination, not `/tmp/` |
| Parallel downloads fail | Batch in groups of ~5, retry failures |
| `Notion:search` skill fails | Use ToolSearch to load tools directly |
