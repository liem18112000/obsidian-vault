---
ai_hash: 75e12c4587623463
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22 JEV J6
status: seedling
tags:
- python
- uv
- pip
- docker
- reproducibility
- gotcha
title: A pip-built Docker image ignores uv.lock — pin extras in pyproject for reproducibility
type: lesson
---

# A pip-built Docker image ignores uv.lock — pin extras in pyproject for reproducibility

A Dockerfile that installs with `pip install ".[extra]"` does **not** consult `uv.lock` — pip resolves each dependency fresh from the index at build time. So an unpinned extra (e.g. `typesafe-sdk` with no specifier) pulls whatever the latest version is *into the production image*, even though `uv.lock` pins it for local `uv` dev. The lock protects your machine, not the image.

**Fix:** pin the version in `pyproject.toml` itself (`"typesafe-sdk==0.7.1"`), not just via the lock. That is the only thing a pip-based image build honors. Changing the pyproject constraint invalidates `uv.lock`, so re-run `uv lock` (resolution is unchanged if the pin matches what was already locked) and confirm with `uv lock --check`.

Surfaced wiring the JEV `jev` extra into the test-agent-v2 deploy image (J6). The image did `pip install ".[bridge,gcp]"`; adding `jev` unpinned would have shipped an untested early-access SDK to prod.

Related: [[Calibrate a cascade threshold against the exact gate condition, not a looser proxy]].

## Related

- [[Piping a Python CLI through tail block-buffers stdout, looking like a hang]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 image built only from pyproject + src + main.py]]

%% ai-graph-end %%