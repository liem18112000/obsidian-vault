---
title: "Python venv layout: Scripts on Windows vs bin on POSIX breaks Linux-authored test runners"
created: 2026-09-21
type: gotcha
status: seedling
source: "session 2026-09-21 (customer360-agent E2E)"
tags: [python, venv, windows, pytest, cross-platform, gotcha]
---

# Python venv layout: Scripts on Windows vs bin on POSIX breaks Linux-authored test runners

A Python virtualenv puts its interpreter and activate script in **`Scripts/`** on Windows but **`bin/`** on Linux/macOS. A test runner authored for POSIX (e.g. `run_unit_tests.sh` that checks `"$VENV/bin/python"` and `source "$VENV/bin/activate"`) therefore **silently fails on Windows Git Bash** — the `bin/` path never exists, so it either re-creates the venv every run or errors on the missing activate.

Consequences & fixes:
- Such a runner is fine in CI (Linux) but not for local Windows dev. Do not "fix" it by assuming it is broken — it works where it runs.
- To run the suite on Windows: build the venv yourself and call the interpreter by its real path — `python -m venv .venv` then `.venv/Scripts/python.exe -m pytest`. Prefer a scratch-dir venv so you do not pollute an existing one.
- Pick the right base interpreter: a brand-new Python (e.g. 3.14) often lacks prebuilt wheels for `pydantic-core`/`litellm`, forcing a from-source (Rust) build that fails; use the version the project already targets (here 3.12).

Discovered running the customer360-agent pytest suite on Windows.
