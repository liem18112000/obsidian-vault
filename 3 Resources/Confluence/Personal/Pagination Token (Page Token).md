---
ai_hash: bed6e74234fbf18b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49398120454'
confluence_path: Overview
created: 2026-05-11
entities: []
source: Confluence · ~71202087b0f7f1aaab4406a25dfa0fc075c4d4 - liem.doanvanthanh
status: reference
tags:
- confluence
title: Pagination Token (Page Token)
type: source
updated: 2026-05-11
url: https://axonivy.atlassian.net/wiki/spaces/~71202087b0f7f1aaab4406a25dfa0fc075c4d4/pages/49398120454/Pagination+Token+Page+Token
---

# Pagination Token (Page Token)

*Confluence source · Overview · [view original](https://axonivy.atlassian.net/wiki/spaces/~71202087b0f7f1aaab4406a25dfa0fc075c4d4/pages/49398120454/Pagination+Token+Page+Token) · updated 2026-05-11*

## Definition

A **page token** (a.k.a. cursor) is an **opaque string** that the server gives the client to say *"if you want the next batch of results, send this back to me."* It encodes where the previous page ended, so the server knows where to resume.

It's the modern alternative to classic `offset`/`limit` pagination — and for your Post SAP use case (large invoice datasets, daily batch sync), it's the right choice.

------------------------------------------------------------------------

## Offset vs Cursor (Token) — Why Tokens Win

|  |  |  |
|----|----|----|
| Aspect | Offset/Limit (`?page=5&size=50`) | Cursor/Token (`?pageToken=abc123`) |
| Performance on large tables | Slow — DB must skip N rows | Fast — uses indexed WHERE |
| Consistency under inserts | Duplicates / missed rows | Stable |
| Stateless | Yes | Yes (token carries state) |
| Random page jumps | Yes (page 1 → page 99) | No (sequential only) |
| Best for | Small UI tables | Large datasets, batch sync, APIs |

For your case (daily sync of cleared invoices from SAP), **cursor wins easily** — new payments are constantly being cleared, so offsets would shift between calls.

------------------------------------------------------------------------

## How It Works — The Flow

![[image-20260511-011743.png]]

**Key rule:** when `nextPageToken` is absent (or null/empty), the client knows it has reached the end.

------------------------------------------------------------------------

## What's *Inside* the Token?

The token is **opaque to the client** — it should never parse it. But on the server, it typically encodes:

```
{
  "lastClearingDate": "2026-04-22T08:42:00Z",
  "lastInvoiceId": "123457",
  "filter": "updatedSince=2026-04-20T00:00:00Z",
  "version": 1
}
```

Then Base64-encode (and optionally sign or encrypt) it:

```
eyJsYXN0Q2xlYXJpbmdEYXRlIjoiMjAyNi0wNC0yMlQwODo0MjowMFoiLCJsYXN0SW52b2ljZUlkIjoiMTIzNDU3In0=
```

## **Why include the filter?**

So clients can't mix tokens with different `updatedSince` values and get inconsistent results. The server validates that the token's filter matches the current request's filter.

## Best Practices

1.  **Stable ordering is mandatory.** Always order by `(clearing_date, invoice_id)` — never just by timestamp, because ties cause skipped/duplicated rows.

2.  **Fetch N+1 rows** to detect whether a next page exists without a separate `COUNT(*)`.

3.  **Treat the token as opaque on the client.** Never parse it; never construct one client-side.

4.  **Validate filter consistency.** Reject tokens whose embedded filter differs from the current query — prevents inconsistent results.

5.  **Sign or encrypt the token** if it embeds sensitive info (HMAC-SHA256 is enough for tamper-detection).

6.  **Set a sane max page size** (e.g., cap at 1000) to prevent abuse.

7.  **Token lifetime:** for batch sync, tokens can live indefinitely; for paged UI, you may expire them after a few hours.

8.  **Document explicitly** that absence of `nextPageToken` means end-of-stream.

%% ai-graph-start %%

**Related notes:**
- [[A pagination token is an opaque cursor, and it must carry the filter it was issued under]]
- [[Confluence CQL search paginates by opaque cursor, not start offset]]

%% ai-graph-end %%