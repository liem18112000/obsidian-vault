---
ai_hash: e582a9f281c88d93
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-19
entities: []
source: session 2026-09-19
status: seedling
tags:
- github-actions
- docker
- ci-cd
- gotcha
- debugging
title: '"Unable to find image locally" is normal Docker pre-pull output, not the failure'
type: lesson
---

# "Unable to find image locally" is normal Docker pre-pull output, not the failure

When a GitHub Actions step uses a **Docker container action** (like `pypa/gh-action-pypi-publish`), the log line `Unable to find image 'ghcr.io/...:tag' locally` is Docker's **normal pre-pull message** — it always prints before pulling any image not already cached. It is *not* an error.

The image almost always pulls fine right after; look for `Status: Downloaded newer image for <image>` a few lines down, with all layers showing `Pull complete`. The **actual failure is further down**, after the container starts running.

**Debugging lesson:** never diagnose from the first scary-looking line. Scroll to the *end* of the failed step (`gh run view <id> --log-failed | tail`) and read the real `##[error]` there. Chasing the "image not found" red herring wastes time on a non-problem.

One concrete case of the real error hiding below it: [[PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims]].

## Related

- [[PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims]]

%% ai-graph-start %%

**Related notes:**
- [[PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims]]
- [[CI build Docker image on every run, push only on non-PR]]
- [[Publish a Docker image to GHCR from GitHub Actions with GITHUB_TOKEN]]
- [[Local deploy pull from GHCR needs a token with readpackages — gh default token lacks it (403)]]
- [[Cloud Run can only pull images from Artifact Registry or GCR, not GHCR]]

%% ai-graph-end %%