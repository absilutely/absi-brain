# Journey: absi-brain — main (agent memory over MCP)

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

## What it is / who it's for
absi-brain is Absi's personal, single-owner memory: a fork of upstream gbrain
(github.com/garrytan/gbrain), run as-is on his Windows machine apart from one uncommitted local
patch (see `features/local-ollama-provider/SPEC.md`). Its one real user is Absi's personal agent
bot (absi-agent-bot), which saves and recalls durable facts about Absi's people, projects and
decisions through gbrain's MCP tools over HTTP.

Evidence of the wiring (outside this repo): pm2 app `gbrain` runs `src/cli.ts serve --http
--suppress-bootstrap-token` from this checkout; absi-agent-bot `src/config.js:103` dials
`GBRAIN_MCP_URL` (default `http://localhost:3131/mcp`) with a bearer token.

## The core user journey (the ultimate happy path)
1. The brain server is up → `gbrain serve --http` listens on port 3131 by default
   (`src/commands/serve.ts:83`), bound to loopback `127.0.0.1` unless `--bind` is passed
   (`src/commands/serve-http.ts:407`); `GET /health` answers without auth (`serve-http.ts:753`). Test: `test/serve-http-health.test.ts`.
2. The bot connects with its bearer token → `POST /mcp` is guarded by `requireBearerAuth`
   (`serve-http.ts:1437`) → `src/core/oauth-provider.ts` verifies OAuth tokens, falling back to
   legacy `access_tokens` SHA-256 hashes (`oauth-provider.ts:645`). Legacy tokens are minted with
   `gbrain auth create <name>` (`src/commands/auth.ts:485`) or the admin API (journey `issue-api-key.md`).
   Test: `test/e2e/serve-http-oauth.test.ts` :154 (OAuth token accepted), :782 (legacy-key path), :190 (no header → 401) — Postgres-only.
3. The bot saves a fact → `put_page` writes a markdown page with frontmatter, chunks + embeds it
   (`src/core/operations.ts:725`). Because the call is remote, automatic link + timeline
   extraction is SKIPPED by design (`src/core/operations.ts:950`).
   Tests: `test/put-page-provenance.test.ts`, `test/put-page-namespace.test.ts`.
4. The bot recalls → `search` (cheap hybrid: keyword + vector, no query expansion — keyword-only
   only if the `search.mcp_keyword_only` setting is on, `operations.ts:1414`) or `query` (full
   hybrid with expansion) returns ranked pages (`operations.ts:1391`, `:1450`; `src/core/search/hybrid.ts`).
   Tests: `test/e2e/serve-http-oauth.test.ts:167` (search over MCP, Postgres-only), `test/hybrid-search-lite.serial.test.ts`.
5. The bot asks an open question → `think` gathers evidence and returns a synthesized, cited
   answer from the chat model (`src/core/operations.ts:1792`, `src/core/think/`).
   Test: `test/think-pipeline.serial.test.ts`.
6. **Goal achieved: a later chat recalls what an earlier chat stored** → `get_page` / `search`
   on a new session returns the page written in step 3.
   Test: none found for save-then-recall over MCP (gap) — the independent verifier walks it live.

Confirmed by usage (request log, 2026-10-04): 245 saves and 152 searches vs 21 `think` calls by the bot — save-then-recall
(step 6) is the payoff; `think` is occasional.

_Updated by autobuild (Absi's build skill) when a build changes the core experience. This journey
is what its independent end-to-end verifier walks._
