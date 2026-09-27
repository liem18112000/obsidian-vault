---
ai_hash: 369054dbb1ecc200
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Testing statistics and remaining impediment after implementation
  (LUZ)'
status: seedling
tags:
- caching
- batch-processing
- external-api
- data-modelling
- confluence-distilled
title: Split a batch against a cache and forward only the misses, tracking the residual
type: lesson
---

# Split a batch against a cache and forward only the misses, tracking the residual

When a batch request is served by an expensive external provider, the batch should be **split against a cache first** and only the misses forwarded. The design becomes much clearer once the residual count is an explicit field rather than something inferred.

Two tables, from an address-normalisation service:

**`address_batch_request`** — one row per incoming batch (up to 1,000 addresses)

| Field | Meaning |
|---|---|
| `data` | the **encrypted** raw original addresses |
| `unnormalized_address_amount` | how many still need normalising *after* cache lookup — *"send 1,000, 900 already cached → this is 100"* |
| `result` | mapping from each raw address's uuid to the hash key of its normalised form: `uuid1:hashkey1;uuid2:hashkey2` |
| `status` | `WAIT_FOR_NORMALIZING` → `PROCESSING` → `FINISHED` / `ERROR` |
| `provider_running_batch` | pointer to the batch actually sent to the external provider |
| `normalized_data` | raw response from the provider |

**`address_cache`** — normalised results keyed by hash, shared across all requests and tenants.

**Why the explicit residual count earns its field.** `unnormalized_address_amount` is the difference between what the caller asked for and what you must pay for. It makes cache effectiveness observable per request, tells you when the batch is complete (it reaches zero), and — usefully — is the number to watch: if it stops falling, your cache is not warming.

**Why `result` stores a mapping rather than the values.** Each request keeps only `uuid → hash key` pointers into the shared cache. The same normalised address is stored once no matter how many requests reference it, and the per-request row stays small.

> [!tip] Separate the caller's batch from the provider's batch
> `provider_running_batch` points from the request to the *different* batch sent downstream. They are not the same shape — one caller's 1,000 addresses might become 100 outgoing, and several callers' misses could be coalesced into one provider call. Modelling them as separate entities is what makes that possible.

> [!warning] A shared cache keyed by content is a cross-tenant channel
> Any content-addressed cache shared across tenants leaks the fact that a value has been seen before — inferable from latency or from a cache-hit count. Here the raw input is encrypted at rest in `data`, which is right; if the addresses are personal data, also consider whether cache entries need a retention policy and whether hit/miss timing is observable to callers.

Related: [[Claim work across pods with an expiring lease column on the row]] — the status enum here wants the same treatment once multiple workers process batches.

Source: [[Testing statistics and remaining impediment after implementation]] (LUZ, Confluence).

## Related

- [[Claim work across pods with an expiring lease column on the row]]

%% ai-graph-start %%

**Related notes:**
- [[Testing statistics and remaining impediment after implementation]]
- [[Claim work across pods with an expiring lease column on the row]]
- [[Persist raw third-party results before mapping them to your domain shape]]
- [[N+1 hides at the service-call layer too, not just in the ORM]]
- [[Batch Processor Library - NodeJS]]

%% ai-graph-end %%