---
ai_hash: 69524c782d511bcd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities:
- test-agent-v2 Docker image
- pyproject.toml
- src/
- main.py
- Dockerfile
- docs/
- tests/
- skills/
- '*.md'
- .venv/
- .env
- uv.lock
- pip install ".[bridge,gcp]"
- Cloud Build
- git rev-parse --short HEAD
- deploy.sh
- terraform apply
- image tag
- terraform.tfvars
- SKIP_BUILD
- test-agent-v2 Cloud Run services
- -v2 name suffix
source: session 2026-09-13 deploy 2db8605
status: seedling
tags:
- docker
- cloud-build
- test-agent-v2
- deploy
title: test-agent-v2 image built only from pyproject + src + main.py
type: lesson
---

# test-agent-v2 image built only from pyproject + src + main.py

The test-agent-v2 Docker image content is fully determined by just three inputs the Dockerfile copies: `pyproject.toml`, `src/`, and `main.py`. Everything else is dockerignored — `docs/`, `tests/`, `skills/`, `*.md`, `.venv/`, `.env`, and `uv.lock` (deps install via `pip install ".[bridge,gcp]"` from pyproject, NOT from the lockfile).

**Consequence:** uncommitted working-tree changes to docs/PNGs/excalidraw or an untracked `uv.lock` do NOT affect the built image. So tagging the image with the current `git rev-parse --short HEAD` is accurate even with a dirty working tree, as long as pyproject.toml + src/ + main.py are clean. (Cloud Build submits the working tree, so only those three matter.)

`deploy.sh` builds the image BEFORE the terraform apply and passes `-var=image=<tag>`; the tag also lives in the gitignored `terraform.tfvars` (bump it there to keep SKIP_BUILD applies consistent).

## Related

- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]

%% ai-graph-start %%

**Related notes:**
- [[Deploy a unique image tag to force a Cloud Run rollout via terraform]]
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]
- [[Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)]]
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]

**Relations:**
- test-agent-v2 Docker image — *is built from* — pyproject.toml
- test-agent-v2 Docker image — *is built from* — src/
- test-agent-v2 Docker image — *is built from* — main.py
- Dockerfile — *copies* — pyproject.toml
- Dockerfile — *copies* — src/
- Dockerfile — *copies* — main.py
- docs/ — *is dockerignored* — test-agent-v2 Docker image
- tests/ — *is dockerignored* — test-agent-v2 Docker image
- skills/ — *is dockerignored* — test-agent-v2 Docker image
- *.md — *is dockerignored* — test-agent-v2 Docker image
- .venv/ — *is dockerignored* — test-agent-v2 Docker image
- .env — *is dockerignored* — test-agent-v2 Docker image
- uv.lock — *is dockerignored* — test-agent-v2 Docker image
- uv.lock — *is not used for* — deps install
- pip install ".[bridge,gcp]" — *installs dependencies from* — pyproject.toml
- Cloud Build — *submits* — working tree
- git rev-parse --short HEAD — *is used for* — image tag
- deploy.sh — *builds* — test-agent-v2 Docker image
- deploy.sh — *passes* — image tag
- image tag — *is passed to* — terraform apply
- image tag — *lives in* — terraform.tfvars
- terraform.tfvars — *is* — gitignored
- SKIP_BUILD — *is related to* — terraform apply
- test-agent-v2 Cloud Run services — *use* — -v2 name suffix

%% ai-graph-end %%