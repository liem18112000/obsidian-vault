---
title: "S2 — Materialized n‑gram / Trigram Field (native, self‑hosted, keeps substring semantics)"
created: 2026-06-25
updated: 2026-07-01
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49533485089/S2+Materialized+n+gram+Trigram+Field+native+self+hosted+keeps+substring+semantics
confluence_id: "49533485089"
confluence_path: "Team Kepler > Developer note > Full‑Text Document Search — Performance Analysis & Proposals"
tags: [confluence, performance, search]
---

# S2 — Materialized n‑gram / Trigram Field (native, self‑hosted, keeps substring semantics)

*Confluence source · Team Kepler › Developer note › Full‑Text Document Search — Performance Analysis & Proposals · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49533485089/S2+Materialized+n+gram+Trigram+Field+native+self+hosted+keeps+substring+semantics) · updated 2026-07-01*

## Core idea in one picture

A **trigram** is a 3‑character sliding window over a string.

Two facts make trigrams a substring index:

- **Necessary condition:** if string *S* contains substring *q* (and `|q| ≥ 3`), then **every trigram of *****q***** is also a trigram of *****S***. So "does S contain q?" ⇒ "does trigrams(S) ⊇ trigrams(q)?".

- **Not sufficient:** the reverse can be false — a doc may contain all of *q*'s trigrams in *different places* and still not contain *q*. → **false positives**, removed by a cheap residual regex on the (tiny) candidate set.

So the search becomes: **index‑filter by trigrams (fast, approximate) → verify by regex (exact, on few docs).**

Worked example — generate trigrams for `traw`:

```
"traw"  → windows of 3 → { "tra", "raw" }
```

- A document whose blob is `…strawberry…` produces `… str, tra, raw, awb, wbe, ber, err, rry …`.

- It contains **both** `tra` and `raw`, so it is a *candidate* for query `traw`.

- The residual regex `/traw/i` then runs on that candidate and rejects it (because `strawberry` does not actually contain `traw`).

- A document containing the literal `traw` passes both.

**=\> Substring semantics preserved.**

![[image-20260625-015234.png]]

## Normalization

The blob and the query term must be normalized the same way, or trigrams won't line up:

1.  **Concatenate** the searchable fields into one blob (space‑joined).

2.  **Lowercase** (matches the current `$options:"i"`).

3.  **Diacritic fold** (optional, recommended): `é→e` so `café` matches `cafe`.

4.  **Collapse whitespace** to single spaces.

```
fields → "Strawberry Invoice 2026"  (title + sender + …)
 lower  → "strawberry invoice 2026"
 trigrams (unique set) →
   { "str","tra","raw","awb","wbe","ber","err","rry","ry ",
     "y i"," in","inv","nvo","voi","oic","ice","ce ","e 2"," 20","202","026" }
```

Store the **unique set** in an array field, e.g. `_searchTrigrams`.

## Write path — where trigrams come from

Computed at write time and backfilled for existing docs, reusing the machinery already on this branch.

![[image-20260625-015334.png]]

## Query path — filter then verify

```
// term "traw" → trigrams ["tra","raw"]
[
  // 1. TRIGRAM FILTER — multikey index seek (approximate, necessary condition)
  { $match: { _searchTrigrams: { $all: ["tra", "raw"] } } },

  // 2. RESIDUAL VERIFY — exact substring, on the tiny candidate set only
  { $match: { $or: [
      { title:               { $regex: "traw", $options: "i" } },
      { documentTitle:       { $regex: "traw", $options: "i" } },
      { senderName:          { $regex: "traw", $options: "i" } },
      /* … the same fields as today … */
  ] } },

  // 3. structured + security (materialized, indexed) — no $lookup
  { $match: { folderIds: { $in: [ ObjectId("6a04…") ] },
              "letterInfo.mediaType": { $nin: [ /* … */ ] },
              _isBeingCreated: { $ne: true },
              $or: [ { _isPublic: true },
                     { _effectiveSecurityClassCodes: { $in: [ "LIEM01_1", /* … */ ] } } ] } },

  // 4. sort + page (candidate set is small → cheap)
  { $sort: { _updatedDate: -1 } }, { $skip: 0 }, { $limit: 10 },
  { $project: { /* excludes */ } }
]
```

- How MongoDB runs stage 1: with a multikey index and `$all`, the planner picks the **most selective trigram** to seek the index, then intersects/filters the rest — so `keysExamined` ≈ postings of the rarest trigram, not 128 000.

- The residual regex in stage 2 is **un‑indexed but runs over a handful of candidates**, so its O(N) cost is now O(few).

- Target: stage 1 = `IXSCAN` on `idx_trigrams`, `docsExamined` = candidate count (tens–hundreds), no `COLLSCAN`.

![[image-20260625-015348.png]]

## Cost, cardinality & the short‑term problem

### **Index size / write amplification**

- The trigram alphabet (a‑z, 0‑9, space, a few) is ~38 symbols → at most ~38³ ≈ 54 000 distinct trigrams.

- A metadata blob yields tens–low‑hundreds of unique trigrams per doc; a full OCR body can saturate thousands.

- Index entries = Σ(unique trigrams per doc). Honest trade‑offs:

  - **Bigger index + slower writes** (more multikey entries to maintain per upsert).

  - Mitigate by: **metadata‑only blob**, capping body length, or a separate opt‑in body‑trigram field.

### **Selectivity**

- Common trigrams (e.g. `e 2`, `ion`) have huge postings; rare ones are selective.

- `$all` lets the planner lead with the rarest, so even common‑looking terms usually seek well.

- Very common short queries are the worst case — still bounded by the residual verify.

### **Short terms (**`|q| < 3`**).**

A 1‑ or 2‑char query has no trigram → can't use the index. Handle explicitly:

- **Min‑3 rule** in the UI/API (most search boxes already debounce to ≥3 chars), with the current regex path as fallback for shorter; or

- maintain a small companion bigram/prefix field (extra cost) if 1–2 char search is a hard requirement.

![[image-20260625-015416.png]]

## Sample Query on Mongo DB

> [!note]- Details Query
>
>
>
> ```
> db.documents.find(
>     {"$and":[
>         {"$and":[
>                 {"$and":[
>                         {"$and":[
>                                 {"$or":[
>                                         {"title":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"documentTitle":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"documentDescription":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"senderName":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"documentTextContent":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"tags":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"documentTypes":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"subject":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"senderEndToEndId":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"fileName":{"$regex":"^.*traw.*$","$options":"i"}}
>                                     ]},
>                                 {"letterInfo.mediaType":{"$nin":[
>                                             "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
>                                             "application/vnd.ch.klara.epost.smartletter.template.v1+json",
>                                             "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"]}}
>                             ]},
>                         {"_isBeingCreated":{"$ne":true}},
>                         {"$or":[{"personal":{"$exists":false}},{"personal":{"$ne":true}}]},
>                         {"$or":[{"_deletionStatus":{"$exists":false}},{"_deletionStatus":"false"}]}
>                     ]},
>                 {"$or":[
>                         {"securityClassCodes":{"$exists":false}},
>                         {"securityClassCodes":{"$size":0}},
>                         {"securityClassCodes":null},
>                         {"securityClassCodes":{"$in":["SC14_10","SC33_12","LIEM09_9","LIEM03_3","LIEM07_7","LIEM04_4","LIEM05_5","THIENVIEW1_13","LIEM06_6","SC22_11","LIEM08_8","LIEM02_2","LIEM01_1"]}}
>                     ]},
>                 {"_isBeingCreated":{"$ne":true}},
>                 {"$or":[{"personal":{"$exists":false}},{"personal":{"$ne":true}}]}
>             ]}
>     ]},
>     {
>         "_files.referenceTsq":0,"_files.thumbnail512":0,"_files.thumbnail256":0,
>         "uploadedHistoryEntry":0,"storageHistoryEntries":0,"printHistoryEntries":0,
>         "downloadHistoryEntries":0,"exportHistoryEntries":0,"deletingHistoryEntries":0,
>         "undoDeletingHistoryEntries":0,"tagModificationHistoryEntries":0,
>         "securityClassModificationHistoryEntries":0,"documentTypeModificationHistoryEntries":0,
>         "restoringHistoryEntries":0,"documentTextContent":0
>     }
> ).sort({ "_updatedDate": -1 }).skip(0).limit(10)
> ```
>
>
>

![[image-20260630-072455.png]]

> [!note]- Details Query
>
>
>
> ```
> db.documents.find(
>     {"$and":[
>         {"_searchTrigrams":{"$all":["tra","raw"]}},
>         {"$and":[
>                 {"$and":[
>                         {"$and":[
>                                 {"$or":[
>                                         {"title":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"documentTitle":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"documentDescription":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"senderName":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"documentTextContent":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"tags":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"documentTypes":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"subject":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"senderEndToEndId":{"$regex":"^.*traw.*$","$options":"i"}},
>                                         {"fileName":{"$regex":"^.*traw.*$","$options":"i"}}
>                                     ]},
>                                 {"letterInfo.mediaType":{"$nin":[
>                                             "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
>                                             "application/vnd.ch.klara.epost.smartletter.template.v1+json",
>                                             "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"]}}
>                             ]},
>                         {"_isBeingCreated":{"$ne":true}},
>                         {"$or":[{"personal":{"$exists":false}},{"personal":{"$ne":true}}]},
>                         {"$or":[{"_deletionStatus":{"$exists":false}},{"_deletionStatus":"false"}]}
>                     ]},
>                 {"$or":[
>                         {"securityClassCodes":{"$exists":false}},
>                         {"securityClassCodes":{"$size":0}},
>                         {"securityClassCodes":null},
>                         {"securityClassCodes":{"$in":["SC14_10","SC33_12","LIEM09_9","LIEM03_3","LIEM07_7","LIEM04_4","LIEM05_5","THIENVIEW1_13","LIEM06_6","SC22_11","LIEM08_8","LIEM02_2","LIEM01_1"]}}
>                     ]},
>                 {"_isBeingCreated":{"$ne":true}},
>                 {"$or":[{"personal":{"$exists":false}},{"personal":{"$ne":true}}]}
>             ]}
>     ]},
>     {
>         "_files.referenceTsq":0,"_files.thumbnail512":0,"_files.thumbnail256":0,
>         "uploadedHistoryEntry":0,"storageHistoryEntries":0,"printHistoryEntries":0,
>         "downloadHistoryEntries":0,"exportHistoryEntries":0,"deletingHistoryEntries":0,
>         "undoDeletingHistoryEntries":0,"tagModificationHistoryEntries":0,
>         "securityClassModificationHistoryEntries":0,"documentTypeModificationHistoryEntries":0,
>         "restoringHistoryEntries":0,"documentTextContent":0
>     }
> ).sort({ "_updatedDate": -1 }).skip(0).limit(10)
> ```
>
>
>

![[image-20260630-072624.png]]

Index size:

![[image-20260630-075234.png]]
