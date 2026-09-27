---
title: "CD secrets must be wired into cd.yml deploy step env, not just added to GitHub"
created: 2026-09-12
type: lesson
status: seedling
source: "session 2026-09-12"
tags: [leo-customer360, github-actions, ci-cd, secrets, gotcha]
---

# CD secrets must be wired into cd.yml deploy step env, not just added to GitHub

In this repo's GitHub Actions CD (`.github/workflows/cd.yml`), adding a secret in GitHub settings is **not** enough for a deploy script to see it — the secret must **also** be forwarded in the "Deploy app containers" step's `env:` block as `NAME: ${{ secrets.NAME }}`.

**Why:** that step just runs `bash deploy-all.sh <env> --only <services> -y` locally on the runner, which dispatches to per-service scripts (e.g. `deploy-monitoring.sh`). Those scripts read secrets from the **process env** or a git-ignored per-service `.env` file — and that `.env` does not exist in CI. So a secret that is not in the step `env:` block is simply absent at runtime.

**Symptom seen:** `PORTAINER_ADMIN_PASSWORD` was added as a repo secret but `deploy-monitoring.sh` still printed `PORTAINER_ADMIN_PASSWORD not in .env — add the env in the UI` and skipped Portainer agent auto-registration. Fix was one line: add `PORTAINER_ADMIN_PASSWORD: ${{ secrets.PORTAINER_ADMIN_PASSWORD }}` to the deploy step `env:` block. The pre-existing `sso/.env` provisioning (KEYCLOAK_ADMIN_PASSWORD) is the alternate pattern: write the secret into a `.env` file before running.

## Related
[[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]

## Related

- [[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]
