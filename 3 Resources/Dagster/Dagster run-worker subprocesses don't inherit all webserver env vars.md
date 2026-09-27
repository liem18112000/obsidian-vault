---
ai_hash: eca89165713cf0f8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: customer360 UAT incident 2026-09-10
status: seedling
tags:
- dagster
- s3
- compute-logs
- gotcha
- env
title: Dagster run-worker subprocesses don't inherit all webserver env vars
type: lesson
---

# Dagster run-worker subprocesses don't inherit all webserver env vars

A Dagster run executes in a **separate worker subprocess** (e.g. `dagster api execute_run` under `DefaultRunLauncher`), which does **not** automatically inherit every environment variable the webserver/daemon sees. If the instance config (`dagster.yaml`) references env names via `{ env: NAME }` that are set for the control plane but **unset in the worker subprocess**, the op body runs but post-processing fails.

Concrete failure: an `S3ComputeLogManager` block referencing `DAGSTER_LOGS_BUCKET` / `DAGSTER_S3_ENDPOINT_URL` that were never exported to the worker → `PostProcessingError` on compute-log upload → **every run fails** at the end even though the compute succeeded.

**Fix:** render the instance config against env var names that the worker actually receives (the ones written into the container `--env-file`, e.g. `MINIO_BUCKET` / `S3_ENDPOINT_URL`), and make sure those names are exported into the run-worker environment. Seen on customer360 backend-system UAT (fix commit 2440e6e, 2026-09).

## Related
[[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

## Related

- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

%% ai-graph-start %%

**Related notes:**
- [[Dagster { env VAR } config is resolved in the run-worker subprocess, so the var must be in the container env]]
- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]
- [[Split Dagster webserver and daemon must share fail-closed Postgres storage]]
- [[Dagster S3ComputeLogManager credentials via boto3 env, path-style via AWS config file]]
- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]

%% ai-graph-end %%