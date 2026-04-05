# Notion MCP Reference

## Fetch Format

`mcp__plugin_Notion_notion__notion-fetch` returns JSON with `metadata`, `title`, `url`, and `text`. The `text` field is **NOT markdown** — it's Notion's XML-like format:

```xml
<page url="...">
  <ancestor-path>...</ancestor-path>
  <properties>{"title":"Page Title"}</properties>
  <content>
    ... text content ...
    <image source="FULL_S3_SIGNED_URL"></image>
    <empty-block/>
  </content>
</page>
```

Extract image URLs from `<image source="...">` tags.

## Image Download Rules

All 7 critical rules for downloading images from Notion S3 URLs:

1. **Full URL required** — include everything after the `?`. Truncated URLs return a 243-byte AccessDenied XML.
2. **Single quotes** around the URL — prevents shell `&` interpretation.
3. **Final destination only** — never use `/tmp/`; files may be cleaned between commands.
4. **Verify each download:**
   ```bash
   file /path/to/image.png
   ```
   Valid: `PNG image data, 1920 x 1080`. Invalid: `XML document` or `ASCII text`.
5. **Batch downloads in groups of ~5** — too many parallel curls can cause failures. Retry failed ones individually.
6. **Expired URLs** — re-fetch the page for fresh URLs. S3 signed URLs expire in ~1 hour.
7. **Name descriptively:** `01-welcome-screen.png`, `05-tier-discovery.png`.
