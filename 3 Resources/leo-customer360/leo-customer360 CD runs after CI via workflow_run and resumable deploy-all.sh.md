---
title: "leo-customer360 CD runs after CI via workflow_run and resumable deploy-all.sh"
created: 2026-09-18
type: howto
status: seedling
source: "session 2026-09-18"
tags: [leo-customer360, ci-cd, github-actions, deployment]
---

# leo-customer360 CD runs after CI via workflow_run and resumable deploy-all.sh

In leo-customer360, **CD is a separate GitHub Actions workflow** from CI. It does not trigger on `push`; it triggers via `workflow_run` **after the CI workflow succeeds**. So a red CD run has to be read against the CI run that spawned it — pushing a fix re-runs CI first, then CD only if CI is green.

The CD `deploy` job runs `deployments/server/deploy-all.sh <env> apply`, which deploys services **in a fixed order**:

`db-schema → backend → sso-realm → api → frontend → ads → tracking → docs-search`

It stops at the first failing service and prints the resume command. To restart from a given service instead of the top:

```bash
./deploy-all.sh <env> apply --from <service>
```

Each service has its own `deploy-<svc>.sh` (e.g. `deploy-backend.sh` ships `backend-system/` to the VM over SSH and runs docker there).

## Related

- [[SSH keepalive prevents broken-pipe exit 255 on long remote docker pulls]]
