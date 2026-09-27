---
title: "Windows gcloud emits CRLF and command substitution keeps the CR"
created: 2026-09-25
type: lesson
status: seedling
source: "session 2026-09-25 — test-agent-v2 rotation script"
tags: [shell, bash, windows, git-bash, gcloud, gotcha]
---

# Windows gcloud emits CRLF and command substitution keeps the CR

`gcloud` on Windows (via Git Bash / MSYS) terminates output lines with `\r\n`. Bash's `$(...)` strips the trailing **newline only** — the carriage return survives. So this:

```bash
v="$(gcloud secrets versions list "$s" --format='value(name)' --limit=1)"
```

produces `v` = `"59\r"`, not `"59"`. Verified with `od -c`: the bytes are `5 9 \r \n`.

The failure is nasty because **the error message looks correct**. Feeding that value into another command yields:

```
ERROR: Invalid Secret ID [projects/…/secrets/…/versions/59] does not match the
expected format [projects/*/secrets/*/versions/*]
```

The printed ID matches the stated pattern perfectly — because the `\r` is invisible, and worse, it makes the terminal overwrite the start of the line so the message itself appears garbled/interleaved. You lose time doubting the pattern rather than the bytes.

Same exposure applies to any Windows CLI consumed this way: `terraform output -raw`, `kubectl`, `az`, anything shelling out through cmd.exe.

**Fix — wrap it once, at the point of capture:**

```bash
gc() { gcloud "$@" | tr -d '\r'; }
```

Then capture through `gc`, never bare `gcloud`. With `set -o pipefail` on, the wrapper still propagates gcloud's exit status rather than `tr`'s.

`$(...)` word-splitting does not save you either — `for v in $(...)` splits on IFS (space/tab/newline), so the `\r` stays glued to each token.

**Debug reflex:** when a CLI rejects a string that looks exactly right, pipe it to `od -c` before questioning the format.

## Related

- [[Secret Manager versions accumulate silently — one per deploy, all left enabled]]
- [[Cloud Run resolves a latest secret reference at instance start, not per request]]
