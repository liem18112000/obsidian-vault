---
title: "LEO email_engine dispatch resolves DB config then SMTP env then mock"
created: 2026-09-16
type: design
status: seedling
source: "session 2026-09-16"
tags: [leo-customer360, email, smtp, design-decision]
---

# LEO email_engine dispatch resolves DB config then SMTP env then mock

The email dispatch pipeline in \`backend-system/email_engine\` is optional — it runs end-to-end with **no SMTP credentials**, defaulting to a no-network \`mock\` adapter. \`provider_config.load_email_config(tenant_id, conn)\` resolves config **once per campaign run**, in this order:

1. **Active per-tenant DB row** — \`customer360.crm_email_provider_config WHERE tenant_id=... AND is_active\`. Source of truth; the SMTP password never leaves Postgres. Managed via \`PUT /api/v1/admin/email-provider-config\`.
2. **Static \`SMTP_*\` env vars** — only if no DB row (\`EMAIL_DISPATCH_ADAPTER\`, \`SMTP_HOST/PORT/USERNAME/PASSWORD/USE_TLS\`, \`EMAIL_FROM_ADDRESS/NAME\`).
3. **\`mock\`** — default when neither is set.

\`adapters.build_adapter\` returns \`SMTPDispatchAdapter\` only for provider \`smtp\`; any unknown/typo provider **falls back to mock with a warning**, so a misconfig never silently starts real sends. Adding a provider (e.g. SES) = one subclass + a branch, callers unchanged.

So SMTP is needed only when a specific tenant wants real delivery — not for the platform to run.

## Related
[[LEO Customer360 UAT runs email in mock mode (backend.env omits SMTP vars)]]

## Related

- [[LEO Customer360 UAT runs email in mock mode (backend.env omits SMTP vars)]]
