---
title: "Dagster { env: VAR } config is resolved in the run-worker subprocess, so the var must be in the container env"
created: 2026-09-09
type: gotcha
status: seedling
source: "session 2026-09-09"
tags: [dagster, env, config, s3, gotcha]
---

# Dagster { env: VAR } config is resolved in the run-worker subprocess, so the var must be in the container env

In a Dagster `dagster.yaml`, an `{ env: VAR }` value (e.g. in the `compute_logs` S3ComputeLogManager `bucket`/`endpoint_url`) is NOT resolved once at daemon boot — it is resolved lazily in whichever process instantiates that component, which for run execution is the **run-worker / code-server subprocess**. So the env var must exist in the container environment (via `docker run --env-file` or `-e`) so every subprocess inherits it.

**Failure mode:** if the var is missing, the daemon boots fine (no error), but each run/sensor tick fails at execution with `DagsterInvalidConfigError` -> `PostProcessingError: You have attempted to fetch the environment variable "X" which is not set`. Symptom looks like "runs fail immediately / SUCCESS never climbs" while the daemon looks healthy.

**Gotcha that caused it:** the renderers `s3_ready()` probe accepted old OR new env names (`DAGSTER_LOGS_BUCKET or MINIO_BUCKET`), but the emitted `S3_BLOCK` hard-coded the NEW names while the deploy only wrote the OLD ones -> probe passes, config fails in the child. Fix: emit `{ env: ... }` names that the deploy actually writes and that are confirmed present in `docker exec <c> printenv`.

## Related

- [[Dagster runs stuck in QUEUED forever = orphaned runs leak all concurrency slots]]
