---
title: "Connection count, not tenant count, sizes a multi-tenant Postgres cluster"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Postgres Architecture Blueprint V2023 (LUZ)"
tags: [postgres, multi-tenancy, pgbouncer, connection-pooling, capacity-planning, confluence-distilled]
---

# Connection count, not tenant count, sizes a multi-tenant Postgres cluster

Multi-tenant Postgres layouts are usually argued on isolation and blast radius. The constraint that actually decides it is **connection count**, because every idle Postgres connection costs memory whether or not it is doing anything — a minimum of **~1.5 MB each** with default memory settings.

The two layouts:

| Layout | Separation | Grouping |
|---|---|---|
| **One database per tenant** | All of a tenant's data (across modules) in one logical DB; modules separated by **schema** | Many such DBs grouped into a cluster |
| **One database per module** | All of a module's data in one DB; tenants separated by **schema** | Clusters may hold several such DBs |

"Logical database" is the key phrase — it is how the data is *viewed*, and it does not have to match the physical cluster layout. That indirection is what lets you shard later: in the per-module variant, odd-numbered tenants can live on `cluster2` and even-numbered on `cluster3` without the application's view changing.

**The math that decides it.** Work it out before choosing, using your real fleet shape:

```
100 backend modules × 2 pods            =   200 module instances
200 instances × 20 max pool connections = 4,000 connections
4,000 connections × 1.5 MB              =    ~6 GB of connection memory alone
```

That is the floor, before any query does work, and it scales with **pods × pool size**, *not* with tenant count. The number of tenants (100,000 in this design) drives how many logical databases or schemas exist; the number of *services* drives connections. Confusing the two is the usual planning error.

> [!tip] This is why PgBouncer is not optional at scale
> A connection pooler in front (here, 3 PgBouncer nodes) lets thousands of application-side connections multiplex onto a far smaller number of real backend connections. Without it, the per-connection memory floor sets your cluster size. With it, you size for concurrent *active* queries instead.

> [!warning] Migrate incrementally, and expect a mixed world
> The blueprint explicitly plans for partial migration: one module fully moved, another with `public` data and *some* tenants moved while others stay on the old layout, a third untouched. Any routing layer therefore has to answer "where does tenant X's module Y live?" per tenant, not per module — build that lookup before the first migration, not after.

Source: [[Postgres Architecture Blueprint V2023]] (LUZ, Confluence).
