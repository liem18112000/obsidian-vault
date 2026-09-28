---
ai_hash: c4209927058fc5a1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-28'
created: 2026-09-28
entities:
- Windows PowerShell 5.1
- '&& operator'
- '|| operator'
- PowerShell 7
- pwsh
- powershell.exe
- bash-style chain
- parser error
- A; B
- A; if ($?) { B }
- Git Bash
- target file's modification time
- Windows
- unconditional sequence
- conditional sequence
- Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8
source: session 2026-09-27 (git remote set-url that never ran)
status: seedling
tags:
- powershell
- windows
- shell
- gotcha
- cli
- debugging
title: Windows PowerShell 5.1 has no && operator, so a chained command silently never
  runs
type: gotcha
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

%% ai-graph-start %%

**Related notes:**
- [[Windows PowerShell 5.1 reads BOM-less scripts as ANSI, breaking on em-dashes]]
- [[PowerShell 5.1 eats inner double-quotes passed to native exes like gcloud]]
- [[Bash collapses backslashes before PowerShell stdin, breaking Windows-path JSON]]
- [[PowerShell here-string @'...'@ silently corrupts git commit messages in the Bash tool]]
- [[Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8]]

**Relations:**
- Windows PowerShell 5.1 — *lacks* — && operator
- Windows PowerShell 5.1 — *lacks* — || operator
- && operator — *introduced_in* — PowerShell 7
- || operator — *introduced_in* — PowerShell 7
- PowerShell 7 — *is_identified_by_executable* — pwsh
- Windows PowerShell 5.1 — *is_identified_by_executable* — powershell.exe
- bash-style chain — *causes* — parser error
- parser error — *prevents* — command execution
- A; B — *is_an_equivalent_for* — unconditional sequence
- A; if ($?) { B } — *is_an_equivalent_for* — conditional sequence
- Git Bash — *supports* — && operator
- target file's modification time — *is_a_diagnostic_for* — command execution
- Windows PowerShell 5.1 — *runs_on* — Windows
- Git Bash — *runs_on* — Windows
- Windows PowerShell 5.1 — *is_related_to* — Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8

%% ai-graph-end %%