---
title: "A Dagster poll sensor without run_key mints a duplicate run every tick"
created: 2026-09-10
type: lesson
status: seedling
source: "customer360 UAT live check 2026-09-10"
tags: [dagster, sensor, run-key, idempotency, gotcha]
---

# A Dagster poll sensor without run_key mints a duplicate run every tick

A Dagster `@sensor` that calls `yield RunRequest()` (or `return RunRequest()`) with **no `run_key`** creates a **brand-new run on every tick** — there is no deduplication. At `minimum_interval_seconds=30` that is ~120 runs/hour, per sensor, indefinitely. Combined with a stalled drain, the run queue explodes.

`run_key` is Dagsters idempotency handle: the daemon will **not** launch a second RunRequest with a `run_key` it has already launched for that sensor. Omitting it means "always launch".

**Two anti-patterns seen on customer360 UAT:**
- `identity_resolution_poll_sensor`: unconditional `yield RunRequest()` every tick, no guard, no `run_key` -> 1,513 queued dupes.
- `segmentation_poll_sensor`: has a cursor "changes since last check" guard (good — mostly `SkipReason`), but when changes exist it still emits `RunRequest()` with no `run_key`, so a busy window mints one dupe per tick -> 2,262 queued.

**Fix:** add a stable `run_key` (e.g. derived from the data window / cursor value), and/or gate behind a real change-check, and/or lengthen `minimum_interval_seconds`, and/or use a schedule instead of a poll sensor.

## Related
[[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
[[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

## Related

- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]
