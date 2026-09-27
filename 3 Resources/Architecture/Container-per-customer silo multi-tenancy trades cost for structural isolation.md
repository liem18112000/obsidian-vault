---
ai_hash: 61df6cb1c225865f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: AxonivyCloud Infrastructure Diagram EKS Proposal (AII)'
status: seedling
tags:
- multi-tenancy
- kubernetes
- eks
- isolation
- silo
- cost
- confluence-distilled
title: Container-per-customer silo multi-tenancy trades cost for structural isolation
type: concept
---

# Container-per-customer silo multi-tenancy trades cost for structural isolation

The simplest multi-tenancy model is not to share anything: **every customer gets their own application container and their own database container**, scheduled onto a shared cluster.

> Each customer will have an ivy container + a PostgreSQL container created in the EKS cluster. Depending on how many containers, the EKS cluster will **auto add/remove nodes**.

This is the **silo** model (as opposed to pooled, where one application instance serves all tenants behind a tenant id).

**What it buys:**

- **Isolation is physical, not logical.** No tenant-id filter can be forgotten, because there is no shared table to forget it in. The class of bug where one tenant sees another's data is structurally absent.
- **Blast radius is one customer.** A runaway query, a memory leak, a bad migration — all contained.
- **Per-customer versions become possible.** Customers can run different releases, which pooled architectures make very hard.
- **Deletion is trivial**: destroy the namespace.

**What it costs:**

- **Resources scale linearly with customers**, not with load. A thousand mostly-idle customers is a thousand idle container pairs, each with its own memory floor. Node autoscaling manages the *capacity*, not the *waste*.
- **Operational work scales with customers too** — upgrades, migrations, backups and certificate rotation are per-customer operations now. Automation is mandatory rather than nice to have (hence Ansible in this design, driving deployments, services, nginx config and sub-domain creation).
- **Per-customer databases mean per-customer schema drift** if a migration fails for some and not others.

> [!tip] Silo suits low-count, high-value tenants
> The model fits when customers are few, large, and pay for isolation — enterprise or regulated deployments. It stops making sense when tenants are many and small, where the fixed per-tenant footprint dominates everything. Ask what a tenant costs you **while idle**: if that number times your tenant count is uncomfortable, you want pooled.

> [!note] The surrounding network design is the standard shape
> One VPC, **two public and two private subnets**, with only a bastion host and NAT instance holding public IPs; workers sit in private subnets and reach the internet through NAT; application traffic is exposed via nginx. Nothing exotic — but note that SSH-via-bastion is itself a per-customer access path to audit when every customer has their own containers.

Related: [[Draw the tenant boundary at legal data ownership, and make it reassignable]] · [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]] — the pooled counterpart.

Source: [[AxonivyCloud - Infrastructure Diagram EKS Proposal]] (AII, Confluence).

## Related

- [[Draw the tenant boundary at legal data ownership, and make it reassignable]]

%% ai-graph-start %%

**Related notes:**
- [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]]
- [[Postgres Architecture Blueprint V2023]]
- [[AxonivyCloud - Infrastructure Diagram EKS Proposal]]
- [[Draw the tenant boundary at legal data ownership, and make it reassignable]]
- [[Embedding a search library means building its control plane yourself]]

%% ai-graph-end %%