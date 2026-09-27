---
ai_hash: f4195c79d615df4f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-04
entities:
- SHA-pinned docker pulls
- docker pull
- docker rm -f
- docker container prune -f
- docker image prune -a -f
- docker builder prune -a -f
- docker
- leo-customer360 vServer scripts
- deployments/server/deploy-*.sh
- small deploy VM disks
- VM
- container
- image
- SHA-pinned images
- mutable tag
- GitHub Actions deploy job
- uat
- api VM
- '2026-09-04'
- Prune disk before any write in an SSH heredoc so it works on a full disk
- env file
- No space left on device
- deploy
- old images
- stale images
- distinct image reference
- 'Issue: SHA-pinned docker pulls accumulate and fill small deploy VM disks'
- new image
source: session 2026-09-04, GH Actions run 33886578303
status: seedling
tags:
- leo-customer360
- docker
- deploy
- disk-space
- gotcha
title: SHA-pinned docker pulls accumulate and fill small deploy VM disks
type: lesson
---

# SHA-pinned docker pulls accumulate and fill small deploy VM disks

Each deploy in the leo-customer360 vServer scripts (`deployments/server/deploy-*.sh`) does a SHA-pinned `docker pull` of a new image but only `docker rm -f` the container — it never removes old images. Over many deploys the stale SHA-pinned images pile up and eventually fill a small VM's disk. The failure surfaces on the *next* deploy at the very first disk write — `cat: write error: No space left on device` while writing the env file — not as an obvious image/registry error, which makes it easy to misread as a code bug.

**Why it happens:** pinning to `@sha256:...` means every build produces a distinct image reference; none share a mutable tag, so nothing is ever overwritten/GC'd automatically.

**Fix:** add a best-effort reclaim step to each deploy (`docker container prune -f`; `docker image prune -a -f`; `docker builder prune -a -f`). `image prune -a` is safe to run mid-deploy because the currently-running container still holds its image at that moment, so only stale images are dropped.

Seen in a GitHub Actions deploy job (uat, api VM) on 2026-09-04.

## Related

- [[Prune disk before any write in an SSH heredoc so it works on a full disk]]

%% ai-graph-start %%

**Related notes:**
- [[Prune disk before any write in an SSH heredoc so it works on a full disk]]
- [[Docker json-file logs are unbounded; cap them with --log-opt on high-volume containers]]
- [[Pin deploys by @sha256 of the TAGGED manifest, not the newest push (buildx multi-arch creates untagged sibling manifests)]]
- [[A git merge can silently revert a merged PR when two branches edit the same region]]
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]

**Relations:**
- Issue: SHA-pinned docker pulls accumulate and fill small deploy VM disks — *describes* — SHA-pinned docker pulls
- Issue: SHA-pinned docker pulls accumulate and fill small deploy VM disks — *affects* — small deploy VM disks
- leo-customer360 vServer scripts — *execute* — deploy
- deploy — *uses* — docker pull
- docker pull — *pulls* — new image
- deploy — *uses* — docker rm -f
- docker rm -f — *removes* — container
- deploy — *fails to remove* — old images
- SHA-pinned images — *accumulate* — 
- SHA-pinned images — *fill* — small deploy VM disks
- small deploy VM disks — *causes* — No space left on device
- SHA-pinned docker pulls — *result in* — distinct image reference
- distinct image reference — *lacks* — mutable tag
- Solution includes — *command* — docker container prune -f
- Solution includes — *command* — docker image prune -a -f
- Solution includes — *command* — docker builder prune -a -f
- docker image prune -a -f — *removes* — stale images
- Issue — *observed in* — GitHub Actions deploy job
- GitHub Actions deploy job — *targets* — uat
- GitHub Actions deploy job — *targets* — api VM
- Issue — *observed on* — 2026-09-04
- Prune disk before any write in an SSH heredoc so it works on a full disk — *is related to* — Issue: SHA-pinned docker pulls accumulate and fill small deploy VM disks
- docker — *command* — docker pull
- docker — *command* — docker rm -f
- docker — *command* — docker container prune -f
- docker — *command* — docker image prune -a -f
- docker — *command* — docker builder prune -a -f
- container — *holds* — image
- No space left on device — *affects* — env file
- deployments/server/deploy-*.sh — *are scripts for* — leo-customer360 vServer scripts
- small deploy VM disks — *are part of* — VM
- old images — *is a type of* — image
- stale images — *is a type of* — image
- SHA-pinned images — *is a type of* — image
- deploy — *is defined in* — deployments/server/deploy-*.sh
- new image — *is a type of* — image

%% ai-graph-end %%