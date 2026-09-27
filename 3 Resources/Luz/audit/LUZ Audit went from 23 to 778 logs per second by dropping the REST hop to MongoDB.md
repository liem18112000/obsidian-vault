---
title: "LUZ Audit went from 23 to 778 logs per second by dropping the REST hop to MongoDB"
created: 2026-09-27
type: observation
status: seedling
source: "Confluence: Luz Audit System - Performance Optimization Proposal (2025-11-04)"
tags: [luz-audit, performance, quarkus, virtual-threads, mongodb, architecture, kepler]
---

# LUZ Audit went from 23 to 778 logs per second by dropping the REST hop to MongoDB

A prototype (`luz-demo`) rewrote the LUZ Audit write path and measured **23 → 778 logs/sec** on a single tenant (10 000 logs: 441 s → 12.9 s), and **~63 → 623 logs/sec** across 50 tenants.

The four changes, in rough order of how much they bought:

1. **Direct MongoDB driver instead of going through `luz_jsonstore` over REST.** Every audit write had been an HTTP round-trip to another service that then talked to Mongo. Removing that hop removes serialization, a network leg, and someone else's thread pool from the hot path.
2. **Quarkus + virtual threads (Java 21) instead of WildFly EJB.** Audit writes are I/O-bound, so a platform thread per write wastes a thread on waiting. Virtual threads let ~1000 tenants proceed concurrently without a 1000-thread pool.
3. **Ring buffer with CAS for batching** (~200 ns to add) instead of sequential EJB calls, so records accumulate into batches without a lock.
4. **A 1000-connection direct pool**, sized for that parallelism.

The transferable lesson is #1: **a REST hop in front of the database is invisible in the code but dominant in the profile.** The "service owns its datastore" rule is right, but when one service's *only* job is to proxy another's writes, the hop is pure latency tax — and it is usually the cheapest thing on this list to remove.

Note the numbers are prototype-vs-production, so the 34x is an upper bound; it conflates the architecture change with the absence of production middleware.

## Related

- [[LUZ Audit spent 5 database operations per log entry, ~47ms, from chain maintenance]]
- [[A hash-chained audit log cannot be written in parallel]]

## Related

- [[LUZ Audit spent 5 database operations per log entry]]
- [[~47ms]]
- [[from chain maintenance]]
