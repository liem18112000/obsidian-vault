---
ai_hash: 54ae153d1dde3c46
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: KLARA Documents Concept Solution Design (LUZ)'
status: seedling
tags:
- multi-tenancy
- domain-modelling
- privacy-by-design
- gdpr
- architecture
- confluence-distilled
title: Draw the tenant boundary at legal data ownership, and make it reassignable
type: lesson
---

# Draw the tenant boundary at legal data ownership, and make it reassignable

The tenant boundary in a multi-tenant system should be drawn where **legal ownership of the data** sits — not where the org chart, the UI, or the billing plan suggests. Get this wrong and no amount of access-control work fixes it, because the wrong thing owns the data.

Worked through for a document archive:

- **A business is the legal proprietor of documents sent to it** → each business is its own tenant, with **one physically separated repository** per tenant.
- **Individuals** can own data → an individual is also a tenant.
- **Households are the hard case.** A family is generally the legal proprietor of documents addressed to more than one member (taxes, broadcasting fees). But a document addressed to *one* member may be owned by that individual alone, by some members, or by all.

The design resolves the household ambiguity by **not** trying to encode family law in the schema. Instead it gives the box owner the controls:

- A tenant can create as many **letter boxes** as needed; by default one per household.
- A letter box is identified by a housing address plus **one or several legal entities** — so a consortium or a shared flat is representable, and several boxes can exist at one address.
- The letter-box administrator creates users, appoints further administrators, and defines **logical access rules** per action: access, dispatch, process, duplicate.

> [!important] Ownership must be reassignable, because life events happen
> Divorce, a child reaching legal majority, a member moving out — each can transfer proprietorship of **both archived documents and future incoming mail** to a specific person. If your model treats tenant-ownership as immutable, every one of these becomes a manual data-surgery ticket. Design the transfer path before launch.

**Two supporting principles from the same design:**

- **Privacy by design, via separation first.** Physical separation — or logical separation where the infrastructure guarantees it — is the primary control. **Encryption is complementary**, addressing the hacking risk specifically, and it is not free: encrypting document *indexes* forces either physical separation anyway or symmetric encrypt/decrypt on every access, with real CPU cost. Separation is the cheap guarantee; encryption is the expensive top-up.
- **Sharing outside the boundary means duplication, and you should expect it.** Inside a household or business, sharing can be logical and temporary. Outside, documents are duplicated in practice — and even when a DMS offers a secure link, recipients copy the file anyway, both to file it in their own structure and **to be safe if the link is revoked**. Plan for copies to exist rather than assuming the link is the only access path.

Related: [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]] — once the boundary is right, this is the capacity question it creates.

Source: [[KLARA Documents Concept - Solution Design]] (LUZ, Confluence).

## Related

- [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]]

%% ai-graph-start %%

**Related notes:**
- [[KLARA Documents Concept - Solution Design]]
- [[Container-per-customer silo multi-tenancy trades cost for structural isolation]]
- [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]]
- [[Postgres Architecture Blueprint V2023]]
- [[Per-tenant encryption keys make GDPR deletion a key destruction]]

%% ai-graph-end %%