---
title: "A pagination token is an opaque cursor, and it must carry the filter it was issued under"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Pagination Token (Page Token) (2026-05-11)"
tags: [pagination, api-design, cursor, backend, sap, consistency]
---

# A pagination token is an opaque cursor, and it must carry the filter it was issued under

A **page token** (cursor) is an **opaque string** the server hands back meaning "send this to resume where you stopped". The client must never parse it. Server-side it typically encodes the last sort key plus tie-breaker, base64'd and optionally signed:

```json
{ "lastClearingDate": "2026-04-22T08:42:00Z", "lastInvoiceId": "123457",
  "filter": "updatedSince=2026-04-20T00:00:00Z", "version": 1 }
```

Why it beats `offset`/`limit` on anything large or live:

| | offset/limit | cursor |
|---|---|---|
| Large tables | slow — the DB skips N rows every page | fast — indexed `WHERE key > last` |
| Rows inserted mid-scan | **duplicates and skipped rows** | stable |
| Jump to page 99 | yes | no, sequential only |

The consistency point is the one that bites in batch sync. Offsets address by *position*, so any insert before your cursor shifts every later row down — you re-read one row and never see another. A cursor addresses by *value*, so inserts elsewhere are irrelevant.

Two rules that are easy to miss:

- **End-of-results is signalled by an absent/empty `nextPageToken`** — not by a short page. A full-size final page followed by an empty one is normal.
- **Embed the filter in the token and validate it matches the current request.** Otherwise a client can replay a token issued under `updatedSince=X` against a request for `updatedSince=Y` and silently get an incoherent mix. The token is a resume point *for a specific query*, not a global position.

The trade you accept: no random page jumps. For batch sync and infinite scroll that costs nothing; for a UI with numbered pages it is disqualifying.

## Related

- [[CQRS splits read and write models architecturally, CQS only splits methods]]
