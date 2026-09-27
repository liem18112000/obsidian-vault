---
title: "Per-tenant encryption keys make GDPR deletion a key destruction"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: IR - System Design (AI)"
tags: [gdpr, crypto-shredding, multi-tenancy, search, opensearch, confluence-distilled]
---

# Per-tenant encryption keys make GDPR deletion a key destruction

Deleting a tenant's data from a search index is normally expensive and hard to prove: segments are immutable, deletes are tombstones, and the bytes survive until a merge rewrites them. **Per-tenant encryption keys** turn the problem inside out — destroy the key and the ciphertext is unrecoverable, whether or not the bytes are still on disk.

The design pairs three choices that reinforce each other:

- **Per-tenant indices** — physical separation, not a filter on a shared index.
- **Per-tenant keys** — each index encrypted under its own key.
- **Crypto-shredding** — GDPR erasure becomes *destroy the key*, which is atomic, instant, and provable, instead of a long-running delete-and-merge you have to verify.

**A third benefit falls out of the same split:** small per-tenant **HNSW graphs**. Approximate-nearest-neighbour graphs degrade as they grow; many small graphs are cheaper to build, faster to search, and — as the design notes — something Lucene handles well. Per-tenant isolation bought for compliance also buys retrieval performance.

**How the encryption is actually applied.** OpenSearch has an `opensearch-storage-encryption` plugin doing transparent encryption at the **Lucene Directory level** with per-index keys via AWS KMS, using a block cache and read-ahead to limit the performance penalty. Encrypting at the Directory layer means the index format is untouched — no bespoke field-level scheme to maintain.

> [!warning] Crypto-shredding is only as good as your key hygiene
> The guarantee collapses if a key was ever backed up somewhere you cannot destroy, cached in a process that outlives the deletion, or escrowed for recovery. "Destroy the key" must mean *every* copy — which in practice means a KMS whose deletion semantics you trust, and no application-side key caching that survives the shred.

> [!tip] It does not remove the need for ordinary deletion
> Crypto-shredding answers *"is this tenant's data gone?"* It does not reclaim disk, and it does not help with partial erasure — one user inside a tenant still needs a real delete. Use it as the tenant-level guarantee, not as a substitute for record-level deletes.

Related: [[Envelope encryption with Vault transit keeps Vault off the data path]] — the same key-custody idea applied to object storage.

Source: [[IR - System Design]] (AI, Confluence).

## Related

- [[Envelope encryption with Vault transit keeps Vault off the data path]]
