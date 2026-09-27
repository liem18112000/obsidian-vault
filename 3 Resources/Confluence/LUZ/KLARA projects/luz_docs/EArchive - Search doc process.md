---
ai_hash: 4376801c32f25346
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49352802305'
confluence_path: LUZ Home > KLARA projects > luz_docs
created: 2026-04-22
entities: []
source: Confluence · LUZ - LUZ
status: reference
tags:
- confluence
- earchive
- luz-docs
- search
title: 'EArchive : Search doc process'
type: source
updated: 2026-04-24
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49352802305/EArchive+Search+doc+process
---

# EArchive : Search doc process

*Confluence source · LUZ Home › KLARA projects › luz_docs · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49352802305/EArchive+Search+doc+process) · updated 2026-04-24*

### 1. luz-docs-view-controller

> [!note]- luz-docs-view-controller
>
> API:
>
>
>
> ```
> /luz_docs_view_controller/api/v2/25b064c2-e2fd-4f72-aff8-0bfff9177ae1/letters/search?languageCode=en&language=en&excludes=_files.referenceTsq&excludes=_files.thumbnail512&excludes=_files.thumbnail256&excludes=uploadedHistoryEntry&excludes=storageHistoryEntries&excludes=printHistoryEntries&excludes=downloadHistoryEntries&excludes=exportHistoryEntries&excludes=deletingHistoryEntries&excludes=undoDeletingHistoryEntries&excludes=tagModificationHistoryEntries&excludes=securityClassModificationHistoryEntries&excludes=documentTypeModificationHistoryEntries&excludes=restoringHistoryEntries&value=&isStored=true&hasRootStorage=true&skip-security-classes=false&onlyList=false&from=0&size=48&sortField=_updatedDate&sortMode=DESC
> ```
>
>
>
> Param:
>
>
> <table>
> <tbody>
> <tr>
> <th><p>**No**</p></th>
> <th><p>**Group Param**</p></th>
> <th><p>**Param Name**</p></th>
> <th><p>**Exact Value from URL**</p></th>
> </tr>
> &#10;<tr>
> <td><p>1</p></td>
> <td><p>**Path Parameter**</p></td>
> <td><p>*(tenantId)*</p></td>
> <td><p>`25b064c2-e2fd-4f72-aff8-0bfff9177ae1`</p></td>
> </tr>
> <tr>
> <td><p>2</p></td>
> <td rowspan="4"><p>**Pagination & Sorting**</p></td>
> <td><p>`from`</p></td>
> <td><p>`0`</p></td>
> </tr>
> <tr>
> <td><p>3</p></td>
> <td><p>`size`</p></td>
> <td><p>`48`</p></td>
> </tr>
> <tr>
> <td><p>4</p></td>
> <td><p>`sortField`</p></td>
> <td><p>`_updatedDate`</p></td>
> </tr>
> <tr>
> <td><p>5</p></td>
> <td><p>`sortMode`</p></td>
> <td><p>`DESC`</p></td>
> </tr>
> <tr>
> <td><p>6</p></td>
> <td><p>**Search Value & Keyword**</p></td>
> <td><p>`value`</p></td>
> <td><p>*(empty string)*</p></td>
> </tr>
> <tr>
> <td><p>7</p></td>
> <td rowspan="2"><p>**Storage & Lifecycle**</p></td>
> <td><p>`isStored`</p></td>
> <td><p>`true`</p></td>
> </tr>
> <tr>
> <td><p>8</p></td>
> <td><p>`hasRootStorage`</p></td>
> <td><p>`true`</p></td>
> </tr>
> <tr>
> <td><p>9</p></td>
> <td rowspan="18"><p>**Response Shaping**</p></td>
> <td><p>`skip-security-classes`</p></td>
> <td><p>`false`</p></td>
> </tr>
> <tr>
> <td><p>10</p></td>
> <td><p>`onlyList`</p></td>
> <td><p>`false`</p></td>
> </tr>
> <tr>
> <td><p>11</p></td>
> <td><p>`languageCode`</p></td>
> <td><p>**en**</p></td>
> </tr>
> <tr>
> <td><p>12</p></td>
> <td><p>`language`</p></td>
> <td><p>**en**</p></td>
> </tr>
> <tr>
> <td><p>13</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`_files.referenceTsq`</p></td>
> </tr>
> <tr>
> <td><p>14</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`_files.thumbnail512`</p></td>
> </tr>
> <tr>
> <td><p>15</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`_files.thumbnail256`</p></td>
> </tr>
> <tr>
> <td><p>16</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`uploadedHistoryEntry`</p></td>
> </tr>
> <tr>
> <td><p>17</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`storageHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>18</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`printHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>19</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`downloadHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>20</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`exportHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>21</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`deletingHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>22</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`undoDeletingHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>23</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`tagModificationHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>24</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`securityClassModificationHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>25</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`documentTypeModificationHistoryEntries`</p></td>
> </tr>
> <tr>
> <td><p>26</p></td>
> <td><p>`excludes`</p></td>
> <td><p>`restoringHistoryEntries`</p></td>
> </tr>
> </tbody>
> </table>
>
>

### 2. luz-docs

> [!note]- luz-docs
>
> Json Object Query
>
>
>
> ```
> {
>   "query": {
>     "and": [
>       {
>         "or": [
>           {
>             "term": {
>               "isStored": true
>             }
>           },
>           {
>             "exists": {
>               "field": "folderIds",
>               "type": "array"
>             }
>           },
>           {
>             "and": [
>               {
>                 "not": {
>                   "exists": {
>                     "field": "folderIds",
>                     "type": "array"
>                   }
>                 }
>               },
>               {
>                 "term": {
>                   "origin": "User uploaded"
>                 }
>               }
>             ]
>           }
>         ]
>       },
>       {
>         "not": {
>           "terms": {
>             "letterInfo.mediaType": [
>               "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
>               "application/vnd.ch.klara.epost.smartletter.template.v1+json",
>               "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"
>             ]
>           }
>         }
>       }
>     ]
>   },
>   "excludes": [
>     "_files.referenceTsq",
>     "_files.thumbnail512",
>     "_files.thumbnail256",
>     "storageHistoryEntries",
>     "printHistoryEntries",
>     "downloadHistoryEntries",
>     "exportHistoryEntries",
>     "undoDeletingHistoryEntries",
>     "tagModificationHistoryEntries",
>     "documentTypeModificationHistoryEntries",
>     "restoringHistoryEntries",
>     "uploadedHistoryEntry",
>     "deletingHistoryEntries",
>     "securityClassModificationHistoryEntries"
>   ],
>   "from": 0,
>   "size": 48,
>   "sort": {
>     "_updatedDate": "DESC"
>   }
> }
> ```
>
>
>
> Param
>
>
> <table>
> <tbody>
> <tr>
> <th><p>**No**</p></th>
> <th><p>**Group Param**</p></th>
> <th><p>**Param Name**</p></th>
> <th><p>**Exact Value from URL**</p></th>
> </tr>
> &#10;<tr>
> <td><p>1</p></td>
> <td><p>**Path Parameter**</p></td>
> <td><p>tenantId</p></td>
> <td><p>`25b064c2-e2fd-4f72-aff8-0bfff9177ae1`</p></td>
> </tr>
> <tr>
> <td><p>2</p></td>
> <td rowspan="4"><p>**Query Param**</p></td>
> <td><p>include-deleted-documents</p></td>
> <td><p>false</p></td>
> </tr>
> <tr>
> <td><p>3</p></td>
> <td><p>skip-security-classes</p></td>
> <td><p>false</p></td>
> </tr>
> <tr>
> <td><p>4</p></td>
> <td><p>include-folder-name</p></td>
> <td><p>true</p></td>
> </tr>
> <tr>
> <td><p>5</p></td>
> <td><p>exclude-total-count</p></td>
> <td><p>false</p></td>
> </tr>
> <tr>
> <td><p>6</p></td>
> <td><p>**Request Body**</p></td>
> <td><p>JsonObject</p></td>
> <td><p>*(Above data)*</p></td>
> </tr>
> </tbody>
> </table>
>
>

#### Result

> [!note]- Timeline of one POST /documents/search call
>
>
> |  |  |  |
> |----|----|----|
> | **Phase** | **Time** | **Note** |
> | luzsec/public-keys/active on main thread | ~1.0 s | JWT signature verification path — blocks |
> | DocumentResource processing (pre-Mongo) | ~0.6 s | payload logging, SearchRequest build |
> | ① Aggregate \#1 (count) | ~22.1 s | The big \$unwind+\$lookup+\$group+\$count pipeline |
> | ② Aggregate \#2 (list) | ~13.4 s | Same \$match re-executed + pagination + lookup |
> | Response serialisation | ~2.5 s | 88 KB JSON with 48 documents |
> | Total wall | ~40.1 s |  |
>
>

#### Change

> [!note]- Original query - Call 1: list (22 s)
>
> POST /luz_jsonstore/api/mdb/\<tenant\>/documents/aggregate (no collation param)
>
>
>
> ```
> [
>     {
>       "$match": {
>         "$and": [
>           {
>             "$and": [
>               {
>                 "$or": [
>                   { "isStored": true },
>                   { "folderIds.0": { "$exists": true } },
>                   {
>                     "$and": [
>                       {
>                         "$or": [
>                           { "folderIds": { "$exists": false } },
>                           { "folderIds": { "$size": 0 } }
>                         ]
>                       },
>                       { "origin": "User uploaded" }
>                     ]
>                   }
>                 ]
>               },
>               {
>                 "$and": [
>                   {
>                     "letterInfo.mediaType": {
>                       "$not": {
>                         "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.draft\\.v1\\+json$",
>                         "$options": "i"
>                       }
>                     }
>                   },
>                   {
>                     "letterInfo.mediaType": {
>                       "$not": {
>                         "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.template\\.v1\\+json$",
>                         "$options": "i"
>                       }
>                     }
>                   },
>                   {
>                     "letterInfo.mediaType": {
>                       "$not": {
>                         "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.receipt\\.v1\\+json$",
>                         "$options": "i"
>                       }
>                     }
>                   }
>                 ]
>               }
>             ]
>           },
>           {
>             "$or": [
>               { "securityClassCodes": { "$exists": false } },
>               { "securityClassCodes": { "$size": 0 } },
>               { "securityClassCodes": null }
>             ]
>           },
>           { "_isBeingCreated": { "$ne": true } },
>           {
>             "$or": [
>               { "personal": { "$exists": false } },
>               { "personal": { "$ne": true } }
>             ]
>           },
>           {
>             "$or": [
>               { "_deletionStatus": { "$exists": false } },
>               { "_deletionStatus": "false" }
>             ]
>           }
>         ]
>       }
>     },
>     {
>       "$lookup": {
>         "from": "folders",
>         "let": { "folderIds": "$folderIds" },
>         "pipeline": [
>           { "$match": { "$expr": { "$in": [ { "$toString": "$_id" }, "$$folderIds" ] } } },
>           {
>             "$project": {
>               "_id": { "$toString": "$_id" },
>               "name": "$name",
>               "securityClassCodes": {
>                 "$concatArrays": [
>                   { "$ifNull": [ "$securityClassCodes", [] ] },
>                   { "$ifNull": [ "$inheritedSecurityClassCodes", [] ] }
>                 ]
>               }
>             }
>           }
>         ],
>         "as": "_folders"
>       }
>     },
>     {
>       "$match": {
>         "$or": [
>           { "_folders.securityClassCodes": [] },
>           { "_folders.securityClassCodes": { "$exists": false } }
>         ]
>       }
>     },
>     {
>       "$addFields": {
>         "_folders": {
>           "$filter": {
>             "input": "$_folders",
>             "cond": {
>               "$eq": [
>                 { "$size": { "$ifNull": [ "$$this.securityClassCodes", [] ] } },
>                 0
>               ]
>             }
>           }
>         }
>       }
>     },
>     { "$sort":  { "_updatedDate": -1 } },
>     { "$skip":  0 },
>     { "$limit": 48 },
>     {
>       "$project": {
>         "_files.referenceTsq": 0,
>         "_files.thumbnail512": 0,
>         "_files.thumbnail256": 0,
>         "uploadedHistoryEntry": 0,
>         "storageHistoryEntries": 0,
>         "printHistoryEntries": 0,
>         "downloadHistoryEntries": 0,
>         "exportHistoryEntries": 0,
>         "deletingHistoryEntries": 0,
>         "undoDeletingHistoryEntries": 0,
>         "tagModificationHistoryEntries": 0,
>         "securityClassModificationHistoryEntries": 0,
>         "documentTypeModificationHistoryEntries": 0,
>         "restoringHistoryEntries": 0
>       }
>     }
>   ]
> ```
>
>
>

> [!note]- Original query - Call 2: count (14 s)
>
> POST /luz_jsonstore/api/mdb/\<tenant\>/documents/aggregate?collation=%22locale%22%3A%22en%22%2C%22caseFirst%22%3A%22UPPER%22
>
>
>
> ```
> [
>     {
>       "$match": {
>         "$and": [
>           {
>             "$and": [
>               {
>                 "$or": [
>                   { "isStored": true },
>                   { "folderIds.0": { "$exists": true } },
>                   {
>                     "$and": [
>                       {
>                         "$or": [
>                           { "folderIds": { "$exists": false } },
>                           { "folderIds": { "$size": 0 } }
>                         ]
>                       },
>                       { "origin": "User uploaded" }
>                     ]
>                   }
>                 ]
>               },
>               {
>                 "$and": [
>                   { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.draft\\.v1\\+json$",    "$options": "i" } } },
>                   { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.template\\.v1\\+json$", "$options": "i" } } },
>                   { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.receipt\\.v1\\+json$",  "$options": "i" } } }
>                 ]
>               }
>             ]
>           },
>           {
>             "$or": [
>               { "securityClassCodes": { "$exists": false } },
>               { "securityClassCodes": { "$size": 0 } },
>               { "securityClassCodes": null }
>             ]
>           },
>           { "_isBeingCreated": { "$ne": true } },
>           { "$or": [ { "personal": { "$exists": false } },       { "personal":        { "$ne": true } } ] },
>           { "$or": [ { "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" } ] }
>         ]
>       }
>     },
>     { "$unwind":    { "path": "$folderIds", "preserveNullAndEmptyArrays": true } },
>     { "$addFields": { "folderId": { "$toObjectId": "$folderIds" } } },
>     {
>       "$lookup": {
>         "from":         "folders",
>         "localField":   "folderId",
>         "foreignField": "_id",
>         "as":           "_folders"
>       }
>     },
>     {
>       "$addFields": {
>         "_folders.securityClassCodes": {
>           "$reduce": {
>             "input": {
>               "$concatArrays": [
>                 { "$ifNull": [ "$_folders.securityClassCodes", [] ] },
>                 { "$ifNull": [ "$_folders.inheritedSecurityClassCodes", [] ] }
>               ]
>             },
>             "initialValue": [],
>             "in": { "$concatArrays": [ "$$value", "$$this" ] }
>           }
>         }
>       }
>     },
>     {
>       "$match": {
>         "$or": [
>           { "_folders.securityClassCodes": [] },
>           { "_folders.securityClassCodes": { "$exists": false } }
>         ]
>       }
>     },
>     { "$group": { "_id": "$_id" } },
>     { "$count": "totalRecordCount" }
>   ]
> ```
>
>
>

> [!note]- New query - one single aggregate
>
> POST /luz_jsonstore/api/mdb/\<tenant\>/documents/aggregate (no collation param)
>
> \[
> {
> "\$match": {
> "\$and": \[
> {
> "\$and": \[
> {
> "\$or": \[
> { "isStored": true },
> { "folderIds.0": { "\$exists": true } },
> {
> "\$and": \[
> {
> "\$or": \[
> { "folderIds": { "\$exists": false } },
> { "folderIds": { "\$size": 0 } }
> \]
> },
> { "origin": "User uploaded" }
> \]
> }
> \]
> },
> {
> "letterInfo.mediaType": {
> "\$nin": \[
> "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
> "application/vnd.ch.klara.epost.smartletter.template.v1+json",
> "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"
> \]
> }
> }
> \]
> },
> {
> "\$or": \[
> { "securityClassCodes": { "\$exists": false } },
> { "securityClassCodes": { "\$size": 0 } },
> { "securityClassCodes": null }
> \]
> },
> { "\_isBeingCreated": { "\$ne": true } },
> { "\$or": \[ { "personal": { "\$exists": false } }, { "personal": { "\$ne": true } } \] },
> { "\$or": \[ { "\_deletionStatus": { "\$exists": false } }, { "\_deletionStatus": "false" } \] }
> \]
> }
> },
> {
> "\$lookup": {
> "from": "folders",
> "let": { "folderIds": "\$folderIds" },
> "pipeline": \[
> { "\$match": { "\$expr": { "\$in": \[ { "\$toString": "\$\_id" }, "\$\$folderIds" \] } } },
> {
> "\$project": {
> "\_id": { "\$toString": "\$\_id" },
> "name": "\$name",
> "securityClassCodes": {
> "\$concatArrays": \[
> { "\$ifNull": \[ "\$securityClassCodes", \[\] \] },
> { "\$ifNull": \[ "\$inheritedSecurityClassCodes", \[\] \] }
> \]
> }
> }
> }
> \],
> "as": "\_folders"
> }
> },
> {
> "\$match": {
> "\$or": \[
> { "\_folders.securityClassCodes": \[\] },
> { "\_folders.securityClassCodes": { "\$exists": false } }
> \]
> }
> },
> {
> "\$addFields": {
> "\_folders": {
> "\$filter": {
> "input": "\$\_folders",
> "cond": {
> "\$eq": \[
> { "\$size": { "\$ifNull": \[ "\$\$this.securityClassCodes", \[\] \] } },
> 0
> \]
> }
> }
> }
> }
> },
> {
> "\$facet": {
> "results": \[
> { "\$sort": { "\_updatedDate": -1 } },
> { "\$skip": 0 },
> { "\$limit": 48 },
> { "\$addFields": { "\_id": { "\$toString": "\$\_id" } } },
> {
> "\$project": {
> "\_files.referenceTsq": 0,
> "\_files.thumbnail512": 0,
> "\_files.thumbnail256": 0,
> "uploadedHistoryEntry": 0,
> "storageHistoryEntries": 0,
> "printHistoryEntries": 0,
> "downloadHistoryEntries": 0,
> "exportHistoryEntries": 0,
> "deletingHistoryEntries": 0,
> "undoDeletingHistoryEntries": 0,
> "tagModificationHistoryEntries": 0,
> "securityClassModificationHistoryEntries": 0,
> "documentTypeModificationHistoryEntries": 0,
> "restoringHistoryEntries": 0
> }
> }
> \],
> "count": \[
> { "\$count": "totalRecordCount" }
> \]
> }
> }
> \]
>

> [!note]- The stage by stage
>
>
> <table>
> <colgroup>
> <col style="width: 33%" />
> <col style="width: 33%" />
> <col style="width: 33%" />
> </colgroup>
> <tbody>
> <tr>
> <th><p>**No**</p></th>
> <th><p>**Original Query**</p></th>
> <th><p>**New Query**</p></th>
> </tr>
> &#10;<tr>
> <td><p>1</p></td>
> <td><p>LIST (22.0 s)</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`[
> &#10;    { "$match": { /* user query, with the 3× $not/$regex clauses on letterInfo.mediaType */ } },
> &#10;    { "$lookup": { /* let+pipeline, joins folders via $toString(_id) == folderIds */ "as": "_folders" } },
> &#10;    { "$match":   { /* _folders.securityClassCodes $or group */ } },
> &#10;    { "$addFields": { "_folders": { "$filter": { /* keep only allowed folders */ } } } },
> &#10;    { "$sort":  { "_updatedDate": -1 } },
> &#10;    { "$skip":  0 },
> &#10;    { "$limit": 48 },
> &#10;    { "$project": { /* user&#39;s excludes */ } }
> &#10;  ]`</pre>
> </td>
> <td rowspan="2">
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`[
>     { "$match": { /* user query — now with {$nin: [...]} instead of 3× $not/$regex */ } },
> &#10;    // Shared security stages — run ONCE, feed both branches below
>     { "$lookup": { /* same let+pipeline lookup → _folders */ } },
>     { "$match":   { /* _folders.securityClassCodes $or group */ } },
>     { "$addFields": { "_folders": { "$filter": { /* keep only allowed folders */ } } } },
> &#10;    // One $facet, two branches
>     { "$facet": {
>         "results": [
>           { "$sort":  { "_updatedDate": -1 } },
>           { "$skip":  0 },
>           { "$limit": 48 },
>           { "$addFields": { "_id": { "$toString": "$_id" } } },   // so json serialisation returns a string
>           { "$project": { /* user&#39;s excludes */ } }
>         ],
>         "count": [
>           { "$count": "totalRecordCount" }
>         ]
>     }}
>   ]`</pre>
> </td>
> </tr>
> <tr>
> <td><p>2</p></td>
> <td><p>COUNT (13.4 s)</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`  [
>     { "$match": { /* same user query / } },
>     { "$unwind": { "path": "$folderIds", "preserveNullAndEmptyArrays": true } },
>     { "$addFields": { "folderId": { "$toObjectId": "$folderIds" } } },
>     { "$lookup": {
>         "from": "folders", "localField": "folderId", "foreignField": "_id", "as": "_folders"
>     }},
>     { "$addFields": {
>         "_folders.securityClassCodes": { "$reduce": { / merge sc + inheritedSc */ } }
>     }},
>     { "$match": { "$or": [ {"_folders.securityClassCodes": []}, {"_folders.securityClassCodes": {"$exists": false}} ] } },
>     { "$group": { "_id": "$_id" } },
>     { "$count": "totalRecordCount" }
>   ]`</pre>
> </td>
> </tr>
> </tbody>
> </table>
>
>

> [!note]- Detail changing for each stage
>
> ## Op 1: The letterInfo.mediaType filter
>
>
> <table>
> <colgroup>
> <col style="width: 50%" />
> <col style="width: 50%" />
> </colgroup>
> <tbody>
> <tr>
> <th><p>**Before**</p></th>
> <th><p>**After**</p></th>
> </tr>
> &#10;<tr>
> <td><p>{ "$and": [<br />
> { "letterInfo.mediaType": {<br />
> "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.draft\\.v1\\+json$",<br />
> "$options": "i" } } },<br />
> { "letterInfo.mediaType": {<br />
> "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.template\\.v1\\+json$",<br />
> "$options": "i" } } },<br />
> { "letterInfo.mediaType": {<br />
> "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.receipt\\.v1\\+json$",<br />
> "$options": "i" } } }<br />
> ]}</p></td>
> <td><p>{ "letterInfo.mediaType": {<br />
> "$nin": [<br />
> "application/vnd.ch.klara.epost.smartletter.draft.v1+json",<br />
> "application/vnd.ch.klara.epost.smartletter.template.v1+json",<br />
> "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"<br />
> ]<br />
> }}</p></td>
> </tr>
> <tr>
> <td><p>Why it's slow</p>
> <ul>
> <li><p>Three separate predicates → three passes per document.</p></li>
> <li><p>$regex with $options: "i" is case-insensitive → MongoDB cannot use a regular index on letterInfo.mediaType.</p></li>
> <li><p>Each $not { $regex } forces a per-document string-match evaluation.</p></li>
> </ul></td>
> <td><p>Why it's faster</p>
> <ul>
> <li><p>One predicate → one comparison per document.</p></li>
> <li><p>$nin is a set membership test against fixed strings → index-usable (if you add letterInfo.mediaType_1).</p></li>
> <li><p>No regex engine, no case-insensitivity work.</p></li>
> </ul></td>
> </tr>
> </tbody>
> </table>
>
>
> ## Op5: Sort, skip, limit (the "results" path)
>
>
> <table>
> <colgroup>
> <col style="width: 50%" />
> <col style="width: 50%" />
> </colgroup>
> <tbody>
> <tr>
> <th><p>**Before**</p></th>
> <th><p>**After**</p></th>
> </tr>
> &#10;<tr>
> <td><p>{ "$sort": { "_updatedDate": -1 } },<br />
> { "$skip": 0 },<br />
> { "$limit": 48 }</p></td>
> <td><p>{ "$facet": {<br />
> "results": [<br />
> { "$sort": { "_updatedDate": -1 } },<br />
> { "$skip": 0 },<br />
> { "$limit": 48 },<br />
> …<br />
> ],<br />
> "count": [ … ]<br />
> }</p></td>
> </tr>
> <tr>
> <td><p>Inline at the top level, part of the list pipeline only</p></td>
> <td><p>Moved inside the $facet.results branch.</p>
> <p>What changed: identical stages, different location. Same semantic — sort and take page. The only reason they moved is because they belong to one of the two branches the $facet splits into.</p></td>
> </tr>
> </tbody>
> </table>
>
>
> ## Op6: The total countThe total count
>
>
> <table>
> <colgroup>
> <col style="width: 50%" />
> <col style="width: 50%" />
> </colgroup>
> <tbody>
> <tr>
> <th><p>**Before -** count runs as its own separate pipeline</p></th>
> <th><p>**After -** count is a branch of the same $facet</p></th>
> </tr>
> &#10;<tr>
> <td><p>{ "$match": <user query> },<br />
> { "$unwind": { "path": "$folderIds", "preserveNullAndEmptyArrays": true<br />
> } },<br />
> { "$addFields":{ "folderId": { "$toObjectId": "$folderIds" } } },<br />
> { "$lookup": { "from": "folders",<br />
> "localField": "folderId",<br />
> "foreignField": "_id",<br />
> "as": "_folders" } },<br />
> { "$addFields":{ "_folders.securityClassCodes": { "$reduce": { … } } } },<br />
> { "$match": <folders sc empty> },<br />
> { "$group": { "_id": "$_id" } }, // re-dedup after unwind<br />
> { "$count": "totalRecordCount" }</p></td>
> <td><p>{ "$facet": {<br />
> "results": [ … ],<br />
> "count": [ { "$count": "totalRecordCount" } ]<br />
> }}</p></td>
> </tr>
> <tr>
> <td><p>Problems</p>
> <ul>
> <li><p>Separate round-trip to MongoDB.</p></li>
> <li><p>$unwind folderIds → a doc in N folders becomes N rows; per-row<br />
> $toObjectId conversion; per-row lookup.</p></li>
> <li><p>$group: { _id: "$_id" } is purely to re-collapse what $unwind just<br />
> expanded.</p></li>
> <li><p>The same $match is re-evaluated from scratch (doesn't share work with<br />
> the list call).</p></li>
> </ul></td>
> <td><p>Why it works without $unwind + $group<br />
> The shared stages upstream ($match → $lookup → security filter) already<br />
> produced one row per document (no row explosion), so counting is just<br />
> $count. We don't need to re-dedup because there was never a duplication.</p>
> <p>Net saving: the entire sequence $unwind → $addFields(toObjectId) →<br />
> $lookup(localField) → $addFields(reduce) → $match → $group vanishes. Count<br />
> becomes a single cheap stage.</p></td>
> </tr>
> </tbody>
> </table>
>
>
> ## Op7: User's excludes projection
>
>
> <table>
> <colgroup>
> <col style="width: 50%" />
> <col style="width: 50%" />
> </colgroup>
> <tbody>
> <tr>
> <th><p>**Before** (list path only, at top level)</p></th>
> <th><p>**After** (moved into $facet.results)</p></th>
> </tr>
> &#10;<tr>
> <td><p>{ "$project": {<br />
> "_files.referenceTsq": 0,<br />
> "_files.thumbnail512": 0,<br />
> "_files.thumbnail256": 0,<br />
> "uploadedHistoryEntry": 0,<br />
> … (14 fields)<br />
> }}</p></td>
> <td><p>{ "$facet": {<br />
> "results": [<br />
> …,<br />
> { "$project": {<br />
> "_files.referenceTsq": 0,<br />
> … (same 14 fields)<br />
> }}<br />
> ],<br />
> "count": [ … ]<br />
> }}</p></td>
> </tr>
> <tr>
> <td></td>
> <td><p>What changed: same stage, just lives inside the results branch because<br />
> that's the only branch that returns documents.</p></td>
> </tr>
> </tbody>
> </table>
>
>
> ## Op8: NEW stage: convert \_id to string (only in results branch)
>
>
> <table>
> <colgroup>
> <col style="width: 50%" />
> <col style="width: 50%" />
> </colgroup>
> <tbody>
> <tr>
> <th><p>**Before**</p></th>
> <th><p>**After**</p></th>
> </tr>
> &#10;<tr>
> <td><p>each top-level document returned by aggregate had its _id<br />
> converted from ObjectId → hex string by luz-jsonstore's setDocId. That<br />
> code only touches the top-level _id of each result row.</p>
> <p>With $facet, the aggregate returns a single wrapper row:<br />
> [<br />
> { "results": [ {doc1}, {doc2}, … ], "count": [ {totalRecordCount: 48} ]<br />
> }<br />
> ]</p>
> <p>setDocId sees the wrapper — it has no _id. The real documents are nested<br />
> inside results[] and their _id stays as an ObjectId (serialised as<br />
> {"$oid": "…"}), which later blows up getString("_id") in luz-docs.</p></td>
> <td><p>Added stage (inside $facet.results)</p>
> <p>{ "$addFields": { "_id": { "$toString": "$_id" } } }</p>
> <p>What it does: converts _id from ObjectId → hex string inside the pipeline,<br />
> so by the time luz-jsonstore serialises to JSON each nested doc has _id<br />
> as a plain string.</p></td>
> </tr>
> </tbody>
> </table>
>
>

%% ai-graph-start %%

**Related notes:**
- [[Analytics Analyze API call when accessing eArchive]]
- [[WIP Analyze subfolder search API (542ms)]]
- [[Performance Issue Slow Document Listing Query in MongoDB - eArchive page]]
- [[eArchive – Reproduce performance issue and understand the issue on DEV]]
- [[How to run export API for specific tenant and date - Manual export]]

%% ai-graph-end %%