---
ai_hash: 6d95160cf5f595a6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-23
entities: []
tags:
- git
- squash-merge
- branches
- gotcha
title: git merge-base --is-ancestor returns false after a SQUASH merge even though
  the branch's changes are merged
type: gotcha
---

# git merge-base --is-ancestor returns false after a SQUASH merge even though the branch's changes are merged

After a branch is **squash-merged** (GitHub "Squash and merge"), `git merge-base --is-ancestor <branch-tip> origin/main` returns FALSE (exit 1) — because a squash merge creates a brand-NEW single commit on main containing all the changes, and does NOT record the original branch commits as ancestors. So the branch's content IS in main, but its commit SHAs are not in main's history.

Don't conclude "the work isn't merged" from an ancestry check alone. Instead:
- Look at `git log --oneline origin/main` for the squash commit — it's usually titled with the PR title + "(#N)".
- Or diff the actual FILE CONTENT (`git show origin/main:path | grep <marker>`), which is the ground truth.

Corollary: a new branch cut from the post-squash main will CONTAIN all the squashed work (via main) while showing only its own new commits in `git log origin/main..HEAD`. That's expected, not a lost-work situation.

Encountered: leo-customer360 — PR #18 (branch infras/cicd/v3) squash-merged to main as commit 5ec85fe "...(#18)"; a follow-up branch showed 2 commits over main yet its files contained all of #18's changes. 2026-08-23.

%% ai-graph-start %%

**Related notes:**
- [[Verify a branch is fully merged before deleting git log main..branch count 0]]
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]
- [[ReviewPR scope diff the branch's real base, not main, when base is ahead of main]]
- [[A git merge can silently revert a merged PR when two branches edit the same region]]
- [[Bitbucket PR merge lags git fetch; don't conclude not-merged from one originmain check]]

%% ai-graph-end %%