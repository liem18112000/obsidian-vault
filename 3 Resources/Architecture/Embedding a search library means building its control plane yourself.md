---
title: "Embedding a search library means building its control plane yourself"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: IR - System Design (AI)"
tags: [build-vs-buy, lucene, search, multi-tenancy, scaling, confluence-distilled]
---

# Embedding a search library means building its control plane yourself

Embedding a search library rather than running a managed cluster gives you real advantages — and hands you a distributed database's **control plane** to build. The honest accounting is what makes this decision tractable.

**What embedded Lucene buys:**

- any storage layout you want, and **true physical isolation** per tenant
- **tiny per-tenant HNSW graphs**, which search and rebuild faster than one large one
- **per-tenant strategy versions** — tenant A on v1, tenant B on v2
- **in-process search, no network hop**
- **trivially cheap tenant deletion**

**What you must then build yourself** — none of it exotic, all of it required:

- tenant → node placement, and rebalancing when it changes
- replication and failover
- an index-open **LRU pool** (you cannot hold every tenant's index open at once)
- query routing
- observability
- the entire blue/green migration orchestration

The estimate in the design is the sentence worth remembering: this is **60–70% of your engineering budget, and it is undifferentiated** — work that makes your product no better than a competitor who bought it.

**The scaling shape is equally concrete:** excellent up to roughly **low thousands of active tenants per node pool**, then increasingly dominated by **heap, file handles, and placement logic**. That is a specific, checkable threshold rather than a vague "it won't scale" — you can compare it against your actual tenant count and growth curve today.

> [!tip] Make the build-vs-buy argument in these three terms
> **What does building uniquely enable?** (here: physical isolation, per-tenant versions, cheap deletion) · **What undifferentiated work does it force?** (the control-plane list) · **Where does the approach stop working?** (thousands of tenants per node pool). A comparison that produces all three is decidable. One that lists only features is not.

> [!warning] The control-plane work does not appear in the prototype
> An embedded index looks wonderful in a proof of concept, because a PoC has one node, a handful of tenants, and no failover. Every item on the cost list becomes necessary only at production scale — which is precisely when changing direction is most expensive.

Related: [[Per-tenant encryption keys make GDPR deletion a key destruction]] — one of the capabilities the embedded route is bought for.

Source: [[IR - System Design]] (AI, Confluence).

## Related

- [[Per-tenant encryption keys make GDPR deletion a key destruction]]
