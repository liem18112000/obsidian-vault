---
title: "Borrow the Kubernetes resource shape for objects in a schemaless store"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Synapse ServerlessWorkflow Database analysis (FUT)"
tags: [kubernetes, data-modelling, redis, schema-evolution, conventions, confluence-distilled]
---

# Borrow the Kubernetes resource shape for objects in a schemaless store

You can borrow Kubernetes' **resource-oriented** object model without running on Kubernetes. Synapse, a serverless-workflow engine, stores its objects in Redis as documents shaped exactly like K8s manifests:

```json
{
  "apiVersion": "synapse.io/v1",
  "kind": "Workflow",
  "metadata": {
    "namespace": "default",
    "name": "order-processing",
    "labels":      { "app": "ecommerce" },
    "annotations": { "description": "Process customer orders" },
    "creationTimestamp": "2025-11-25T10:30:00Z"
  },
  "spec": { "versions": [ { "document": { "dsl": "1.0.0-alpha5", … } } ] }
}
```

**What adopting the convention gives you, essentially for free:**

- **`apiVersion` is schema evolution built in.** `synapse.io/v1` → `/v2` lets two shapes coexist, with the version declared on every stored object rather than inferred from a migration table.
- **`namespace` + `name` is a natural composite key** and a ready-made multi-tenancy boundary in a store that has no schemas.
- **`labels` are the query index; `annotations` are the payload.** Labels are for selecting ("everything belonging to `app: ecommerce`"), annotations for descriptive data nobody filters on. Keeping them in separate maps stops the searchable set from growing without bound.
- **`spec` versus status separation.** `spec` is desired state, declared by a user. Anything the system computes belongs elsewhere — which is what makes reconciliation loops possible.

**Why this fits a document store particularly well.** Redis (here via a Garnet-compatible API) has no schema to enforce shape, so the convention *is* the schema. Every object is self-describing: given one document you know its type, version, identity, and which fields are user intent.

> [!tip] The generalisable move
> When you need an object model in a schemaless store, adopting an existing well-worn convention beats inventing one. You inherit the naming, the evolution story, and — usefully — the fact that most engineers already know what `metadata.labels` means. Familiarity is a real design property.

> [!warning] The shape implies a reconciler; do not adopt it half-way
> `spec` as *desired* state is only meaningful if something converges actual state toward it. Copy the manifest shape onto a plain CRUD store and you get K8s-looking documents with none of the semantics — worse than a plain model, because readers will assume declarative behaviour that is not there. Adopt the vocabulary only if you are also adopting the control loop.

Source: [[Synapse - ServerlessWorkflow Database analysis|Synapse - ServerlessWorkflow  Database analysis]] (FUT, Confluence).
