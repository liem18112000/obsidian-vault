---
title: "Installer skill templates drift from the live scripts they install"
created: 2026-09-27
type: lesson
status: seedling
source: "luz-skills-plugin sync PR, 2026-09-27"
tags: [claude-code, skills, installers, gotcha]
---

# Installer skill templates drift from the live scripts they install

An installer skill that ships `_tpl/` copies of the scripts it deploys holds a **second copy** of every file. Refactor the live deployed script without re-syncing the template and the installer keeps working on the author machine — where the live files are already correct — while every fresh install is broken. There is no build step to catch it and no test that runs the installer, so the drift is invisible until someone else installs.

The sharpest version of this is **extracting a shared module**. `telegram-hook-installation` deploys `telegram-notify.sh` and `telegram-input-poller.py`; both were refactored to `import telegram_md` after the Markdown to Telegram-HTML renderer was pulled into its own module. The live hooks got the new file. The skill did not: `_tpl/` had no `telegram_md.py`, `install.sh` never copied it, and `SKILL.md` never listed it — so a fresh install would `ImportError` on the first hook event.

Extracting a shared module out of an installed script is not done until **three** places in the installer name the new file:

1. the template set (`_tpl/<new-module>`),
2. the installer copy list *and* its pre-flight existence check,
3. the file manifest in `SKILL.md` (both the "files in this skill" tree and the "what gets installed" list).

Generally: treat "live artifact" and "installer template" as two copies that must be diffed *deliberately* and periodically, not assumed in sync. On Windows, diff with `diff --strip-trailing-cr` — a repo with CRLF normalization reports every line as changed otherwise, and the real one-line delta hides in the noise.

Related: [[Cooperating Claude Code hooks on one event need a shared claim file]]

## Related

- [[Cooperating Claude Code hooks on one event need a shared claim file]]
