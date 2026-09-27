---
ai_hash: d5c2c8dea941a2f6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: How to implement an audit log using Hibernate Envers (LUZFIN)'
status: seedling
tags:
- hibernate
- envers
- audit-logging
- jpa
- java
- confluence-distilled
title: Hibernate Envers generates audit tables from an annotation
type: howto
---

# Hibernate Envers generates audit tables from an annotation

You rarely need to hand-write audit logging for JPA entities. **Hibernate Envers** generates versioned audit tables from an annotation, capturing a full snapshot of each entity at every revision.

**Setup.** Envers ships as a separate artifact:

```xml
<dependency>
  <groupId>org.hibernate</groupId>
  <artifactId>hibernate-envers</artifactId>
  <version>${hibernate.version}</version>
</dependency>
```

> [!note] On Wildfly it may already be there
> Wildfly 10 bundles `hibernate-envers-5.0.7.Final.jar` under `{wildfly}/modules/system/layers/base/org/hibernate/main`. Adding a second copy at a different version is a classic source of `NoSuchMethodError` at runtime — check the container before you add the dependency.

**Marking what to audit.** `@Audited` goes on the `@Entity` for the whole thing, or on individual columns for specific properties:

```java
@Entity
@Table(name = "booking_header")
@Audited
@AuditOverride(forClass = BaseEntity.class, isAudited = true)
public class BookingHeaderEntity extends BaseEntity {

    @OneToMany(mappedBy = "bookingHeader", fetch = FetchType.LAZY,
               cascade = { PERSIST, REMOVE, MERGE })
    @OrderBy("seq ASC")
    @AuditMappedBy(mappedBy = "bookingHeader")
    private Set<BookingDetailEntity> bookingDetails;
}
```

Three annotations worth knowing individually:

- **`@Audited`** — audit this entity (or property).
- **`@AuditOverride(forClass = BaseEntity.class, isAudited = true)`** — inherited fields from a mapped superclass are *not* audited by default. If your `BaseEntity` holds `createDate`/`updateBy`, you must opt them in explicitly or they vanish from the audit trail.
- **`@AuditMappedBy`** — tells Envers the owning side of a bidirectional collection so it can reconstruct the relationship per revision. Without it, collection changes are recorded less usefully.

**What gets created.** Two kinds of table:

- **`revinfo`** — one row per revision: a `rev` (bigserial) and `revtstmp` (epoch millis). This is the global revision clock; every audited change in a transaction shares one `rev`.
- **`<table>_aud`** — one row per entity *per revision*, holding the entity snapshot plus `rev` and a revision-type column (add / modify / delete).

> [!warning] It is snapshot-per-revision, not a diff
> Every audited change writes a **full copy** of the row. On a wide, frequently-updated table the audit table outgrows the live table quickly. Plan retention and archival up front, and consider auditing selected columns rather than the whole entity where the history of one field is all anyone will query.

The transferable point: audit trails built by hand drift out of sync with the schema the moment someone adds a column. Generating them from the mapping means the trail cannot silently miss a field — the cost is storage, which is the cheaper problem.

Source: [[How to implement an audit log using Hibernate Envers]] (LUZFIN, Confluence).

%% ai-graph-start %%

**Related notes:**
- [[How to implement an audit log using Hibernate Envers]]
- [[Persistence layer implementation]]
- [[Hibernate Envers on luz_store SubscriptionEntity is field-scoped and omits price_plan]]
- [[How to resolve hibernate N+1 select's problem]]

%% ai-graph-end %%