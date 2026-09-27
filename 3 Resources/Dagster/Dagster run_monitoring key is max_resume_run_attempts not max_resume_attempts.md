---
title: "Dagster run_monitoring key is max_resume_run_attempts not max_resume_attempts"
created: 2026-09-09
type: gotcha
status: seedling
source: "LEO CDP UAT incident 2026-09-09"
tags: [dagster, config, gotcha, run-monitoring]
---

# Dagster run_monitoring key is max_resume_run_attempts not max_resume_attempts

In `dagster.yaml` the run-monitoring resume-attempts key is **`max_resume_run_attempts`**, not `max_resume_attempts`. The wrong key raises `DagsterInvalidConfigError` — `Received unexpected config entry "max_resume_attempts" at path root:run_monitoring` — while loading the instance. That aborts `dagster dev` at startup, and with a container run under `--restart unless-stopped` it **crash-loops the container** (looks like a redeploy that "won't come up").

**Valid `run_monitoring` keys:** `enabled`, `start_timeout_seconds`, `cancel_timeout_seconds`, `cancellation_thread_poll_interval_seconds`, `free_slots_after_run_end_seconds`, `max_resume_run_attempts`, `max_runtime_seconds`, `poll_interval_seconds`.

The error message helpfully prints the full accepted schema — read it instead of guessing. Set `max_resume_run_attempts: 0` to disable auto-resume of crashed runs (fail them instead).

Surfaced while enabling `run_monitoring` to fix [[Dagster runs stuck in QUEUED forever = orphaned runs leak all concurrency slots]].

## Related

- [[Dagster runs stuck in QUEUED forever = orphaned runs leak all concurrency slots]]
