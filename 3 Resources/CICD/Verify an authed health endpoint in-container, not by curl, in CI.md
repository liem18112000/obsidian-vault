---
ai_hash: f79a12b94f98f114
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: session 2026-09-16
status: seedling
tags:
- cicd
- healthcheck
- docker
- github-actions
- auth
title: Verify an authed health endpoint in-container, not by curl, in CI
type: lesson
---

# Verify an authed health endpoint in-container, not by curl, in CI

An authenticated HTTP health endpoint cannot be smoke-tested from a CI runner with a plain curl -- it returns 401 with no bearer token, and minting one in CI is heavy. If the deploy already has an SSH/exec session to the box, run the SAME check the endpoint serves directly in the container instead of over HTTP.

Example (leo-customer360 CD): GET /api/v1/metadata/smtp is auth-gated, so deploy-api.sh does, right after `docker run`:

  docker exec customer360-api python -c 'from core.repositories.metadata_repository import MetadataRepository as M; print(M().get_smtp_health().get("status",""))'

This validates the exact code path (connect + STARTTLS + login) with the container's real env, no token needed. Keep it NON-FATAL for a non-critical dependency (email): print the status and a GH `::warning::` on failure but never `exit 1`, so a relay hiccup can't block a release. The `::warning::` relayed over ssh stdout is still parsed by GitHub Actions into a run annotation.

Trade-off: this bypasses the HTTP/auth layer, so it does not prove the route is reachable+authorized -- only that the underlying logic passes. Acceptable for a post-deploy dependency check; add a token-based curl only if you must prove the HTTP surface too.

## Related
[[SMTP health check stays out of auth-exempt GET metadata login-path|SMTP health check stays out of auth-exempt GET /metadata login-path]]
[[LEO CI pushes images only on main/tags; feature branches build-only]]

## Related

- [[SMTP health check stays out of auth-exempt GET metadata login-path|SMTP health check stays out of auth-exempt GET /metadata login-path]]

%% ai-graph-start %%

**Related notes:**
- [[SMTP health check stays out of auth-exempt GET metadata login-path]]
- [[Health checks should probe dependencies and split critical vs fail-open]]
- [[Verify uat customer360-api health publicly at beta.leocdp.comc360apihealth]]
- [[Feature-branch images are never pushed to GHCR in leo-customer360 CI]]
- [[customer360-api tenant-admin router tests must inject an admin request.state.user]]

%% ai-graph-end %%