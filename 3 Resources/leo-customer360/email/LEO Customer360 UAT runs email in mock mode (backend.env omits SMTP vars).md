---
title: "LEO Customer360 UAT runs email in mock mode (backend.env omits SMTP vars)"
created: 2026-09-16
type: lesson
status: seedling
source: "session 2026-09-16"
tags: [leo-customer360, email, smtp, uat, deployment, gotcha]
---

# LEO Customer360 UAT runs email in mock mode (backend.env omits SMTP vars)

UAT currently sends **no real email** — campaigns run the full pipeline (segment -> render -> ledger in \`cdp_campaign_dispatch_logs\`) but dispatch nothing, unless a tenant has a \`crm_email_provider_config\` row.

Why: \`email_engine\` ships inside the **backend-system** image as one Dagster code location (not a standalone service), deployed in \`deploy-all.sh\` phase 3 ("backend") by \`deployments/server/deploy-backend.sh\`. That script generates \`/opt/c360/backend.env\` (passed to the container via \`--env-file\`) containing **only DB + S3 + Redis vars — no \`EMAIL_DISPATCH_ADAPTER\`, no \`SMTP_*\`**. No UAT overlay/tfvars/.env sets them either. With no env and (usually) no DB row, \`load_email_config\` falls to the \`mock\` default.

To enable real UAT sending, either:
- create a per-tenant provider config via \`PUT /api/v1/admin/email-provider-config\` (preferred — password stays in Postgres), or
- add \`EMAIL_DISPATCH_ADAPTER=smtp\` + \`SMTP_*\` to the \`backend.env\` heredoc in \`deploy-backend.sh\` (secrets via TF_VAR / GH secrets, same pattern as \`DB_PASSWORD\`) and redeploy backend.

Apply a change with the *Admin (UAT)* workflow -> \`deployments/admin-uat.sh restart-backend\` (or \`redeploy-apps\` to re-pull the image).

## Related
[[LEO email_engine dispatch resolves DB config then SMTP env then mock]]

## Related

- [[LEO email_engine dispatch resolves DB config then SMTP env then mock]]
