# Roadmap: absi-brain (Absi's fork of gbrain)
_Updated by autobuild (Absi's build skill) after each run. Newest at top of each section._

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Sources: git history (fork cloned
> 2026-06-29, 0 own commits, 1,457 behind upstream), the only fork PR (#1, this onboarding), the
> uncommitted diff in the live checkout, file dates under `~/.gbrain` and `~/.gbrain-family`, and the
> known issues in `bugs.md`. This roadmap tracks only fork-level work (running gbrain as Absi's
> memory). Upstream gbrain's own backlog is `TODOS.md` — not duplicated here. There was no root
> `ROADMAP.md` to fold in. The ~295 other branches on the fork are mirrored upstream branches, not fork work.

## Now (in flight)
- [ ] Decide the fate of the uncommitted Ollama chat patch (fails 2 upstream tests) — local-ollama-provider — [spec](features/local-ollama-provider/SPEC.md)

## Next (queued)
- [ ] Catch up with upstream (v0.42.53 → v0.60.45, 1,457 commits) — upgrade-and-migrations — [spec](features/upgrade-and-migrations/SPEC.md)
- [ ] Make the bot's saved pages get graph links (scheduled extract, or fix the bot prompt's claim) — page-memory — [spec](features/page-memory/SPEC.md)
- [ ] Confirm which model answers `think` and pin it explicitly — think-synthesis — [spec](features/think-synthesis/SPEC.md)

## Later (someday / deferred)
- [ ] Allow 1024-dim local embeddings (bge-m3) — local-ollama-provider — [spec](features/local-ollama-provider/SPEC.md)
- [ ] An auth test on PGLite (all end-to-end auth tests need Postgres today) — mcp-http-server — [spec](features/mcp-http-server/SPEC.md)
- [ ] UNCLEAR FROM CODE — confirm: turn on any upstream background work (overnight "dream" cycle, ingestion)? Needs an Anthropic key or the `agent.use_gateway_loop` setting (`src/commands/doctor.ts:2758`) — page-memory — [spec](features/page-memory/SPEC.md)

## Done
- [x] 2026-10-04 — onboarded: `.ai/` memory reconstructed from code — https://github.com/absilutely/absi-brain/pull/1 — [vision](vision.md) (onboarding has no feature spec)
- [x] 2026-07-11 — family brain created (`~/.gbrain-family`, served on :3132 under pm2 `gbrain-family`) — [spec](features/isolated-brain-homes/SPEC.md)
- [x] 2026-07-07 — personal brain moved under pm2 `gbrain` (replacing the `run-gbrain.cmd` loop; up since 2026-09-30) — [spec](features/mcp-http-server/SPEC.md)
- [x] 2026-07-02 — personal brain serving the bot (bot token + admin bootstrap token minted) — [spec](features/admin-dashboard/SPEC.md)
- [x] 2026-06-30 — brain initialised: PGLite + local Ollama embeddings (`~/.gbrain/config.json`) — [spec](features/local-ollama-provider/SPEC.md)
- [x] 2026-06-29 — fork cloned from absilutely/absi-brain at upstream v0.42.53.0 — [spec](features/upgrade-and-migrations/SPEC.md)
