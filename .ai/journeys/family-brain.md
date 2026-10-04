# Journey: a second, isolated brain (the family brain)

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

The same checkout serves a second brain with its own database, config and tokens.

Evidence: pm2 app `gbrain-family` runs `bun run src/cli.ts serve --http --port 3132
--suppress-bootstrap-token` with `GBRAIN_HOME=C:/Users/absi/.gbrain-family`.

1. Set `GBRAIN_HOME=<dir>` → every config/data path resolves under `<dir>/.gbrain`
   (`src/core/config.ts:1023-1026`; `test/gbrain-home-isolation.test.ts` — relative paths rejected).
2. `gbrain init` + `gbrain auth create` under that home → a separate PGLite file + separate tokens.
3. `gbrain serve --http --port 3132` → a second MCP endpoint, fully separate from :3131.
4. **Goal: nothing written to one brain is ever visible from the other.**

UNCLEAR FROM CODE — confirm: who connects to the family brain on :3132 today? absi-agent-bot's
config says the family tenant's brain is `null` (skipped) (`absi-agent-bot/src/config.js:221`).
