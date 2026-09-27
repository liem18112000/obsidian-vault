---
ai_hash: d46965152539a33d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Efficient way to write data parallelly into Postgres and MongoDB
  (HACKA)'
status: seedling
tags:
- migration
- dual-write
- dao
- dependency-injection
- mongodb
- postgres
- confluence-distilled
title: Dual-write to a new datastore via a composite implementation of the existing
  interface
type: lesson
---

# Dual-write to a new datastore via a composite implementation of the existing interface

To migrate from one database to another without a flag day, write to **both** for a while. The clean way to do that is a **composite implementation of the interface callers already use** — so no caller changes at all.

The structure:

```
interface IBaseDao          // insert, update, delete, findById …
   ↑           ↑           ↑
PostgresDao  MongoDao  ParallelDao      // ParallelDao holds the other two
```

Services depend on the interface. Which implementation they get is a configuration choice — inject `ParallelDao` and every existing call site starts dual-writing, with no edits to service code. `EletterDao` here stands for the real per-aggregate DAOs (`DeliveryDao`, `DocumentDao`, `RecipientTrackingDao`, …); the pattern repeats per aggregate.

**Why this beats the alternatives that were considered** — adding Mongo handling *inside* the existing Postgres DAO, or standing up a separate module:

- **The seam already exists.** If callers depend on an interface, a composite slots in behind it. Nothing above the DAO layer knows a migration is happening.
- **Rollback is a config change**, not a deploy — switch the injected implementation back.
- **It is the path to the end state.** When Mongo becomes authoritative, inject `MongoDao` and delete the composite. The dual-write is explicitly scaffolding: the design notes the solution is *"also viable for later only using MongoDB and no longer using Postgres."*

> [!warning] Dual-write is not atomic, and the identity problem is real
> Two datastores, two writes, no shared transaction: a crash between them leaves the stores disagreeing. Decide **which store is authoritative** during the transition, make the secondary write failure non-fatal but *logged and reconcilable*, and plan a reconciliation job. Do not pretend the composite gives you a transaction.
>
> The design flags the specific trap: **MongoDB has no mechanism for IDENTITY ids.** If Postgres generates sequence ids that Mongo must match, the id has to be generated *before* either write — by the application, not the database — or the two stores end up with different keys for the same record. This is the same argument as [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]: when two systems must agree on an identity, assign it upstream.

> [!tip] Give the composite an end date
> Dual-writes are cheap to start and easy to forget. Write down which store becomes authoritative and when the composite gets deleted — otherwise you are permanently paying double writes and maintaining two schemas.

Source: [[Efficient way to write data parallelly into both Postgres and MongoDB]] (HACKA, Confluence).

## Related

- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]

%% ai-graph-start %%

**Related notes:**
- [[Efficient way to write data parallelly into both Postgres and MongoDB]]
- [[Migration-free idempotent upserts via deterministic uuid5 primary keys]]
- [[Postgres ON CONFLICT DO UPDATE references the target row by unqualified table name]]
- [[Whole-aggregate read-modify-write for a per-child toggle causes lost updates under concurrent sibling writes]]
- [[Mongo unique-index insert as CAS when the cache has no putIfAbsent]]

%% ai-graph-end %%