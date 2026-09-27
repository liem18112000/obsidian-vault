---
ai_hash: a57dc477188943a3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Rhine API Explained (AI)'
status: seedling
tags:
- event-driven
- streaming
- avro
- kafka
- data-lake
- event-time
- confluence-distilled
title: Event records should carry new state keyed by entity id and stamped with event
  time
type: concept
---

# Event records should carry new state keyed by entity id and stamped with event time

An event message in a data-lake ingest stream is not a notification that something changed — it is the **new state of an entity** after the change. That distinction decides what consumers have to do.

> *"The record represents the state, not the state change."*

Three meta fields carry the design:

**`meta.id` — the entity, stable for its whole lifetime.**
Every message about the same user session uses that session's identifier. So `id` is not a message id; it is the key that groups a stream of records into the history of one thing. Consumers can group, deduplicate, and build a latest-state view keyed on it.

**`meta.method` — what caused the change: `create`, `update`, `delete`.**
The lifecycle is explicit rather than inferred. A session emits `create` at sign-in, any number of `update`s, and `delete` at sign-out or timeout. A consumer that only wants openings can filter without parsing the body.

**`meta.timestamp` — when the state change *occurred*.**
Explicitly *"the real time when that session was created regardless of any latency caused by messaging."* This is **event time**, not ingestion time — the distinction that decides whether windowed aggregations are correct. Compute "average session length" on ingestion time and a broker backlog silently corrupts the answer.

**Why state beats deltas here.** A consumer holding the latest record per `id` has the current truth with no replay, no ordering assumptions, and no need to have seen earlier messages. A late or duplicated message is harmless — it is the same state again. With deltas, a consumer must see *every* message, in order, exactly once, or its view diverges permanently.

**The topic and schema frame:**

- A **topic** is a named stream, and it is **bound to a schema** — the note's own analogy is a relational table, where every row shares a record type.
- Schemas **evolve, with multiple compatible versions live in parallel**; a message declares which version its record uses. This is why the encoding is Avro, which carries the schema and defines compatibility rules, rather than bare JSON.

> [!warning] "State, not state change" costs message size and carries a privacy tail
> Every event repeats the entity's full body — larger messages, and personal data duplicated across every event in the stream rather than sitting in one row. Retention and deletion requests then apply to the whole topic history, not a single record. Worth deciding before the topic has a year of traffic in it.

Related: [[Client-assigned idempotency keys with a unique constraint beat distributed locks]] — the other half of making event consumers safe against redelivery.

Source: [[Rhine API Explained]] (AI, Confluence).

## Related

- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]

%% ai-graph-start %%

**Related notes:**
- [[Rhine API Explained]]
- [[Lightweight-but-scalable web event collector pattern]]
- [[Store pod-level facts once, not copied into every user key]]
- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]
- [[Deep Dive EPC Notification - User Connection Registry - Data Model]]

%% ai-graph-end %%