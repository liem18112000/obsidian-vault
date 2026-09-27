---
ai_hash: f960d791081bd2e3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21 (customer360-agent E2E)
status: seedling
tags:
- python
- venv
- windows
- pytest
- cross-platform
- gotcha
title: 'Python venv layout: Scripts on Windows vs bin on POSIX breaks Linux-authored
  test runners'
type: gotcha
---

# Python venv layout: Scripts on Windows vs bin on POSIX breaks Linux-authored test runners

A Python virtualenv puts its interpreter and activate script in **`Scripts/`** on Windows but **`bin/`** on Linux/macOS. A test runner authored for POSIX (e.g. `run_unit_tests.sh` that checks `"$VENV/bin/python"` and `source "$VENV/bin/activate"`) therefore **silently fails on Windows Git Bash** — the `bin/` path never exists, so it either re-creates the venv every run or errors on the missing activate.

Consequences & fixes:
- Such a runner is fine in CI (Linux) but not for local Windows dev. Do not "fix" it by assuming it is broken — it works where it runs.
- To run the suite on Windows: build the venv yourself and call the interpreter by its real path — `python -m venv .venv` then `.venv/Scripts/python.exe -m pytest`. Prefer a scratch-dir venv so you do not pollute an existing one.
- Pick the right base interpreter: a brand-new Python (e.g. 3.14) often lacks prebuilt wheels for `pydantic-core`/`litellm`, forcing a from-source (Rust) build that fails; use the version the project already targets (here 3.12).

Discovered running the customer360-agent pytest suite on Windows.

%% ai-graph-start %%

**Related notes:**
- [[Batch files must use call for activate.bat or the script stops there]]
- [[New .sh CI runners must be git-staged with --chmod=+x on Windows or the [ -x ] gate skips them]]
- [[Windows Python resolves a leading-slash path to C-colon-tmp, not Git Bash tmp]]
- [[spawn python ENOENT on Windows — resolve a real interpreter, not the Store alias]]
- [[load_dotenv() in package __init__ leaks .env into pytest, breaking offline tests]]

%% ai-graph-end %%