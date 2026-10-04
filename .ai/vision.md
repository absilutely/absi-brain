# Vision: absi-brain

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation. Sources: upstream `README.md`
> (gbrain's own pitch), the bot's memory prompt (absi-agent-bot `prompts/memory.md`), the live wiring
> (pm2 apps `gbrain` / `gbrain-family`, `~/.gbrain/config.json`, absi-agent-bot `src/config.js:103`,
> `tenants/family.json`), and git history (cloned 2026-06-29, zero fork commits since).

## Vision (the world if it succeeds)
Absi's agents never forget. Anything he or his bot learned once — about a person, a project, a
decision, his health or travel — is there the next time, in any chat, on any channel, without him
re-explaining it.

## Mission (what it does, for whom)
Run upstream gbrain unchanged, on Absi's own machine, as the durable private memory behind his
personal agent bot: the bot saves facts as pages over MCP (`put_page`) and gets them back with
search and cited synthesis (`search` / `query` / `think`). A second, fully separate brain does the
same for the bot's family setup.

## The problem it solves
Chat sessions forget. Before gbrain the bot kept memory in a plain markdown folder (`C:\ai\code\brain`,
now a frozen backup per `prompts/memory.md:2`) with no search ranking, no embeddings, no synthesis.

## Who it's for
- Absi, through his personal agent bot (absi-agent-bot) — the main brain's user on :3131 (UNCLEAR FROM CODE — confirm: is it the only writer? `/admin/api/api-keys` lists every key).
- The bot's family tenant — its own brain on :3132, never mixed with Absi's.

## Non-goals
- Not a product of its own: features come from upstream gbrain; this fork does not develop gbrain.
- Not exposed to the network: both servers bind to loopback only (`src/commands/serve-http.ts:407`).
- Not a shared brain: personal and family memory never merge (`GBRAIN_HOME` isolation).
- UNCLEAR FROM CODE — confirm: none of upstream's ingestion daemons (meetings, email, the overnight
  "dream" cycle) run here. Deliberate non-goal, or just not set up yet?

## How success would show
- A fact saved in one chat is recalled correctly in a later, unrelated chat (journey `journeys/main.md`).
- Both servers stay up (`/health` 200 on :3131 and :3132) and the bot never hits "not connected".
- Upstream updates can be pulled in without breaking the bot (journey `journeys/upstream-sync.md`).
- Cost stays near zero: embeddings are local Ollama; only answer-writing uses a paid model.
