---
ai_hash: 29369c78983f73f0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: session 2026-08-20, cd.yml run 32366406386
status: seedling
tags:
- ci
- shell
- windows
- git
- gotcha
- exit-126
title: In CI invoke repo shell scripts via bash script.sh, not ./script.sh (Windows
  drops the +x bit)
type: lesson
---

# In CI invoke repo shell scripts via bash script.sh, not ./script.sh (Windows drops the +x bit)

A shell script committed from a Windows working copy usually has **no git executable bit**, so on a Linux CI runner `./script.sh` fails with `Permission denied` and **exit code 126** (command found but not executable — distinct from 127 = not found).

Two fixes:
- **Invoke via the interpreter**: `bash script.sh args` — works regardless of the mode bit. Preferred in CI/workflows.
- **Set the bit in git** (persists cross-platform): `git update-index --chmod=+x script.sh` then commit, or `git add --chmod=+x`.

Real case: `cd.yml` ran `./deploy-all.sh …` and hit exit 126 on the GitHub runner because the deploy scripts were authored on Windows. Changed to `bash deploy-all.sh …`. Note `deploy-all.sh` already invoked its own sub-scripts as `bash "$path"`, so only the top-level `./` call needed fixing. Related: [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]].

## Related

- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]

%% ai-graph-start %%

**Related notes:**
- [[New .sh CI runners must be git-staged with --chmod=+x on Windows or the [ -x ] gate skips them]]
- [[gradlew committed from Windows loses the exec bit - fix with git update-index chmod]]
- [[CRLF line endings on a shell script shebang cause Docker exit 127 (env bash^M not found)]]
- [[A locally-sourced shell function is not defined inside an ssh bash -s remote heredoc]]
- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]

%% ai-graph-end %%