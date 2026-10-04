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

## Live status
> ✅ CONFIRMED from the live system on 2026-10-04 (read-only probes + one `think` call).
- `think` answers with **`ollama:qwen3:30b-a3b`** (local, $0) — reported as `modelUsed` by a live call. This
  only works because of the local Ollama chat patch (see `../local-ollama-provider/SPEC.md`).
- 21 `think` calls since 2026-07-03: 12 succeeded, 9 failed (timeouts in July, GPU out-of-memory in September);
  every call since 2026-09-30 succeeded, in 35–50 s.

## Decision (2026-10-04)
Keep `think` local; no paid fallback (a fallback would quietly send personal memory to a third party).
Revisit if more than 30% of calls fail over 30 days.
