---
ai_hash: 6d9e5a58d3859906
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Luz Kubernetes Terraform (LUZ)'
status: seedling
tags:
- terraform
- kustomize
- iac
- kubernetes
- cloud-run
- coupling
- confluence-distilled
title: Two IaC surfaces need an explicit naming contract at the seam
type: lesson
---

# Two IaC surfaces need an explicit naming contract at the seam

Most real estates end up with **two infrastructure-as-code surfaces**, not one — and the seam between them is where outages come from.

A concrete split:

| Surface | Owns | Tool |
|---|---|---|
| `terraform/` | Cloud Run services, Pub/Sub, IAM | Terraform, remote state in GCS (`luz-terraform`, prefix `state`), environments as **workspaces** |
| `kubernetes/` + `kubernetes-overlays/` | Apps on GKE, per-environment customisation | Kustomize overlays |

The split is defensible — Terraform is good at cloud resources and IAM, Kustomize is good at per-environment manifest variation — and the README says the important part out loud:

> *"Coordinate naming (URLs, gateways, regions) when changing either side."*

**Why that line is the whole point.** Neither tool can see the other's state. Terraform creates a Cloud Run service with a generated URL; a Kustomize overlay hardcodes that URL into a ConfigMap or an Istio route. Rename a service, move a region, or recreate a gateway on the Terraform side and nothing fails at apply time — the manifests still apply cleanly, they just now point at something that no longer exists. The failure surfaces at runtime, in the other repo, owned by other people.

**What to do about the seam:**

- **Write the contract down** — the exact set of names, URLs, regions and gateway identifiers that cross the boundary. That list is the API between the two surfaces.
- **Generate rather than copy** where you can: have Terraform emit outputs that the manifest side consumes, so a rename propagates instead of drifting. (Cloud Run makes this worse than usual — see [[Cloud Run generates a per-project URL hash, breaking environment config parity]].)
- **Treat a change on either side as a change to both.** A PR that renames a Terraform resource should reference the manifest PR.

> [!tip] Environments: workspaces vs overlays
> Note the two surfaces even model *environments* differently — Terraform **workspaces** on one side, **overlays** on the other. That is fine, but it means "what is deployed to test?" has two different answers in two different shapes. Anyone debugging an environment needs to know both.

> [!warning] Remote state is shared mutable state
> A single GCS bucket and prefix backing all workspaces means concurrent `apply` runs contend. Confirm state locking is on before two people (or a pipeline and a person) ever run it at once.

Source: [[Luz Kubernetes Terraform]] (LUZ, Confluence).

## Related

- [[Cloud Run generates a per-project URL hash, breaking environment config parity]]

%% ai-graph-start %%

**Related notes:**
- [[Route user-edited business rules through git and CI instead of a database]]
- [[Cloud Run generates a per-project URL hash, breaking environment config parity]]
- [[Shift GKE traffic to Cloud Run with Istio weighted routing, not a cutover]]
- [[Luz Kubernetes Terraform]]
- [[Deployment with terraform]]

%% ai-graph-end %%