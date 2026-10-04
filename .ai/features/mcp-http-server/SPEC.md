# Spec: MCP server over HTTP (how the bot reaches the brain)

**Status:** shipped (upstream)   ·   **Slug:** mcp-http-server

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
`gbrain serve --http` exposes every brain operation as an MCP tool at `/mcp`, guarded by bearer
tokens (OAuth 2.1 plus legacy `gbrain auth create` tokens), with an unauthenticated `/health`
probe and an admin surface.

## Files
- `src/commands/serve.ts` (default port 3131, :83; `--http` dispatches to serve-http, :74-76)
- `src/commands/serve-http.ts` — the live server: loopback bind by default (:281), `/health` (:753),
  `POST /mcp` behind `requireBearerAuth` (:1437)
- `src/core/oauth-provider.ts` — token verification; legacy `access_tokens` fallback (:645)
- `src/commands/auth.ts` (`gbrain auth create <name>`, :485)
- Operation contract + trust boundary: `src/core/operations.ts` (`remote = true` for MCP callers)
- Legacy, test-only: `src/mcp/http-transport.ts` (superseded per `serve.ts:74-76`)

## Acceptance-shaped behaviors
- Server binds to `127.0.0.1` unless `--bind` is passed (`serve-http.ts:281`); neither pm2 app passes it,
  so both brains are loopback-only.
- `/health` answers without a token (`serve-http.ts:753`); probe logic: `test/serve-http-health.test.ts`
- IF the admin bootstrap token is weak THEN the server refuses to start; `--suppress-bootstrap-token`
  keeps its value out of logs (`serve-http.ts:86`, `:492-506`; `test/serve-http-bootstrap-token.test.ts`)
- Token rules (valid → tools list, revoked → 401, malformed params → `invalid_params`, one
  `mcp_request_log` row per request) are tested in `test/e2e/http-transport.test.ts` — but against the
  LEGACY transport on Postgres, not the `serve-http` path pm2 runs.

## Open questions
- Gap: no end-to-end test found for `serve-http`'s `/mcp` bearer-auth path. Worth adding if this fork
  ever carries changes here.
