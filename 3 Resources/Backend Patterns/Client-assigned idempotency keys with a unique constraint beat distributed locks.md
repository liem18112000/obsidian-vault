---
ai_hash: 0f1e815b9c02eaaf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Common service architecture'
status: seedling
tags:
- idempotency
- event-driven
- pubsub
- cloud-run
- distributed-systems
- acid
- confluence-distilled
title: Client-assigned idempotency keys with a unique constraint beat distributed
  locks
type: concept
---

# Client-assigned idempotency keys with a unique constraint beat distributed locks

In an event-driven system on autoscaling compute, the same event *will* arrive more than once — Pub/Sub redelivers, clients retry, and two instances can pick up the same message concurrently. The cheapest way to make handlers idempotent is a dedicated table whose **primary key is an id the client assigns**, plus a unique constraint doing the enforcement.

```
table: idempotency
  idempotency_id  uuid  PK     -- assigned by the CLIENT, e.g. 4f3fb963-85bf-…
  expiry_time     timestamp NOT NULL  -- request received time + request timeout
```

**The client assigns the id, not the server.** This is the part that is easy to get wrong. If the server generates the key on arrival, a redelivery gets a *new* key and the dedup silently does nothing. The publisher must stamp a UUID on the message when it is first created, and reuse that same UUID on every retry. The same key then covers both transports — a Pub/Sub message and a plain HTTP call are deduplicated by one mechanism.

**The unique constraint is the lock.** Processing starts with an insert. If it succeeds, this instance owns the event. If it violates the primary key, someone else already has it and this instance drops the message. That is a single atomic operation — no read-then-write race, no distributed lock to hold, and it is correct across any number of replicas.

**`expiry_time` bounds the table and handles crashes.** It is computed as *request received time + that feature's request timeout*, so each Cloud Run service contributes its own timeout. An instance that dies mid-processing leaves a row that expires, allowing a legitimate retry later; without expiry a crash would permanently block reprocessing and the table would grow forever.

**Why this forces a SQL database.** The requirement is strong ACID guarantees on a write-heavy, highly concurrent "insert-or-fail" path — exactly what the unique-constraint trick depends on. That ruled out NoSQL and narrowed the candidates to distributed SQL engines with Postgres compatibility and full ACID (CockroachDB was the lead candidate). Note the shape of the argument: the *idempotency mechanism* dictated the *datastore choice*, not the other way round.

> [!note] Cold start shapes the language choice too
> The same design compared runtimes for Cloud Run, where startup is on the request path: **Quarkus native ~500 ms** vs **Node.js ~100 ms**. Worth knowing when a service is spiky enough to scale from zero often.

Related: [[In-memory job throttles silently break when you scale to multiple replicas]] — the same "check-then-act must be one atomic operation" rule, in the scheduling case.

Source: [[Common service architecture]] (Confluence).

## Related

- [[In-memory job throttles silently break when you scale to multiple replicas]]

%% ai-graph-start %%

**Related notes:**
- [[Common service architecture]]
- [[Claim work across pods with an expiring lease column on the row]]
- [[Score async API designs on crash recovery and multi-instance, not latency]]
- [[Mongo unique-index insert as CAS when the cache has no putIfAbsent]]
- [[In-memory job throttles silently break when you scale to multiple replicas]]

%% ai-graph-end %%