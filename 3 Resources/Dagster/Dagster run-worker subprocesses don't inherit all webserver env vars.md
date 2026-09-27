---
title: "Dagster run-worker subprocesses don't inherit all webserver env vars"
created: 2026-09-10
type: lesson
status: seedling
source: "customer360 UAT incident 2026-09-10"
tags: [dagster, s3, compute-logs, gotcha, env]
---

# Dagster run-worker subprocesses don't inherit all webserver env vars

A Dagster run executes in a **separate worker subprocess** (e.g. `dagster api execute_run` under `DefaultRunLauncher`), which does **not** automatically inherit every environment variable the webserver/daemon sees. If the instance config (`dagster.yaml`) references env names via `{ env: NAME }` that are set for the control plane but **unset in the worker subprocess**, the op body runs but post-processing fails.

Concrete failure: an `S3ComputeLogManager` block referencing `DAGSTER_LOGS_BUCKET` / `DAGSTER_S3_ENDPOINT_URL` that were never exported to the worker → `PostProcessingError` on compute-log upload → **every run fails** at the end even though the compute succeeded.

**Fix:** render the instance config against env var names that the worker actually receives (the ones written into the container `--env-file`, e.g. `MINIO_BUCKET` / `S3_ENDPOINT_URL`), and make sure those names are exported into the run-worker environment. Seen on customer360 backend-system UAT (fix commit 2440e6e, 2026-09).

## Related
[[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

## Related

- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]
