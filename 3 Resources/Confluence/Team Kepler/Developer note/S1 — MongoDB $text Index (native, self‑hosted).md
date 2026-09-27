---
ai_hash: 32571ddf318db21b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49533583375'
confluence_path: Team Kepler > Developer note > Full‑Text Document Search — Performance
  Analysis & Proposals
created: 2026-06-25
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- mongodb
- performance
- search
title: S1 — MongoDB $text Index (native, self‑hosted)
type: source
updated: 2026-06-25
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49533583375/S1+MongoDB+text+Index+native+self+hosted
---

# S1 — MongoDB $text Index (native, self‑hosted)

*Confluence source · Team Kepler › Developer note › Full‑Text Document Search — Performance Analysis & Proposals · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49533583375/S1+MongoDB+text+Index+native+self+hosted) · updated 2026-06-25*

## Summary

- Replaces the regex `$or` with a single `{$text:{$search}}` by one compound text inverted index.

- It is the **lowest‑effort, fully‑native** fix and easily hits sub‑second at 128 k — **but** it matches whole stemmed *words*, not substrings, so it changes the current "contains" behaviour.

- Choose when word/prefix search is acceptable;

## What a `$text` index actually is

A MongoDB **text index** is an **inverted index**: instead of mapping *document → fields*, it maps *token → list of documents that contain it*. Looking up a word becomes an index seek (`IXSCAN`) instead of reading every document.

At index‑build time MongoDB runs each indexed string field through a pipeline:

1.  **Tokenize** — split text on word boundaries/punctuation into terms.

2.  **Lowercase** — case folding (text search is always case‑insensitive).

3.  **Diacritic fold** — `café → cafe` (text index v3).

4.  **Stop‑word removal** — language‑specific noise words (`the, and, der, die, …`) are dropped.

5.  **Stem** — reduce to a root via a language stemmer: `invoices → invoic`, `running → run`. Both the indexed terms *and* the query terms are stemmed, so they meet in the middle.

The surviving stems become the index keys; each key points to the documents (and internal weights) that contain it.

![[image-20260625-014211.png]]

## Index definition

One **compound text index** spanning every field the current regex `$or` searches (minus the ones that aren't natural language):

```
use <tenantDb>;
db.documents.createIndex(
  {
    title:               "text",
    documentTitle:       "text",
    documentDescription: "text",
    senderName:          "text",
    subject:             "text",
    tags:                "text",
    documentTypes:       "text",
    fileName:            "text",
    documentTextContent: "text"
  },
  {
    name: "idx_fulltext",
    weights: {                 // relevance weighting (optional)
      title: 10, documentTitle: 10, subject: 5,
      documentTextContent: 1   // body matches count least
    },
    default_language: "none",  // "none" = no stemming/stopwords;
    background: true
  }
);
```

Notes:

- **Only ONE text index is allowed per collection.**

- `weights` lets a hit in `title` outrank a hit in `documentTextContent` when sorting by relevance (`$meta:"textScore"`).

- We sort by `_updatedDate`, so weights are optional — but cheap to keep.

- `default_language` chooses the stemmer. Mixed‑locale tenants are a problem; `"none"` disables stemming/stop‑words for predictable, literal token matching.

- `senderEndToEndId` and `folderIds` are **not** natural‑language fields — keep them as exact/`$in` predicates, not in the text index.

![[image-20260625-011937.png]]

## Query execution flow

- Full‑text clause is a 10‑field regex `$or` inside the first `$match`.

- Now it becomes a single `$text` stage, which MongoDB resolves via the inverted index.

### Rewritten Aggregation Pipeline

```
[
  // 1. FULL-TEXT MATCH — index seek, must be the FIRST stage (see constraint below)
  { $match: { $text: { $search: "traw" } } },

  // 2. structured narrowing (indexed) — folder, media type, housekeeping
  { $match: {
      folderIds: { $in: [ ObjectId("6a04490ccc937202f8258b4a") ] },
      "letterInfo.mediaType": { $nin: [ "...draft...", "...template...", "...receipt..." ] },
      _isBeingCreated: { $ne: true },
      $or: [ { _deletionStatus: { $exists: false } }, { _deletionStatus: "false" } ]
  } },

  // 3. security on materialized fields (S4) — NO $lookup
  { $match: { $or: [
      { _isPublic: true },
      { _effectiveSecurityClassCodes: { $in: [ "LIEM01_1", "LIEM03_3", /* … */ ] } }
  ] } },

  // 4. sort + page (small candidate set → in-memory sort is cheap)
  { $sort: { _updatedDate: -1 } },
  { $skip: 0 }, { $limit: 10 },
  { $project: { /* excludes */ } }
]
```

What `explain("executionStats")` should now show for stage 1: a `TEXT` / `IXSCAN` node over `idx_fulltext` with `keysExamined` ≈ number of docs containing the token (small), **not** a `COLLSCAN` with `docsExamined` = 128 000.

![[image-20260625-012100.png]]

%% ai-graph-start %%

**Related notes:**
- [[Full‑Text Document Search — Performance Analysis & Proposals]]
- [[A MongoDB text index matches stemmed words, not substrings]]
- [[S2 — Materialized n‑gram - Trigram Field (native, self‑hosted, keeps substring semantics)]]
- [[Trigram Search — Performance-Env Benchmark]]
- [[Index Impact on MongoDB searchByFacets]]

%% ai-graph-end %%