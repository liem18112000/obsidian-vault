---
ai_hash: 65f0ac4cc884d387
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities:
- CD
- cd.yml
- deploy-all.sh
- UAT
- GHCR
- '@sha256 digest'
- deployments/lib/ghcr.sh::image_ref
- GitHub packages API
- vServer
- docker pull
- sha-<commit> tag
- main
- ads-server
- GHCR registry-consistency blip
- e197ff7
- latest
- backend
- api
- frontend
- CD-path deploy script
- deploy-{ads,frontend,api,backend,docs-search,tracking}.sh
- lib/ghcr.sh
- REMOTE SSH heredoc
- gh run rerun <run-id> --failed
- deploy-all.sh uat apply --from <service>
- select job
- workflow_run
- feature branch
- deploy-tracking.sh uat needs GHCR auth gh auth token or BUILD_LOCAL=1
- CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend
  or IPs via secrets
- leo-customer360
- service image
- bounded retry
- Failed deploy
- UAT mixed-version
tags:
- leo-customer360
- cd
- ghcr
- deployment
- gotcha
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

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]
- [[Feature-branch images are never pushed to GHCR in leo-customer360 CI]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]
- [[CD didn't fire on an infra-only merge (CI paths-ignore); re-run a workflow_run deploy via workflow_dispatch, not run rerun]]
- [[leo-customer360 CD runs after CI via workflow_run and resumable deploy-all.sh]]

**Relations:**
- CD — *is for* — leo-customer360
- CD — *uses* — cd.yml
- cd.yml — *invokes* — deploy-all.sh
- deploy-all.sh — *deploys to* — UAT
- Deployment — *pulls* — service image
- service image — *from* — GHCR
- service image — *identified by* — @sha256 digest
- @sha256 digest — *resolved from* — sha-<commit> tag
- deployments/lib/ghcr.sh::image_ref — *resolves* — sha-<commit> tag
- deployments/lib/ghcr.sh::image_ref — *uses* — GitHub packages API
- vServer — *executes* — docker pull
- docker pull — *fails due to* — GHCR registry-consistency blip
- ads-server — *is a* — service
- backend — *is a* — service
- api — *is a* — service
- frontend — *is a* — service
- CD-path deploy script — *wraps* — docker pull
- CD-path deploy script — *includes* — bounded retry
- deploy-{ads,frontend,api,backend,docs-search,tracking}.sh — *is a type of* — CD-path deploy script
- docker pull — *runs in* — REMOTE SSH heredoc
- lib/ghcr.sh — *not in scope in* — REMOTE SSH heredoc
- gh run rerun <run-id> --failed — *reruns* — Failed deploy
- deploy-all.sh uat apply --from <service> — *reruns* — Failed deploy
- select job — *deploys UAT for* — main
- workflow_run — *from* — feature branch
- workflow_run — *skips* — deploy
- Failed deploy — *results in* — UAT mixed-version
- Note — *related to* — deploy-tracking.sh uat needs GHCR auth gh auth token or BUILD_LOCAL=1
- Note — *related to* — CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets
- e197ff7 — *is a* — commit
- ads-server — *image tagged with* — sha-<commit> tag
- ads-server — *image tagged with* — latest
- push to main — *triggers* — CD
- bounded retry — *mitigates* — GHCR registry-consistency blip
- CD — *can fail with* — GHCR registry-consistency blip
- CD — *has* — fallback
- fallback — *is* — gh run rerun <run-id> --failed
- fallback — *is* — deploy-all.sh uat apply --from <service>

%% ai-graph-end %%