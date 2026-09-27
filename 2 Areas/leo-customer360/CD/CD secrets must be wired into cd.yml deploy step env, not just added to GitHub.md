---
ai_hash: 0c6d16f556890cc7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-12
entities:
- CD secrets
- cd.yml
- deploy step env
- GitHub
- GitHub Actions CD
- .github/workflows/cd.yml
- deploy script
- '"Deploy app containers" step'
- 'env: block'
- deploy-all.sh
- deploy-monitoring.sh
- process env
- .env file
- CI
- PORTAINER_ADMIN_PASSWORD
- repo secret
- Portainer agent auto-registration
- sso/.env
- KEYCLOAK_ADMIN_PASSWORD
- leo-customer360 push to main skips the monitoring step; deploy Portainer agents
  manually
source: session 2026-09-12
status: seedling
tags:
- leo-customer360
- github-actions
- ci-cd
- secrets
- gotcha
title: CD secrets must be wired into cd.yml deploy step env, not just added to GitHub
type: lesson
---

# CD secrets must be wired into cd.yml deploy step env, not just added to GitHub

In this repo's GitHub Actions CD (`.github/workflows/cd.yml`), adding a secret in GitHub settings is **not** enough for a deploy script to see it — the secret must **also** be forwarded in the "Deploy app containers" step's `env:` block as `NAME: ${{ secrets.NAME }}`.

**Why:** that step just runs `bash deploy-all.sh <env> --only <services> -y` locally on the runner, which dispatches to per-service scripts (e.g. `deploy-monitoring.sh`). Those scripts read secrets from the **process env** or a git-ignored per-service `.env` file — and that `.env` does not exist in CI. So a secret that is not in the step `env:` block is simply absent at runtime.

**Symptom seen:** `PORTAINER_ADMIN_PASSWORD` was added as a repo secret but `deploy-monitoring.sh` still printed `PORTAINER_ADMIN_PASSWORD not in .env — add the env in the UI` and skipped Portainer agent auto-registration. Fix was one line: add `PORTAINER_ADMIN_PASSWORD: ${{ secrets.PORTAINER_ADMIN_PASSWORD }}` to the deploy step `env:` block. The pre-existing `sso/.env` provisioning (KEYCLOAK_ADMIN_PASSWORD) is the alternate pattern: write the secret into a `.env` file before running.

## Related
[[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]

## Related

- [[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]
- [[Adding a step to always-on CD provision ALL its required env, and make it skip (not die) on missing secrets]]
- [[GitHub secrets are write-only; run in Actions to use a CD key, push-trigger a feature branch to avoid main]]
- [[leo-customer360 frontend SSO=false because CD deploys the API with SSO_LOGIN=false]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]

**Relations:**
- CD secrets — *must be wired into* — deploy step env
- CD secrets — *are not just added to* — GitHub
- GitHub Actions CD — *is defined in* — .github/workflows/cd.yml
- adding a secret in GitHub settings — *is insufficient for* — deploy script
- deploy script — *to see* — secret
- secret — *must be forwarded in* — "Deploy app containers" step
- "Deploy app containers" step — *uses* — env: block
- "Deploy app containers" step — *runs* — deploy-all.sh
- deploy-all.sh — *dispatches to* — deploy-monitoring.sh
- deploy-monitoring.sh — *reads secrets from* — process env
- deploy-monitoring.sh — *reads secrets from* — .env file
- secret — *not in* — env: block
- secret not in env: block — *is* — absent at runtime
- PORTAINER_ADMIN_PASSWORD — *was added as* — repo secret
- deploy-monitoring.sh — *failed due to missing* — PORTAINER_ADMIN_PASSWORD
- missing PORTAINER_ADMIN_PASSWORD — *caused skipping of* — Portainer agent auto-registration
- Fix — *involved adding* — PORTAINER_ADMIN_PASSWORD
- adding PORTAINER_ADMIN_PASSWORD — *to* — deploy step env
- sso/.env — *is an alternate pattern for* — secret provisioning
- sso/.env — *provisions* — KEYCLOAK_ADMIN_PASSWORD
- alternate pattern — *involves writing secret to* — .env file
- leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually — *is related to* — CD secrets

%% ai-graph-end %%