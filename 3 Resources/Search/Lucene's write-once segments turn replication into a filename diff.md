---
title: "Lucene's write-once segments turn replication into a filename diff"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: IR - System Design (AI)"
tags: [lucene, search, replication, immutability, segments, confluence-distilled]
---

# Lucene's write-once segments turn replication into a filename diff

Lucene never modifies a segment once written. That single property — **write-once, merge-driven** — is what makes near-real-time index replication almost trivially cheap.

**The segment lifecycle:**

1. Documents accumulate in a **RAM buffer**.
2. **Refresh** — the buffer is flushed to a new segment in the **OS page cache**, making it searchable **without an fsync**. This is why "near real time" is cheap: visibility does not wait on disk.
3. **Flush** — an actual `fsync`, clearing the translog. Durability, separated from visibility.
4. **Merge** — background threads coalesce small segments under a **tiered merge policy** (logarithmic merging).

**Why replication becomes a filename diff.** Because segment files are never changed after creation, syncing a primary to a replica means comparing **file names in the index directory — not their contents**. Any new file must be copied; any file already copied never needs copying again. No block-level diffing, no checksums over existing data, no conflict resolution.

**The design consequence worth carrying beyond Lucene:** *immutability turns synchronisation into set difference.* Any system whose on-disk units are append-only gets cheap replication, cheap snapshots, and cheap caching for the same reason. The costs are equally structural — deletes become tombstones, and space is reclaimed only by merges, which is why compaction/merge tuning dominates operational work in these systems.

> [!tip] Refresh and flush are different knobs
> **Refresh** controls *how soon a write is searchable*; **flush** controls *how soon it is durable*. Teams chasing "why isn't my document showing up?" usually want the refresh interval, and teams chasing write throughput usually want to think about flush and merge pressure. Conflating them leads to tuning the wrong one.

> [!note] Disk-first is a feature in constrained environments
> Lucene is characterised as *disk-optimised, not memory-dependent* — it behaves "like a constantly expanding library" rather than a database that wants the working set in RAM. That makes it a good fit where memory is the scarce resource, and it is a large part of why it underpins Elasticsearch, OpenSearch and MongoDB Atlas.

Related: [[Redis TTL should express liveness and be refreshed by a heartbeat]] — the contrasting case, where the store is memory-first and liveness is explicit.

Source: [[IR - System Design]] (AI, Confluence).

## Related

- [[Redis TTL should express liveness and be refreshed by a heartbeat]]
