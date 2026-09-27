---
title: "Sourced shell env file must quote values with spaces"
created: 2026-09-16
type: gotcha
status: seedling
source: "session 2026-09-16"
tags: [shell, bash, env-file, gotcha, docker]
---

# Sourced shell env file must quote values with spaces

A value with a space in a shell env file that is \`source\`d must be quoted, or the shell word-splits it and tries to run the rest as a command.

\`EMAIL_FROM_NAME=LEO CDP\` when sourced sets \`EMAIL_FROM_NAME=LEO\` and then executes \`CDP\` -> "CDP: command not found". Fix: \`EMAIL_FROM_NAME="LEO CDP"\`.

Note the asymmetry: a Docker \`--env-file\` does NOT need quoting (everything after \`=\` is the literal value), but a file consumed via \`source\`/\`. file\` runs through normal shell parsing and DOES. Same file text behaves differently depending on how it is loaded. Caught while wiring deployments/server/smtp.<env>.env.

## Related
[[LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)]]

## Related

- [[LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)]]
