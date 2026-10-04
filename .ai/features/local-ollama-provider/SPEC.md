# Spec: Local Ollama provider (embeddings; chat patch uncommitted)

**Status:** shipped (embeddings, upstream) · chat = uncommitted local patch   ·   **Slug:** local-ollama-provider

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
Embeddings come from local Ollama (`ollama:nomic-embed-text`, 768 dims) — free and offline.

## Files
- `src/core/ai/recipes/ollama.ts`
- Gateway: `src/core/ai/`

## The uncommitted local patch (live checkout only, not on any branch)
The live checkout has an uncommitted edit to `src/core/ai/recipes/ollama.ts` adding a `chat`
touchpoint (any local model, no tools, no subagent loop, $0). Upstream deliberately treats Ollama
as embedding-only, and the patch **fails 2 upstream tests** in `test/ai/gateway-chat.test.ts`
(lines 54 and 110). The live config's chat model is OpenRouter, so the patch is not exercised today.

## Acceptance-shaped behaviors
- Recipe registry stays stable: `test/ai/recipes-existing-regression.test.ts`
- Upstream rule: embedding-only providers don't declare chat (`test/ai/gateway-chat.test.ts:54`)

## Open questions
- UNCLEAR FROM CODE — confirm: keep the local chat patch (commit it + update those 2 tests) or drop it?
