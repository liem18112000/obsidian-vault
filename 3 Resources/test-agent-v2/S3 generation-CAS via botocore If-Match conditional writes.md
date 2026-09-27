---
ai_hash: 1e9c47dd0d1902b1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- s3
- minio
- boto3
- cas
- object-store
- test-agent
title: S3 generation-CAS via botocore If-Match conditional writes
type: lesson
---

# S3 generation-CAS via botocore If-Match conditional writes

The memory bank uses GCS **generation-CAS** (`get_blob(p).generation` -> `blob(p).upload_from_string(if_generation_match=g)`) to avoid lost updates. To run that contract on S3/MinIO, map it onto S3 **conditional writes**: keep the generation in object user-metadata, and close each CAS write with a conditional PUT — `If-None-Match: *` for a create (generation 0) and `If-Match: <current ETag>` for a replace. A racing writer makes the server 412 -> translate to the domain `CASConflict`. Re-read the live generation at write time first so a stale snapshot is caught before the PUT.

KEY ENABLER: `botocore` PutObject supports `IfMatch` AND `IfNoneMatch` (verified via `service_model("s3").operation_model("PutObject").input_shape.members`). CEILING: needs an S3 server that honours If-Match on PUT (recent MinIO / AWS do). Implemented in `common/store/s3.py` (S3ObjectStore). See [[ObjectStore is test-agent cross-service shared state]].

## Related

- [[ObjectStore is test-agent cross-service shared state]]

%% ai-graph-start %%

**Related notes:**
- [[GCS compare-and-set with if_generation_match (optimistic concurrency)]]
- [[ObjectStore is test-agent cross-service shared state]]
- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]

%% ai-graph-end %%