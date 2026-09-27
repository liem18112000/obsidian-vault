---
title: "Docker can report OOMKilled=false on a cgroup memcg OOM — check dmesg"
created: 2026-09-16
type: lesson
status: seedling
source: "session 2026-09-16 leo-customer360 jaeger 502"
tags: [docker, oom, cgroup, debugging, gotcha]
---

# Docker can report OOMKilled=false on a cgroup memcg OOM — check dmesg

On a container that keeps restarting, `docker inspect` can show `OOMKilled=false` and `ExitCode=0` even though the kernel killed it for exceeding its cgroup **memory** limit. When the memcg OOM-killer reaps the main process *inside* the container cgroup (rather than SIGKILLing the container init), containerd records what looks like a clean exit instead of the usual `137` / `OOMKilled=true`.

So never diagnose OOM from `docker inspect` alone — confirm against the kernel log:

```bash
sudo dmesg -T | grep -iE "oom|kill"
# tell: "Memory cgroup out of memory: Killed process ... oom_memcg=/system.slice/docker-<id>.scope"
#       "constraint=CONSTRAINT_MEMCG"  and a huge total-vm line
```

**Tell-tale pattern:** a container with an enormous `RestartCount`, exit code 0, restarting on a fixed ~30s cadence — that regular beat is the process booting, hitting the mem cap, and being reaped over and over. `docker stats` sitting pinned near the `--memory` cap corroborates it.

Root fix is always: lower the memory footprint or raise the cap — see [[Jaeger on a small box: use in-memory bounded storage, not badger, to avoid OOM 502]] for a concrete case.

## Related

- [[Jaeger on a small box: use in-memory bounded storage]]
- [[not badger]]
- [[to avoid OOM 502]]
