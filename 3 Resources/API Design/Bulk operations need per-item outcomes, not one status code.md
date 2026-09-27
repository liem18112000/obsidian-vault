---
ai_hash: 88816caf80ffe75c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Bulk operations
- Per-item outcomes
- Single status code
- HTTP status
- 200 HTTP status
- 400 HTTP status
- 500 HTTP status
- Single-item action
- 207 Multi-Status
- Message-list API
- notFoundDocuments
- permissionDeniedDocuments
- unknownErrorDocuments
- Failure reason grouping
- Flat list
- Client response
- Stale UI state
- User
- Idempotent outcome
- Idempotent implementation
- Partial application
- Rollback
- Transaction
- All-or-nothing
- Clients
- Response body
- Naive client code
- Partial failure
- Complete success
- Documentation
- Testing
- Score async API designs on crash recovery and multi-instance, not latency
- List selection Error handling on multi-selection actions
- Helios
- Confluence
- Partial success
- Succeeded items
- Main hazard
source: 'Confluence: List selection Error handling on multi-selection actions (Helios)'
status: seedling
tags:
- api-design
- rest
- http-207
- bulk-operations
- error-handling
- confluence-distilled
title: Bulk operations need per-item outcomes, not one status code
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Tight updateMany filter makes HTTP 207 a reliable partial-write signal]]
- [[Encode a benign-error decision in a dedicated exception type, not a swallowed catch on a magic status code]]
- [[Reject input you cannot fully handle; never silently drop part of it]]
- [[404 addresses a missing resource; an empty filter result is a successful query]]
- [[Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore SC_MULTI_STATUS as benign]]

**Relations:**
- Bulk operations — *require* — Per-item outcomes
- Bulk operations — *should not use* — Single status code
- Single status code — *cannot represent* — Per-item outcomes
- 200 HTTP status — *indicates* — everything worked
- 400 HTTP status — *indicates* — nothing worked
- 500 HTTP status — *indicates* — nothing worked
- 200 HTTP status — *is inadequate for* — Partial failure
- 400 HTTP status — *is inadequate for* — Partial success
- 500 HTTP status — *is inadequate for* — Partial success
- Single-item action — *returns* — 200 HTTP status
- Bulk operations — *should return* — 207 Multi-Status
- 207 Multi-Status — *contains* — notFoundDocuments
- 207 Multi-Status — *contains* — permissionDeniedDocuments
- 207 Multi-Status — *contains* — unknownErrorDocuments
- Failure reason grouping — *is superior to* — Flat list
- Failure reason grouping — *enables varied* — Client response
- notFoundDocuments — *suggests* — Stale UI state
- permissionDeniedDocuments — *requires message to* — User
- permissionDeniedDocuments — *should not be retried* — true
- unknownErrorDocuments — *can be retried* — true
- Failure reason grouping — *informs client of* — Succeeded items
- Idempotent outcome — *is more important than* — Idempotent implementation
- 207 Multi-Status — *implies* — Partial application
- Partial application — *lacks* — Rollback
- All-or-nothing — *requires* — Transaction
- Transaction — *returns* — Single status code
- Clients — *must parse* — Response body
- Naive client code — *misinterprets* — 207 Multi-Status
- Naive client code — *misinterprets 207 Multi-Status as* — Complete success
- Misinterpretation of 207 Multi-Status — *is a* — Main hazard
- Main hazard — *necessitates* — Documentation
- Main hazard — *necessitates* — Testing
- Bulk operations — *are related to* — Score async API designs on crash recovery and multi-instance, not latency
- List selection Error handling on multi-selection actions — *is source for* — Bulk operations
- Helios — *is source for* — List selection Error handling on multi-selection actions
- Confluence — *is source for* — List selection Error handling on multi-selection actions

%% ai-graph-end %%