---
ai_hash: 27a4b70eefdea6e0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Merkle Tree - Complete Guide with Mermaid Diagrams (2025-11-18)'
status: seedling
tags:
- cryptography
- merkle-tree
- data-structures
- distributed-systems
- blockchain
title: A Merkle tree proves one item belongs to a set without revealing or transferring
  the set
type: concept
---

# A Merkle tree proves one item belongs to a set without revealing or transferring the set

A **Merkle tree** (hash tree) hashes each data block into a leaf, then repeatedly hashes pairs of nodes upward until a single **root hash** summarises the whole set.

Its defining property is the **inclusion proof**: to convince someone that block *X* is in a set of *n* blocks, you send *X* plus the **sibling hash at each level** — about `log₂(n)` hashes — and they recompute the root. For a million blocks that is ~20 hashes instead of a million. They never need the other blocks, and you never have to reveal them.

Why this keeps reappearing:

- **Bitcoin / Ethereum** — prove a transaction is in a block without downloading the block.
- **Git** — a commit hash transitively fixes every byte of the tree it points at.
- **Certificate Transparency** — prove a TLS certificate was logged.
- **Cassandra / DynamoDB anti-entropy** — compare two replicas by exchanging roots, then walk *only* the subtrees whose hashes differ, so a sync costs O(differences) not O(dataset).
- **Audit logs** — see [[Merkle checkpoints restore parallel writes to a hash-chained audit log]].

The two properties that make it worth reaching for: any single-bit change propagates to the root (tamper-evidence), and the tree can be **built in parallel** because sibling subtrees are independent — unlike a linear hash chain.

## Related

- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]
- [[A hash-chained audit log cannot be written in parallel]]

## Related

- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]

%% ai-graph-start %%

**Related notes:**
- [[Merkle Tree - Complete Guide with Mermaid Diagrams]]
- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]
- [[Per-record signatures prove authenticity but not completeness, so deletion and reordering go undetected]]
- [[A hash chain proves integrity but not authorship, so a database admin can silently rebuild it]]
- [[Solution - Enhanced Chain-Signature Hybrid]]

%% ai-graph-end %%