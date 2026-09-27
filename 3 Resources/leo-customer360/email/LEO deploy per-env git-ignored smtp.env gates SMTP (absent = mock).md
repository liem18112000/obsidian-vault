---
ai_hash: 925d30547b8a976e
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
- deployment
- secrets
- uat
title: LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)
type: howto
---

# LEO deploy per-env git-ignored smtp.env gates SMTP (absent = mock)

To turn real SMTP sending on for one deployed env without polluting the terraform variable space (and without committing a secret), both deploy scripts source a per-env file \`deployments/server/smtp.<env>.env\`:

- deploy-api.sh + deploy-backend.sh both \`cd\` to \`deployments/server\`, so they \`[[ -f "smtp.$ENV.env" ]] && { set -a; source "smtp.$ENV.env"; set +a; }\`.
- The file holds \`EMAIL_DISPATCH_ADAPTER=smtp\` + \`SMTP_*\` + \`EMAIL_FROM_*\`. Its values are folded into the container env blob (backend.env / api.env), base64-transported over ssh so the SMTP key never sits in argv cleartext.
- **Absent file => adapter stays mock** (no real email). This is the whole mechanism by which prod stays mock: there is simply no \`smtp.prod.env\`. Enabling prod later = drop in \`smtp.prod.env\`, nothing else.
- Secret stays out of git: \`smtp.*.env\` is added to \`deployments/server/.gitignore\`; a committed \`smtp.env.example\` documents the keys.

Why not \`overlays/<env>.tfvars\`? Those are passed to \`terraform apply -var-file\`; an undeclared \`smtp_host\` there triggers "Value for undeclared variable" warnings. A plain \`.env\`-style file read only by the deploy scripts avoids the terraform var space entirely.

Chosen for the c360 UAT Brevo SMTP rollout (feat/uat-smtp-brevo-healthcheck).

## Related
[[LEO email_engine dispatch resolves DB config then SMTP env then mock]]
[[LEO Customer360 UAT runs email in mock mode (backend.env omits SMTP vars)]]
[[Sourced shell env file must quote values with spaces]]

## Related

- [[LEO email_engine dispatch resolves DB config then SMTP env then mock]]
- [[Sourced shell env file must quote values with spaces]]

%% ai-graph-start %%

**Related notes:**
- [[LEO email_engine dispatch resolves DB config then SMTP env then mock]]
- [[LEO Customer360 UAT runs email in mock mode (backend.env omits SMTP vars)]]
- [[Sourced shell env file must quote values with spaces]]
- [[SMTP health check stays out of auth-exempt GET metadata login-path]]
- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]

%% ai-graph-end %%