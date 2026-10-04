# Spec: Isolated brain homes (GBRAIN_HOME)

**Status:** shipped (upstream)   ·   **Slug:** isolated-brain-homes

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
`GBRAIN_HOME=<dir>` moves all config, database and tokens to `<dir>/.gbrain`, so one checkout can
serve several fully separate brains (here: personal on :3131, family on :3132).

## Files
- `src/core/config.ts` — `configDir()` honors `GBRAIN_HOME` (:1023-1026)

## Acceptance-shaped behaviors
- `configDir()` returns `<GBRAIN_HOME>/.gbrain` when set, falls back to home when unset, rejects
  relative paths (`test/gbrain-home-isolation.test.ts`)

## Open questions
- See `.ai/journeys/family-brain.md` — who uses the family brain today?
