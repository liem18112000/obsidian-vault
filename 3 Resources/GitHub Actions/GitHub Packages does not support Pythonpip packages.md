---
ai_hash: 3dd9a48e0ac89ce0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-19
entities: []
source: session 2026-09-19
status: seedling
tags:
- github-packages
- pypi
- python
- packaging
- ci-cd
- reference
title: GitHub Packages does not support Python/pip packages
type: term
---

# GitHub Packages does not support Python/pip packages

GitHub Packages (GitHub's built-in package registry) supports a **fixed set of ecosystems** — and **Python/pip is not one of them**. Supported: **npm** (JavaScript), **Maven & Gradle** (Java), **NuGet** (.NET), **RubyGems** (Ruby), and the **container registry** (`ghcr.io`, Docker/OCI images). There is no `pip install` simple index on GitHub Packages, so a Python package cannot be hosted there.

**Don't confuse the two "GitHub"s:** *GitHub Actions* (the CI runner) can absolutely publish a Python package — but it publishes it **to PyPI**, not to GitHub Packages. "GitHub publishes it" usually means the Actions workflow, not GitHub-as-a-registry.

**If you want to keep a Python package on GitHub anyway** (no PyPI): attach the wheel/sdist to a **GitHub Release** (`pip install <release-asset-url>`), or install directly from git (`pip install "git+https://…#subdirectory=<pkg>"`), or run a self-hosted PEP 503 index. None of these is a true package registry — for a public pip-installable package, PyPI remains the correct target.

Related: [[PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims]].

## Related

- [[PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims]]

%% ai-graph-start %%

**Related notes:**
- [[PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims]]
- [[Unable to find image locally is normal Docker pre-pull output, not the failure]]
- [[Publish a Docker image to GHCR from GitHub Actions with GITHUB_TOKEN]]

%% ai-graph-end %%