---
title: "exclude= does not lift PQS precision because cloud-discover re-promotes 8 services"
created: 2026-09-14
type: lesson
status: seedling
source: "session 2026-09-14 run-cd156028"
tags: [testing-agent, pqs, exclude, precision, gotcha, luz-158230]
---

# exclude= does not lift PQS precision because cloud-discover re-promotes 8 services

On the deployed Testing-Agent build, passing `exclude=` to `gather_knowledge` on an existing context does **not** raise the Pack Quality Score's `ctx_precision`. It removes the named nodes, but the KGA cloud-discover step **unconditionally promotes 8 cloud services into every pack** (top 8 of ~600), so excluding 8 salary/broker services just freed slots for 8 antivirus services. The re-gather also injected fresh off-topic web noise (towardsdatascience, k8s cron docs, a YouTube embed, batch-job demo endpoints) **and dropped the `codegraph:` node** that a prior `gather_codebase` had attached — so you must re-run `gather_codebase` afterward to restore grounding.

Net on run-cd156028 (LUZ-158230): PQS stayed `0.75`, ctx_precision `0.00`, ctx_recall `1.00`, leaked `[]` — identical before and after the exclude.

**Rule of thumb:** `precision=0.00 + recall=1.00 + leaked=[]` is the *complete-but-noisy* signature — safe to proceed; the noise is cloud-discover clutter, not a hard-negative leak. To actually cut precision noise you'd need to cap cloud promotion (`KGA_CLOUD_MAX_SERVICES`) or use a fresh context, not `exclude=`.

Related: [[exclude is a no-op on a converged KGA exploration]], [[PQS precision can be 0 with no hard-negative leak]]

## Related

- [[exclude is a no-op on a converged KGA exploration]]
- [[PQS precision can be 0 with no hard-negative leak]]
