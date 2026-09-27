---
title: "CRLF line endings on a shell script shebang cause Docker exit 127 (env: bash^M not found)"
created: 2026-09-09
type: gotcha
status: seedling
source: "session 2026-09-09"
tags: [docker, crlf, windows, shebang, gotcha, git-autocrlf]
---

# CRLF line endings on a shell script shebang cause Docker exit 127 (env: bash^M not found)

A container that crash-loops with exit code 127 and logs `/usr/bin/env: 'bash\r': No such file or directory` has a shell script with **CRLF line endings**. The `\r` becomes part of the interpreter name in the shebang (`#!/usr/bin/env bash\r`), so the kernel looks for a program literally named `bash<CR>` and fails -> 127.

**How it happens:** on Windows, git `core.autocrlf=true` checks scripts out as CRLF. A Docker build that COPYies those files (or a `BUILD_LOCAL` tar-over-ssh of the working tree) bakes the CRLF shebang into the image. `chmod +x` does NOT fix it. CI builds on Linux use LF and are immune, so it only bites local/Windows builds.

**Detect:** `head -1 entrypoint.sh | cat -A` shows `#!/usr/bin/env bash^M$`. `file entrypoint.sh` says "with CRLF line terminators".

**Fix:** add a `.gitattributes` with `*.sh text eol=lf` (and re-normalize), or `dos2unix`/`sed -i 's/\r$//'` the scripts in the Dockerfile before use, or ship with `tr -d '\r'`. A `.py` invoked via `python x.py` tolerates CRLF (no shebang executed), which is why only the shell entrypoint broke.
