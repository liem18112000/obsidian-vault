---
title: "Cloud Run generates a per-project URL hash, breaking environment config parity"
created: 2026-09-27
type: gotcha
status: seedling
source: "Confluence: Serverless with Google Cloud Run (LUZ)"
tags: [gcp, cloud-run, serverless, configuration, deployment, confluence-distilled]
---

# Cloud Run generates a per-project URL hash, breaking environment config parity

Cloud Run assigns each service a generated hostname containing a random project/region hash:

```
https://luz-thumbnail-run-q5rqhzn2uq-as.a.run.app
                        ^^^^^^^^^^ generated, not chosen
```

The hash differs per **project**, so the same logical service has a different URL in dev, test, and prod. Any caller that stores the URL in config now needs a different value per environment, and you cannot derive it from the service name — you have to deploy first, read the URL back, then configure the caller. That breaks the usual pattern where environment config is a template with the environment name substituted in.

**Ways out, in rough order of effort:**

- **Internal load balancer** in front of the Cloud Run services, giving a stable internal hostname per environment.
- **A proxy/service entry on the Kubernetes side** so in-cluster callers use a normal service name and the indirection is one place.
- **Deploy-time injection** — read the URL after deploy and write it into the consumer's config/secret. Simplest, but adds ordering between deploys.

Two related facts worth keeping with this:

- **Auth is on by default.** Cloud Run requires a `Bearer` token for the *Cloud Run Invoker* IAM role. You can disable it and supply your own authorization, but "it works locally with curl" usually means invoker auth is still doing the work.
- **Ingress can be restricted to internal traffic**, which is what you usually want when the caller is your own cluster — the generated URL stays but is no longer publicly reachable.

> [!note] Why it was worth it anyway
> In a 10-thumbnail benchmark Cloud Run averaged **550 ms** against **700 ms** for the equivalent Kubernetes pod, and it scales to zero — so for spiky, stateless, CPU-bound work the URL friction buys real idle-cost savings. Concurrency is tunable (max simultaneous requests per instance) and autoscaling defaults to 60% CPU utilisation. Treat these numbers as one workload's result, not a general benchmark.

Source: [[Serverless with Google Cloud Run]] (LUZ, Confluence).

## Related

- [[Serverless with Google Cloud Run]]
