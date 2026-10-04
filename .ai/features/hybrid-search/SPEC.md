# Spec: Search (keyword + vector hybrid)

**Status:** shipped (upstream)   ·   **Slug:** hybrid-search

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
`search` is cheap hybrid (keyword + vector, fused, no LLM query expansion); it drops to keyword-only
only when the `search.mcp_keyword_only` setting is on. `query` is full hybrid with expansion and
search modes that trade cost for quality. (The `search` op's own description string still says
"Keyword search" — it is out of date.)

## Files
- `src/core/operations.ts` — `search` (:1391; keyword-only switch :1414, cheap-hybrid path :1429-1436), `query` (:1450)
- `src/core/search/` — `hybrid.ts`, `keyword.ts`, `rerank.ts`, `mode.ts`, `expansion.ts`, …
- Search-mode rationale: upstream `CLAUDE.md` § "Search Mode"; `search.mode` is a database setting
  (`src/core/config.ts:840`), not a `config.json` key

## Acceptance-shaped behaviors
- Keyword path on PGLite (token budget, cache, intent): `test/hybrid-search-lite.serial.test.ts`
  (vector search is not enabled in that test)
- CLI dispatch: `test/cli-search-dispatch.test.ts`, `test/commands-search.test.ts`

## Open questions
- UNCLEAR FROM CODE — confirm: which search mode is this brain on? Check with `gbrain search modes`
  (needs the pm2 server stopped — PGLite is single-writer).
- Gap: no test found that guards vector recall quality on PGLite.
