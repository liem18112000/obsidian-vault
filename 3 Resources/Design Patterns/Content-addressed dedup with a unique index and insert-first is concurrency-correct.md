---
ai_hash: 74c1bc0e8ad672c0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities: []
source: luz_docs_import LUZ-158230 · 2026-08-11
status: seedling
tags:
- dedup
- idempotency
- mongodb
- concurrency
- hashing
title: Content-addressed dedup with a unique index and insert-first is concurrency-correct
type: concept
---

# Content-addressed dedup with a unique index and insert-first is concurrency-correct

For idempotent batch import, make dedup identity **content-addressed** and enforce it with a database **unique index** using **insert-first** semantics:

- Batch identity: `(tenantId, zipSha256)` — SHA-256 of the archive bytes, not its (caller-controlled, non-unique) name.
- Item identity: `(tenantId, contentSha256, normalizedPath)` — SHA-256 of the file bytes + the NFC path.
- Enforcement: `createIndex({tenantId, contentSha256, normalizedPath}, {unique:true})`, then **INSERT before create**; treat the duplicate-key error (Mongo **E11000**) as the "already imported" signal.

**Why it beats read-then-check:** a `contains()` over previously-imported paths is (a) racy under concurrency — two workers both read "absent" and both create — and (b) content-blind, so it cannot tell a *revised* file (new content hash ⇒ should import) from a *true* duplicate (same hash ⇒ skip), and it collapses two unrelated batches that share a name. The unique index makes the datastore the single arbiter; insert-first is atomic.

Compute the hashes cheaply in passes you already do: the archive hash folds into the upload stream (`DigestInputStream`), the per-file hash into the read that the create already performs.

Related: [[NFC-normalize dedup keys so macOS NFD and Windows NFC file re-exports converge]]

## Related

- [[NFC-normalize dedup keys so macOS NFD and Windows NFC file re-exports converge]]

%% ai-graph-start %%

**Related notes:**
- [[NFC-normalize dedup keys so macOS NFD and Windows NFC file re-exports converge]]
- [[luz-docs-import dedup identity is the uploaded zip filename (importZipName)]]
- [[luz-docs-import ZIP import timing fresh 100-doc ~40s vs deduped sub-second]]
- [[luz-docs-import importZipName comes from the uploaded multipart filename, not the on-disk zip]]
- [[Mongo unique-index insert as CAS when the cache has no putIfAbsent]]

%% ai-graph-end %%