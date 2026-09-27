---
title: "Technical Details - Pre-compute Security Class Code"
created: 2026-05-04
updated: 2026-05-19
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49387733004/Technical+Details+-+Pre-compute+Security+Class+Code
confluence_id: "49387733004"
confluence_path: "Team Kepler > Developer note > Pre-compute Security Class Code - Eliminate Lookup Query"
tags: [confluence, security]
---

# Technical Details - Pre-compute Security Class Code

*Confluence source · Team Kepler › Developer note › Pre-compute Security Class Code - Eliminate Lookup Query · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49387733004/Technical+Details+-+Pre-compute+Security+Class+Code) · updated 2026-05-19*

## Prerequisite

- Read the section [Pre-compute Security Class Code - Eliminate Lookup Query](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49357979662/Pre-compute+Security+Class+Code+-+Eliminate+Lookup+Query) to get context

- eArchive Issue [https://axonivy.atlassian.net/wiki/x/AwCLews](https://axonivy.atlassian.net/wiki/x/AwCLews)

## What

- Pre-computes per-document **effective security class** (union of own + inherited folder codes)

- Per-folder document counts so search queries skip the runtime folder join

|  |  |  |
|----|----|----|
| Property | Default | Purpose |
| `trigger.security.class.job` | `false` | Master switch for `RequestFilter` |
| `TRIGGER_SECURITY_CLASS_JOB_IN_MINUTES` | `60` | Throttle window between backfill triggers per tenant |
| `SECURITY_CLASS_JOB_TIMEOUT_IN_MINUTES` | `30` | Run-flag TTL |
| `SECURITY_CLASS_JOB_BATCH_SIZE` | `500` | Mongo seek-pagination batch size |
| `SECURITY_CLASS_JOB_PARALLELISM` | `1` | Worker count inside `MaterializeParallelRunner` |
| `MATERIALIZE_FACET_CACHE_TTL_SECONDS` | `30` | Facet-response cache TTL |
| `MATERIALIZE_FACET_CACHE_MAX_BYTES` | `100000` | Facet-response cache size cap |

![[materialize.png]]

## How

### Document Search/Count Gate

`MaterializeFacade.shouldUseMaterialized()` gates use of the materialized fields. Requires both:

- Backfill complete: `countUnmaterialized == 0` mean all document is migrated

![[image-20260504-161700.png]]

### Data Migration

#### Throttle / coordination

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p>Mechanism</p></th>
<th><p>Scope</p></th>
<th><p>Purpose</p></th>
</tr>
&#10;<tr>
<td><p>`tenantLocks`</p></td>
<td><p>per-tenant</p>
<p>per-pod</p></td>
<td><p>Atomicity of read-decide-write on the trigger-window stamp</p></td>
</tr>
<tr>
<td><p>`CACHE_KEY_BACKFILL_LAST_TRIGGERED`</p></td>
<td><p>per-tenant</p>
<p>cross-pod</p></td>
<td><p>Throttle window — at most one trigger per `TRIGGER_SECURITY_CLASS_JOB_IN_MINUTES`</p></td>
</tr>
<tr>
<td><p>`CACHE_KEY_BACKFILL_JOB`</p></td>
<td><p>per-tenant</p>
<p>cross-pod</p></td>
<td><p>Run-flag — second observer in window sees `isRunning` and bails. Crash auto-clears via TTL</p></td>
</tr>
<tr>
<td><p>`isCompleteLocks`</p></td>
<td><p>per-tenant</p>
<p>per-pod</p></td>
<td><p>Stampede guard — funnel cache-miss readers through one Mongo count</p></td>
</tr>
</tbody>
</table>

#### Drain loop guard

`drainUnmaterialised` re-checks `countUnmaterialized` after each pass. If `remaining` doesn't shrink between passes, persistent failures are present — log and abort instead of looping forever.

![[image-20260504-162048.png]]

### Folder-cascade

When folder-tree security changes (own or inherited codes), every doc under it has stale `_effectiveFolderSecurityClassCodes`. `MaterializeFacade.cascadeFolder()` re-materializes them.

Lock TTL = 30 min — bounds wait if pod dies mid-cascade.

![[image-20260504-162620.png]]

### Create new document - precompute apply

`apply()` is the inline path used when interact with a single document's metadata

- Compute security code on the fly

- Decorates the response without touching Mongo.

|  |  |
|----|----|
| New Field | Meaning |
| `_effectiveSecurityClassCodes` | Union of self-document `securityClassCode` + `securityClassCode` + `inheritedSecurityClassCode` across all folders the doc belongs to |
| `_isPublic` | `true` if any folder has no codes (open). Also acts as the "materialized" sentinel — absence ⇒ doc not yet processed |
| \_folderNames | List (can have duplicate folder name) of all folder name that directly contain the document |

![[image-20260505-005428.png]]

## What didn't change

- The **non-security predicate** (`_isBeingCreated`, `personal`, `_deletionStatus`, `letterInfo.mediaType`, `securityClassCodes`, …) is still applied as a second `$match` after the gate regardless of whether the OLD or NEW pipeline runs.

- Sort / skip / limit / projection stages are unchanged.

## What change

- **Stages dropped**: `$lookup folders`, `$unwind folderIds`, `$addFields effectiveSecurityClassCode`, `$addFields filteredFolders`, `$group`.

- **Stages added**: one `$match` on `_hasUnrestrictedFolder` / `_effectiveFolderSecurityClassCodes`

- **Write amplification**: each document / folder update now also runs the hook (cascade).

### Pipelines comparison

#### List page (root, paged)

OLD pipeline has 11 stages, runs `$lookup` over the folders collection and unwinds folder arrays in-memory.

> [!note]- Details Query
>
>
>
> ```
> [
>   { "$match": { "$and": [
>     { "$and": [
>       { "$or": [
>         { "isStored": true },
>         { "folderIds.0": { "$exists": true } },
>         { "$and": [
>           { "$or": [ { "folderIds": { "$exists": false } }, { "folderIds": { "$size": 0 } } ] },
>           { "origin": "User uploaded" }
>         ]}
>       ]},
>       { "letterInfo.mediaType": { "$nin": [
>         "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
>         "application/vnd.ch.klara.epost.smartletter.template.v1+json",
>         "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"
>       ] } }
>     ]},
>     { "$or": [
>       { "securityClassCodes": { "$exists": false } },
>       { "securityClassCodes": { "$size": 0 } },
>       { "securityClassCodes": null }
>     ]},
>     { "_isBeingCreated": { "$ne": true } },
>     { "$or": [ { "personal": { "$exists": false } }, { "personal": { "$ne": true } } ] },
>     { "$or": [ { "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" } ] }
>   ] } },
>   { "$lookup": { "from": "folders", "localField": "folderIds", "foreignField": "_id", "as": "_folders" } },
>   { "$unwind": { "path": "$_folders", "preserveNullAndEmptyArrays": true } },
>   { "$addFields": { "_folders.effectiveSecurityClassCode": { "$ifNull": [
>     "$_folders.securityClassCodes",
>     { "$ifNull": [ "$_folders.inheritedSecurityClassCodes", [] ] }
>   ] } } },
>   { "$match": { "$or": [
>     { "_folders.effectiveSecurityClassCode": { "$size": 0 } },
>     { "_folders.effectiveSecurityClassCode": { "$exists": false } }
>   ] } },
>   { "$addFields": { "filteredFolders": { "$cond": [ /* … */ ] } } },
>   { "$sort": { "_updatedDate": -1 } },
>   { "$skip": 0 },
>   { "$limit": 48 },
>   { "$project": { /* exclude history-entries / thumbnail blobs */ } }
> ]
> ```
>
>
>

NEW Pipeline has same intent, Filter eligibility flows through equality + `$or` on the two materialized fields.

- **No** `$lookup`**.**

- **No** `$unwind`**.**

- **No** `$addFields` **for synthetic security fields.**

> [!note]- Details Query
>
>
>
> ```
> [
>   { "$match": { "$or": [
>     { "_hasUnrestrictedFolder": true },
>     { "_effectiveFolderSecurityClassCodes": { "$size": 0 } }
>   ] } },
>   { "$match": { "$and": [
>     { "$or": [
>       { "securityClassCodes": { "$exists": false } },
>       { "securityClassCodes": { "$size": 0 } },
>       { "securityClassCodes": null }
>     ]},
>     { "_isBeingCreated": { "$ne": true } },
>     { "$or": [ { "personal": { "$exists": false } }, { "personal": { "$ne": true } } ] },
>     { "$or": [ { "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" } ] }
>   ] } },
>   { "$sort": { "_updatedDate": -1 } },
>   { "$skip": 0 },
>   { "$limit": 48 },
>   { "$project": { "documentTextContent": 0 } }
> ]
> ```
>
>
>

#### Per-folder count

OLD (`JsonStoreQuerySearchUtil.buildDocumentQueryFilterWithSecurityClass(countOnly=true)`):

> [!note]- Details Query
>
>
>
> ```
> [
>   { "$match": { /* user query incl. folderIds: <id> */ } },
>   { "$unwind": { "path": "$folderIds", "preserveNullAndEmptyArrays": true } },
>   { "$addFields": { "folderId": { "$toObjectId": "$folderIds" } } },
>   { "$lookup": { "from": "folders", "localField": "folderId", "foreignField": "_id", "as": "_folders" } },
>   { "$addFields": { /* effectiveSecurityClassCode concat */ } },
>   { "$match": { /* user codes ⊆ effective codes */ } },
>   { "$group": { "_id": "$_id" } },
>   { "$count": "totalRecordCount" }
> ]
> ```
>
>
>

NEW (`MaterializeQueryBuilder.buildDocumentQueryFilter(countOnly=true)` + caller-appended `$count`):

The folder predicate (`folderIds: <id>`) is applied directly on the documents collection because materialized fields make joins unnecessary.

> [!note]- Details Query
>
>
>
> ```
> [
>   { "$match": { "$and": [
>     { "$or": [ { "_hasUnrestrictedFolder": true }, { "_effectiveFolderSecurityClassCodes": { "$size": 0 } } ] },
>     { "_isBeingCreated": { "$ne": true } },
>     { "$or": [ { "personal": { "$exists": false } }, { "personal": { "$ne": true } } ] },
>     { "_deletionStatus": "false" },
>     { "folderIds": "<folder-id>" }
>   ] } },
>   { "$count": "totalRecordCount" }
> ]
> ```
>
>
>

## Open Points

### Count branch exhausts Mongo's 100 MB sort limit

- Without the totalOpen short-circuit, every paged search runs the `$facet` count branch for every request.

```
MongoCommandException: Command failed with error 292 (QueryExceededMemoryLimitNoDiskUseAllowed):
'Exceeded memory limit for $group, but didn't allow external sort. Pass allowDiskUse:true to opt in.'
```

- The fix needs to happen in `luz_jsonstore`

For now this workaround is applied: Total count read — no aggregate at all.

```
db.materialize_stats.find({ statKey: "totalOpen" }, /* limit */ 1).toArray()
// returns: [{ _id: ObjectId(...), statKey: "totalOpen", count: 128002 }]
```

- Get the `materialize_stats.totalOpen` into the search response as `totalRecordCount`

> [!note]- Code
>
>
>
> ```
> int totalCount = (int) Math.min(totalOpen.get(), Integer.MAX_VALUE);
> return Json.createObjectBuilder()
>         .add(Constants.SEARCH_RESULTS, results == null ? JsonValue.EMPTY_JSON_ARRAY : results)
>         .add(Constants.SEARCH_TOTAL_COUNT, totalCount)
>         .build();
> ```
>
>
>

- `materialize_stats.totalOpen` is calculated:

> [!note]- Code
>
>
>
> ```
> JsonObject filter = MaterializeQueryBuilder.buildOpenForNoCodesPredicate();
> JsonArray result  = jsonStore.countCollections(tenantId, DOCUMENT_COLLECTION, filter, token);
> long count = (result == null || result.isEmpty()) ? 0L
>            : result.getJsonObject(0).getInt(SEARCH_TOTAL_COUNT, 0);
> upsertTotalOpenCount(tenantId, token, count);
> ```
>
>
>

Pipeline that reaches MongoDB:

```
[
  { "$match": { "$or": [{ "_hasUnrestrictedFolder": true }, { "_effectiveFolderSecurityClassCodes": { "$size": 0 } } ] } },
  { "$count": "totalRecordCount" }
]
```

### Cache down

`luz-cache` is a soft dependency. Three call sites guard against it:

|  |  |
|----|----|
| Call site | Effect |
| `MaterializeBackfillJob.isComplete` | Read path serves immediately. Tenant flips to "materialized" mode and search uses the gate as if backfill finished. Risk: if backfill *hasn't* actually finished, gate-only reads under-count; but tenant-wide functionality is preserved. |
| `MaterializeCascadeService.acquireLock` | Cascade event is **dropped silently**. The doc set under the changed folder will not be re-materialised on this event. Backfill loop catches up later. |
| `MaterializeCascadeService.isLocked` | Caller sees "another cascade is running" and skips this one. |

Trade-off:

- Long cache outage means cascade events queued during that window are never processed; the periodic backfill is the only recovery.

- If `MATERIALIZE_BACKFILL_INTERVAL_MIN` is low (default 5 min), recovery is fast.

- On a tenant with no backfill schedule (custom config), data will stay stale until cache returns.

### Folder missing during compute

`MaterializeService.compute(...)` is called per document during `apply` / `materializeDocument`.

It loads the doc's folder list via `MaterializeRepository.loadFolders` and walks each:

> [!note]- Code
>
>
>
> ```
> for (var folderId : distinctIds) {
>     var folder = byId.get(folderId);
>     if (folder == null) continue;
>     ...
> }
> ```
>
>
>

How to reproduce:

- Folder was deleted between `MaterializeFacade.apply` getting the doc and `compute` running.

- `loadFolders` returned a partial map (e.g. `getCollectionsByFilter` hit a transient Mongo error and silently truncated).

- Doc holds a dangling `folderIds` reference (data integrity issue, pre-existing).

Effect on the doc's `EffectiveSecurity`:

- **The skipped folder**'s own + inherited codes are **omitted from the union** in `_effectiveFolderSecurityClassCodes`.

- **The skipped folder** cannot contribute `_hasUnrestrictedFolder=true`. If every other folder of the doc is restricted (or there are no other folders), the doc lands restricted that wouldn't have been if the missing folder were present.

Failure shape is silent.

- The doc is materialized with whatever folder data was visible.

- There's no `partial=true` flag on the document and no per-doc retry.

- The next cascade or scheduled backfill recomputes from scratch and (assuming the folder is back) corrects the value.

### Single-doc materialize fails

`MaterializeService.materializeDocument(...)` wraps the per-doc compute + write in `try / catch RuntimeException`:

> [!note]- Code
>
>
>
> ```
> try {
>     var folderIds = readFolderIds(document);
>     var effective = compute(tenantId, folderIds, token);
>     repository.updateEffectiveSecurity(tenantId, token, id, effective);
> } catch (RuntimeException e) {
>     LOGGER.log(Level.WARNING, "[materializeDocument] failed for tenant %s doc %s".formatted(tenantId, id), e);
> }
> ```
>
>
>

The batch loop does not stop on a per-doc failure. The doc is **left without **`_hasUnrestrictedFolder`** / **`_effectiveFolderSecurityClassCodes`** set**.

From the search path's point of view the doc is unmaterialized, so:

- The doc disappears from search results until materialize succeeds.

Recovery via `drainUnmaterialised`:

```
while (true) {
    int processed = repository.stream(tenantId, filter, docs -> materializer.materialize(tenantId, docs, token), token);
    if (batch == null || batch.isEmpty()) break;
    int remaining = repository.countUnmaterialized(tenantId, token);
    if (remaining == 0) { ...DONE...; break; }
    if (remaining >= prevRemaining) break;       // ← gives up rather than spinning
    prevRemaining = remaining;
}
```

The loop walks all unmaterialized docs, then re-counts. If the `remaining` number doesn't shrink between passes the loop concludes that the residue is "persistently failing" and aborts. Net behavior:

- Transient errors self-heal: next pass picks the doc up again, count shrinks, loop continues.

- Deterministic per-doc errors eventually trigger the `>=` check and the loop bails.

- The remaining docs stay invisible to search until the underlying data issue is fixed AND the next scheduled backfill runs.

- Loop never spins forever, but there's no escalation path . No metric, no alert, no cascade-retry.

## Need Improvement

Point to reconsider:

- Cascade job are using the resource for the recompute so it block other resource of different tenant. This has performance issue.

- We change the metadata of document so the integrity is lost.

- Scan to identify the truly unused data.
