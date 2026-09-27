---
title: "A nullable column with no constraint becomes an NPE in a downstream service"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Tenant token issue (TS)"
tags: [debugging, null-safety, data-integrity, sql, microservices, jwt, confluence-distilled]
---

# A nullable column with no constraint becomes an NPE in a downstream service

A nullable column with no enforcing constraint eventually becomes a `NullPointerException` — and it surfaces in whichever service *dereferences* it, not the one that *owns* it. The owning service looks healthy the whole time.

The chain, from a token-generation incident:

**1. The data.** Some `companyinfo` rows reference a `tenant` with no `type`. The anti-join finds them:

```sql
select c.*
from companyinfo c
left join tenant t on c.tenantid = t.tenantid
where t."type" is null
```

**2. The code.** The JWT generator uses `type` to name a claim, dereferencing it unconditionally:

```java
@Override
public TokenGenerator addCompanyTenant(CompanyTenant t) {
    this.claimSetBuilder.claim(t.getType().getName(), t);   // ← NPE when type is null
    this.claimSetBuilder.claim(TokenConstants.CLAIM_TENANT_ID, t.getTenantId());
    return this;
}
```

**3. The symptom.** `luztenant-service` returns `200` for `GET /company-info`; 43 ms later `jwt-service` throws `UnhandledException: EJBException: NullPointerException` on `POST /access/tokens`. The tenant service — which owns the bad data — reports no error at all. Anyone paging through *its* dashboards sees a healthy service.

> [!tip] `LEFT JOIN … WHERE right IS NULL` is the orphan-finder
> This anti-join is the reusable half. Whenever a nullable FK or an optional attribute is suspected, it enumerates exactly the rows that will break a dereference — and it is the query to run *before* shipping a change that starts using that column.

**Where the fix belongs — both places, and they are not alternatives:**

- **At the data layer.** If `type` is required, make it `NOT NULL` and backfill. A constraint fixes the class of bug; a null check fixes one instance of it. Until the constraint exists, new bad rows keep arriving.
- **At the boundary.** A service consuming another service's model should not assume optional fields are populated. Validate on ingest and fail with a message naming the tenant id, rather than dereferencing and throwing a bare NPE 114 frames deep.

> [!warning] A bare NPE names nothing
> The stack trace here identifies the *line*, not the *tenant*. Every minute of that incident went into finding which tenant was malformed — work the code could have done for free by checking and throwing with the id in the message. When a field is optional in the schema but required by your logic, say so in an error that names the offending record.

Related: [[Deleting a shared reference entity prefer the design whose cost stays constant per consumer]] — the same theme of one service's data decisions breaking another's runtime.

Source: [[Tenant token issue]] (TS, Confluence).

## Related

- [[Deleting a shared reference entity prefer the design whose cost stays constant per consumer]]
