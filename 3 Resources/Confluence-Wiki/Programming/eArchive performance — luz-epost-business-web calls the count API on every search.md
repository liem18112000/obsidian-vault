---
title: "eArchive performance — luz-epost-business-web calls the count API on every search"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49492099078/eArchive+performance+luz-epost-business-web+calls+the+count+API+on+every+search
space: "TK"
topic: programming
relevance: 0.755
depth: 2.81
updated: 2026-06-10
attachments: 0
tags:
  - confluence
  - programming
  - space/tk
---

# eArchive performance — luz-epost-business-web calls the count API on every search

> [!info] Imported from Confluence
> Space **TK** · updated 2026-06-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49492099078/eArchive+performance+luz-epost-business-web+calls+the+count+API+on+every+search)
> Relevance 0.755 · topic `programming`

<div hasbody="true" macro-id="11144835-31cc-46e1-b3a5-5609b9e5f45e" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

For **Team Miracle** (owner of `luz_epost_business_web`) + PO. The backend eArchive performance work (Kepler, `luz-docs`) is landing, but a frontend integration issue is largely cancelling out the gains. **Verified in the** **`luz_epost_business_web`** **code (master, 2026-06-10)** — exact location and mechanism below.

</div>

</div>

## Background — the backend change (done)

- Recent eArchive performance work focused on the backend (`luz-docs`): a **new search approach** + **optimized data structure**, giving significant performance gains.

- A key finding: **combining search and count in a single request** was inefficient and a major contributor to response times. So the two were **split into separate APIs**: a fast **search API**, and a separate **count API** for the total record count when needed.

- All consuming applications were asked to adopt this split; other clients have done so successfully.

## The problem — verified in the code

<div hasbody="true" macro-id="994d0a70-5d85-4d69-99f7-51904280f1cf" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

**Confirmed:** `luz_epost_business_web` fires the count API on **every** search and **blocks the search response on it**. The split is implemented, but joined back synchronously — so every search is still as slow as the count.

</div>

</div>

**Where:** `src/ch/klara/luz/epost/business/letterbox/repository/rest/LetterRestClient.java` — method `search(LetterLookupOptions)` (the only place in the repo with this pattern):

- It submits **two futures in parallel**: `searchLetterAsync(...)` (the fast search, correctly called with `exclude-total-count=true`) and `getTotalSearchLetter(...)` (POST to the `letters/count` endpoint on `luz_docs_view_controller`, proxying the `luz-docs` count API, with the same filter set).

- Then `AsyncFunctionUtil.waitAsyncFunctions(...)` **waits for BOTH futures** before building the result (`getLetterSearchResult(letterSearchTotalCount.get())`). Effective response time = max(search, count) ≈ **the slow count**, on every single search.

- The code's own comment marks this as the LUZ-142076 performance change ("1 add exclude-total-count for query faster, 2 call other api get total count") — the split was adopted, but the synchronous join cancels the benefit.

**Multiple searches per page — also confirmed:** three use-case interactors all run searches through this same repository port, each triggering its own paired count: `LetterSearchingInteractor`, `KlaraBusinessAGStorageLetterReadingInteractor`, `DeletedFromStorageLetterReadingInteractor` (all in `…/letterbox/core/usecase/`). So one page load can pay the count penalty several times over.

## Why it matters

The point of the split is to **render results from the fast search immediately and pay for the count only when the total is actually needed** (totals display / pagination). Blocking every search on the count — multiplied across the page's several searches — re-introduces exactly the slow path the backend work removed, and largely negates the eArchive performance gains for this module.

## The ask — for Miracle

- <span class="placeholder-inline-tasks">In `LetterRestClient.search(...)`: **do not block the search result on the count future** — return the search response as soon as it arrives; resolve the count independently/lazily.</span>
- <span class="placeholder-inline-tasks">Call `getTotalSearchLetter(...)`**only when the total is actually needed** (totals display / pagination), not unconditionally per search.</span>
- <span class="placeholder-inline-tasks">Across the multi-search page flow (the three interactors above), **avoid one count per search** — fetch the count once per filter-set (or debounce/dedupe).</span>

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

**Verification.** Confirmed against `axonivy-prod/luz_epost_business_web` (`master`, 2026-06-10): `LetterRestClient.search()` / `getTotalSearchLetter()` / `addLetterQueryParameters()` (sets `exclude-total-count=true`), plus the three calling interactors. A code-search confirms the count-pairing exists only in `LetterRestClient` — i.e. one precise fix point. Background as reported by the Kepler / `luz-docs` backend team.

</div>

</div>
