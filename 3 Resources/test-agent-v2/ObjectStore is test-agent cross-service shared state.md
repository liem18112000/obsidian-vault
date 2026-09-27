---
title: "ObjectStore is test-agent cross-service shared state"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22"
tags: [test-agent, object-store, gotcha, architecture]
---

# ObjectStore is test-agent cross-service shared state

In the test-agent-v2 stack the four agent containers (kga/tpd/tev/admin) are separate processes; the **only** state they share across the gather -> refine -> define -> implement pipeline is the **memory bank**, reached through the `ObjectStore` port (GCS in prod). 

Consequence: an *in-memory-per-container* store (`STORE_BACKEND=memory`) silently breaks the pipeline — KGA writes the pack but TPD, a different container, never sees it. A local multi-service run therefore needs a **shared** store. That is why `LocalFsObjectStore` (`STORE_BACKEND=local`, `common/store/local.py`) exists: a shared-filesystem blob store on a docker volume, with CAS via a generation counter guarded by a portable `O_EXCL` spin-lock (correct on Windows + Linux). Contrast: task/session/A2A state is per-container and does *not* need sharing for a single-instance run. See [[Run test-agent-v2 locally with docker-compose (no GCP)]].

## Related

- [[Run test-agent-v2 locally with docker-compose (no GCP)]]
