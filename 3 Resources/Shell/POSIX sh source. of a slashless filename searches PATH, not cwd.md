---
ai_hash: b691734eb3411d6d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- bash
- posix
- shell
- sourcing
- gotcha
title: 'POSIX sh: source/. of a slashless filename searches PATH, not cwd'
type: lesson
---

# POSIX sh: source/. of a slashless filename searches PATH, not cwd

In **POSIX sh** (and bash running in POSIX mode, e.g. when invoked as `sh script.sh` — on Git Bash `sh` is bash-as-sh), the `.` / `source` builtin treats an argument **with no slash** as a **$PATH lookup**, NOT a current-directory file. So `source .env` searches PATH, fails, and reports `source: .env: file not found` even though ./.env exists.

**Normal (non-POSIX) bash** differs: with the `sourcepath` shopt on (default), it searches PATH and then **falls back to cwd** — so `source .env` works interactively / via a `#!/usr/bin/env bash` shebang, but breaks the moment someone runs the same script with `sh`.

**Fix:** give the arg a slash so it's a path, not a name: `source ./.env`. Works in bash AND POSIX sh. Same rule applies to `. ./file`.

**Also:** running `sh script.sh` ignores the script's shebang and can silently switch interpreters — prefer `./script.sh` or `bash script.sh` for a bash script (which also keeps `[[ ]]`, arrays, etc.). Diagnosed on deployments/postgres/deploy.sh (leo-customer360).

## Related

- [[Make a Terraform wrapper script idempotent with plan -detailed-exitcode and a saved plan]]

%% ai-graph-start %%

**Related notes:**
- [[Sourced shell env file must quote values with spaces]]
- [[In CI invoke repo shell scripts via bash script.sh, not .script.sh (Windows drops the +x bit)]]
- [[CRLF line endings on a shell script shebang cause Docker exit 127 (env bash^M not found)]]
- [[Docker Compose path resolution env_file vs build context vs dockerfile]]
- [[Windows Python resolves a leading-slash path to C-colon-tmp, not Git Bash tmp]]

%% ai-graph-end %%