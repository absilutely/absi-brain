# Spec: Admin dashboard + API keys

**Status:** shipped (upstream)   ·   **Slug:** admin-dashboard

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Upstream gbrain feature, used as-is by this fork.

## What it does
A web dashboard at `/admin` on each brain server: log in with the bootstrap token, mint and list API
keys, see connected agents, the request log, background jobs and calibration. On this machine it is
how agent keys get minted (journey `.ai/journeys/issue-api-key.md`).

## Files
- UI: `admin/src/` (`App.tsx`, `api.ts`, `pages/*.tsx`), built into `admin/dist` / `src/admin-embedded.ts`
  (`bun run build:admin`)
- Server routes: `src/commands/serve-http.ts` — `POST /admin/login` (:764), `GET|POST /admin/api/api-keys`
  (:1222, :1235), static SPA (:1380)
- Design: root `DESIGN.md`, `admin/DESIGN.md` (see `.ai/design/DESIGN.md`)

## Acceptance-shaped behaviors
- IF login has no token THEN 400; IF the token is wrong THEN 401; success sets the `gbrain_admin` cookie (:764-781)
- IF a key request has no name THEN 400; success returns the token once and stores only its hash (:1235-1247)
- Admin SPA and sub-routes are served (`test/e2e/serve-http-oauth.test.ts:244`, `:251`); admin stats need
  the cookie (`:335`, `:342`) — Postgres-only
- Spend view: `test/admin-agents-spend.test.ts`

## Open questions
- See `.ai/journeys/issue-api-key.md`.
