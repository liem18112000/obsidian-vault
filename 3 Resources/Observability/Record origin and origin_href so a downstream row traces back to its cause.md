---
title: "Record origin and origin_href so a downstream row traces back to its cause"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: OneAPI Architecture overview (LUZ)"
tags: [traceability, billing, debugging, microservices, data-modelling, confluence-distilled]
---

# Record origin and origin_href so a downstream row traces back to its cause

A downstream record — an SMS charge, a print job, a billing line — is useless for investigation if it only says *what* happened. Add two columns saying **who caused it** and **where to look**, and the trail crosses service and database boundaries without a distributed tracing system.

From an `sms_tracking` table in one service's database:

| Column | Example | Meaning |
|---|---|---|
| `origin` | `OneAPI` | which system produced this |
| `origin_href` | `epost/v2/deliveries/3/status` | the upstream resource that caused it |
| `tenant_id` | `6eace38d-…` | whose it is |
| `company_id` | `1` | |
| `cost_center` | `MARKETING` | billing attribution |
| `amount`, `billed` | `1`, `false` | |

Given one row, the investigation is mechanical: `origin` says it came via OneAPI (so `luz-eletter`), `origin_href` names delivery `3`, `tenant_id` names the schema — go to that tenant's schema in the `luz-eletter` database, look up the delivery and its documents. No guessing which service emitted the charge, no correlating by timestamp.

**Why `origin` and `origin_href` are two columns, not one string:**

- **`origin` is enumerable.** You can group by it — "how many SMS did OneAPI cause this month versus the scan centre?" — which is the billing and capacity question.
- **`origin_href` is addressable.** A path, not prose. Resolvable by a human and, if you want, by a tool.

> [!tip] This is cheap traceability that survives everything
> Distributed tracing gives richer causality but is sampled, short-retention, and gone by the time a billing dispute arrives months later. Two varchar columns on the record itself are permanent, survive service rewrites, and answer the question that actually gets asked: *"why were we charged for this?"* Add them to any record created **as a consequence** of something in another system.

> [!warning] Make the href stable, and never make it a secret
> `origin_href` is only useful if the referenced resource is still addressable later — so use durable ids, not ephemeral ones, and keep the shape versioned (`epost/v2/...`) so old rows remain interpretable after the API changes. And since these rows land in billing exports and support tickets, the href must not embed credentials or tokens.

Related: [[Client-assigned idempotency keys with a unique constraint beat distributed locks]] — the other case where an id assigned upstream makes downstream reasoning possible.

Source: [[OneAPI Architecture overview]] (LUZ, Confluence).

## Related

- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]
