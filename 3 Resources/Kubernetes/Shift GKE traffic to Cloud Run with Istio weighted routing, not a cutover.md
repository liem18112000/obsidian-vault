---
title: "Shift GKE traffic to Cloud Run with Istio weighted routing, not a cutover"
created: 2026-09-27
type: howto
status: seedling
source: "Confluence: Migrate GKE to CloudRun luz-antivirus (TK/LUZ)"
tags: [istio, service-mesh, cloud-run, gke, migration, canary, confluence-distilled]
---

# Shift GKE traffic to Cloud Run with Istio weighted routing, not a cutover

To move a service out of Kubernetes and onto Cloud Run without touching a single caller, put an Istio `VirtualService` in front of the in-cluster name and shift traffic by **weight**. Callers keep addressing the same Kubernetes service; the mesh decides how much of that traffic leaves the cluster.

The migration, for `luz-antivirus`:

1. **Deploy the Cloud Run service alongside** the existing GKE deployment — both live at once.
   ```bash
   ./deploy_terraform.sh prod --target=module.luz-antivirus
   ```
2. **Resolve the generated Cloud Run hostname** and write it into the routing manifest:
   ```bash
   read s p <<< "$(gcloud run services describe prod-luz-antivirus \
     --region europe-west6 --project klara-prod \
     --format='value(metadata.name,metadata.namespace)')" \
     && echo "${s}-${p}.europe-west6.run.app"
   ```
3. **Set the weights** in `istio-cloudrun-routing.yaml` per environment. A 50/50 split means callers hitting the in-cluster `luz-antivirus` get half GKE, half Cloud Run.

**Why this beats a cutover:**

- **Zero caller changes.** Nothing downstream learns a new hostname, so the blast radius of a mistake is the weight value, not a config rollout across N services.
- **Rollback is a number.** Set the Cloud Run weight to 0 and traffic returns to GKE immediately — no redeploy, no image rebuild.
- **You can calibrate on real traffic.** Start at 5%, watch error rate and latency against the GKE baseline running *concurrently* on the same requests, then climb. Synthetic load cannot give you that comparison.

> [!warning] Weighted routing assumes the two backends are interchangeable
> It splits requests, not sessions. If the service holds any per-instance state, caches locally, or has side effects that must not double-apply, a 50/50 split produces two divergent worlds. This pattern fits **stateless request/response** services — an antivirus scanner is close to ideal. Check statelessness before you check the weights.

> [!note] The placeholder is the Cloud Run URL problem again
> The manifests ship with `REPLACE_WITH_CLOUDRUN_HOSTNAME` and a per-environment command to fill it in — exactly the symptom described in [[Cloud Run generates a per-project URL hash, breaking environment config parity]]. The mesh route is, in effect, the "stable internal hostname" workaround: callers use the Kubernetes name and only one file knows the generated URL.

Source: [[ Migrate GKE to CloudRun - Gradually migrate the luz-antivirus to Cloud Run]] (TK/LUZ, Confluence).

## Related

- [[Cloud Run generates a per-project URL hash, breaking environment config parity]]
