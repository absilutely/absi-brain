# Spec: MCP server over HTTP (how the bot reaches the brain)

**Status:** shipped (upstream)   ·   **Slug:** mcp-http-server

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
`gbrain serve --http` exposes every brain operation as an MCP tool at `/mcp`, guarded by bearer
tokens, with an unauthenticated `/health` probe and an admin surface.

## Files
- `src/commands/serve.ts` (default port 3131, line 83) → `src/commands/serve-http.ts`
- `src/mcp/http-transport.ts` (bearer auth), `src/mcp/server.ts`, `src/mcp/dispatch.ts`, `src/mcp/tool-defs.ts`
- `src/commands/auth.ts` (`gbrain auth create <name>`, line 485)
- Operation contract + trust boundary: `src/core/operations.ts` (`remote = true` for MCP callers)

## Acceptance-shaped behaviors (from tests)
- `/health` → 200 without a token (`test/e2e/http-transport.test.ts` test 1, `test/serve-http-health.test.ts`)
- Valid bearer → `tools/list` returns the op list; `tools/call list_pages` round-trips (e2e tests 2–3)
- IF the token is revoked THEN 401 (e2e test 4)
- Every request logs a row in `mcp_request_log` (e2e test 7)
- IF params are malformed THEN an `invalid_params` error result, not a crash (e2e test 8)
- IF the admin bootstrap token is weak THEN the server refuses to start; `--suppress-bootstrap-token`
  keeps its value out of logs (`src/commands/serve-http.ts:86`, `:492-506`; `test/serve-http-bootstrap-token.test.ts`)

## Open questions
- UNCLEAR FROM CODE — confirm: is :3131 reachable only on localhost, or also over Tailscale?
