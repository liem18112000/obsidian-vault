---
title: "SMTP health check stays out of auth-exempt GET /metadata login-path"
created: 2026-09-16
type: lesson
status: seedling
source: "session 2026-09-16"
tags: [leo-customer360, email, smtp, healthcheck, fastapi, auth]
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
