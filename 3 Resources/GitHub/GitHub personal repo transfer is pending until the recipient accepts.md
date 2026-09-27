---
ai_hash: 0546d59ed6cd4b0d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27
status: seedling
tags:
- github
- gh-cli
- gotcha
- api
title: GitHub personal repo transfer is pending until the recipient accepts
type: lesson
---

# GitHub personal repo transfer is pending until the recipient accepts

﻿Transferring a GitHub repo to another **personal** account (`POST /repos/{owner}/{repo}/transfer` with `new_owner`) returns **202 Accepted** and a response body that still shows the OLD owner. That is not a failure — a personal-to-personal transfer creates a pending invitation that only completes when the RECIPIENT accepts it.

So `gh api repos/<old-owner>/<repo> --jq .owner.login` keeps reporting the old owner until acceptance, and the repo stays fully functional at its old URL meanwhile.

How the recipient accepts (logged in as the new owner):
- GitHub email / notification → "Accept transfer", or
- `gh api -X PATCH /user/repository_invitations/{invitation_id}` after listing with `gh api /user/repository_invitations`.

Do NOT retry the POST when the body shows the old owner — the invitation already exists; repeating it just re-issues it. Verify by having the recipient check their invitations, not by re-reading the source repo.

(Transfers to an **organization** you admin complete immediately — no acceptance step. The pending-invitation behaviour is specific to user-to-user.)

## Related
[[git push --all skips remote-tracking-only branches]]

%% ai-graph-start %%

**Related notes:**
- [[GitHub user-to-user repo transfer is a pending invitation, not an immediate move]]
- [[git push --all skips remote-tracking-only branches]]
- [[git push sends current branch to its upstream not same-name branch]]
- [[git checkout -B branch originmain rehomes upstream to originmain, so a bare push targets main]]

%% ai-graph-end %%