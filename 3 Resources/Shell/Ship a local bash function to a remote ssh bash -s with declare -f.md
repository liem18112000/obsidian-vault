---
title: "Ship a local bash function to a remote ssh bash -s with declare -f"
created: 2026-09-14
type: howto
status: seedling
source: "session 2026-09-14, leo-customer360 PR #67"
tags: [shell, bash, ssh, declare-f, process-substitution, heredoc, technique]
---

# Ship a local bash function to a remote ssh bash -s with declare -f

To reuse a bash function (defined locally, e.g. sourced from a lib) inside a remote `ssh "$host" 'bash -s'` script, **prepend its definition to the remote stdin** with `declare -f`. Process substitution keeps the `ssh` line and its positional args exactly in place:

```sh
ssh "${SSH_OPTS[@]}" "$host" 'bash -s' <arg1> <arg2> < <(declare -f my_fn; cat <<'REMOTE'
set -euo pipefail
ARG1="$1"; ARG2="$2"
my_fn "$ARG1"        # now defined in the remote shell
REMOTE
)
```

`declare -f my_fn` emits the function source; `cat <<'REMOTE'` emits the (quoted, literal) body; both stream to the remote `bash -s`, which reads them as one script. The `< <(...)` redirect must be closed with `)` after the heredoc terminator. Requires bash on BOTH the caller (process substitution) and remote (`declare -f` syntax); the caller must have already sourced/defined `my_fn`.

Keeps a single source of truth (no pasting the function into every heredoc). Used to fix the leo-customer360 CD deploy across 6 deploy scripts (PR #67). This is the fix for [[A locally-sourced shell function is not defined inside an ssh bash -s remote heredoc]].

## Related

- [[A locally-sourced shell function is not defined inside an ssh bash -s remote heredoc]]
