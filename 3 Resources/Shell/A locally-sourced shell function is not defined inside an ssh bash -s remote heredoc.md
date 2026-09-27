---
title: "A locally-sourced shell function is not defined inside an ssh bash -s remote heredoc"
created: 2026-09-14
type: lesson
status: seedling
source: "session 2026-09-14, leo-customer360 PR #67"
tags: [shell, bash, ssh, heredoc, gotcha, exit-127]
---

# A locally-sourced shell function is not defined inside an ssh bash -s remote heredoc

A shell function defined by `source`-ing a library **in the local shell** is NOT available inside a remote `ssh "$host" 'bash -s' <<'REMOTE' ... REMOTE` heredoc. The heredoc body runs in a *separate* bash on the remote host, which inherits none of the caller-shell function definitions — only what is on the remote PATH and the positional args passed to `bash -s`.

Symptom: `bash: line NN: <fn>: command not found` → the remote step exits `127`.

This bit the leo-customer360 CD deploy: PR #66 swapped `sudo docker pull "$IMAGE"` for `docker_pull_retry "$IMAGE"` inside the remote heredocs, but `docker_pull_retry` was only sourced (from `deployments/lib/ghcr.sh`) in the local runner shell. Every ghcr-mode deploy died with exit 127.

Fix: ship the function into the remote shell — see [[Ship a local bash function to a remote ssh bash -s with declare -f]]. Related: [[Feature-branch images are never pushed to GHCR in leo-customer360 CI]].

## Related

- [[Ship a local bash function to a remote ssh bash -s with declare -f]]
