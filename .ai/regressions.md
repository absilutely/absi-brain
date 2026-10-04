# Regressions worth guarding

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

Behaviors the bot depends on. Any change (including an upstream sync) must keep these green.

| What would break | Guard |
|---|---|
| Server leaks its admin token / starts with a weak one | `test/serve-http-bootstrap-token.test.ts` |
| Health probe changes shape | `test/serve-http-health.test.ts` |
| Token auth rules (revoked → 401 etc.) | `test/e2e/http-transport.test.ts` — legacy transport, Postgres-only (needs `DATABASE_URL`); does NOT cover the `serve-http` path pm2 runs (gap) |
| Personal and family brains bleed into each other | `test/gbrain-home-isolation.test.ts` |
| Saved pages lose provenance / namespace | `test/put-page-provenance.test.ts`, `test/put-page-namespace.test.ts` |
| Keyword search path changes | `test/hybrid-search-lite.serial.test.ts` (keyword-only; vector recall is unguarded on PGLite) |
| `think` breaks | `test/think-pipeline.serial.test.ts` |
| An upgrade can't migrate the existing brain | `test/apply-migrations.test.ts`, `test/migration-resume.test.ts`, `test/e2e/fresh-install-pglite.test.ts` (PGLite); `test/e2e/migrate-chain.test.ts` is Postgres-only |
| Local embeddings provider drifts | `test/ai/recipes-existing-regression.test.ts`, `test/ai/gateway-chat.test.ts` |

Known red today (live checkout only): the uncommitted Ollama chat patch fails 2 tests in
`test/ai/gateway-chat.test.ts` (lines 54, 110). Clean `master` is unaffected.

Full gates (upstream): `bun run test` (unit), `bun run verify`, `bun run ci:local` (Docker) — see `AGENTS.md`.
Any edit to `AGENTS.md` / `CLAUDE.md` must be followed by `bun run build:llms` (`test/build-llms.test.ts`).
