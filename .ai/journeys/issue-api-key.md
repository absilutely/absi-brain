# Journey: give a new agent its own key to a brain

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

How the bot's key was minted (per Absi's agent memory note on absi-brain, 2026-07-02): through the
admin dashboard API rather than the CLI, because the CLI can't open the brain while the server holds
the PGLite lock.

1. Log in to the admin surface with the bootstrap token → `POST /admin/login {token}`
   (`src/commands/serve-http.ts:764`); missing token → 400, wrong token → 401; success sets the
   `gbrain_admin` session cookie.
2. Mint a key → `POST /admin/api/api-keys {name}` (`serve-http.ts:1235`); missing name → 400; returns
   `{ name, token, id }` and stores only the token's hash in `access_tokens`.
3. Hand the `gbrain_…` token to the agent (the bot reads it from its own config) → the agent calls
   `/mcp` with `Authorization: Bearer <token>` (journey `main.md` step 2).
4. **Goal: the new agent can save and recall; nobody else's key changed.**

The same dashboard (`admin/src/pages/`: Dashboard, Agents, Request log, Jobs, Calibration) is served at
`/admin` (`serve-http.ts:1380`). End-to-end coverage (Postgres-only): `test/e2e/serve-http-oauth.test.ts`
(admin SPA served :244, admin stats need the cookie :335).

Confirmed 2026-10-04: the personal brain has exactly one key (`absi-agent-bot`, minted 2026-07-02 via the
dashboard); no other dashboard use was found. The family bot connects with a key named `family-agent`.
