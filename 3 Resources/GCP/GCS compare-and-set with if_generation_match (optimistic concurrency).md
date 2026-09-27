---
ai_hash: c025434cdad5fbde
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27 — kga memory.py
status: seedling
tags:
- gcs
- concurrency
- cas
- gcp
- python
- testing
title: GCS compare-and-set with if_generation_match (optimistic concurrency)
type: lesson
---

# GCS compare-and-set with if_generation_match (optimistic concurrency)

Google Cloud Storage has no locks, but every object has a **generation** number that changes on each write — use it for **optimistic concurrency (compare-and-set)** on a shared file like an index:

1. Read the object; capture its `generation` (via `bucket.get_blob(name).generation`). If the object is absent, use generation **0**.
2. Mutate in memory.
3. Write with `blob.upload_from_string(data, if_generation_match=<generation>)`. Semantics: `if_generation_match=0` = "create only if it does NOT exist"; `=N` = "write only if current generation is still N".
4. On mismatch GCS raises `google.api_core.exceptions.PreconditionFailed` (HTTP 412) — **reload, re-apply the mutation, retry** in a bounded loop. This is lost-update-safe without any lock.

**Testing:** inject the bucket (duck-typed) so a tiny in-memory fake can emulate it — track a per-name generation dict, and in the fake `upload_from_string` raise `PreconditionFailed` when `if_generation_match` != current. To exercise the retry path deterministically, bump the fake generation inside the mutate callback on the first attempt.

Context: kga `memory.py` index writer (LUZ-159671 test-agent). Note pattern nearby: store data as a JSON sidecar (source of truth) + a rendered Markdown file (human artifact); merge across runs by unioning on a canonical key.

## Related

- [[Cloud Run v2 has startup_probe + liveness_probe, no readiness probe]]

%% ai-graph-start %%

**Related notes:**
- [[S3 generation-CAS via botocore If-Match conditional writes]]
- [[luz-jsonstore optimistic-concurrency PATCH version-number is an equality CAS]]
- [[Mongo unique-index insert as CAS when the cache has no putIfAbsent]]
- [[Write-stub bank proxy test a persistence-writing pipeline against live reads without mutating state]]
- [[Shared aggregate write targets need CAS, not plain $set]]

%% ai-graph-end %%