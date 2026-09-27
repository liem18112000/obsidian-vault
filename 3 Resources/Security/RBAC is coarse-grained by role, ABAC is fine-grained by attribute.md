---
title: "RBAC is coarse-grained by role, ABAC is fine-grained by attribute"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Authentication & Authorization Architecture (FUT)"
tags: [authorization, rbac, abac, access-control, security, confluence-distilled]
---

# RBAC is coarse-grained by role, ABAC is fine-grained by attribute

**RBAC** grants permissions to *roles*; every user holding a role gets identical access. **ABAC** evaluates *attributes* at request time, so two users with the same role can get different answers.

**RBAC — coarse-grained.** A user is assigned a role (`manager`, `employee`, `auditor`) and the role carries a broad permission set: *"all managers can view all employee records."* Simple to reason about, simple to audit, and the permission set is knowable without seeing a request.

**ABAC — fine-grained.** A policy combines attributes with boolean logic across four categories:

- **User** — `role: manager`, `department: finance`, `clearance: top_secret`
- **Resource** — `file_type: document`, `sensitivity: confidential`, `owner: john.doe`
- **Action** — `read`, `write`, `delete`
- **Environment** — `time_of_day: 9am-5pm`, `network: corporate_vpn`, `device: mobile`

> A user can **read** a **confidential** document if their **department** is **finance** and the **time of day** is between **9 AM and 5 PM**.

Note what ABAC can express that RBAC structurally cannot: *ownership* (`owner == user`), *context* (on-VPN, during business hours), and *resource sensitivity*. In RBAC each of those becomes a new role — `finance-manager-confidential-daytime` — which is how role explosion starts.

**Choosing:**

- Reach for **RBAC** when access follows org structure, the resource set is uniform, and you need to answer "what can this person do?" from a table.
- Reach for **ABAC** when decisions depend on the *specific resource* or the *circumstances* of the request.
- Most real systems are **both**: RBAC for the coarse cut, ABAC for the rules RBAC would turn into a combinatorial mess. Role is then just one more user attribute.

> [!warning] ABAC's cost is that permissions stop being enumerable
> With RBAC you can answer "who can read this document?" with a query. With ABAC the honest answer is "run the policy against every user and every context", because the decision depends on request-time facts. That hurts access reviews, compliance reporting, and debugging *"why was I denied?"* — budget for a decision-log and a policy simulator, not just the engine.

Related: [[Authorization has four named parts PAP, PDP, PEP, PIP]] — the architecture that evaluates whichever model you pick.

Source: [[Authentication & Authorization Architecture]] (FUT, Confluence).

## Related

- [[Authorization has four named parts PAP, PDP, PEP, PIP]]
