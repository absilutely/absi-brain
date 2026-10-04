# Spec: Search (keyword + vector hybrid)

**Status:** shipped (upstream)   ·   **Slug:** hybrid-search

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
`search` is keyword search; `query` is hybrid (keyword + embedding similarity, fused and re-ranked)
with search modes that trade cost for quality.

## Files
- `src/core/operations.ts` — `search` (:1391), `query` (:1450)
- `src/core/search/` — `hybrid.ts`, `keyword.ts`, `rerank.ts`, `mode.ts`, `expansion.ts`, …
- Search-mode rationale: upstream `CLAUDE.md` § "Search Mode"

## Acceptance-shaped behaviors
- Hybrid search on PGLite: `test/hybrid-search-lite.serial.test.ts`
- CLI dispatch: `test/cli-search-dispatch.test.ts`, `test/commands-search.test.ts`

## Open questions
- UNCLEAR FROM CODE — confirm: which search mode is this brain on (the init cost matrix asks)?
  `~/.gbrain/config.json` sets none explicitly, so the upstream default applies.
