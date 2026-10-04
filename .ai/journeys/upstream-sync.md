# Journey: keep the fork current with upstream gbrain

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

`origin` = github.com/absilutely/absi-brain, `upstream` = github.com/garrytan/gbrain. As of
2026-10-04 `origin/master` has **0 commits of its own** and is **1,457 commits behind** upstream
(local checkout at v0.42.53.0; upstream is at v0.60.45.0).

1. `git fetch upstream` → see what's new (an "available updates" report was produced on
   2026-10-04 in the live checkout's gitignored `.temp/`).
2. Merge/fast-forward upstream into a branch → PR → merge to `master` → pull into the live checkout.
3. `bun install` + `gbrain apply-migrations --yes` → schema migrated
   (`src/commands/apply-migrations.ts`; `test/apply-migrations.test.ts`, `test/migration-resume.test.ts`).
   PGLite is single-writer, so stop the pm2 servers first (see `first-run-setup.md`).
4. `pm2 restart gbrain gbrain-family` → both servers on the new version.
5. **Goal: the bot keeps working (journey `main.md` walks green) on the new version.**

⚠ Do NOT use `gbrain upgrade` here: it self-updates the CLI, and because this checkout's remotes
include `garrytan/gbrain` it is treated as a linked dev checkout and runs `git pull --ff-only` on
the live checkout (`src/commands/upgrade.ts:26-34`, detection `:662`) — bypassing the branch → PR flow in step 2.

Config today: `self_upgrade.mode = notify` (`~/.gbrain/config.json`; default set at
`src/commands/upgrade.ts:296-299`) — gbrain only notifies, it never self-upgrades.

UNCLEAR FROM CODE — confirm: do you intend to stay a pure mirror (only ever take upstream), or
carry your own patches (like the uncommitted Ollama chat change)?
