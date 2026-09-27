---
ai_hash: 4d5af5f85a43b943
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Refactor Enricher process update PATCH (LUZ)'
status: seedling
tags:
- api-design
- rest
- patch
- put
- performance
- concurrency
- confluence-distilled
title: PATCH removes the read-modify-write round trips that PUT-replace forces
type: lesson
---

# PATCH removes the read-modify-write round trips that PUT-replace forces

A `PUT`-style replace endpoint forces every writer into **read-modify-write**: you cannot send a partial document, so you must first fetch the current one, merge your field in, and send the whole thing back. `PATCH` moves the merge to the server and the reads disappear.

Measured on one document-enrichment process:

| | Endpoint | Calls |
|---|---|--:|
| **PUT** | `…/mdb/{tenant}/{collection}/{doc-id}/replace` | **11** |
| **PATCH** | `…/mdb/{tenant}/{collection}/{doc-id}` | **2** |

The PUT breakdown shows where the cost hides: **8 of the 11 calls were just fetching the latest metadata** before each write, plus 2 replaces and 1 status update. With PATCH the same work is two calls — one per enrichment phase, with the status folded into the second.

**The reads are not the only thing you delete.** Read-modify-write over a document that other writers also touch is a **lost-update race**: two enrichment phases each read version *N*, each merge their own field, each write back a full document, and whichever lands second erases the other's change. Fixing that under PUT means optimistic concurrency — ETags or a version column plus retry-on-conflict — which is more code than the read you were trying to avoid. A field-scoped PATCH sidesteps it, because the two writers are no longer sending overlapping state.

> [!tip] When PUT is still right
> `PUT` is correct when the client genuinely owns the whole resource and you *want* last-writer-wins replace semantics — config documents, single-writer records, idempotent full-state sync. The trap is using replace semantics for *partial* updates, which is what turns one logical write into a fetch-and-rewrite.

Two caveats worth carrying:

- **PATCH is not automatically idempotent.** A merge-patch that sets fields is; one that appends to an array or increments a counter is not. Retries then need an idempotency key — see [[Client-assigned idempotency keys with a unique constraint beat distributed locks]].
- **Agree on the patch format.** JSON Merge Patch (RFC 7396) and JSON Patch (RFC 6902) behave differently, especially around `null` and arrays — merge-patch treats `null` as *delete this field*, which surprises people.

Source: [[Refactor Enricher process - update PATCH]] (LUZ, Confluence).

## Related

- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]

%% ai-graph-start %%

**Related notes:**
- [[Refactor Enricher process - update PATCH]]
- [[luz-jsonstore optimistic-concurrency PATCH version-number is an equality CAS]]
- [[json-patch-independent-translation-breaks-reset-then-append]]
- [[Shared aggregate write targets need CAS, not plain $set]]
- [[Full-object PUT instead of dedicated endpoint is a REST caller anti-pattern]]

%% ai-graph-end %%