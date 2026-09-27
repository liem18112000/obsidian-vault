---
title: "Creating and Verifying _shard Indexes in MongoDB"
created: 2026-06-23
updated: 2026-06-23
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49526145028/Creating+and+Verifying+_shard+Indexes+in+MongoDB
confluence_id: "49526145028"
confluence_path: "Team Kepler > Developer note > Count Fan-out (K) Benchmark on Performance Env"
tags: [confluence, mongodb, performance]
---

# Creating and Verifying _shard Indexes in MongoDB

*Confluence source · Team Kepler › Developer note › Count Fan-out (K) Benchmark on Performance Env · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49526145028/Creating+and+Verifying+_shard+Indexes+in+MongoDB) · updated 2026-06-23*

## Overview

Backs the count fan-out: each K-th sub-count is `<materialize filter> AND _shard ∈ [lo, hi)`. `_shard` is the **last** key so the equality-prefix narrows first, then the range seeks its slice (ESR). Without these, a fanned count does K full scans instead of K index range-seeks.

- **DB**: the tenant database. **Collection**: `documents`.

- Build all three. `idx_shard` covers the no-filter count; the two compounds cover the `_isPublic` and `_effectiveSecurityClassCodes` count paths.

## Indexes

|  |  |
|----|----|
| Name | Key |
| `idx_shard` | `{ _shard: 1 }` |
| `idx_isPublic_shard` | `{ _isPublic: 1, _shard: 1 }` |
| `idx_effectiveSecurityClassCodes_shard` | `{ _effectiveSecurityClassCodes: 1, _shard: 1 }` |

## Create (mongosh)

```
use <tenantDb>;            // tenant database
db = db.getSiblingDB("<tenantDb>");

db.documents.createIndex(
  { _shard: 1 },
  { name: "idx_shard", background: true });

db.documents.createIndex(
  { _isPublic: 1, _shard: 1 },
  { name: "idx_isPublic_shard", background: true,
    partialFilterExpression: { _isPublic: true } });

db.documents.createIndex(
  { _effectiveSecurityClassCodes: 1, _shard: 1 },
  { name: "idx_effectiveSecurityClassCodes_shard", background: true });
```

## Verify

```
db.documents.getIndexes();   // expect the 3 names above
```
