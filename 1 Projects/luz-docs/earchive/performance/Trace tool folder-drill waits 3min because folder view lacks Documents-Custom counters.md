---
ai_hash: d6feee02c540f12f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-21
entities:
- Trace tool
- folder-drill view
- eArchive
- Documents (N) counter
- Custom (M) counter
- collectPageEnter
- Manage access rights
- scenario 4
- trace-earchive.js
- requireCounts param
- folder view header
- back-arrow
- folder name
- K Files
- URL compare
- JS-heap marker
- readRec polling
- full navigation
- 3 minutes
- 90 s poll timeout
- Dev eArchive baseline items in 6s but count badges take 22-41s
- Playwright full-nav detection needs a JS-heap marker not URL compare
- Trace tool folder-drill scenario
- Manage access rights wait
- navigation detection
- clicking a folder
source: trace run 2026-07-21
status: seedling
tags:
- earchive
- playwright
- gotcha
title: Trace tool folder-drill waits 3min because folder view lacks Documents-Custom
  counters
type: lesson
---

# Trace tool folder-drill waits 3min because folder view lacks Documents-Custom counters

In the eArchive folder-drill view there is no "Documents (N)" / "Custom (M)" counter, and clicking a folder is not a full navigation (no "Manage access rights" re-render). The trace tool's collectPageEnter completion check required both counters, so scenario 4 always burned its full 90 s poll timeout — plus another 90 s waiting for "Manage access rights" — making the folder scenario take ~3 minutes of pure timeout on top of real load time.

**Fixed (2026-07-21)** in `trace-earchive.js`: collectPageEnter gained a `requireCounts` param (passed `false` for folder drills, so the done-check drops the two counters), and the "Manage access rights" wait after the folder click was **removed outright** — the folder view header is back-arrow + folder name + "K Files" and never renders that landmark, so gating the wait on navigation detection (tried URL compare, then a JS-heap marker) was moot: the wait can never succeed there. collectPageEnter's readRec polling alone handles readiness, including a full nav re-injecting the harness. Saves ~3 min per run.

## Related

- [[Dev eArchive baseline items in 6s but count badges take 22-41s]]
- [[Playwright full-nav detection needs a JS-heap marker not URL compare]]

%% ai-graph-start %%

**Related notes:**
- [[eArchive company-root and Trash tiles never render K Files badges]]
- [[eArchive counter metrics timed from page-load start to skeleton replacement]]
- [[Playwright full-nav detection needs a JS-heap marker not URL compare]]
- [[Dev eArchive baseline items in 6s but count badges take 22-41s]]
- [[eArchive perf test plan 5 scenarios, all automated by trace tool]]

**Relations:**
- Trace tool folder-drill scenario — *waits* — 3 minutes
- folder-drill view — *lacks* — Documents (N) counter
- folder-drill view — *lacks* — Custom (M) counter
- eArchive — *contains* — folder-drill view
- collectPageEnter — *is a check for* — Trace tool
- collectPageEnter — *required* — Documents (N) counter
- collectPageEnter — *required* — Custom (M) counter
- scenario 4 — *uses* — collectPageEnter
- scenario 4 — *burned* — 90 s poll timeout
- scenario 4 — *included* — Manage access rights wait
- clicking a folder — *is not* — full navigation
- full navigation — *renders* — Manage access rights
- Trace tool folder-drill scenario — *was improved by* — trace-earchive.js
- collectPageEnter — *gained* — requireCounts param
- requireCounts param — *set to* — false
- requireCounts param — *applies to* — folder-drill view
- Manage access rights wait — *was removed from* — Trace tool folder-drill scenario
- folder view header — *contains* — back-arrow
- folder view header — *contains* — folder name
- folder view header — *contains* — K Files
- folder view header — *does not render* — Manage access rights
- navigation detection — *attempted with* — URL compare
- navigation detection — *attempted with* — JS-heap marker
- readRec polling — *handles* — readiness
- readRec polling — *handles* — full navigation
- trace-earchive.js — *saves* — 3 minutes
- Trace tool folder-drill scenario — *related to* — Dev eArchive baseline items in 6s but count badges take 22-41s
- Trace tool folder-drill scenario — *related to* — Playwright full-nav detection needs a JS-heap marker not URL compare

%% ai-graph-end %%