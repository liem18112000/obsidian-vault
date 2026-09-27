---
title: "Git Bash on Windows ignores TZ; compute offset with date -u -d for correct local timestamp"
created: 2026-09-09
type: gotcha
status: seedling
source: "session 2026-09-09"
tags: [git-bash, windows, timezone, date, gotcha]
---

# Git Bash on Windows ignores TZ; compute offset with date -u -d for correct local timestamp

On Windows Git Bash, `TZ='Asia/Ho_Chi_Minh' date` often does NOT shift the clock (the msys build ships no zoneinfo), so it prints UTC wall-clock time. If you also hardcode the offset in the format string (e.g. `date '+%Y-%m-%dT%H:%M:%S+07:00'`), you get a timestamp that is wrong by the whole offset — UTC numbers wearing a `+07:00` label.

**Symptom:** the produced time is N hours behind the real local time (7h for +07:00), yet looks plausible.

**Fix:** compute the offset explicitly from UTC. For UTC+7: `date -u -d '+7 hours' '+%Y-%m-%dT%H:%M:%S+07:00'`. GNU `date -d` works in Git Bash even though TZ zone names do not. Cross-check against a known-good clock (another synced host) when a timestamp looks off by a round number of hours.
