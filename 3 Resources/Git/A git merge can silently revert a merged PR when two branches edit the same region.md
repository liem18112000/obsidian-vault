---
ai_hash: 6ab8f32f3b93b4b0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: session 2026-09-05, leo-customer360 deploy-api.sh
status: seedling
tags:
- git
- merge
- conflict
- gotcha
- code-review
title: A git merge can silently revert a merged PR when two branches edit the same
  region
type: lesson
---

# A git merge can silently revert a merged PR when two branches edit the same region

When two branches change the **same region** of a file and the branches are merged, resolving the conflict toward one side **discards the other side's edits for that region** — even if those edits were an already-merged, reviewed PR on the target branch. The reverted PR still shows as "merged" in its own history, so nothing flags that its changes vanished from the current tip. It only surfaces if you actually read the merged file.

**Where it bit (leo-customer360):** a local disk-reclaim change to `deploy-api.sh` was branched from the pre-PR base. Meanwhile PR #31 (temp env file -> `/opt/c360/.tmp` instead of `/tmp`) merged to main in the same `mktemp` region. The merge that combined them kept the reclaim side, silently reverting PR #31; the tip had `env_file=$(mktemp)` (default /tmp) again with no conflict marker left behind.

**How to catch / avoid it:**
- After a merge that touched a file another PR recently changed, diff the merge result against that PR: `git show <pr-merge>:path > /tmp/a; git show HEAD:path > /tmp/b; diff a b`, or `git log --oneline <base>..HEAD -- path` then read the file.
- Rebasing the feature branch onto latest main *before* it merges makes the same overlap show up as an explicit conflict you must resolve, instead of a silent auto-resolution.
- Treat "my branch was cut before that PR landed and touches the same lines" as a review trigger.

Related: [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]].

## Related

- [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]]

%% ai-graph-start %%

**Related notes:**
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]
- [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]]
- [[ReviewPR scope diff the branch's real base, not main, when base is ahead of main]]
- [[Finding intentional k8s config PRs in luz_kubernetes filter out image-hash + merge drift]]
- [[Prune disk before any write in an SSH heredoc so it works on a full disk]]

%% ai-graph-end %%