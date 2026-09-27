---
ai_hash: 2a8b3ef3d6d969d9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities: []
source: session 2026-09-18, leo-customer360 CD run 35340347947
status: seedling
tags:
- ssh
- ci-cd
- gotcha
- docker
title: SSH keepalive prevents broken-pipe exit 255 on long remote docker pulls
type: lesson
---

# SSH keepalive prevents broken-pipe exit 255 on long remote docker pulls

When you run a command over SSH that stays quiet for minutes — a big `docker pull`, a long build — the TCP session can be reaped by a NAT/firewall/idle timeout even though the remote command is still working. The client then dies with `client_loop: send disconnect: Broken pipe` and **exit 255** (the generic SSH connection-failure code), taking the whole remote command down with it.

**Fix:** add keepalive probes to the SSH options so the connection is never idle:

```bash
-o ServerAliveInterval=30 -o ServerAliveCountMax=10
```

The client sends a probe every 30s and only gives up after 10 unanswered (~5 min). This holds the connection open across a slow-but-alive operation.

**Gotcha:** if the remote command has its own retry loop, that retry runs *inside* the single SSH session — a dropped session kills the retry too, so keepalive on the client side is what actually saves you.

Real case: leo-customer360 CD deploy broke here — a ~5m17s `docker pull` on the target VM tripped the reap every time until keepalive was added to `SSH_OPTS` in `deployments/server/deploy-backend.sh`.

## Related

- [[leo-customer360 CD runs after CI via workflow_run and resumable deploy-all.sh]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 CD runs after CI via workflow_run and resumable deploy-all.sh]]
- [[A locally-sourced shell function is not defined inside an ssh bash -s remote heredoc]]
- [[Prune disk before any write in an SSH heredoc so it works on a full disk]]
- [[Self-restarting kubectl port-forward keeps long-running scripts alive through drops]]
- [[Decouple long agent work from the harness task lifecycle]]

%% ai-graph-end %%