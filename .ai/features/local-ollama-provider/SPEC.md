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
(lines 54 and 110). Confirmed 2026-10-04: the patch IS in use — `think` runs on `ollama:qwen3:30b-a3b` through it.
Backed up (not merged) on branch `absi-agent/ollama-chat-backup` so it can't be lost.
Upstream has since shipped Ollama chat AND per-model embedding dims (incl. 1024-dim bge-m3), so the patch
becomes unnecessary at the upstream catch-up.

## Acceptance-shaped behaviors
- Recipe registry stays stable: `test/ai/recipes-existing-regression.test.ts`
- Upstream rule: embedding-only providers don't declare chat (`test/ai/gateway-chat.test.ts:54`)

## Open questions
- Decided 2026-10-04: keep running it until the upstream catch-up, then drop it (pure-mirror policy) and
  re-check that `think` still answers on qwen3. If it doesn't, re-apply the backup branch.
