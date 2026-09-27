---
title: "Review/PR scope: diff the branch's real base, not main, when base is ahead of main"
created: 2026-09-16
type: lesson
status: seedling
source: "session 2026-09-16"
tags: [git, code-review, pull-request, workflow, gotcha]
---

# Review/PR scope: diff the branch's real base, not main, when base is ahead of main

When reviewing "my change" on a feature branch, diff against the branch's ACTUAL base, not `main`. If the feature was cut from an integration branch (e.g. `dev-uat`) that is itself ahead of `main`, then `git merge-base HEAD main` points far back and `git diff <that>..HEAD` mixes in the whole integration backlog (dozens of unrelated files) -- making the review scope wrong and noisy.

Fix: diff `origin/<actual-base>..HEAD` (e.g. `origin/dev-uat..HEAD`). Confirm the base with `git log --oneline origin/dev-uat..HEAD` -- it should show ONLY your commits. Same logic for the PR: target the branch you actually branched from, or the PR diff balloons with the backlog. (Here: retargeted the PR base main -> dev-uat with `gh pr edit <n> --base dev-uat`, which shrank it to just the 3 real commits.)

## Related
[[LEO CI pushes images only on main/tags; feature branches build-only]]

## Related

- [[LEO CI pushes images only on main/tags; feature branches build-only]]
