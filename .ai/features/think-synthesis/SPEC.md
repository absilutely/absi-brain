# Spec: Think (synthesized, cited answers)

**Status:** shipped (upstream)   ·   **Slug:** think-synthesis

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
`think` takes an open question, gathers evidence from the brain, and asks a chat model for an
answer with citations back to pages.

## Files
- `src/core/operations.ts` — `think` (:1792)
- `src/core/think/` — `gather.ts`, `intent.ts`, `prompt.ts`, `cite-render.ts`, `sanitize.ts`, `entity-extract.ts`
- Model choice: `src/core/think/index.ts:234-239` resolves `models.think` → `models.default` →
  `models.tier.deep` (database settings) → env → built-in default `anthropic:claude-opus-4-7`
  (`src/core/model-config.ts:74-79`). The `chat_model` in `~/.gbrain/config.json`
  (`openrouter:openai/gpt-5.2`) is NOT in that chain.

## Acceptance-shaped behaviors
- `test/think-pipeline.serial.test.ts`, `test/think-intent.test.ts`, `test/think-gateway-adapter.test.ts`,
  `test/think-trajectory-injection.test.ts`

## Open questions
- UNCLEAR FROM CODE — confirm: which model actually answers `think` today? Unverified — the database
  settings couldn't be read while the server holds the PGLite lock. If none are set it falls back to
  Anthropic Opus 4.7, which needs an Anthropic key.
