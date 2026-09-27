---
ai_hash: eeb1496d7eab9bbb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Rethink eArchive access right concept (TP2020)'
status: seedling
tags:
- access-control
- data-modelling
- inheritance
- permissions
- confluence-distilled
title: 'Folder access rights: stored on the folder or derived from its contents'
type: concept
---

# Folder access rights: stored on the folder or derived from its contents

Does a folder's access level live **on the folder**, or is it **derived from what is inside it**? The choice looks like a modelling detail and decides your read performance, your write semantics, and what "public" even means.

The two approaches, compared question by question:

| Question | **Direct** — classes on the folder | **Indirect** — derived from contents |
|---|---|---|
| How do you know a folder's access right? | Read it from the folder's metadata | **Compute recursively** from the security classes of every document inside |
| What is a public document? | No class in its own metadata **or inherited from containing folders** | No class in its own metadata |
| What is a public folder? | No class in its metadata | Determined by its contents |

Note the asymmetry in row 2: under **direct**, documents **inherit** from the folders above them, so a document's effective access is metadata + ancestry. Under **indirect**, a document stands alone and the folder aggregates upward. Inheritance flows down in one model and up in the other.

**Direct — stored, explicit.**
- Reads are O(1): the answer is a field.
- Writes are the cost: re-classifying a folder must propagate to everything beneath it, or the inherited answer goes stale.
- "Public folder" is meaningful on its own, even when empty.

**Indirect — derived, always consistent.**
- Nothing can go stale, because nothing is stored — the answer is recomputed.
- Reads are the cost: answering "can this user see this folder?" means walking the subtree. Listing a hundred folders means a hundred subtree walks, which is where this design meets [[N+1 hides at the service-call layer too, not just in the ORM]].
- An **empty folder** has no defined access level, which is a genuine gap: you cannot secure a folder before putting anything in it.

> [!tip] Pick by which operation is hot, then mitigate the other
> Access checks run on **every read** and folder re-classification is rare — which usually argues for **direct**, with propagation handled as a background job. If you choose indirect for correctness, plan to materialise the computed value anyway; at that point you have direct storage with derived semantics, and you must handle invalidation explicitly.

> [!warning] Write down what "public" means before choosing
> The table shows the two models disagreeing about the *definition*, not just the implementation. A document with no class inside a classified folder is **non-public** under direct and **public** under indirect. That is a security-relevant divergence, and the safe default — inherit restrictions downward — is the direct model's.

Related: [[Per-tenant encryption keys make GDPR deletion a key destruction]] · [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]].

Source: [[Rethink eArchive access right concept]] (TP2020, Confluence).

## Related

- [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]]

%% ai-graph-start %%

**Related notes:**
- [[Rethink eArchive access right concept]]
- [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]]

%% ai-graph-end %%