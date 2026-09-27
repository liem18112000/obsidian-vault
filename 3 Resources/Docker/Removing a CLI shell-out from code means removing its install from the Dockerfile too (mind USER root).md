---
ai_hash: eff39eb7f620e7be
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-07
entities: []
source: session 2026-08-07 luz_docs_import
status: seedling
tags:
- docker
- dockerfile
- dead-code
- kubectl
- gotcha
- luz-docs
title: Removing a CLI shell-out from code means removing its install from the Dockerfile
  too (mind USER root)
type: lesson
---

# Removing a CLI shell-out from code means removing its install from the Dockerfile too (mind USER root)

When you delete application code that **shells out to a CLI tool**, trace the removal through to the **image build** and drop that tool's install too — otherwise it lingers as a dead build-time dependency (extra layers + a network fetch that can fail the build offline).

Concrete case (luz_docs_import): removing `PodUtil.isPodRunning` (which ran `kubectl get pod ...`) made the Dockerfile's kubectl install (`curl -LO .../kubectl && chmod +x && mv /usr/local/bin`) redundant, so it was removed.

**Gotcha when removing those Dockerfile lines:** keep the `USER root` directive if later `RUN` steps still need root. Here the kubectl block introduced `USER root`, but the *following* steps (WildFly `replace_lines.sh`/`append_after_lines.sh` editing `standalone.xml`, and the WAR `COPY`) relied on being root. Dropping `USER root` with the kubectl lines would have silently changed the user for all subsequent layers. Verified by a full local `docker compose build` + `up` (WildFly deployed the WAR, `/openapi` returned 200).

Also check docker-compose for the same tool/env (here docker-compose.yml had no kube reference — nothing to change).

Related: [[luz_docs_import idempotent re-import replaces view-controller search-based file dedup|luz_docs_import: idempotent re-import replaces view-controller search-based file dedup]]

## Related

- [[luz_docs_import idempotent re-import replaces view-controller search-based file dedup|luz_docs_import: idempotent re-import replaces view-controller search-based file dedup]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_import detects dead async-worker pods via kubectl during status polling]]
- [[luz_docs_import idempotent re-import replaces view-controller search-based file dedup]]
- [[A local bring-up script must pin kubectl --context or it deploys to the wrong cluster]]
- [[Run luz_docs_statistic locally with docker-compose]]
- [[Verify kubectl context before GKE rollout - _context file can disagree]]

%% ai-graph-end %%