---
title: "Route google-cloud clients to local emulators via *_EMULATOR_HOST"
created: 2026-09-22
type: howto
status: seedling
source: "session 2026-09-22"
tags: [gcp, emulator, pubsub, gcs, local-dev]
---

# Route google-cloud clients to local emulators via *_EMULATOR_HOST

The `google-cloud-storage` and `google-cloud-pubsub` clients transparently route to a **local emulator** when the right env var is set, using anonymous credentials (no GCP account): `STORAGE_EMULATOR_HOST` for GCS (e.g. fake-gcs-server), `PUBSUB_EMULATOR_HOST` for Pub/Sub (the `gcloud beta emulators pubsub` server). This is how the test-agent-v2 local stack runs the Pub/Sub distributed-generation path with zero GCP: `PUBSUB_EMULATOR_HOST=pubsub:8085` + a topic/push-subscription created via the emulator REST API (`PUT /v1/projects/<p>/topics/<t>` and `.../subscriptions/<s>` with a `pushConfig.pushEndpoint`). The PublisherClient needs a project id but no creds. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
