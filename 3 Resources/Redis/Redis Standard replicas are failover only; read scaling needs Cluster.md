---
ai_hash: 7b4cade8b1e8ed26
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Memorystore for Redis vs Redis Cluster (LUZ)'
status: seedling
tags:
- redis
- memorystore
- gcp
- scaling
- sharding
- confluence-distilled
title: Redis Standard replicas are failover only; read scaling needs Cluster
type: lesson
---

# Redis Standard replicas are failover only; read scaling needs Cluster

Managed Redis comes in two shapes that look similar and scale completely differently. On GCP the split is **Memorystore for Redis** versus **Memorystore for Redis Cluster**:

| | Redis (Basic & Standard) | Redis **Cluster** |
|---|---|---|
| **Scaling** | Vertical — bigger node | **Horizontal — add shards** |
| **Max data** | 300 GB | **10+ TB** |
| **Performance** | Capped by a single node's CPU | **Multi-node, multi-threaded** |
| **HA** | Standard tier: 1 replica | **Multiple replicas per shard** |
| **Read scaling** | **No** — the replica is for failover only | **Yes — clients can read from replicas** |

**The decision:**

- **Standard** for a single-node workload, basic caching, no complex scaling need.
- **Cluster** when you need high throughput, automatic sharding, multi-node scaling, or **read replicas for throughput**.

**The non-obvious line is read scaling.** On the non-cluster tier the replica exists purely for failover — it does not serve reads. Teams routinely assume "Standard tier has a replica, so we have twice the read capacity", and then discover their read throughput is still bounded by one node. If reads are your bottleneck, the replica count on the Standard tier will not help you; only Cluster will.

**The second is that vertical scaling has a hard ceiling** at 300 GB, and it is not a gradual slope — you scale up until you cannot, and then the migration to Cluster is a topology change, not a config change. Worth knowing the number before your dataset is in sight of it.

> [!warning] Cluster mode changes what your client can do
> Horizontal sharding means keys live on different nodes, so **multi-key operations and transactions across keys stop working** unless the keys hash to the same slot. `MGET`, Lua scripts touching several keys, and `MULTI/EXEC` all need hash tags to co-locate their keys. Migrating to Cluster is therefore an application change, not just an infrastructure one — audit multi-key usage before committing.

> [!tip] Pick on the axis you will actually hit
> Ask which limit you reach first: **data size** (→ Cluster), **read throughput** (→ Cluster, for replica reads), **write throughput** (→ Cluster, for multi-node), or **none of them** (→ Standard, and save the operational complexity). A cache that comfortably fits one node and serves a modest read rate gains nothing from sharding except more ways to go wrong.

Related: [[Store pod-level facts once, not copied into every user key]] — Redis data-modelling choices that reduce how much you need to scale in the first place.

Source: [[Memorystore for Redis Vs Memorystore for Redis Cluster]] (LUZ, Confluence).

## Related

- [[Store pod-level facts once, not copied into every user key]]

%% ai-graph-start %%

**Related notes:**
- [[Memorystore for Redis Vs Memorystore for Redis Cluster]]
- [[Run an event-broker Redis as a dedicated co-located instance, not the cache Redis]]
- [[Broker Redis needs opposite config from cache Redis, run it separately]]
- [[Memorystore Redis is always VPC-internal — no public endpoint]]
- [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]]

%% ai-graph-end %%