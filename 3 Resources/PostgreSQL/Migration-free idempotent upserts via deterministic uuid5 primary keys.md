---
title: "Migration-free idempotent upserts via deterministic uuid5 primary keys"
created: 2026-09-09
type: lesson
status: seedling
source: "session 2026-09-09 SCRUM-94"
tags: [postgresql, idempotency, upsert, uuid5, leo-cdp]
---

# Migration-free idempotent upserts via deterministic uuid5 primary keys

When a Postgres table has only a surrogate UUID primary key and **no natural unique constraint**, you can still make writes idempotent without adding a migration: derive the primary key deterministically instead of letting the DB generate it.

## The technique

Compute the PK as a `uuid5` of a fixed namespace plus the row's natural key, then upsert on the PK:

```python
CRM_SYNC_NAMESPACE = uuid.uuid5(uuid.NAMESPACE_URL, "leocdp.io/crm/segment-sync")

def _deterministic_id(*parts):
    return uuid.uuid5(CRM_SYNC_NAMESPACE, "|".join(str(p) for p in parts))

# lead_id = _deterministic_id(tenant_id, "lead", master_profile_id)
# INSERT ... VALUES (:id, ...) ON CONFLICT (lead_id) DO UPDATE SET ...
```

Re-running the same logical operation regenerates the **same** id, so `ON CONFLICT (pk) DO UPDATE` converges on the existing row instead of inserting a duplicate. Idempotency becomes *structural* (a property of the key) rather than transactional (needing locks or "check-then-insert").

## Why it beats the alternatives

- No schema migration to add a unique constraint on a log/fact table that legitimately allows many rows per entity.
- No SELECT-then-INSERT race window.
- Namespace the key with `tenant_id` so multi-tenant rows never collide across tenants.

## Variant: reuse an existing partial unique index

If the table already has a natural unique index, target `ON CONFLICT` on those columns instead of a synthetic PK. Example from `crm_transactions`:

```sql
ON CONFLICT (tenant_id, source_system, source_transaction_id)
  WHERE source_transaction_id IS NOT NULL
DO UPDATE SET amount = EXCLUDED.amount, ...
```

(The partial-index predicate must be repeated in the `ON CONFLICT` clause for Postgres to match it, and every synced row must set a non-null `source_transaction_id`.)

## Discovered in

LEO CDP `customer360-api` SCRUM-94 segment->CRM sync engine (`core/crud/crm_sync.py`): routes segment members into `crm_lead`/`crm_contact`/`crm_customer_contacts`/`crm_transactions`, and re-running the same segment must not duplicate target facts.

## Related

- [[PostgreSQL]]
