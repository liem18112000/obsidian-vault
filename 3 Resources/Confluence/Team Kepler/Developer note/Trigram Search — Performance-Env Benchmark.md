---
ai_hash: b3310630441a8477
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49545969673'
confluence_path: Team Kepler > Developer note > Full‑Text Document Search — Performance
  Analysis & Proposals > S2 — Materialized n‑gram / Trigram Field (native, self‑hosted,
  keeps substring semantics)
created: 2026-06-30
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- performance
- search
title: Trigram Search — Performance-Env Benchmark
type: source
updated: 2026-06-30
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49545969673/Trigram+Search+Performance-Env+Benchmark
---

# Trigram Search — Performance-Env Benchmark

*Confluence source · Team Kepler › Developer note › Full‑Text Document Search — Performance Analysis & Proposals › S2 — Materialized n‑gram / Trigram Field (native, self‑hosted, keeps substring semantics) · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49545969673/Trigram+Search+Performance-Env+Benchmark) · updated 2026-06-30*

## Overview

Latency of `POST /documents/search` (full-text substring term `traw`) against the **performance** environment, tenant `45b05710-b9d4-4d3e-935e-83c4525369fa`, seeded with **128,000 documents / 10 folders**.

### What we tested

- **API:** `POST /luz_docs/api/45b05710-b9d4-4d3e-935e-83c4525369fa/documents/search`

> [!note]- Query params
>
>
> |                             |         |
> |-----------------------------|---------|
> | param                       | value   |
> | `include-deleted-documents` | `true`  |
> | `include-file`              | `false` |
> | `include-folder-name`       | `true`  |
> | `skip-security-classes`     | `false` |
> | `from`                      | `0`     |
> | `size`                      | `10`    |
>
>

> [!note]- Request body
>
>
>
> ```
> {
>   "query": {
>     "$and": [
>       { "$and": [
>         { "$and": [
>           { "$or": [
>             { "title":               { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "documentTitle":        { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "documentDescription":  { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "senderName":           { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "documentTextContent":  { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "tags":                 { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "documentTypes":        { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "subject":              { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "senderEndToEndId":     { "$regex": "^.*traw.*$", "$options": "i" } },
>             { "fileName":             { "$regex": "^.*traw.*$", "$options": "i" } }
>           ] }
>         ] },
>         { "letterInfo.mediaType": { "$nin": [
>             "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
>             "application/vnd.ch.klara.epost.smartletter.template.v1+json",
>             "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"
>         ] } }
>       ] },
>       { "_isBeingCreated": { "$ne": true } },
>       { "$or": [ { "personal": { "$exists": false } }, { "personal": { "$ne": true } } ] },
>       { "$or": [ { "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" } ] }
>     ]
>   },
>   "excludes": [
>     "_files.referenceTsq", "_files.thumbnail512", "_files.thumbnail256",
>     "uploadedHistoryEntry", "storageHistoryEntries", "printHistoryEntries",
>     "downloadHistoryEntries", "exportHistoryEntries", "deletingHistoryEntries",
>     "undoDeletingHistoryEntries", "tagModificationHistoryEntries",
>     "securityClassModificationHistoryEntries", "documentTypeModificationHistoryEntries",
>     "restoringHistoryEntries", "documentTextContent"
>   ],
>   "from": 0,
>   "size": 10,
>   "sort": { "_updatedDate": "DESC" }
> }
> ```
>
>
>

- The server ANDs the trigram prefilter (`_searchTrigrams:{$all:[...]}`) onto this query **only when the gate verdict is "complete"**.

- In the legacy variant (no `_searchTrigrams` field) the gate is incomplete, so the query runs exactly as above — a pure 10-field infix-regex `$or`.

### TL;DR — legacy vs trigram

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Variant** | **DB plan** | **API steady avg** | **min** | **max** | **stddev** |
| **Legacy** regex `$or` (no trigram) | COLLSCAN | **16.838 s** (runs 2–11) | 16.439 | 17.308 | 0.273 |
| Trigram, **no index** (field only) | COLLSCAN | 7.993 s (runs 3–11) | 7.589 | 8.874 | 0.393 |
| Trigram **+** `idx_trigrams` | IXSCAN | **0.949 s** (runs 2–11) | 0.910 | 1.003 | 0.029 |

- **Legacy → trigram+index ≈ 17.7× faster.**

- The trigram field alone is a trap (still a COLLSCAN); the index is what makes S2 work.

## Setup

|  |  |
|----|----|
| **Element** | **Description** |
| Env | performance (`klara-performance`, ns `performance`) |
| Tenant | `45b05710-b9d4-4d3e-935e-83c4525369fa` (Mongo cluster `luz-mongodb04`) |
| Data | 128,000 documents, 10 folders, all `_isPublic=true`, `_searchTrigrams` + materialize fields stamped inline |
| luz-docs image | `049e6af2a` (trigram write-path + `NgramResponseFilter` gate-warming) |
| Query | raw `$and`: 10-field `$regex:"^.*traw.*$"` `$or` + `letterInfo.mediaType $nin` + `_isBeingCreated $ne true` + `personal`/`_deletionStatus` guards; `skip-security-classes=false`; `from=0 size=10`; `sort _updatedDate:-1` |
| Path | performance api-forwarder → `localhost:8080` |
| Method | 11 runs; **run \#1 discarded as warm-up**; latency = curl `time_total` |

![[image-20260630-025320.png]]

### Legacy baseline

> [!note]- Full data run - 11 trials
>
>
> |     |      |         |                     |
> |-----|------|---------|---------------------|
> | Run | HTTP | Seconds |                     |
> | 1   | 200  | 11.445  | warm-up (discarded) |
> | 2   | 200  | 16.635  |                     |
> | 3   | 200  | 17.308  |                     |
> | 4   | 200  | 16.802  |                     |
> | 5   | 200  | 16.926  |                     |
> | 6   | 200  | 16.554  |                     |
> | 7   | 200  | 17.025  |                     |
> | 8   | 200  | 17.229  |                     |
> | 9   | 200  | 16.620  |                     |
> | 10  | 200  | 16.839  |                     |
> | 11  | 200  | 16.439  |                     |
>
>

- **Measured (runs 2–11):** n=10, min 16.439 s, max 17.308 s, **avg 16.838 s**, stddev 0.273 s.

- Every query is a COLLSCAN of all 128k docs.

### Trigram field and no index

> [!note]- Full data run - 11 trials
>
>
> |     |      |         |                           |
> |-----|------|---------|---------------------------|
> | Run | HTTP | Seconds |                           |
> | 1   | 200  | 19.472  | warm-up (discarded)       |
> | 2   | 200  | 14.823  | gate caches still warming |
> | 3   | 200  | 8.442   |                           |
> | 4   | 200  | 8.874   |                           |
> | 5   | 200  | 7.936   |                           |
> | 6   | 200  | 7.994   |                           |
> | 7   | 200  | 7.937   |                           |
> | 8   | 200  | 7.649   |                           |
> | 9   | 200  | 7.589   |                           |
> | 10  | 200  | 7.852   |                           |
> | 11  | 200  | 7.663   |                           |
>
>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| Set | n | min | max | avg | stddev |
| **Measured** (runs 2–11, per "ignore first run") | 10 | 7.589 | 14.823 | **8.676** | 2.083 |
| **Steady** (runs 3–11, also dropping still-warming run 2) | 9 | 7.589 | 8.874 | **7.993** | 0.393 |

**Root cause:**

- `_searchTrigrams` **field exist** but does **not** create the index.

- Without it the multikey `$all` cannot seek → full scan → this gives no speed-up over the legacy regex.

**Fix — create the index:**

```
db.documents.createIndex({ _searchTrigrams: 1 }, { name: "idx_trigrams" })
```

### Trigram + `idx_trigrams` — indexed

|     |      |         |                     |
|-----|------|---------|---------------------|
| Run | HTTP | Seconds |                     |
| 1   | 200  | 4.105   | warm-up (discarded) |
| 2   | 200  | 0.978   |                     |
| 3   | 200  | 0.910   |                     |
| 4   | 200  | 0.922   |                     |
| 5   | 200  | 0.933   |                     |
| 6   | 200  | 0.933   |                     |
| 7   | 200  | 1.003   |                     |
| 8   | 200  | 0.963   |                     |
| 9   | 200  | 0.941   |                     |
| 10  | 200  | 0.930   |                     |
| 11  | 200  | 0.983   |                     |

**Measured (runs 2–11):** n=10, min 0.910 s, max 1.003 s, **avg 0.949 s**, stddev 0.029 s.

The remaining ~0.95 s (vs the 3 ms DB seek) is the API round-trip: api-forwarder hop, residual regex `$or` + security/sort over the candidate set, folder-name enrichment, and response serialization — not the trigram lookup.

![[image-20260630-032541.png]]

%% ai-graph-start %%

**Related notes:**
- [[S2 — Materialized n‑gram - Trigram Field (native, self‑hosted, keeps substring semantics)]]
- [[Full‑Text Document Search — Performance Analysis & Proposals]]
- [[luz-docs ngram search shipped code indexes the OCR body and prefilters fail-open]]
- [[S1 — MongoDB $text Index (native, self‑hosted)]]
- [[Trigram index makes substring search indexable filter by 3-grams, then verify by regex]]

%% ai-graph-end %%