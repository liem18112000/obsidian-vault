---
title: "Full‑Text Document Search — Performance Analysis & Proposals"
created: 2026-06-25
updated: 2026-06-25
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49533517825/Full+Text+Document+Search+Performance+Analysis+Proposals
confluence_id: "49533517825"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, performance, search]
---

# Full‑Text Document Search — Performance Analysis & Proposals

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49533517825/Full+Text+Document+Search+Performance+Analysis+Proposals) · updated 2026-06-25*

## Overview

**Goal:** make `POST /luz_docs/api/{tenant}/documents/search` (full‑text mode) return in **\< 1 s** on a tenant with **~128 000 documents**.

**Scope of this document**

1.  A detailed code walk‑through of how full‑text search works today.

2.  A root‑cause analysis of why it is slow at 128 k docs.

3.  A catalogue of *every* viable solution, each with mechanism, expected latency, cost, risk, and a diagram.

4.  A recommended, phased roadmap and a validation/benchmark plan.

> Deployment fact that constrains the options: Luz Mongo is **self‑hosted MongoDB replica sets** (`luz-mongodbNN-cluster-rs-{0,1,2}` on GKE), **not** MongoDB Atlas. So Atlas Search is only available *after* a migration to Atlas; the self‑hosted options (`$text`, n‑gram field, external engine) are the realistic near‑term levers.

## The request

```
{
  "query": { "$and": [
    { "$and": [
      { "$and": [ { "$or": [
        { "title":               { "$regex": "^.*traw.*$", "$options": "i" } },
        { "documentTitle":       { "$regex": "^.*traw.*$", "$options": "i" } },
        { "documentDescription": { "$regex": "^.*traw.*$", "$options": "i" } },
        { "senderName":          { "$regex": "^.*traw.*$", "$options": "i" } },
        { "documentTextContent": { "$regex": "^.*traw.*$", "$options": "i" } },
        { "tags":                { "$regex": "^.*traw.*$", "$options": "i" } },
        { "documentTypes":       { "$regex": "^.*traw.*$", "$options": "i" } },
        { "subject":             { "$regex": "^.*traw.*$", "$options": "i" } },
        { "senderEndToEndId":    { "$regex": "^.*traw.*$", "$options": "i" } },
        { "fileName":            { "$regex": "^.*traw.*$", "$options": "i" } }
      ] } ] },
      { "$or": [ { "folderIds": { "$regex": "^6a04490ccc937202f8258b4a$", "$options": "i" } } ] },
      { "letterInfo.mediaType": { "$nin": [ "...draft...", "...template...", "...receipt..." ] } }
    ] },
    { "_isBeingCreated": { "$ne": true } },
    { "$or": [ { "personal": { "$exists": false } }, { "personal": { "$ne": true } } ] },
    { "$or": [ { "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" } ] }
  ] },
  "excludes": [ "...history...", "documentTextContent", "_files.*thumbnail*" ],
  "from": 0, "size": 10,
  "sort": { "_updatedDate": "DESC" }
}
```

The reported slow query is an infix ("contains") full‑text search for the string `traw`, fanned across 10 metadata/content fields, scoped to one folder, with a media‑type exclusion, ordered newest‑first, page size 48:

![[image-20260625-003547.png]]

The single most important property: **every full‑text clause is a case‑insensitive, unanchored (**`^.*…​.*$`**)** `$regex`. That shape cannot use any B‑tree index — see §3.

## Root‑cause analysis

**R1 — Unindexable full‑text predicate (dominant).**

- A `$regex` can only use an index when it is **anchored at the start (**`^…`**) AND case‑sensitive**.

- Our predicate is `^.*traw.*$` with `$options:"i"`: the `^.*` makes it effectively unanchored *and* the `i` flag forces case folding.

- MongoDB therefore performs a **COLLSCAN** and runs the regex automaton against the field on every document.

**R2 — The** `$or` **multiplies the work by 10.**

- Each scanned document is tested against up to 10 separate regex automata.

- `documentTextContent` is the worst: it is a *large* OCR/text body, so the regex walks kilobytes per document.

**R3 — Cost is O(N) and N is the whole collection.**

- Selective filters that *could* shrink N — `folderIds`, `_deletionStatus`, `mediaType $nin` — sit in the same `$match`:

  - `folderIds` is itself written as a `$regex` rather than an indexed `$in`/equality

  - the optimizer still has to evaluate the regex `$or` for the candidates.

- At 128 k docs the scan + 10×regex is the wall.

**R4 —** `$lookup` **per surviving document (non‑materialized path) - RESOLVED with Materialized.**

- Folder join + `$filter` runs for every document that passes the match — extra random I/O and CPU.

**R5 — Blocking** `$sort` **after the scan.**

- `_updatedDate` sort happens *after* match/lookup.

- Because the match isn't index‑ordered, Mongo buffers the candidate set and sorts in memory (risking the 100 MB sort‑spill), instead of streaming the newest N from an index.

**R6 — Extra network hop & (de)serialization -  NO HOPE TO FIX.**

- luz_docs builds the pipeline as JSON and POSTs it to **luz_jsonstore** over HTTP, which talks to MongoDB;

- Results are serialized back.

- Fixed overhead on every call.

![[image-20260625-004125.png]]

## Solution catalogue

### S1 — MongoDB `$text` index

- **Mechanism:**

  - One compound **text index** over the searchable string fields.

  - Replace the regex `$or` with a single `{ $text: { $search: "traw" } }` stage.

  - Mongo tokenizes + stems, stores an inverted index, and the match becomes an **index seek**.

- **Latency:** typically **tens of ms** for the term lookup; total dominated by security match + sort. Realistic **\< 1 s**.

- **Effort:** low–medium. One index; translate full‑text clauses to `$text`; keep the structured clauses as the post‑filter.

- **Self‑hosted:** ✅ built‑in.

- **Trade‑offs / limits:**

  - **Word/stem matching, not substring.** `$text` matches whole tokens (and stems), so `traw` will *not* match `strawberry`. If the product genuinely needs infix ("contains") matching, `$text` alone changes semantics — accept word‑boundary search.

  - **Only one text index per collection** — all full‑text fields must share it; per‑field weights are possible (`weights`).

  - Language/stemming config per tenant locale; `$text` sort is by relevance `score`, but we still sort by `_updatedDate` (fine).

  - Can't combine two `$text` stages; the multi‑field `$or` collapses into the single index, which is the point.

- Link: [S1 — MongoDB \$text Index (native, self‑hosted)](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49533583375/S1+MongoDB+text+Index+native+self+hosted)

![[image-20260625-005617.png]]

### S2 — Materialized n‑gram / trigram field

- **Mechanism:**

  - At write time, compute a lowercased, concatenated **search blob** of the 10 fields and store its **trigrams** (overlapping 3‑char shingles) into an indexed array field.

  - A query for `traw` is decomposed into trigrams `{tra, raw}` and matched with `$all`/`$in` against the **multikey index** — an index seek that preserves true substring ("contains") matching, including across the original `^.*…​.*$` semantics.

- **Latency:** index‑backed; **tens–low‑hundreds of ms**. \< 1 s comfortably.

- **Effort:** medium. Needs a compute step (reuse the `materialize/` compute pattern), a backfill migration (reuse the `_shard` backfill machinery), and query rewriting (term → trigram set). Short terms (\< 3 chars) fall back to scan or are disallowed.

- **Self‑hosted:** ✅ pure MongoDB.

- **Trade‑offs:**

  - Larger index (trigrams are many);

  - Write amplification; a post‑filter regex on the candidate set is still needed to eliminate trigram false‑positives.

  - Most faithful drop‑in for current "contains" UX without leaving MongoDB.

![[image-20260625-010110.png]]

### S3 — External search engine (Elasticsearch / OpenSearch) *(or Atlas Search if migrating)*

- **Mechanism:**

  - Index documents into a Lucene engine via a **MongoDB change‑stream** sync (or dual‑write).

  - Full‑text queries hit the engine;

  - Mongo stays the system of record.

- **Latency:** **single‑digit–tens of ms** at 128 k and far beyond; best scaling headroom.

- **Effort:** high. New stateful service, sync pipeline, security‑filter parity (must replicate security‑class/folder rules into the engine or post‑filter), reindex/backfill, ops & cost. **Atlas Search** gives the same engine managed *inside* Mongo but requires moving to Atlas.

- **Self‑hosted:** ✅ (ES/OpenSearch sidecar); Atlas Search ❌ until Atlas migration.

- **Trade‑offs:**

  - Most powerful and most operationally heavy;

  - Eventual‑consistency lag;

  - Security‑model duplication is the main correctness risk.

![[image-20260625-010348.png]]
