---
ai_hash: 837efb5dae2cd03f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- docker
- build
- overlay
- pip
- technique
- low-disk
title: Thin overlay image to refresh app code when full rebuilds keep failing
type: howto
---

# Thin overlay image to refresh app code when full rebuilds keep failing

TECHNIQUE that unstuck a stack whose full `docker compose build` kept getting killed (slow ~1.2MB/s network re-downloading the whole dep tree, low disk): build a THIN OVERLAY on the existing (stale) image instead of rebuilding from base. Only NEW deps + current source are added, so it finishes in seconds with a tiny download.

Dockerfile.patch:
  FROM <project>-<service>:latest   # stale image already has all heavy deps
  USER root
  WORKDIR /app
  COPY pyproject.toml ./ ; COPY src ./src ; COPY main.py worker.py redis_worker.py ./
  RUN pip install <only-the-new-dep> && pip install --no-deps --force-reinstall .   # reinstall MY package from current src, no dep re-download
  USER appuser
Then `docker build -f Dockerfile.patch -t <project>-kga:latest .` and `docker tag` it to every other service image; `docker compose up -d` (no --build) recreates containers on it. Here it added boto3 + refreshed code in ~13s vs a full rebuild that repeatedly died. CAVEAT: it is a stopgap layered on a stale base — do a clean `docker compose build` once the environment can sustain it. Depends on the stale image already having every other dependency (only boto3 was new). See [[A failed docker compose --build leaves latest on the OLD image (silent stale run)|A failed docker compose --build leaves :latest on the OLD image (silent stale run)]].

## Related

- [[A failed docker compose --build leaves latest on the OLD image (silent stale run)|A failed docker compose --build leaves :latest on the OLD image (silent stale run)]]

%% ai-graph-start %%

**Related notes:**
- [[A failed docker compose --build leaves latest on the OLD image (silent stale run)]]
- [[Repeated compose up -d can corrupt the bridge network — down+up to rebuild it]]
- [[test-agent-v2 image built only from pyproject + src + main.py]]
- [[Two Dockerfiles differing only in entrypoint should be one image plus compose override]]
- [[Cloud Run won't redeploy on a latest digest change — apply by immutable digest]]

%% ai-graph-end %%