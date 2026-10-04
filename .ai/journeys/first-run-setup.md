# Journey: stand up a brain on this machine

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

How a fresh brain gets from nothing to serving the bot. Mirrors how `~/.gbrain/config.json`
is set up today (PGLite engine, `ollama:nomic-embed-text` 768-dim embeddings,
`openrouter:openai/gpt-5.2` chat model, schema pack `gbrain-base-v2`).

1. `bun install` in the checkout → dependencies installed; `postinstall` tries
   `gbrain apply-migrations` (`package.json` scripts.postinstall).
2. `gbrain init --pglite` → a local PGLite brain at `~/.gbrain/brain.pglite`
   (`src/commands/init.ts:30`; `test/e2e/init-fresh-pglite.test.ts`,
   `test/e2e/fresh-install-pglite.test.ts`).
3. Point embeddings at local Ollama (`ollama pull nomic-embed-text`) →
   `src/core/ai/recipes/ollama.ts` setup hint.
4. `gbrain auth create <name>` → a bearer token for the bot (`src/commands/auth.ts:485`).
5. `gbrain serve --http` under pm2 → server on :3131 (journey `main.md` step 1).
6. `gbrain doctor` → health report (`src/commands/doctor.ts`).

UNCLEAR FROM CODE — confirm: the chat model is OpenRouter (paid) while embeddings are local
Ollama (free). Intentional split, or should synthesis also be local?
