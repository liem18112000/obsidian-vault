---
ai_hash: 0b83e4d73b5a7ac6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities: []
source: session 2026-09-03
status: seedling
tags:
- git-bash
- windows
- msys
- shell
- detach
- gotcha
title: Git Bash on Windows has no setsid; detach with nohup and disown
type: lesson
---

# Git Bash on Windows has no setsid; detach with nohup and disown

Git Bash / MSYS2 on Windows does **not** ship `setsid`, so `setsid mycmd &` fails with `setsid: command not found` and the intended process never starts — a silent trap when porting Linux detach idioms. To fully detach a background process in Git Bash, use **`nohup <cmd> >> log 2>&1 </dev/null & disown`**: `nohup` ignores SIGHUP so the child survives the parent shell exit, `</dev/null` detaches stdin, redirecting stdout/stderr frees the terminal, and `disown` removes it from the shells job table. 

Also on Windows/MSYS, `pkill -f` and `kill` often fail to signal native `.exe` children (kubectl.exe, node.exe) or MSYS bash procs reliably — kill by command-line match with PowerShell instead: `Get-CimInstance Win32_Process | ? { $_.CommandLine -match <pattern> } | Stop-Process -Force` (exclude your own `powershell.exe` / `$PID` or the killer kills itself mid-script). Used while managing a detached worker in [[Decouple long agent work from the harness task lifecycle]].

## Related

- [[Decouple long agent work from the harness task lifecycle]]

%% ai-graph-start %%

**Related notes:**
- [[Decouple long agent work from the harness task lifecycle]]
- [[Windows child processes survive when only the parent is killed]]
- [[Node child_process.kill on Windows doesn't kill descendant processes]]
- [[Git Bash mangles absolute POSIX paths meant for a remote kubectl exec target]]

%% ai-graph-end %%