---
title: "Verify an authed health endpoint in-container, not by curl, in CI"
created: 2026-09-16
type: lesson
status: seedling
source: "session 2026-09-16"
tags: [cicd, healthcheck, docker, github-actions, auth]
---

# Verify an authed health endpoint in-container, not by curl, in CI

An authenticated HTTP health endpoint cannot be smoke-tested from a CI runner with a plain curl -- it returns 401 with no bearer token, and minting one in CI is heavy. If the deploy already has an SSH/exec session to the box, run the SAME check the endpoint serves directly in the container instead of over HTTP.

Example (leo-customer360 CD): GET /api/v1/metadata/smtp is auth-gated, so deploy-api.sh does, right after `docker run`:

  docker exec customer360-api python -c 'from core.repositories.metadata_repository import MetadataRepository as M; print(M().get_smtp_health().get("status",""))'

This validates the exact code path (connect + STARTTLS + login) with the container's real env, no token needed. Keep it NON-FATAL for a non-critical dependency (email): print the status and a GH `::warning::` on failure but never `exit 1`, so a relay hiccup can't block a release. The `::warning::` relayed over ssh stdout is still parsed by GitHub Actions into a run annotation.

Trade-off: this bypasses the HTTP/auth layer, so it does not prove the route is reachable+authorized -- only that the underlying logic passes. Acceptable for a post-deploy dependency check; add a token-based curl only if you must prove the HTTP surface too.

## Related
[[SMTP health check stays out of auth-exempt GET /metadata login-path]]
[[LEO CI pushes images only on main/tags; feature branches build-only]]

## Related

- [[SMTP health check stays out of auth-exempt GET /metadata login-path]]
