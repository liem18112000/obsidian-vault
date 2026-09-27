---
title: Windows Git Bash can mangle non-ASCII typed into a command to cp1252
tags: [windows, git-bash, encoding, telegram, gotcha]
created: 2026-09-14
---

# Windows Git Bash can mangle non-ASCII typed into a command to cp1252

Non-ASCII characters (em dash `—`, emoji, accented letters) placed **directly**
into a Git Bash command line or heredoc on Windows can be encoded by the
terminal as **cp1252 single bytes**, not UTF-8 — producing invalid UTF-8.

Symptom hit in practice: Telegram `sendMessage` returned
`400 Bad Request: strings must be encoded in UTF-8` for a message containing `—`,
while the same call with only ASCII succeeded.

Key point: this is a **shell-input** problem, not a file problem. Files written
by an editor/tool are proper UTF-8 and work fine (GitHub Actions runners are
UTF-8, so the workflow YAML with emoji is correct).

**Fixes:**
- Write the non-ASCII payload to a file as UTF-8 (e.g. via Python `io.open(..., encoding="utf-8")`),
  then `curl --data-urlencode "text@file.txt"`.
- Or use `\uXXXX` escapes in Python rather than pasting the glyph.
- Verify a file is valid UTF-8 with `python -c "yaml.safe_load(open(f, encoding='utf-8'))"`
  (parsing succeeds → bytes are valid UTF-8).

Related: [[git push sends current branch to its upstream not same-name branch]]
