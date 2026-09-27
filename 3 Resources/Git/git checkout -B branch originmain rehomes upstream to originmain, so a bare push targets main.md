---
ai_hash: 73644de25f72c7c8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: session 2026-08-20
status: seedling
tags:
- git
- upstream
- push
- gotcha
- force-with-lease
title: git checkout -B branch origin/main rehomes upstream to origin/main, so a bare
  push targets main
type: lesson
---

# git checkout -B branch origin/main rehomes upstream to origin/main, so a bare push targets main

`git checkout -B <branch> origin/main` (and `git checkout -b <branch> origin/main`) **sets the new branchs upstream to `origin/main`**. A subsequent **bare `git push` then targets `main`**, not the feature branch — an easy way to accidentally push straight to main.

**Symptom:** after the checkout, git prints `branch <branch> set up to track origin/main`, and `git status` says `Your branch and origin/main have diverged`.

**Safe practice — always push the feature branch with an explicit refspec, and reset tracking:**
```
git push -u --force-with-lease origin <branch>      # explicit dst; -u rehomes upstream to origin/<branch>
```
After this, upstream is `origin/<branch>` again and bare `git push` is safe.

Prefer `--force-with-lease` over `--force`: it refuses the push if the remote branch advanced beyond your last-known remote-tracking ref (someone elses commit), so you only overwrite what you actually saw.

## Related

- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]

%% ai-graph-start %%

**Related notes:**
- [[git push sends current branch to its upstream not same-name branch]]
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]
- [[Branch created from current HEAD drags unrelated commits — verify against originmaster]]
- [[ReviewPR scope diff the branch's real base, not main, when base is ahead of main]]
- [[Classify local vs upstream with git merge-base to pick ff or rebase]]

%% ai-graph-end %%