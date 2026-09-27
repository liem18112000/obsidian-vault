---
ai_hash: af1be52a0edb94e6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- 'Vinnstack exe won''t open without a pre-set databaseUrl (chicken-and-egg: it says
  open Settings but quits first)'
- Vinnstack exe won't open without a pre-set databaseUrl (chicken-and-egg it says
  open Settings but quits first)
created: 2026-07-15
entities:
- Vinnstack
- desktop app
- DB
- electron/main.js
- startNextServer()
- config.json.databaseUrl
- process.env.DATABASE_URL
- Settings
- whenReady catch
- fail() dialog
- app.quit()
- Windows 10/11
- lib/core/db.ts
- pool()
- DATABASE_URL
- Interrogation Room
- Chat
- Skills
- Notebook
- Graphify
- Ultracode
- Polaris
- clean-machine sim
- env -u DATABASE_URL
- HTTP 200
- startup log
- cloud-sql-proxy
- gcloud ADC
- PATH
- claude CLI
- AI chat
- gcloud
- Cloud SQL
- DB features
- UI
- optional backend
- Vinnstack exe
- port 3001
- portable stub
- ad-hoc env
- ELECTRON_RUN_AS_NODE=1
- Electron exe
- Node
- database connection
- app window
- error throw
- startup gate
- Fix
source: session 2026-07-15
status: seedling
tags:
- vinnstack
- electron
- startup
- resilience
- clean-install
- database
- ux-bug
title: 'Open-and-degrade beats hard-quit: let a desktop app start without its optional
  DB (Vinnstack clean-Win fix)'
type: lesson
---

# Open-and-degrade beats hard-quit: let a desktop app start without its optional DB (Vinnstack clean-Win fix)

**Bug:** `electron/main.js` `startNextServer()` THREW "No database connection string configured yet. Open Settings and set the database connection (Advanced), then relaunch." (main.js:183) when neither `config.json.databaseUrl` nor `process.env.DATABASE_URL` was set. The throw hit the `whenReady` catch → `fail()` dialog → `app.quit()`. Chicken-and-egg: it told the user to open Settings while quitting, so on a clean Windows 10/11 box (no config, no ambient env) the window never opened and Settings was unreachable.

**Fix (2026-07-15):** `lib/core/db.ts` is LAZY (its `pool()` only connects on the first query), so the startup gate was unnecessary — just skip setting `DATABASE_URL` when absent and open the app. DB-backed features (Interrogation Room) surface a clear error when used; Chat, Skills, Notebook, Graphify, Ultracode and Polaris all work without a DB. Verified on a clean-machine sim (`env -u DATABASE_URL`, no config, no proxy): HTTP 200 + full `startNextServer` happy path in the startup log.

**Testing gotcha that hid this:** an ambient `DATABASE_URL` in the shell made the gate pass on every launch — always test first-run with `env -u DATABASE_URL`.

Other clean-Win prerequisites are runtime/feature-level, NOT startup blockers, and degrade gracefully: cloud-sql-proxy is auto-started best-effort (needs gcloud ADC + the binary on PATH; skipped if absent); the `claude` CLI is needed for AI chat/ultracode; gcloud + Cloud SQL for DB features. The window opens regardless.

**Principle:** a desktop app should OPEN and degrade, never hard-quit before the UI, on a missing optional backend — especially when the remedy (Settings) lives inside that same UI.

## Related

- [[Testing the packaged Vinnstack exe needs databaseUrl in config.json, pins port 3001, portable stub doesn't inherit ad-hoc env]]
- [[ELECTRON_RUN_AS_NODE=1 in the env makes an Electron exe run as Node and 'look broken' — check/clear it before testing]]

%% ai-graph-start %%

**Related notes:**
- [[Testing the packaged Vinnstack exe needs databaseUrl in config.json, pins port 3001, portable stub doesn't inherit ad-hoc env]]
- [[An Electron GUI app can't be smoke-tested from a non-interactive automation session]]
- [[Dead bundling config outlives the runtime code that read it]]
- [[Unsigned Electron app first-launch transient Cannot find module during Defender post-install scan]]
- [[Do not hardcode a real DB password as a source-code fallback for a packaged desktop app]]

**Relations:**
- Vinnstack — *is a* — desktop app
- desktop app — *should start without* — optional backend
- startNextServer() — *is part of* — electron/main.js
- startNextServer() — *throws error if* — config.json.databaseUrl
- startNextServer() — *throws error if* — process.env.DATABASE_URL
- error throw — *hits* — whenReady catch
- whenReady catch — *triggers* — fail() dialog
- fail() dialog — *triggers* — app.quit()
- app.quit() — *prevents access to* — Settings
- Settings — *is unreachable on* — Windows 10/11
- Settings — *configures* — database connection
- pool() — *is part of* — lib/core/db.ts
- lib/core/db.ts — *is* — LAZY
- pool() — *connects on* — first query
- Fix — *skips setting* — DATABASE_URL
- Fix — *allows* — desktop app
- Interrogation Room — *is a* — DB-backed feature
- Interrogation Room — *surfaces error if* — DB
- Chat — *works without* — DB
- Skills — *works without* — DB
- Notebook — *works without* — DB
- Graphify — *works without* — DB
- Ultracode — *works without* — DB
- Polaris — *works without* — DB
- Fix — *verified on* — clean-machine sim
- clean-machine sim — *uses* — env -u DATABASE_URL
- Fix — *resulted in* — HTTP 200
- Fix — *resulted in* — startNextServer
- startNextServer — *happy path in* — startup log
- ambient DATABASE_URL — *caused* — startup gate
- startup gate — *to pass* — null
- env -u DATABASE_URL — *is used for* — first-run testing
- cloud-sql-proxy — *requires* — gcloud ADC
- cloud-sql-proxy — *requires* — PATH
- claude CLI — *requires* — AI chat
- claude CLI — *requires* — Ultracode
- gcloud — *requires* — DB features
- Cloud SQL — *requires* — DB features
- app window — *opens regardless of* — prerequisites
- desktop app — *should open and* — degrade
- desktop app — *should not hard-quit before* — UI
- Settings — *lives inside* — UI
- Vinnstack exe — *needs* — config.json.databaseUrl
- Vinnstack exe — *pins* — port 3001
- portable stub — *does not inherit* — ad-hoc env
- ELECTRON_RUN_AS_NODE=1 — *makes* — Electron exe
- Electron exe — *run as* — Node
- Ultracode — *is a* — feature
- AI chat — *is a* — feature
- DB features — *are* — features
- config.json.databaseUrl — *is a* — configuration parameter
- process.env.DATABASE_URL — *is an* — environment variable
- DATABASE_URL — *is an* — environment variable
- app window — *is a* — UI component
- UI — *contains* — Settings
- DB — *is an* — optional backend
- database connection — *is a* — configuration
- Fix — *is a* — solution
- error throw — *is a* — bug component
- startup gate — *is a* — mechanism
- desktop app — *has* — app window

%% ai-graph-end %%