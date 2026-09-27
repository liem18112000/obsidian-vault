---
ai_hash: f18159fe8f8a1220
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- '404'
- missing resource
- empty filter result
- successful query
- GET /dossiers?onboardingKey=xxx
- '204'
- endpoint
- lookup-by-filter
- dossier not found
- empty data
- collection
- filter
- URL
- query matched nothing
- endpoint does not exist
- client code
- ambiguity
- typo'd path
- empty result set
- GET /dossiers/{id}
- one specific resource
- id does not exist
- GET /things/{id}
- GET /things?filter=...
- filtered collection
- 200 with []
- 200 with empty collection
- 204 No Content
- no body
- pagination metadata
- breaking change
- existing clients
- bug
- status code change
- endpoint migration
- Reject input you cannot fully handle; never silently drop part of it
- Bulk operations need per-item outcomes, not one status code
- Changes to SOB endpoints to align the response status code (FA-6800)
- Arrow
- Confluence
source: 'Confluence: Changes to SOB endpoints to align response status code (Arrow)'
status: seedling
tags:
- rest
- http-status
- api-design
- breaking-changes
- confluence-distilled
title: 404 addresses a missing resource; an empty filter result is a successful query
type: lesson
---

# 404 addresses a missing resource; an empty filter result is a successful query

`GET /dossiers?onboardingKey=xxx` finds nothing. Is that **404** or **204**? The endpoint here was deliberately changed from the first to the second, and the reasoning generalises to every lookup-by-filter.

> Existing behavior: dossier not found → **404**
> New behavior: dossier not found → **204 + empty data**

**The argument for 204 (or 200 with an empty body).** The URL identifies a **collection** (`/dossiers`) with a filter applied. That collection exists and is perfectly reachable; it simply has no members matching your filter. "No results" is a normal, successful outcome of a query — not a missing resource. Returning 404 conflates *"your query matched nothing"* with *"this endpoint does not exist"*, which is exactly the ambiguity that makes client code brittle: a typo'd path and an empty result set become indistinguishable.

**When 404 is still right.** `GET /dossiers/{id}` addresses **one specific resource**. If that id does not exist, the thing you asked for is genuinely not there, and 404 is correct.

**The rule of thumb:**

| Shape | Nothing found → |
|---|---|
| `GET /things/{id}` — addresses one resource | **404** |
| `GET /things?filter=…` — addresses a filtered collection | **200 with `[]`**, or **204** |

> [!tip] Prefer 200 + empty collection over 204
> `204 No Content` means *no body at all*, so clients must special-case it before parsing. `200` with `{"data": []}` keeps one response shape for every successful query — parse once, check length. It also leaves room for pagination metadata alongside the empty set. 204 is defensible; 200-with-empty is usually kinder to callers.

> [!warning] Changing a status code is a breaking change, even toward the "correct" one
> Existing clients almost certainly branch on 404 to mean "not found". Switching to 204 silently routes that case into their success path — and the bug appears in *their* code, not yours. Version the endpoint, or announce and coordinate it. The ticket estimating this at 5 points is accounting for the migration, not the `if` statement.

Related: [[Reject input you cannot fully handle; never silently drop part of it]] · [[Bulk operations need per-item outcomes, not one status code]].

Source: [[Changes to SOB endpoints to align the response status code (FA-6800)]] (Arrow, Confluence).

## Related

- [[Reject input you cannot fully handle; never silently drop part of it]]

%% ai-graph-start %%

**Related notes:**
- [[Changes to SOB endpoints to align the response status code (FA-6800)]]
- [[Bulk operations need per-item outcomes, not one status code]]
- [[luz-jsonstore find returns 200 empty string, not [], on zero matches]]
- [[Reject input you cannot fully handle; never silently drop part of it]]
- [[luz-docs getDocumentById returns empty object not null for missing docs]]

**Relations:**
- 404 — *addresses* — missing resource
- empty filter result — *is* — successful query
- GET /dossiers?onboardingKey=xxx — *finds* — nothing
- endpoint — *changed from* — 404
- endpoint — *changed to* — 204
- reasoning — *generalises to* — lookup-by-filter
- dossier not found — *existing behavior results in* — 404
- dossier not found — *new behavior results in* — 204
- dossier not found — *new behavior includes* — empty data
- URL — *identifies* — collection
- collection — *has* — filter
- collection — *is* — reachable
- collection — *has no members matching* — filter
- empty filter result — *is* — normal outcome of query
- 404 — *conflates* — query matched nothing
- 404 — *conflates* — endpoint does not exist
- ambiguity — *makes* — client code
- typo'd path — *becomes indistinguishable from* — empty result set
- GET /dossiers/{id} — *addresses* — one specific resource
- id does not exist — *implies* — missing resource
- 404 — *is correct for* — missing one specific resource
- GET /things/{id} — *addresses* — one resource
- GET /things/{id} — *nothing found results in* — 404
- GET /things?filter=... — *addresses* — filtered collection
- GET /things?filter=... — *nothing found results in* — 200 with []
- GET /things?filter=... — *nothing found results in* — 204
- 200 with empty collection — *preferred over* — 204
- 204 No Content — *means* — no body
- client code — *must special-case* — 204 No Content
- 200 with empty collection — *keeps* — one response shape
- 200 with empty collection — *allows for* — pagination metadata
- 200 with empty collection — *is* — kinder to callers
- 204 — *is* — defensible
- status code change — *is a* — breaking change
- existing clients — *branch on* — 404
- switching to 204 — *routes into* — successful query
- bug — *appears in* — client code
- breaking change — *requires* — endpoint migration
- current note — *related to* — Reject input you cannot fully handle; never silently drop part of it
- current note — *related to* — Bulk operations need per-item outcomes, not one status code
- current note — *sourced from* — Changes to SOB endpoints to align the response status code (FA-6800)
- Changes to SOB endpoints to align the response status code (FA-6800) — *from* — Arrow
- Changes to SOB endpoints to align the response status code (FA-6800) — *uses* — Confluence

%% ai-graph-end %%