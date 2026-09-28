---
title: "Windows PowerShell 5.1 has no && operator, so a chained command silently never runs"
created: 2026-09-28
type: gotcha
status: seedling
source: "session 2026-09-27 (git remote set-url that never ran)"
tags: [powershell, windows, shell, gotcha, cli, debugging]
---

# Windows PowerShell 5.1 has no && operator, so a chained command silently never runs

Windows PowerShell **5.1** (`powershell.exe`, the default on Windows) does not support the pipeline chain operators `&&` and `||`. They arrived in PowerShell **7** (`pwsh`). Pasting a bash-style chain into 5.1 produces a **parser error** — and the important part is *when*: parsing happens before execution, so **not one command in the chain runs**.

```powershell
cd C:\repo && git remote set-url origin https://...   # parser error, nothing executes
cd C:\repo; git remote set-url origin https://...     # works
```

Why this wastes time rather than failing loudly: the outcome is **no change and no partial state**, which looks exactly like "the command ran and did nothing". I told someone to run a `&&` chain three times; each time the target file's mtime showed it had never been touched, which is what finally gave it away. A half-applied chain would have been *more* obvious.

Equivalents in 5.1:

- unconditional sequence → `A; B`
- run B only if A succeeded → `A; if ($?) { B }`

The lesson generalises past PowerShell: **when handing a command to someone else to run, write it for the shell they actually have.** On Windows that means asking, or offering both forms — Git Bash takes `&&`, PowerShell 5.1 does not. A one-line command that silently no-ops is worse than one that errors visibly.

Diagnostic worth keeping: **check the target file's modification time.** If it predates the attempt, the command never executed — which separates "ran and failed" from "never ran" without needing the terminal output.

## Related

- [[Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8]]

## Related

- [[Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8]]
