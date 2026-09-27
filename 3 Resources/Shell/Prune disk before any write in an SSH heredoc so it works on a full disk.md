---
ai_hash: eae490c05e17b226
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-04
entities: []
source: session 2026-09-04
status: seedling
tags:
- shell
- ssh
- heredoc
- docker
- disk-space
- technique
title: Prune disk before any write in an SSH heredoc so it works on a full disk
type: howto
---

# Prune disk before any write in an SSH heredoc so it works on a full disk

When a deploy runs remote commands via `ssh host 'bash -s' <<'REMOTE' ... REMOTE`, the whole script body is streamed to the remote `bash` over **stdin** — it is never written to a file on the remote disk. That property lets you recover from an already-full remote disk: put a disk-reclaim step (e.g. `docker image prune -a -f`) at the *top* of the heredoc, **before any `mktemp`, env-file write, or `docker pull`**. Because bash reads the commands from the pipe, the prune executes and frees space even when the filesystem has zero bytes free; the subsequent writes then succeed.

**Gotcha / ordering rule:** the reclaim MUST come before the first thing that touches disk. If you place it after `mktemp`/`cat > file`, that write fails first (`No space left on device`) and the prune never runs — self-healing is lost.

Applies to the leo-customer360 `deploy-*.sh` remote blocks. Related root cause: [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]].

## Related

- [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]]

%% ai-graph-start %%

**Related notes:**
- [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]]
- [[SSH keepalive prevents broken-pipe exit 255 on long remote docker pulls]]
- [[A git merge can silently revert a merged PR when two branches edit the same region]]
- [[Docker json-file logs are unbounded; cap them with --log-opt on high-volume containers]]
- [[leo-customer360 CD runs after CI via workflow_run and resumable deploy-all.sh]]

%% ai-graph-end %%