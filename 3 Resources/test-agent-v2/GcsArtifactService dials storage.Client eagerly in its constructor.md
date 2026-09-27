---
title: "GcsArtifactService dials storage.Client eagerly in its constructor"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22"
tags: [test-agent, adk, gcs, gotcha, credentials]
---

# GcsArtifactService dials storage.Client eagerly in its constructor

Google ADK `GcsArtifactService.__init__` calls `storage.Client()` **eagerly** in the constructor, which resolves GCP credentials immediately. Constructing it with no ADC / GCP creds raises `DefaultCredentialsError` at container boot — so merely *wiring* it (even with a dummy bucket) crashes a no-GCP local run before any request.

Fix in `common/adk/services.py` `build_runner`: only use `GcsArtifactService` when `STORE_BACKEND` is the real `gcs` backend; otherwise fall back to `InMemoryArtifactService`. Safe because nothing in the pipeline actually calls `save_artifact`/`load_artifact` — deliverables flow through the `ObjectStore` memory bank, not the ADK artifact service (grep confirmed). General lesson: a client that authenticates in its constructor cannot be safely constructed-then-unused; gate the construction, not the call. See [[ObjectStore is test-agent cross-service shared state]].

## Related

- [[ObjectStore is test-agent cross-service shared state]]
