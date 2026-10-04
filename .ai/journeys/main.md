# Journey: absi-brain — main (agent memory over MCP)

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

## What it is / who it's for
absi-brain is Absi's personal, single-owner memory: a fork of upstream gbrain
(github.com/garrytan/gbrain) run unmodified on his Windows machine. Its one real user is
Absi's personal agent bot (absi-agent-bot), which saves and recalls durable facts about Absi's
people, projects and decisions through gbrain's MCP tools over HTTP.

Evidence of the wiring (outside this repo): pm2 app `gbrain` runs `src/cli.ts serve --http
--suppress-bootstrap-token` from this checkout; absi-agent-bot `src/config.js:103` dials
`GBRAIN_MCP_URL` (default `http://localhost:3131/mcp`) with a bearer token.

## The core user journey (the ultimate happy path)
1. The brain server is up → `gbrain serve --http` listens on port 3131 by default
   (`src/commands/serve.ts:83`); `GET /health` answers 200 without auth
   (`test/e2e/http-transport.test.ts` test 1, `test/serve-http-health.test.ts`).
2. The bot connects with its bearer token → `/mcp` `tools/list` returns the operation list
   (`src/mcp/http-transport.ts:307`, token checked against SHA-256 hashes in `access_tokens`;
   `test/e2e/http-transport.test.ts` test 2). Tokens are minted with `gbrain auth create <name>`
   (`src/commands/auth.ts:485`).
3. The bot saves a fact → `put_page` writes a markdown page with frontmatter, chunks + embeds it
   (`src/core/operations.ts:725`). Because the call is remote, automatic link + timeline
   extraction is SKIPPED by design (`src/core/operations.ts:950`).
4. The bot recalls → `search` (keyword) or `query` (hybrid keyword + vector) returns ranked
   pages (`src/core/operations.ts:1391`, `:1450`; `src/core/search/hybrid.ts`).
5. The bot asks an open question → `think` gathers evidence and returns a synthesized, cited
   answer using the configured chat model (`src/core/operations.ts:1792`, `src/core/think/`).
6. **Goal achieved: a later chat recalls what an earlier chat stored** → `get_page` / `search`
   on a new session returns the page written in step 3.

UNCLEAR FROM CODE — confirm: is step 6 (cross-session recall) the payoff you'd judge this by,
or is `think` (step 5) the one that matters most?

_Updated by autobuild when a build changes the core experience. This journey is what 5b verification walks._
