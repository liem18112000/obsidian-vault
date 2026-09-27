---
ai_hash: 1db4dd4299f16214
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-04
entities:
- Docker
- json-file
- container stdout/stderr
- /var/lib/docker/containers/<id>/<id>-json.log
- size limit
- host disk
- high-volume service
- image accumulation
- leo-customer360 UAT
- data-tracking vServer
- data-tracking-api
- nginx LB
- access_log
- ingestion request
- event
- log rotation
- UAT VM disk
- docker run
- --log-opt max-size=10m
- --log-opt max-file=3
- LOG_OPTS
- newly created containers
- existing container's log
- docker rm -f
- redeploy
- already-bloated old json.log
- removed container
- docker container prune
- image prune
- running container's log file
- /etc/docker/daemon.json
- daemon restart
- per-run flags
- SHA-pinned docker pulls
- small deploy VM disks
source: session 2026-09-04
status: seedling
tags:
- leo-customer360
- docker
- logging
- disk-space
- nginx
- gotcha
title: Docker json-file logs are unbounded; cap them with --log-opt on high-volume
  containers
type: lesson
---

# Docker json-file logs are unbounded; cap them with --log-opt on high-volume containers

Docker's default logging driver (`json-file`) keeps container stdout/stderr in `/var/lib/docker/containers/<id>/<id>-json.log` with **no size limit**. On a high-volume service this silently fills the host disk — a second disk-overload vector separate from image accumulation.

**Where it bit (leo-customer360 UAT):** the data-tracking vServer runs N `data-tracking-api` replicas behind an nginx LB. The LB writes an `access_log` line for *every* ingestion request (one per event), and neither the replicas nor the LB set any rotation → the json logs grow without bound and overload the UAT VM disk.

**Fix:** add rotation to each `docker run`: `--log-opt max-size=10m --log-opt max-file=3` (caps each container to ~30 MB). Define once as a `LOG_OPTS` var and reuse across the replica and LB run commands.

**Gotchas / notes:**
- `--log-opt` only affects **newly created** containers — it does not retroactively rotate an existing container's log. Because the tracking deploy `docker rm -f`'s and recreates every replica + LB, a redeploy both caps future logs and *discards* the already-bloated old json.log with the removed container.
- `docker container prune`/`image prune` do NOT reclaim a running container's log file — only removing/recreating the container does.
- Alternative global fix: set defaults in `/etc/docker/daemon.json` (`log-driver`/`log-opts`), but it needs a daemon restart and still only applies to newly created containers, so per-run flags are simpler here.

Related: [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]].

## Related

- [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]]

%% ai-graph-start %%

**Related notes:**
- [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]]
- [[Jaeger on a small box use in-memory bounded storage, not badger, to avoid OOM 502]]
- [[Prune disk before any write in an SSH heredoc so it works on a full disk]]
- [[Pull customer360-api UAT error logs via SSH (docker logs on the api VM)]]
- [[Docker can report OOMKilled=false on a cgroup memcg OOM — check dmesg]]

**Relations:**
- Docker — *uses* — json-file
- json-file — *is default logging driver for* — Docker
- json-file — *stores* — container stdout/stderr
- container stdout/stderr — *is stored in* — /var/lib/docker/containers/<id>/<id>-json.log
- /var/lib/docker/containers/<id>/<id>-json.log — *has no* — size limit
- size limit — *causes* — host disk
- host disk — *to fill on* — high-volume service
- host disk — *can also fill due to* — image accumulation
- leo-customer360 UAT — *is environment for* — data-tracking vServer
- data-tracking vServer — *runs* — data-tracking-api
- data-tracking-api — *is behind* — nginx LB
- nginx LB — *writes* — access_log
- access_log — *is for* — ingestion request
- ingestion request — *is per* — event
- data-tracking-api — *lacks* — log rotation
- nginx LB — *lacks* — log rotation
- log rotation — *causes* — json logs to grow without bound
- json logs to grow without bound — *overload* — UAT VM disk
- docker run — *configures* — --log-opt max-size=10m
- docker run — *configures* — --log-opt max-file=3
- --log-opt max-size=10m — *and* — --log-opt max-file=3
- --log-opt max-size=10m and --log-opt max-file=3 — *provide* — log rotation
- LOG_OPTS — *is variable for* — --log-opt max-size=10m and --log-opt max-file=3
- --log-opt — *affects only* — newly created containers
- --log-opt — *does not affect* — existing container's log
- docker rm -f — *is part of* — redeploy
- redeploy — *recreates* — containers
- redeploy — *discards* — already-bloated old json.log
- already-bloated old json.log — *is from* — removed container
- docker container prune — *does not reclaim* — running container's log file
- image prune — *does not reclaim* — running container's log file
- removing/recreating the container — *reclaims* — running container's log file
- /etc/docker/daemon.json — *sets global* — log-opts
- global log-opts — *requires* — daemon restart
- global log-opts — *affects only* — newly created containers
- per-run flags — *are simpler than* — global log-opts
- SHA-pinned docker pulls — *accumulate* — 
- SHA-pinned docker pulls — *fill* — small deploy VM disks
- SHA-pinned docker pulls — *is related to* — Docker json-file logs

%% ai-graph-end %%