---
ai_hash: d6202a6afa9ac278
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: session 2026-09-09 SCRUM-92
status: seedling
tags:
- claude-code
- artifacts
- mcp
- cron
- gotcha
title: Live artifacts need a republish loop, not client-side fetch
type: lesson
---

# Live artifacts need a republish loop, not client-side fetch

A published Claude artifact is static HTML behind a strict CSP: it **cannot** fetch from an external or authenticated host (Jira, an internal API) from the page — the host is blocked and no auth/session is available client-side. So "poll the API every N minutes" cannot run in the browser.

**The workaround — republish loop:** run a Claude Code cron/loop (`CronCreate` every N min, or the `/loop` skill) that:
1. re-fetches the data via MCP/tools,
2. edits the artifact HTML files baked-in data object + a `SYNCED_AT` timestamp,
3. re-publishes to the **same** artifact URL (Artifact tool, same `file_path` → same URL).

The page itself only shows a countdown to the next `SYNCED_AT + N` and calls `location.reload()` to pull the freshly republished snapshot.

**Make it degrade honestly.** The loop is *session-only* (dies when the Claude Code session closes) and recurring crons **auto-expire after 7 days**. So bake a "stale" branch into the page: if `now > SYNCED_AT + N + grace`, flip the sync chip to a "Sync paused" state instead of a live countdown — never imply live data when the loop has stopped. For durability beyond the session, move it to a cloud schedule (`/schedule`).

Data model tip: drive all status rendering from a single JS `DATA` array/object so each loop iteration is a mechanical find-and-replace of status fields + `SYNCED_AT`, then republish.

Surfaced building the SCRUM-92 sprint-dashboard artifact (2026-09-09).

## Related

- [[Claude Artifacts]]
- [[Claude Code cron and loop scheduling]]

%% ai-graph-start %%

**Related notes:**
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]
- [[claude.ai Artifact iframe sandbox blocks data-URI downloads]]
- [[Artifacts render mermaid natively — never add a mermaid CDN script (CSP blocks it)]]

%% ai-graph-end %%