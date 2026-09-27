---
title: "test-agent-v2 image built only from pyproject + src + main.py"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13 deploy 2db8605"
tags: [docker, cloud-build, test-agent-v2, deploy]
---

# test-agent-v2 image built only from pyproject + src + main.py

The test-agent-v2 Docker image content is fully determined by just three inputs the Dockerfile copies: `pyproject.toml`, `src/`, and `main.py`. Everything else is dockerignored — `docs/`, `tests/`, `skills/`, `*.md`, `.venv/`, `.env`, and `uv.lock` (deps install via `pip install ".[bridge,gcp]"` from pyproject, NOT from the lockfile).

**Consequence:** uncommitted working-tree changes to docs/PNGs/excalidraw or an untracked `uv.lock` do NOT affect the built image. So tagging the image with the current `git rev-parse --short HEAD` is accurate even with a dirty working tree, as long as pyproject.toml + src/ + main.py are clean. (Cloud Build submits the working tree, so only those three matter.)

`deploy.sh` builds the image BEFORE the terraform apply and passes `-var=image=<tag>`; the tag also lives in the gitignored `terraform.tfvars` (bump it there to keep SKIP_BUILD applies consistent).

## Related

- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]
