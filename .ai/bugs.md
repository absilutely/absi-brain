# Bug log
_Append-only: `YYYY-MM-DD · <slug> · <symptom> · <root cause> · <fix commit/PR> · <repro test>`_

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Known issues found while onboarding;
> none fixed yet. Issues are disabled on the GitHub fork, so there is no issue tracker to pull from.
> Upstream's own TODOs live in `TODOS.md` (68 TODO/FIXME/HACK markers in `src/`, 78 test files with
> skipped/todo tests — all upstream, not tracked here).

- 2026-10-04 · local-ollama-provider · 2 upstream tests fail in the live checkout (`test/ai/gateway-chat.test.ts:54`, `:110`) · uncommitted local patch adds a chat touchpoint to `src/core/ai/recipes/ollama.ts` (this version of upstream treats Ollama as embedding-only); `think` depends on it · kept until the upstream catch-up, backed up on branch `absi-agent/ollama-chat-backup` · `bun test test/ai/gateway-chat.test.ts`
- 2026-10-04 · page-memory · the bot is told "gbrain auto-embeds, auto-extracts entities, and builds the graph" (absi-agent-bot `prompts/memory.md:4`), but writes over MCP skip link + timeline extraction · by design upstream (`src/core/operations.ts:950`); nothing scheduled backfills it here · fix in review: the bot now adds links itself (absi-agent-bot PR #62); 125 pages had 0 links until 2026-10-04 · none
- 2026-10-04 · local-ollama-provider · 1024-dim embedding models (e.g. bge-m3, already pulled) can't be used · the Ollama recipe fixes `default_dims: 768` (`src/core/ai/recipes/ollama.ts:17`) · fixed upstream (per-model dims) — arrives with the catch-up · none
- 2026-10-04 · first-run-setup · `gbrain` CLI commands time out ("Timed out waiting for PGLite lock") while the server runs · PGLite is single-writer; the pm2 server holds the lock · by design — go through MCP or stop the server · none
- 2026-07-09 · mcp-http-server · server crash-looped: 8,490 restarts logged 2026-07-02 → 07-09 under the old `run-gbrain.cmd` loop (`~/.gbrain/serve-run.log`), 8,099 of them exit code 1, ending in a ~30-second burst of 338 exit 0xC000026B (Windows shutting down) on 07-09 · confirmed: every code-1 exit was "Timed out waiting for PGLite lock" (8,099 lines in `~/.gbrain/serve-http.log`) — a second copy of the server kept trying to start while another held the single-writer database · apparently resolved by pm2 (adopted 07-07 personal / 07-11 family; 6 restarts each since, up since 2026-09-30) · none
- 2026-10-04 · think-synthesis · 9 of 21 `think` calls failed (July timeouts, September GPU out-of-memory on the shared RTX 3090) · local qwen3:30b competes with other local models for GPU memory · open — watching; all calls since 2026-09-30 succeeded · none
