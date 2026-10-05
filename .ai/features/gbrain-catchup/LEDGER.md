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
