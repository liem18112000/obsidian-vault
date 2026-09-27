---
ai_hash: f9f9f4ff34ad86f3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities:
- UAT Dagster orchestrator
- vServer
- VKS
- VM
- Docker
- SSH
- s-general-2x4 VM
- s-general-1x2 VM
- customer360-dagster image
- backend-system
- dagster-webserver
- backend-system-daemon
- dagster-daemon
- customer360 vDB
- PostgreSQL
- run/event/schedule state
- S3/vStorage
- compute logs
- SQLite
- local-log
- QueuedRunCoordinator
- max_concurrent_runs
- DAGSTER_MAX_CONCURRENT_RUNS
- run_monitoring reaper
- DefaultRunLauncher
- daemon box
- Phase 0
- Phase 1
- dagster-scaling-uat-vserver.md
- dagster-scaling-analysis.md
- backend-system/deployment.md Mode 1
- s-general-4x8 VM
- s-general flavor family
- VNG Cloud HCM03-1C
- uat.tfvars
source: session 2026-09-09
status: seedling
tags:
- dagster
- leo-customer360
- uat
- vserver
- backend-system
title: 'UAT vServer Dagster topology: split webserver+daemon on one s-general box'
type: observation
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

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]
- [[VNG Cloud HCM03-1C offers only the s-general flavor family]]
- [[Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1]]
- [[VNG vServer flavor resize (s-general-1x2 - 2x4) is an in-place terraform change (0 destroy) but reboots the box]]

**Relations:**
- UAT Dagster orchestrator — *runs on* — vServer
- UAT Dagster orchestrator — *not* — VKS
- vServer — *uses* — VM
- vServer — *uses* — Docker
- vServer — *uses* — SSH
- backend-system — *is* — dagster-webserver
- backend-system-daemon — *is* — dagster-daemon
- customer360-dagster image — *provides* — backend-system
- customer360-dagster image — *provides* — backend-system-daemon
- backend-system — *runs on* — s-general-2x4 VM
- backend-system-daemon — *runs on* — s-general-2x4 VM
- s-general-2x4 VM — *resized from* — s-general-1x2 VM
- backend-system — *accesses* — customer360 vDB
- backend-system-daemon — *accesses* — customer360 vDB
- PostgreSQL — *stores* — run/event/schedule state
- S3/vStorage — *stores* — compute logs
- SQLite — *is fallback for* — PostgreSQL
- local-log — *is fallback for* — S3/vStorage
- QueuedRunCoordinator — *manages* — max_concurrent_runs
- max_concurrent_runs — *default value* — 2
- DAGSTER_MAX_CONCURRENT_RUNS — *controls* — max_concurrent_runs
- run_monitoring reaper — *releases slots for* — orphaned runs
- DefaultRunLauncher — *executes runs on* — daemon box
- daemon box — *is* — s-general-2x4 VM
- UAT Dagster orchestrator — *implements* — Phase 0
- UAT Dagster orchestrator — *implements* — Phase 1
- dagster-scaling-uat-vserver.md — *derived from* — dagster-scaling-analysis.md
- dagster-scaling-uat-vserver.md — *documents* — UAT Dagster orchestrator
- backend-system/deployment.md Mode 1 — *recommends* — s-general-4x8 VM
- s-general-4x8 VM — *recommended for* — UAT Dagster orchestrator
- s-general-4x8 VM — *has* — 4 vCPU
- s-general-4x8 VM — *has* — 8 GB
- s-general-4x8 VM — *has* — 50 GB root disk
- s-general-2x4 VM — *has* — 2 vCPU
- s-general-2x4 VM — *has* — 4 GB
- s-general-2x4 VM — *has* — 20 GB disk
- VNG Cloud HCM03-1C — *offers* — s-general flavor family
- uat.tfvars — *configures flavor for* — UAT Dagster orchestrator

%% ai-graph-end %%