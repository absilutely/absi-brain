# Spec: Think (synthesized, cited answers)

**Status:** shipped (upstream)   ·   **Slug:** think-synthesis

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
`think` takes an open question, gathers evidence from the brain, and asks the chat model for an
answer with citations back to pages.

## Files
- `src/core/operations.ts` — `think` (:1792)
- `src/core/think/` — `gather.ts`, `intent.ts`, `prompt.ts`, `cite-render.ts`, `sanitize.ts`, `entity-extract.ts`
- Chat model from config: `chat_model = openrouter:openai/gpt-5.2` (`~/.gbrain/config.json`)

## Acceptance-shaped behaviors
- `test/think-pipeline.serial.test.ts`, `test/think-intent.test.ts`, `test/think-gateway-adapter.test.ts`,
  `test/think-trajectory-injection.test.ts`

## Open questions
- UNCLEAR FROM CODE — confirm: is paid OpenRouter the intended brain for `think`?
