---
title: "404 addresses a missing resource; an empty filter result is a successful query"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Changes to SOB endpoints to align response status code (Arrow)"
tags: [rest, http-status, api-design, breaking-changes, confluence-distilled]
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
