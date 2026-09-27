---
ai_hash: 00ab655006047125
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- test-agent
- adk
- gcs
- gotcha
- credentials
title: GcsArtifactService dials storage.Client eagerly in its constructor
type: lesson
---

# GcsArtifactService dials storage.Client eagerly in its constructor

Google ADK `GcsArtifactService.__init__` calls `storage.Client()` **eagerly** in the constructor, which resolves GCP credentials immediately. Constructing it with no ADC / GCP creds raises `DefaultCredentialsError` at container boot — so merely *wiring* it (even with a dummy bucket) crashes a no-GCP local run before any request.

Fix in `common/adk/services.py` `build_runner`: only use `GcsArtifactService` when `STORE_BACKEND` is the real `gcs` backend; otherwise fall back to `InMemoryArtifactService`. Safe because nothing in the pipeline actually calls `save_artifact`/`load_artifact` — deliverables flow through the `ObjectStore` memory bank, not the ADK artifact service (grep confirmed). General lesson: a client that authenticates in its constructor cannot be safely constructed-then-unused; gate the construction, not the call. See [[ObjectStore is test-agent cross-service shared state]].

## Related

- [[ObjectStore is test-agent cross-service shared state]]

%% ai-graph-start %%

**Related notes:**
- [[ObjectStore is test-agent cross-service shared state]]
- [[Module-level load_dotenv lets unit tests hit real cloud credentials]]
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]
- [[GCP auth ambient ADC in GCP-hosted runners vs explicit creds in external CI]]
- [[Non-WI GKE Google API auth mount a GSA key at the well-known ADC path]]

%% ai-graph-end %%