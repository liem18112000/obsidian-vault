---
tags: [leo-customer360, cd, ghcr, deployment, gotcha]
created: 2026-09-10
---

# CD deploy can transiently 404 on a just-built GHCR digest — re-run the failed job

The leo-customer360 **CD** (`cd.yml` → `deploy-all.sh`) deploys UAT by pulling each service image from GHCR **pinned to an immutable `@sha256` digest** (`deployments/lib/ghcr.sh::image_ref` resolves the `sha-<commit>` tag → digest via the GitHub *packages API*, then the vServer runs `docker pull …@sha256:…`).

Right after a push to `main`, this can fail on `docker pull` with:

```
Error response from daemon: failed to resolve reference
"ghcr.io/leo-cdp/leo-customer360/ads-server@sha256:…": not found
✗ ads FAILED (exit 1)
```

**It's a transient GHCR registry-consistency blip, not a config bug.** Evidence from run 34431806711 (commit `e197ff7`): the image existed (pushed 03:02:09Z, tagged `sha-<commit>` **and** `latest`), the packages API resolved the digest correctly, and other services (backend/api/frontend) pulled fine in the *same* run with the *same* creds. A re-run ~25 min later pulled the **identical digest** with no changes → `✓ ads done`.

**Now auto-handled:** each CD-path deploy script (`deploy-{ads,frontend,api,backend,docs-search,tracking}.sh`) wraps its remote `docker pull` in a bounded retry — 5 attempts, linear backoff 10/20/30/40s — so a brief blip self-heals. The retry is inlined per script (not a shared `lib/ghcr.sh` function) **because the pull runs inside a quoted `<<'REMOTE'` SSH heredoc on the vServer, where nothing sourced on the runner is in scope.**

**Fallback (longer outage):** re-run the failed deploy — `gh run rerun <run-id> --failed` (or `deploy-all.sh uat apply --from <service>`, which the script prints on failure). The deploy is idempotent and resumes from the failed service.

**Watch out:** a partial failure leaves UAT **mixed-version** — services before the failing one are on the new commit, the rest stay on the previous build — until the re-run completes. The CD's `select` job only deploys UAT for `main`; a `workflow_run` from a feature branch shows as a 5s "success" that **skipped** the deploy, so don't read it as a UAT redeploy.

Related: [[deploy-tracking.sh uat needs GHCR auth gh auth token or BUILD_LOCAL=1]], [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]
