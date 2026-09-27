---
ai_hash: 49444655d7c7bac9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: leo-customer360 deploy-all.sh, 2026-08
status: seedling
tags:
- bash
- set-u
- arrays
- exit-code
- ci
- gotcha
title: set -u trips on a bash array only populated on the failure path, making success
  exit non-zero
type: gotcha
---

# set -u trips on a bash array only populated on the failure path, making success exit non-zero

A bash array that is only ever appended-to on a conditional branch (classic: `FAIL_STEPS+=(...)` only when a step fails) is UNBOUND on the branch where that condition never fires. Under `set -u`, an end-of-run reference like `${#FAIL_STEPS[@]}` in the summary then throws `unbound variable` — on the SUCCESS path — so the script exits non-zero even though everything worked. Nasty because it only bites when nothing went wrong (the happy path is the least-tested), and CI/automation reads the non-zero code as a failed deploy.

**Fix:** initialize the array empty at the top, next to the other vars: `OK_STEPS=(); FAIL_STEPS=()`. (Or guard each use: `${arr[@]:-}` / `${#arr[@]:-0}`, but eager init is cleaner and covers every reference.) Same trap applies to any var conditionally set then unconditionally read under `set -u`.

Related: [[Apostrophe inside bash ${varmessage} breaks the parser|Apostrophe inside bash ${var:?message} breaks the parser]]

## Related

- [[Apostrophe inside bash ${varmessage} breaks the parser|Apostrophe inside bash ${var:?message} breaks the parser]]

%% ai-graph-start %%

**Related notes:**
- [[Apostrophe inside bash ${varmessage} breaks the parser]]
- [[Bash unquoted variable expansion re-splits on whitespace, breaking quoted args]]
- [[Backgrounded shell exit code reflects the last command, not the build]]
- [[Cloud Build treats $VAR in step args as its own substitution; escape shell $ as $$]]
- [[ssh 'bash -s' flattens args, so empty middle args shift positionals]]

%% ai-graph-end %%