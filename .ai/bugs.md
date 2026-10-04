# Bug log
_Append-only: `YYYY-MM-DD · <slug> · <symptom> · <root cause> · <fix commit/PR> · <repro test>`_

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Known issues found while onboarding;
> none fixed yet. Issues are disabled on the GitHub fork, so there is no issue tracker to pull from.
> Upstream's own TODOs live in `TODOS.md` (68 TODO/FIXME/HACK markers in `src/`, 78 test files with
> skipped/todo tests — all upstream, not tracked here).

- 2026-10-04 · local-ollama-provider · 2 upstream tests fail in the live checkout (`test/ai/gateway-chat.test.ts:54`, `:110`) · uncommitted local patch adds a chat touchpoint to `src/core/ai/recipes/ollama.ts`; upstream treats Ollama as embedding-only · open — decide keep or drop · `bun test test/ai/gateway-chat.test.ts`
- 2026-10-04 · page-memory · the bot is told "gbrain auto-extracts entities and builds the graph" (absi-agent-bot `prompts/memory.md:4`), but writes over MCP skip link + timeline extraction · by design upstream (`src/core/operations.ts:950`); nothing scheduled backfills it here · open — the prompt or the setup should change · none
- 2026-10-04 · local-ollama-provider · 1024-dim embedding models (e.g. bge-m3, already pulled) can't be used · the Ollama recipe fixes `default_dims: 768` (`src/core/ai/recipes/ollama.ts:17`) · open, limitation · none
- 2026-10-04 · first-run-setup · `gbrain` CLI commands time out ("Timed out waiting for PGLite lock") while the server runs · PGLite is single-writer; the pm2 server holds the lock · by design — go through MCP or stop the server · none
- 2026-07-09 · mcp-http-server · server crash-looped (338 restarts logged 2026-07-02 → 07-09, last exit code 0xC000026B) · Windows DLL-init failure under the old `run-gbrain.cmd` restart loop (`~/.gbrain/serve-run.log`); now supervised by pm2 (6 restarts since 2026-09-30) · resolved by moving to pm2 (UNCLEAR FROM CODE — confirm) · none
