# Spec: Page memory (save, read, list, delete pages)

**Status:** shipped (upstream)   ·   **Slug:** page-memory

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
Pages are markdown + frontmatter keyed by a slug (`people/…`, `projects/…`). Writing a page chunks
and embeds it; the bot's memory is these pages.

## Files
- `src/core/operations.ts` — `get_page` (:624), `put_page` (:725), `delete_page` (:1254), `list_pages` (:1335)
- Auto-link / auto-timeline after write: `runAutoLink` (:1086)

## Acceptance-shaped behaviors
- `put_page` chunks, embeds and reconciles tags (op description, :726)
- IF the caller is remote (MCP) THEN auto-link + auto-timeline are skipped (`{ skipped: 'remote' }`),
  to stop untrusted pages planting graph links (:928-952). Exceptions: trusted local CLI writes, and
  remote subagent writes restricted to an allowed slug-prefix list (`ctx.allowedSlugPrefixes`, :947-949) —
  and in both cases only when enabled: auto-link via `isAutoLinkEnabled` (:955), auto-timeline via
  `isAutoTimelineEnabled` (:967).
- Provenance + namespace rules: `test/put-page-provenance.test.ts`, `test/put-page-namespace.test.ts`
- Link extraction: `test/link-extraction.test.ts`; listing regression: `test/e2e/list-pages-regression.test.ts`

## Live status
> ✅ CONFIRMED from the live system on 2026-10-04 (read-only probes + one `think` call).
- 125 pages, 0 links until 2026-10-04 (brain health score 46/100, every page orphaned). No background job or
  maintenance cycle has ever run. `add_link` works over MCP: one real link was added on 2026-10-04 as proof.

## Decision (2026-10-04)
- Now: the bot adds links itself with `add_link` (absi-agent-bot PR #62 fixes its prompt, which wrongly said
  gbrain builds the graph). Keep gbrain's skip for MCP writes — it is a deliberate safety design.
- After the upstream catch-up: run link extraction over existing pages once, on a copy first.
