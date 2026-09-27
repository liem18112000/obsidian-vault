---
title: "UAT vServer Dagster topology: split webserver+daemon on one s-general box"
created: 2026-09-09
type: observation
status: seedling
source: "session 2026-09-09"
tags: [dagster, leo-customer360, uat, vserver, backend-system]
---

# UAT vServer Dagster topology: split webserver+daemon on one s-general box

As of 2026-09-09 the leo-customer360 **UAT** Dagster orchestrator (`backend-system`) runs on **vServer** (VM + Docker + SSH), not VKS. Shape:

- **One `s-general-2x4` VM** (2 vCPU / 4 GB), resized up from `1x2` because in-process run workers were OOM/swapping.
- **Two containers from one image** (`customer360-dagster`), started by `deployments/server/deploy-backend.sh uat`: `backend-system` (the `dagster-webserver` on :3000) and `backend-system-daemon` (the **singleton** `dagster-daemon`). Both use `--network host` to reach the private customer360 vDB.
- **Adaptive storage**, rendered at container start by `backend-system/scripts/render_dagster_instance.py`: shared **PostgreSQL** (dedicated `dagster` DB, best-effort created) for run/event/schedule state + **S3/vStorage** compute logs, each probed and used only if reachable (SQLite / local-log fallback otherwise).
- **Bounded run queue always written:** `QueuedRunCoordinator` with `max_concurrent_runs` (default **2**, env `DAGSTER_MAX_CONCURRENT_RUNS`) plus a `run_monitoring` reaper (start_timeout 300s) so orphaned runs release their slot.
- **Runs execute in-process on the daemon box** (`DefaultRunLauncher`) — there are **no worker pools** and **no gRPC-per-code-location split** yet. All run compute lands on the single VM (why the box was resized).

This equals **Phase 0 + Phase 1** of the parent scaling analysis, already done. The vServer/UAT application of that analysis is written up in `deployments/docs/dagster-scaling-uat-vserver.md`, derived from `deployments/docs/dagster-scaling-analysis.md`. `backend-system/deployment.md` Mode 1 still calls for 4 vCPU / 8 GB — the live `2x4` box is under that target.

**Recommended fit for UAT = `s-general-4x8` (4 vCPU / 8 GB) + a 50 GB root disk** — the documented Mode 1 target. The current box is `s-general-2x4` (2 vCPU / 4 GB, 20 GB disk); bumping both flavor and disk is an in-place terraform change (0 destroy) that reboots the box, same mechanics as the `1x2 → 2x4` resize. Since UAT is a single box carrying the control plane *and* the in-process run compute, "which vServer" is just "one flavor big enough to hold it all" — worker VMs (the `1/2/1` pool overlay) are deferred: 6 of 9 code locations are placeholders and vServer has no scale-to-zero. Set the flavor in `deployments/server/overlays/uat.tfvars` server key `"1x2"`.

Flavor context: [[VNG Cloud HCM03-1C offers only the s-general flavor family]].

## Related

- [[VNG Cloud HCM03-1C offers only the s-general flavor family]]
