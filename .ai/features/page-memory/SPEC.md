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
  to stop untrusted pages planting graph links (:928-952). Local CLI writes do run them.
- Provenance + namespace rules: `test/put-page-provenance.test.ts`, `test/put-page-namespace.test.ts`
- Link extraction: `test/link-extraction.test.ts`; listing regression: `test/e2e/list-pages-regression.test.ts`

## Open questions
- UNCLEAR FROM CODE — confirm: since MCP writes skip link extraction, is anything meant to build the
  graph afterwards (e.g. a scheduled `gbrain extract` / autopilot)? No such schedule found on this machine.
