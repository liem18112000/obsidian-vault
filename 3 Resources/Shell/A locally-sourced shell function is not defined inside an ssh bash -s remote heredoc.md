---
ai_hash: c99429240bf9d32a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
source: 'session 2026-09-14, leo-customer360 PR #67'
status: seedling
tags:
- shell
- bash
- ssh
- heredoc
- gotcha
- exit-127
title: A locally-sourced shell function is not defined inside an ssh bash -s remote
  heredoc
type: lesson
---

# A locally-sourced shell function is not defined inside an ssh bash -s remote heredoc

A shell function defined by `source`-ing a library **in the local shell** is NOT available inside a remote `ssh "$host" 'bash -s' <<'REMOTE' ... REMOTE` heredoc. The heredoc body runs in a *separate* bash on the remote host, which inherits none of the caller-shell function definitions — only what is on the remote PATH and the positional args passed to `bash -s`.

Symptom: `bash: line NN: <fn>: command not found` → the remote step exits `127`.

This bit the leo-customer360 CD deploy: PR #66 swapped `sudo docker pull "$IMAGE"` for `docker_pull_retry "$IMAGE"` inside the remote heredocs, but `docker_pull_retry` was only sourced (from `deployments/lib/ghcr.sh`) in the local runner shell. Every ghcr-mode deploy died with exit 127.

Fix: ship the function into the remote shell — see [[Ship a local bash function to a remote ssh bash -s with declare -f]]. Related: [[Feature-branch images are never pushed to GHCR in leo-customer360 CI]].

## Related

- [[Ship a local bash function to a remote ssh bash -s with declare -f]]

%% ai-graph-start %%

**Related notes:**
- [[Ship a local bash function to a remote ssh bash -s with declare -f]]
- [[CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job]]
- [[Feature-branch images are never pushed to GHCR in leo-customer360 CI]]
- [[leo-customer360 CD runs after CI via workflow_run and resumable deploy-all.sh]]
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]

%% ai-graph-end %%