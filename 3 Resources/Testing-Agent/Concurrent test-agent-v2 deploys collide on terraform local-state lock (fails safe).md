---
ai_hash: ce8f9358c9c2bde9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: Testing-Agent run-188f96b8 deploy
status: seedling
tags:
- testing-agent
- terraform
- deploy
- concurrency
- gotcha
title: Concurrent test-agent-v2 deploys collide on terraform local-state lock (fails
  safe)
type: lesson
---

# Concurrent test-agent-v2 deploys collide on terraform local-state lock (fails safe)

The **test-agent-v2** terraform uses a **LOCAL backend** (`terraform.tfstate` on disk + `.terraform.tfstate.lock.info`). So two Claude sessions (or machines) both running `deploy.sh` / `terraform apply` against the same checkout **collide on the file lock**.

**Observed:** with session A's apply holding the lock, session B's `deploy.sh` built its image fine (async Cloud Build SUCCESS) but its final `terraform apply` failed — twice, including the built-in retry — with:
> Error acquiring the state lock ... read terraform.tfstate: The process cannot access the file because another process has locked a portion of the file.

**Why this is actually safe:** the losing apply **never modifies state** — it fails at lock acquisition, so there is NO partial apply and NO corruption. The background `deploy.sh` just exits 1.

**Gotchas / rules:**
- A "failed" deploy may still have **built + pushed the image** (build precedes apply in deploy.sh) — only the apply was skipped.
- **Do NOT `force-unlock`** a lock held by a live parallel apply — that IS how you corrupt state. Only force-unlock a confirmed-stale lock (no terraform process running AND the owning session is dead).
- **Only ONE session should own `terraform apply`.** When two sessions are deploying, pick one to finish; the other stays out.
- A half-finished apply can leave services on **mixed image tags** (e.g. TPD+KGA on the new tag, TEV+gateway still on the old one) even though infra (Memorystore `kga-v2-cache`, VPC connector) was already created — because resource updates are ordered and an interrupted apply stops partway. Re-running the apply converges them.

## Related

- [[Deploying the test-agent-v2 Cloud Run stack (names]]
- [[tags]]
- [[plan)]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]
- [[Deploy a unique image tag to force a Cloud Run rollout via terraform]]
- [[Stale terraform state != live cloud — verify with gcloud before deleting a retired deployment]]
- [[Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)]]

%% ai-graph-end %%