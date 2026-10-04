# Design system: absi-brain

> ⚠ RECONSTRUCTED from code on 2026-10-04 — needs confirmation

This fork has no UI of its own. The only UI is upstream gbrain's admin dashboard (`admin/`, a React +
Vite single-page app served at `/admin` by `src/commands/serve-http.ts:1380`), and its design system
is owned upstream — this file points to it rather than copying it, so upstream syncs stay clean.

- Source of truth: root `DESIGN.md` (voice, color tokens, type, components) and `admin/DESIGN.md`.
- Tokens in code: CSS variables in `admin/src/index.css` (dark theme, e.g. `--bg-primary: #0a0a0f`).
- Screens: `admin/src/pages/` — `Dashboard.tsx`, `Agents.tsx`, `RequestLog.tsx`, `JobsWatch.tsx`,
  `Calibration.tsx`, `Login.tsx`.

Rule for this fork: don't restyle the dashboard here. If a UI change is ever needed, do it upstream
or as a deliberate, documented fork patch.
