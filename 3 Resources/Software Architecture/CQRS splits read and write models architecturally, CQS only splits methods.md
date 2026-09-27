---
title: "CQRS splits read and write models architecturally, CQS only splits methods"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Command Query Responsibility Segregation (CQRS) (2025-12-05)"
tags: [cqrs, architecture, design-patterns, eventual-consistency, backend]
---

# CQRS splits read and write models architecturally, CQS only splits methods

**CQRS** (Greg Young) separates the read model from the write model at the **architecture** level. It descends from Bertrand Meyer's **CQS**, which is a *method*-level rule — a method either changes state or returns data, never both. Conflating the two is the most common misreading: CQS is a coding discipline you can adopt in an afternoon; CQRS is a structural split with real operational cost.

The split:

- **Commands** are imperative (`CreateOrder`, `UpdateCustomer`). They express an *intention to change state*, are validated before execution, may raise domain events, and return only success/failure — **not data**.
- **Queries** are interrogative (`GetOrder`, `ListProducts`). They never mutate, and — the actual payoff — they are free to read from **denormalized, pre-computed views** shaped for how they are read rather than how the data is written.

What you buy: read and write paths **scale independently** (read replicas for a read-heavy system, a tuned command path for a write-heavy one), and the query side stops paying the normalisation tax that exists to protect writes.

What you pay, and the reason not to reach for it by default: the read model is **eventually consistent** with the write model, so the UI can show a user their own change as not-yet-applied. Every CQRS system eventually grows workarounds for that. Adopt it when read and write loads genuinely differ in shape or scale — not because separation sounds cleaner.

Worth noting the LUZ read path applies the same instinct without the full pattern: materialising fields onto documents so reads avoid a join is a denormalized read model in miniature.

## Related

- [[A pagination token is an opaque cursor, and it must carry the filter it was issued under]]
