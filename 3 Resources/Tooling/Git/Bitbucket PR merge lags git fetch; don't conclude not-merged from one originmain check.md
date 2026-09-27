---
ai_hash: 0373de859b94e2ea
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-18
entities: []
source: Vinnstack session 2026-07-18
status: seedling
tags:
- git
- bitbucket
- ci
- gotcha
- vinnstack
title: Bitbucket PR merge lags git fetch; don't conclude not-merged from one origin/main
  check
type: lesson
---

# Bitbucket PR merge lags git fetch; don't conclude not-merged from one origin/main check

A Bitbucket Cloud PR that has been merged does NOT show up on `git fetch origin main` instantly — there is propagation lag between the Bitbucket-side merge and what `origin/main` reports to a git client. In one Vinnstack session the user merged PR #5, but two successive `git fetch origin main` checks still showed the old `origin/main` tip (and the source branch still present, tip not reachable from main) — which looked exactly like "not merged yet". Minutes later the same fetch showed `origin/main` = the PR-#5 merge commit.

Lesson: do NOT conclude "the PR was not merged" from a single `git fetch` snapshot. Signals like "origin/main unchanged", "source branch still exists", "af52dec not reachable from main" can ALL be true simply due to sync lag, not a failed/blocked merge. Before diagnosing a blocked merge (approvals, checks, wrong dest), re-fetch after a short wait, or check the PR state on the Bitbucket side directly.

Related: a background merge-watcher that polls `origin/main` for advancement is the robust way to catch it — it fired correctly for PR #4 once propagation completed.

## Related

- [[Vinnstack release push to main triggers Cloud Build which publishes to GCS latest auto-update channel]]

%% ai-graph-start %%

**Related notes:**
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]
- [[FETCH_HEAD is volatile when an IDE auto-fetches]]
- [[A git merge can silently revert a merged PR when two branches edit the same region]]
- [[git checkout -B branch originmain rehomes upstream to originmain, so a bare push targets main]]
- [[gh CLI is GitHub-only, not Bitbucket-aware]]

%% ai-graph-end %%