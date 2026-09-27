---
title: "Deleting a shared reference entity: prefer the design whose cost stays constant per consumer"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Research on Delete Access class (TP2020)"
tags: [coupling, pubsub, soft-delete, microservices, event-driven, confluence-distilled]
---

# Deleting a shared reference entity: prefer the design whose cost stays constant per consumer

When one service owns a reference entity that many others point at — a security class, a category, a tenant-level code — "delete" is not a local operation. Deleting it in the owner leaves dangling references everywhere else, and the failure surfaces far from the cause.

The concrete symptom: a security class is deleted centrally; documents elsewhere still carry that class; a later request to resolve its details (who is assigned?) returns **not found**, breaking a page nobody touched.

Two workable designs, and the criterion that separates them:

**Option A — the owner publishes a deletion event; each consumer subscribes and reacts** (typically a soft delete or a background cleanup).

**Option B — the owner checks for existing usage before allowing the delete**, refusing until consumers are clean.

Both prevent the dangling reference. The deciding question is **what happens when a new consumer appears**:

- Under **A**, nothing. A new module subscribes to the existing topic; the owner is untouched.
- Under **B**, the owner must be modified — a new usage check for every new consumer. The central service accumulates knowledge of every downstream module, which is precisely the coupling the central service existed to avoid. The source note lists exactly this: a check must be added when `folder class`, then `luz_accounting`, then the next thing starts using it.

> [!tip] The generalisable rule
> Prefer the design whose cost is **constant** as consumers multiply over the one whose cost is **linear**. "Does adding the Nth consumer require editing the owner?" is usually a faster discriminator than weighing the individual pros and cons.

**Two options that were considered and correctly rejected:**

- **Validate on the UI and hide not-found classes.** Pushes the check to every screen forever — every future UI must remember. A correctness rule enforced by convention across N screens is a rule that will be broken.
- **Strip the class from documents before deleting.** Still leaves other systems unaware of the deletion; it fixes the symptom in one consumer while the root problem stands.

> [!warning] Option A trades a hard error for a soft window
> Event-driven cleanup is **eventually** consistent: between publish and consume, references are live-but-doomed. Consumers must tolerate resolving a deleted id gracefully — soft delete rather than hard, and a "deleted" render path rather than an exception. If your domain cannot tolerate that window (financial, legal, audit), B's synchronous refusal is the right trade despite the coupling.

Note the audit dimension the same research raises: deletions need **history tying each action to a person**, retention for compliance, and least-privilege access. A soft delete preserves that record; a hard delete destroys the evidence an auditor will ask for.

Source: [[Research on Delete Access class]] (TP2020, Confluence).

## Related

- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]
