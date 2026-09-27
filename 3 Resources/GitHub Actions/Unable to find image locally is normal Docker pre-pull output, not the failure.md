---
title: "\"Unable to find image locally\" is normal Docker pre-pull output, not the failure"
created: 2026-09-19
type: lesson
status: seedling
source: "session 2026-09-19"
tags: [github-actions, docker, ci-cd, gotcha, debugging]
---

# "Unable to find image locally" is normal Docker pre-pull output, not the failure

When a GitHub Actions step uses a **Docker container action** (like `pypa/gh-action-pypi-publish`), the log line `Unable to find image 'ghcr.io/...:tag' locally` is Docker's **normal pre-pull message** — it always prints before pulling any image not already cached. It is *not* an error.

The image almost always pulls fine right after; look for `Status: Downloaded newer image for <image>` a few lines down, with all layers showing `Pull complete`. The **actual failure is further down**, after the container starts running.

**Debugging lesson:** never diagnose from the first scary-looking line. Scroll to the *end* of the failed step (`gh run view <id> --log-failed | tail`) and read the real `##[error]` there. Chasing the "image not found" red herring wastes time on a non-problem.

One concrete case of the real error hiding below it: [[PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims]].

## Related

- [[PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims]]
