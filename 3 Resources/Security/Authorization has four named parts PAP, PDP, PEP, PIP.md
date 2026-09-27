---
ai_hash: 060e75abf3065679
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Authentication & Authorization Architecture (FUT)'
status: seedling
tags:
- authorization
- xacml
- opa
- rbac
- abac
- architecture
- confluence-distilled
title: 'Authorization has four named parts: PAP, PDP, PEP, PIP'
type: concept
---

# Authorization has four named parts: PAP, PDP, PEP, PIP

An authorization system has four roles, and naming them separately is what lets you change one without rewriting the others. The vocabulary comes from the XACML model and is used well beyond it.

| Component | Role | Typically |
|---|---|---|
| **PAP** — Policy **Administration** Point | Where policies are authored, stored, versioned | Rego files in Git, an admin UI |
| **PDP** — Policy **Decision** Point | Evaluates a request against policy, answers `permit` / `deny` | A standalone service (e.g. OPA) |
| **PEP** — Policy **Enforcement** Point | Actually blocks or allows; sits in the request path | Middleware, a filter, an annotation |
| **PIP** — Policy **Information** Point | Supplies extra attributes the PDP needs to decide | User directory, DB, clock, device posture |

The flow: a request hits the **PEP**, which asks the **PDP**; the PDP reads policy from the **PAP** and pulls missing facts from the **PIP**; the decision comes back and the PEP enforces it.

**Why the separation earns its keep:**

- **Decision logic stops being scattered.** Without a PDP, authorization lives as `if` statements across every service, each subtly different, and answering "who can read this?" means grepping the codebase.
- **Policy becomes reviewable.** Policy in a human-readable language under version control gets code review, history, and rollback — which is exactly what you want for the rules governing access.
- **The PIP is where context enters.** Time of day, network location, device type — attributes the application does not own. Isolating that fetch keeps the PDP pure and testable.

> [!warning] The standalone PDP buys decoupling and costs latency
> Every authorized request now makes a network call. The usual mitigations are running the PDP as a sidecar or in-process library, and caching decisions — but caching authorization decisions has its own hazard: a revoked permission stays live until the entry expires. Decide the staleness you can tolerate before you decide the cache TTL.

> [!tip] You already have a PEP even if you have never used the word
> In a JAX-RS service, annotations like `@PermitAll`, `@PermissionAllowed`, or a custom `@AccessibleWithoutTenant` *are* the enforcement point. Recognising them as a PEP is what prompts the useful question: where is the decision actually being made, and could it move out of the annotation?

Related: [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]] — the models a PDP evaluates.

Source: [[Authentication & Authorization Architecture]] (FUT, Confluence).

## Related

- [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]]

%% ai-graph-start %%

**Related notes:**
- [[Authentication & Authorization Architecture]]
- [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]]
- [[Resource-level consent grants specific instances, not just scopes]]

%% ai-graph-end %%