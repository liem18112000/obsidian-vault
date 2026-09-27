---
ai_hash: 71bad79827f27b92
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities: []
source: session 2026-08-04 — ship luz_finance ivy to dev
status: seedling
tags:
- luz
- ship-ivy
- bitbucket
- git
- gotcha
- ci
title: ship-ivy stage 1 fails on Bitbucket read-replica lag when webclient branch
  already exists
type: lesson
---

# ship-ivy stage 1 fails on Bitbucket read-replica lag when webclient branch already exists

When shipping an ivy module with the `luz-skill-ship-ivy` skill, stage 1 ("ensure webclient branch") can fail on the *first* run even though everything is correct — because it decides whether to create the `luz_webclient` feature branch using `git ls-remote --heads origin <branch>`, a **remote read**. On Bitbucket, a read replica can report the branch ABSENT while the write primary already has it (eventual-consistency / replication lag). The script then attempts the refspec push `git push origin origin/master:refs/heads/<branch>`, which the primary rejects with:

```
! [remote rejected] origin/master -> <branch> (reference already exists)
error: failed to push some refs
```

and the script exits non-zero at the `[webclient]` stage.

**Root cause:** read-replica vs write-primary lag on Bitbucket — a read (`ls-remote`) and a write (`push`) can disagree about whether a ref exists.

**Fix — just re-run:**
1. Sync the local tracking ref: `git -C ~/Kepler/luz_webclient fetch origin --prune`
2. Re-run `bash ~/.claude/skills/luz-skill-ship-ivy/ship_ivy.sh`. On the re-run, `ls-remote` sees the branch and takes the `already exists — reusing it` path, then continues cleanly through service build -> webclient build -> rollout.

The re-run is **safe/idempotent** when the SERVICE (e.g. `luz_finance`) tree is clean and origin is already at HEAD — the commit and push steps get skipped, so re-running only re-triggers the builds and rollout you wanted anyway.

Same replica-vs-primary lag pattern can bite any "check-then-create branch" flow against Bitbucket, not just this skill.

## Related

- [[luz-skill-ship-ivy]]
- [[Bitbucket]]

%% ai-graph-start %%

**Related notes:**
- [[Dev luz-webclient can drift off a freshly-shipped ivy image; verify deployed tag vs latest before assuming the ship stuck]]
- [[Branch created from current HEAD drags unrelated commits — verify against originmaster]]
- [[luz_docs_integration_test AI pipeline branch and PR mechanics]]
- [[How luz_docs_integration_test repo location is resolved on disk]]
- [[Shipping luz_docs_statistic trigger is docs-statistic-service and dev runs a Deployment, not a StatefulSet]]

%% ai-graph-end %%