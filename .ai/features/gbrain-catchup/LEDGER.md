# LEDGER: gbrain-catchup

- 2026-10-05 Worktree created by autobuild (branch absi-agent/gbrain-catchup off origin/master, v0.42.53).
- 2026-10-05 Target re-pinned: upstream v0.60.48 (was 0.60.45 in the first report). Schema 119 -> 199.
- 2026-10-05 Auto-resolved: fast-forward to upstream (fork has no own commits); drop the Ollama chat patch
  (upstream ships it; patch kept on backup branch); rehearse on DB copies with a side-by-side Bun >= 1.4.
- 2026-10-05 Removed from scope: graph-links fix (already fixed on the bot side); optional connectors (own runs);
  repo onboarding (after the upgrade, on the new code).
- 2026-10-05 Charter brief sent for approval. Waiting on: approval, cutover authorization + timing, auto_chronicle, follow-ups.
- 2026-10-05 APPROVED by owner. Cutover: autonomous once every rehearsal check passes, auto-rollback on failure.
  Timing: as soon as rehearsal passes. auto_chronicle: ON via the brain's local model (verify local in rehearsal;
  if it would route to a paid model, keep it OFF and report). Follow-ups for the roadmap: repo onboarding on the
  new code; Gmail open-loop ("who is waiting on me"); ChatGPT/Claude history connectors.
- 2026-10-05 Target moved to upstream v0.60.64 (latest at rebase). Schema 119 -> 207 (88 migrations).
- 2026-10-05 M1 PASS: backups taken with each server paused; copies opened by the old version: personal 131 pages, family 1; search OK.
- 2026-10-05 M2: branch = upstream master + .ai/ only. Typecheck PASS on Bun 1.4.2. Full unit suite is Linux-targeted; a Windows run
  showed 66+ failures in connector-recovery / symlink / isolated-install tests before it was cut by a host restart. Gate decision
  (auto-resolved): the code gate is upstream CI on the exact commit (111 checks: 93 success, 18 skipped, 0 failed, incl. win32-x64 Bun 1.4.2);
  this machine is proven by the M3 rehearsal journey instead.
- 2026-10-05 M3 PASS: all migrations applied on copies; doctor 0 errors; counts equal (personal 131 pages/174 chunks/346 tags/24 timeline;
  family 1/1/2/0); 5/5 known searches hit; save->find OK on both; brains isolated; think answered via ollama:qwen3:30b-a3b.
  Required post-upgrade steps found: `repair safe-chunks --apply` (else remote search withholds old pages) + `projections drain`.
- 2026-10-05 auto_chronicle: personal ON (reasoning tier = local model, $0). Family OFF: no local model configured there, its
  reasoning tier resolves to a paid OpenAI model; owner's choice was "on, free" -> keep off and report.
- 2026-10-05 Cutover approach: upgrade the system Bun in place (old exe kept beside it for rollback) rather than a pinned side copy —
  one runtime, no bot-repo change. Watchdog task already disabled, so no restart race during the window.
- 2026-10-05 PR #2 merged (merge commit). M4 cutover PASS (run as a one-shot pm2 job so a host restart could not cut it):
  fresh backup, code 0.60.64.0, system Bun 1.3.13 -> 1.4.2 in place (old exe kept as bun-1.3.13.exe), migrations +
  safe-chunks + projections on both brains, restart, live journey: counts 132/1 unchanged, search, save, isolation, think on
  local model. ~7 min of memory downtime. No rollback needed.
- 2026-10-05 Behavior change found live: updating/deleting an EXISTING page now needs `expected_revision` (from get_page);
  `request_id` must be a UUID. The agent adapts from the tool description/error; no fork change.
- 2026-10-05 Independent verification (separate agent, real bot conversation via a test topic): ALL PASS 9/9 — versions,
  nothing lost, 5/5 old memories, local think, bot save, bot update of an existing page (sent expected_revision unprompted),
  bot recall, family isolation, cleanup. Note: the bot's first put_page used a non-UUID request_id, retried and succeeded.
- 2026-10-05 Follow-ups added to .ai/roadmap.md per owner's picks.
