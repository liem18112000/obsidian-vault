---
title: "Piping a Python CLI through tail block-buffers stdout, looking like a hang"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22 JEV J4"
tags: [python, stdout, buffering, gotcha, debugging]
---

# Piping a Python CLI through tail block-buffers stdout, looking like a hang

Redirecting a long-running Python CLI through a pipe (e.g. \`... | tail -60\`) makes Python **block-buffer** stdout, because stdout is no longer a TTY. Nothing appears until the process exits (or fills a ~4-8KB buffer), so a slow-but-healthy run is indistinguishable from a hang — you stare at an empty output file and wrongly conclude it stalled.

**Fix:** for live progress on long runs, set `PYTHONUNBUFFERED=1` (or run `python -u`) and redirect straight to a file you poll — do **not** pipe through `tail`/`grep`. Line-buffered output then flushes each `print` immediately.

Bit me on the JEV J4 calibration harness: `... | tail -60` showed nothing for 20 min; the run was actually progressing fine through ~45 median-of-3 Vertex judge calls. Re-run as `PYTHONUNBUFFERED=1 uv run ... > run.log 2>&1 &` and `Read run.log` to watch it advance.

Related: [[An open editor can clobber a mid-session programmatic file edit]] (surfaced in the same session).

## Related

- [[An open editor can clobber a mid-session programmatic file edit]]
