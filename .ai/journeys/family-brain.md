# Journey: a second, isolated brain (the family brain)

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

The same checkout serves a second brain with its own database, config and tokens, used by the
bot's family tenant.

Evidence: pm2 app `gbrain-family` runs `bun run src/cli.ts serve --http --port 3132
--suppress-bootstrap-token` with `GBRAIN_HOME=~/.gbrain-family`;
absi-agent-bot `tenants/family.json` sets `brain.url = http://localhost:3132/mcp` with its own token.
(The comment at absi-agent-bot `src/config.js:221` saying the family brain is `null`/skipped is stale.)

1. Set `GBRAIN_HOME=<dir>` → every config/data path resolves under `<dir>/.gbrain`
   (`src/core/config.ts:1023-1026`; `test/gbrain-home-isolation.test.ts` — relative paths rejected).
2. `gbrain init` + `gbrain auth create` under that home → a separate PGLite file + separate tokens.
3. `gbrain serve --http --port 3132` → a second MCP endpoint, fully separate from :3131.
4. The family tenant of the bot saves/recalls through :3132 exactly as in `main.md`.
5. **Goal: nothing written to one brain is ever visible from the other.**

Confirmed 2026-10-04: it backs the bot's family tenant — a shared household chat — and is barely used so far
(1 page, 0 links; its key is named `family-agent`).
