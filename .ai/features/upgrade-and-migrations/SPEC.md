# Spec: Upgrade + schema migrations

**Status:** shipped (upstream)   ·   **Slug:** upgrade-and-migrations

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
`gbrain apply-migrations --yes` moves an existing brain's schema forward after
new code lands. `gbrain upgrade` self-updates the CLI (on this checkout: a `git pull --ff-only`, `src/commands/upgrade.ts:26-34`, detection `:662` — avoid; see `.ai/journeys/upstream-sync.md`); `self_upgrade.mode` decides whether gbrain only notifies or upgrades itself.

## Files
- `src/commands/upgrade.ts` (`runUpgrade` :8; `self_upgrade` default `notify` :296-299)
- `src/commands/apply-migrations.ts`, `src/commands/migrations/`, `src/commands/self-upgrade.ts`

## Acceptance-shaped behaviors
- `test/apply-migrations.test.ts`, `test/migrations-registry.test.ts`, `test/migration-resume.test.ts`,
  `test/e2e/fresh-install-pglite.test.ts` (PGLite); `test/e2e/migrate-chain.test.ts` is Postgres-only

## Open questions
- None beyond the fork policy question in `.ai/journeys/upstream-sync.md`.
