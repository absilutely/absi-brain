# Regressions worth guarding

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

Behaviors the bot depends on. Any change (including an upstream sync) must keep these green.

| What would break | Guard |
|---|---|
| Bot can't connect / auth bypass | `test/e2e/http-transport.test.ts` (needs `DATABASE_URL`), `test/serve-http-health.test.ts`, `test/serve-http-bootstrap-token.test.ts` |
| Personal and family brains bleed into each other | `test/gbrain-home-isolation.test.ts` |
| Saved pages lose provenance / namespace | `test/put-page-provenance.test.ts`, `test/put-page-namespace.test.ts` |
| Recall quality drops | `test/hybrid-search-lite.serial.test.ts` |
| `think` breaks | `test/think-pipeline.serial.test.ts` |
| An upgrade can't migrate the existing PGLite brain | `test/apply-migrations.test.ts`, `test/e2e/migrate-chain.test.ts`, `test/e2e/fresh-install-pglite.test.ts` |
| Local embeddings provider drifts | `test/ai/recipes-existing-regression.test.ts`, `test/ai/gateway-chat.test.ts` |

Known red today (live checkout only): the uncommitted Ollama chat patch fails 2 tests in
`test/ai/gateway-chat.test.ts` (lines 54, 110). Clean `master` is unaffected.

Full gates (upstream): `bun run test` (unit), `bun run verify`, `bun run ci:local` (Docker) — see `AGENTS.md`.
