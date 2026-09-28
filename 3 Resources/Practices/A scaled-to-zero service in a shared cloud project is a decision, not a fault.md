---
ai_hash: 2811121a2d3e228a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-28'
created: 2026-09-28
entities:
- Scaled-to-zero service
- Shared cloud project
- Decision
- Fault
- Resource
- API
- Colleague
- Idle non-production infrastructure
- Cost saving
- Deliberate shutdown
- Reversible
- Non-destructive
- Debugging posture
- Logs
- Redeploys
- Image bisection
- Annotation lookup
- Audit-log query
- Cloud Run
- lastModifier
- UpdateService audit log
- Re-enabling
- Cost decision
- Resource health
- Cloud Run revision
- 'Ready: True'
- Image
- IAM
- Genuine breakage
- A disabled Cloud Run service 503s at the edge and never reaches your app
- Cloud Run names who changed a service in lastModifier and the UpdateService audit
  log
source: session 2026-09-28 — testing-agent MCP 503
status: seedling
tags:
- debugging
- cloud
- collaboration
- cost
- gotcha
title: A scaled-to-zero service in a shared cloud project is a decision, not a fault
type: lesson
---

# A scaled-to-zero service in a shared cloud project is a decision, not a fault

When a resource in a *shared* cloud project is found switched off — scaled to zero, traffic pinned to 0%%, an API disabled — the default reading should be "a colleague turned this off on purpose" rather than "something broke". Idle non-production infrastructure is the most common thing people disable to stop it billing, and disabling is exactly the reversible, non-destructive lever they reach for instead of deleting.

This flips the debugging posture in two useful ways.

**It changes what you investigate.** A fault invites logs, redeploys and image bisection. A decision invites one annotation lookup and one audit-log query — see [[Cloud Run names who changed a service in lastModifier and the UpdateService audit log]]. The second path is minutes; the first can burn an afternoon on a service that is working perfectly and simply has nobody serving it.

**It changes whether you should fix it.** Re-enabling is technically trivial and easy to justify to yourself — you need the thing working *now*. But it silently overrides a cost decision that someone else made and probably still stands, in an environment you share. Being right about the mechanism does not make you the owner of the choice. Surface it and let the person who needs it decide: "X, Y and Z were scaled to zero by <person> on <date>, looks like a cost saving — want them back on?"

The tell that separates the two cases is that a deliberate shutdown leaves the resource *healthy*: the Cloud Run revision still reports `Ready: True`, the image is the right one, IAM is intact. Genuine breakage rarely looks that tidy.

Related: [[A disabled Cloud Run service 503s at the edge and never reaches your app]].

## Related

- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Cloud Run names who changed a service in lastModifier and the UpdateService audit log]]

%% ai-graph-start %%

**Related notes:**
- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Cloud Run names who changed a service in lastModifier and the UpdateService audit log]]
- [[GCP Data Access logs are off by default, so data-plane calls are unattributable]]
- [[Rotating a service bearer silently invalidates every client config holding the old one]]
- [[Hitting Cloud Run maxScale turns latency into compounding errors, and 2xx throughput falls]]

**Relations:**
- Scaled-to-zero service — *is a* — Decision
- Scaled-to-zero service — *is not a* — Fault
- Scaled-to-zero service — *is in* — Shared cloud project
- Resource — *is in* — Shared cloud project
- Resource — *can be* — Scaled-to-zero service
- API — *can be* — Disabled
- Colleague — *performs* — Deliberate shutdown
- Idle non-production infrastructure — *is a type of* — Resource
- Idle non-production infrastructure — *disabled for* — Cost saving
- Deliberate shutdown — *is* — Reversible
- Deliberate shutdown — *is* — Non-destructive
- Decision — *changes* — Debugging posture
- Fault — *invites* — Logs
- Fault — *invites* — Redeploys
- Fault — *invites* — Image bisection
- Decision — *invites* — Annotation lookup
- Decision — *invites* — Audit-log query
- Cloud Run — *uses* — lastModifier
- Cloud Run — *uses* — UpdateService audit log
- Cloud Run — *identifies changes via* — lastModifier
- Cloud Run — *identifies changes via* — UpdateService audit log
- Re-enabling — *overrides* — Cost decision
- Cost decision — *made by* — Colleague
- Deliberate shutdown — *leaves* — Resource health
- Cloud Run revision — *reports* — Ready: True
- Resource health — *includes* — Image
- Resource health — *includes* — IAM
- Genuine breakage — *lacks* — Resource health
- Scaled-to-zero service — *related to* — A disabled Cloud Run service 503s at the edge and never reaches your app
- Scaled-to-zero service — *related to* — Cloud Run names who changed a service in lastModifier and the UpdateService audit log
- Cloud Run — *related to* — A disabled Cloud Run service 503s at the edge and never reaches your app
- Cloud Run — *related to* — Cloud Run names who changed a service in lastModifier and the UpdateService audit log

%% ai-graph-end %%