---
ai_hash: f2e5b5092987475d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: session 2026-09-16
status: seedling
tags:
- leo-customer360
- email
- smtp
- healthcheck
- fastapi
- auth
title: SMTP health check stays out of auth-exempt GET /metadata login-path
type: lesson
---

# SMTP health check stays out of auth-exempt GET /metadata login-path

In customer360-api, an active SMTP health probe (connect + STARTTLS + login + NOOP) is exposed ONLY as a dedicated authenticated endpoint \`GET /api/v1/metadata/smtp\`, and deliberately NOT folded into the \`_service_status()\` map behind \`GET /metadata\`.

Reason: bare \`GET /api/v1/metadata\` is in \`core.auth.EXEMPT_PATHS\` because the login screen calls it unauthenticated to learn sso_login/sso_config. If the SMTP check lived in \`_service_status()\`, every login-page load would trigger a real SMTP login against the relay -- adding latency and risking provider rate-limits/lockouts (Brevo). Cheap TCP probes (postgres/redis/dagster) are fine there; an authenticating login is not.

So: keep expensive/credential-exercising health checks off any auth-exempt, frequently-hit endpoint; give them their own on-demand route (mirrors \`GET /metadata/dagster\`). The probe reads the SYSTEM/env SMTP config (the email_engine fallback), reports \`disabled\` in mock mode, and is not tenant-scoped.

## Related
[[LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)]]
[[LEO email_engine dispatch resolves DB config then SMTP env then mock]]

## Related

- [[LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)]]

%% ai-graph-start %%

**Related notes:**
- [[Verify an authed health endpoint in-container, not by curl, in CI]]
- [[LEO email_engine dispatch resolves DB config then SMTP env then mock]]
- [[LEO Customer360 UAT runs email in mock mode (backend.env omits SMTP vars)]]
- [[LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)]]
- [[leo-customer360 service dependency + health-probe map]]

%% ai-graph-end %%