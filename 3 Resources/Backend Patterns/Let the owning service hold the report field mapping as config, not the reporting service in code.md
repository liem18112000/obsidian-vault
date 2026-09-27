---
title: "Let the owning service hold the report field mapping as config, not the reporting service in code"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Handle report configuration for new fields (LUZ)"
tags: [configuration, reporting, coupling, microservices, schema, confluence-distilled]
---

# Let the owning service hold the report field mapping as config, not the reporting service in code

When service A reports on data owned by service B, the mapping between *report field* and *entity attribute* has to live somewhere. Putting it in A's **Java classes** means every new field in B requires a code change, a build, and a release of A — for data A does not own.

The problem statement, verbatim in shape:

> *"When a team adds a new table or fields to a service which is part of reporting, these fields should be considered automatically."*
> 1. `entityAttribute` and `entityName` are **hardcoded in Java classes** of the report service.
> 2. In the owning service, the query-table flow has to be **repeated** for each case.

So one field addition costs two code changes in two services, and the report service accumulates knowledge of every reportable entity in the estate — the same linear-cost coupling as [[Deleting a shared reference entity prefer the design whose cost stays constant per consumer|Deleting a shared reference entity: prefer the design whose cost stays constant per consumer]].

**The fix: make the mapping data, and let the owning service hold it.** The report service sends a generic predicate list; the owning service resolves it against its own config:

```json
{
  "offset": 10,
  "limit": 10,
  "predicates": [
    { "reportAttributeName": "additionalAddress",
      "entityAttributeName": "additionalAddress" }
  ]
}
```

Two names, deliberately separate: `reportAttributeName` is the stable public label, `entityAttributeName` is the internal column. Keeping them distinct is what lets the owning service rename a column without breaking every saved report.

**What moving the config to the owning service buys:**

- **Adding a field is a config edit, in one repo** — the one that added the field. No cross-team ticket.
- **The report service stops knowing about entities.** It ships predicates and renders results; it holds no entity list to grow stale.
- **The mapping lives next to the schema it describes**, so the person adding the column is the person updating the mapping, in the same change.

> [!warning] Config-driven means the contract moves from the compiler to runtime
> A hardcoded mapping fails at build time when a field is renamed. A config file fails at **query time**, for one user, in production. Validate the config against the actual schema on startup — or in CI — or you have traded a loud, early failure for a quiet, late one.

> [!tip] Watch the security boundary too
> "Any field is reportable by config" makes it one config line away from exposing a column that should never leave the service. An allow-list of reportable attributes is the config; a pass-through of arbitrary `entityAttributeName` values is an injection surface.

Source: [[Handle report configuration for new fields (12.06.2023)]] (LUZ, Confluence).

## Related

- [[Deleting a shared reference entity prefer the design whose cost stays constant per consumer]]
