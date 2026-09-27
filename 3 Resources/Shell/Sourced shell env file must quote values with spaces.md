---
ai_hash: ae1715b661e50d39
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: session 2026-09-16
status: seedling
tags:
- shell
- bash
- env-file
- gotcha
- docker
title: Sourced shell env file must quote values with spaces
type: gotcha
---

# Sourced shell env file must quote values with spaces

A value with a space in a shell env file that is \`source\`d must be quoted, or the shell word-splits it and tries to run the rest as a command.

\`EMAIL_FROM_NAME=LEO CDP\` when sourced sets \`EMAIL_FROM_NAME=LEO\` and then executes \`CDP\` -> "CDP: command not found". Fix: \`EMAIL_FROM_NAME="LEO CDP"\`.

Note the asymmetry: a Docker \`--env-file\` does NOT need quoting (everything after \`=\` is the literal value), but a file consumed via \`source\`/\`. file\` runs through normal shell parsing and DOES. Same file text behaves differently depending on how it is loaded. Caught while wiring deployments/server/smtp.<env>.env.

## Related
[[LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)]]

## Related

- [[LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)]]

%% ai-graph-start %%

**Related notes:**
- [[LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)]]
- [[LEO Customer360 UAT runs email in mock mode (backend.env omits SMTP vars)]]
- [[POSIX sh source. of a slashless filename searches PATH, not cwd]]
- [[Apostrophe inside bash ${varmessage} breaks the parser]]
- [[LEO CDP SYSTEM_ENV_VARS still requires database-configs.json to exist first]]

%% ai-graph-end %%