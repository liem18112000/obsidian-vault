---
title: "Bulk operations need per-item outcomes, not one status code"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: List selection Error handling on multi-selection actions (Helios)"
tags: [api-design, rest, http-207, bulk-operations, error-handling, confluence-distilled]
---

# Bulk operations need per-item outcomes, not one status code

A bulk endpoint that acts on N items has N outcomes, and a single HTTP status cannot carry them. `200` claims everything worked; `400` or `500` claims nothing did. Both are wrong when three of fifty items failed — and the client cannot tell which three.

The shape that works, from a message-list API:

- **Single-item action** → `200`. One outcome, one status.
- **Bulk action** (delete / undo / hard-delete) → **`207 Multi-Status`** with a per-item breakdown:

```json
{
  "notFoundDocuments":         ["id1", "id2"],
  "permissionDeniedDocuments": ["id3"],
  "unknownErrorDocuments":     []
}
```

**Why grouping by failure reason beats a flat list.** Each bucket maps to a different client response: *not found* is usually stale UI state and warrants a refresh; *permission denied* needs a message to the user and must not be retried; *unknown error* is the only bucket worth retrying. A single `failed: [...]` array forces the client to guess, and it will guess "retry everything".

The buckets also tell the client what **succeeded** — anything not listed. That keeps the response small when most items succeed, which is the common case.

> [!tip] Make the no-op case a success
> The source notes that the same response is returned when items are **already in the requested state** — already deleted, already marked read. That is the right call: a bulk action is naturally retried (double-click, network retry, two tabs), and treating "already done" as failure produces alarming errors for a user whose intent was fully satisfied. Idempotent outcome, not idempotent implementation, is what matters here.

> [!warning] 207 is not free — decide the transactionality first
> Returning per-item results means you have chosen **partial application**: some items changed, others did not, and there is no rollback. That must be a deliberate decision, not an accident of looping. If the operation genuinely needs all-or-nothing, do not reach for 207 — wrap it in a transaction and return a single status. 207 says "I did what I could"; make sure that is what you meant.

> [!note] Clients must actually read the body
> `207` is a 2xx, so naive client code — `if (response.ok) { showSuccess() }` — treats a partial failure as complete success. This is the main hazard of the pattern, and it argues for documenting 207 loudly and testing the partial-failure path explicitly.

Related: [[Score async API designs on crash recovery and multi-instance, not latency]] — the other half of designing operations over many items.

Source: [[List selection Error handling on multi-selection actions]] (Helios, Confluence).

## Related

- [[Score async API designs on crash recovery and multi-instance, not latency]]
