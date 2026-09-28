---
title: "A scaled-to-zero service in a shared cloud project is a decision, not a fault"
created: 2026-09-28
type: lesson
status: seedling
source: "session 2026-09-28 — testing-agent MCP 503"
tags: [debugging, cloud, collaboration, cost, gotcha]
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
