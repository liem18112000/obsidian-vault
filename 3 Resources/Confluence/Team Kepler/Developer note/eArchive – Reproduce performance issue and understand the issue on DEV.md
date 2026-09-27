---
title: "[eArchive] – Reproduce performance issue and understand the issue on DEV"
created: 2026-05-04
updated: 2026-05-08
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49383964814/eArchive+Reproduce+performance+issue+and+understand+the+issue+on+DEV
confluence_id: "49383964814"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, earchive, performance]
---

# [eArchive] – Reproduce performance issue and understand the issue on DEV

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49383964814/eArchive+Reproduce+performance+issue+and+understand+the+issue+on+DEV) · updated 2026-05-08*

Open eArchive page:

\*\*Tenant ID: `037ef4de-cd23-4ec9-b344-30b5068bc033`- **130k documents**

**LUZ-AUDIT (88ms)**

`**APIs flow: luz-docs-view-controller → luz-audit`

**luz_docs_view_controller → luz-audit**

```
POST http://luz-audit:8080/luz_audit/api/037ef4de-cd23-4ec9-b344-30b5068bc033/audits
```

'06:04:54,940 INFO  \[ch.klara.luz.audit.service.AuditEventReceiver\] (Gax-4) \[createAuditLogFromQueue\] - Finish create audit log event tenantId 037ef4de-cd23-4ec9-b344-30b5068bc033, ObjectId null, objectType null, user [thien.laihong@axonactive.com](mailto:thien.laihong@axonactive.com), time 1776917094940

'06:04:54,940 INFO  \[ch.klara.luz.audit.service.FingerprintService\] (Gax-4) \[updateFingerPrintToJsonStore\] update finger print successfully, tenantId: 037ef4de-cd23-4ec9-b344-30b5068bc033

'06:04:54,940 INFO  \[ch.klara.luz.docs.filter.JsonStoreLoggingFilter\] (Gax-4) \[PATCH\] - [http://luz-jsonstore:8080/luz_jsonstore/api/mdb/23bc4385-8eb2-4b30-a959-50382e3440cf/lastFingerPrint/one](http://luz-jsonstore:8080/luz_jsonstore/api/mdb/23bc4385-8eb2-4b30-a959-50382e3440cf/lastFingerPrint/one) request-body={"filter":{"fingerPrint":"a5284d1ad36c395d2303335f4de103a494c17ec87bc0a5332c6b6daa8367517f","tenantId":"037ef4de-cd23-4ec9-b344-30b5068bc033"},"update":{"\$set":{"fingerPrint":"4178c3489a2b90efe9109745bfceeb064be84f71d33148314a3cb82f807c145d","updatedDate":"2026-04-23T04:04:54.874Z","version":555}}} time-consuming=15

'06:04:54,920 INFO  \[ch.klara.luz.docs.filter.JsonStoreLoggingFilter\] (Gax-4) \[PUT\] - [http://luz-jsonstore:8080/luz_jsonstore/api/mdb/23bc4385-8eb2-4b30-a959-50382e3440cf/auditlogs/add](http://luz-jsonstore:8080/luz_jsonstore/api/mdb/23bc4385-8eb2-4b30-a959-50382e3440cf/auditlogs/add) request-body={"eventDateTime":"2026-04-23T04:04:54.874Z","eventDescription":"eArchive [accessed","eventFingerprint":"4178c3489a2b90efe9109745bfceeb064be84f71d33148314a3cb82f807c145d","eventStatus":"SUCCESSFUL","eventSubType":"EARCHIVE","eventType":"ACCESS","softwareModule":"luz_epost_business_web","tenantId":"037ef4de-cd23-4ec9-b344-30b5068bc033","transactionId":"81cd67cd2153b2d6:92436e21f2556cb4:81cd67cd2153b2d6:1","user":"thien.laihong@axonactive.com](mailto:accessed%22,%22eventFingerprint%22:%224178c3489a2b90efe9109745bfceeb064be84f71d33148314a3cb82f807c145d%22,%22eventStatus%22:%22SUCCESSFUL%22,%22eventSubType%22:%22EARCHIVE%22,%22eventType%22:%22ACCESS%22,%22softwareModule%22:%22luz_epost_business_web%22,%22tenantId%22:%22037ef4de-cd23-4ec9-b344-30b5068bc033%22,%22transactionId%22:%2281cd67cd2153b2d6:92436e21f2556cb4:81cd67cd2153b2d6:1%22,%22user%22:%22thien.laihong@axonactive.com)"} time-consuming=11

'06:04:54,899 INFO  \[ch.klara.luz.docs.filter.JsonStoreLoggingFilter\] (Gax-4) \[POST\] - [http://luz-jsonstore:8080/luz_jsonstore/api/mdb/23bc4385-8eb2-4b30-a959-50382e3440cf/lastFingerPrint?collation=%22locale%22%3A%22en%22%2C%22caseFirst%22%3A%22UPPER%22](http://luz-jsonstore:8080/luz_jsonstore/api/mdb/23bc4385-8eb2-4b30-a959-50382e3440cf/lastFingerPrint?collation=%22locale%22%3A%22en%22%2C%22caseFirst%22%3A%22UPPER%22) request-body={"tenantId":"037ef4de-cd23-4ec9-b344-30b5068bc033"} time-consuming=17

'06:04:54,873 INFO  \[ch.klara.luz.audit.service.AuditEventReceiver\] (Gax-4) \[createAuditLogFromQueue\] - Start create audit log event tenantId 037ef4de-cd23-4ec9-b344-30b5068bc033, ObjectId null, objectType null

'06:04:54,851 INFO  \[ch.klara.luz.message.receiver.service.pubsub.receiver.BaseReceiver\] (Gax-4) PUBSUB MESSAGE RECEIVED messageId 18340073175854298 pulled: {"message":"{\\encryptInLuzAudit\\:true,\\completionFlagId\\:\\Audit-log-0feb0394-7868-4a91-9632-d89cf3747bf9-1776917094759\\,\\auditLog\\:{\\eventDescription\\:\\eArchive accessed\\,\\eventStatus\\:\\SUCCESSFUL\\,\\eventSubType\\:\\EARCHIVE\\,\\eventType\\:\\ACCESS\\,\\softwareModule\\:\\luz_epost_business_web\\,\\tenantId\\:\\037ef4de-cd23-4ec9-b344-30b5068bc033\\,\\transactionId\\:\\81cd67cd2153b2d6:92436e21f2556cb4:81cd67cd2153b2d6:1\\,\\user\\:\\[thien.laihong@axonactive.com](mailto:thien.laihong@axonactive.com)\\}}","orderingKey":"037ef4de-cd23-4ec9-b344-30b5068bc033","tenantId":"23bc4385-8eb2-4b30-a959-50382e3440cf","companyId":1}

'06:04:54,832 INFO  \[io.undertow.accesslog\] (default task-1) 127.0.0.6 \[23/Apr/2026:06:04:54 +0200\] luz-uri=POST /luz_audit/api/037ef4de-cd23-4ec9-b344-30b5068bc033/audits HTTP/1.1 status-code=201 bytes-sent=315 time-consuming=81

'06:04:54,830 INFO  \[ch.klara.luz.audit.configuration.RestClientResponseFilter\] (default task-1) \[POST\] - [http://luz-message-broker:8080/luz_message_broker/api/message/23bc4385-8eb2-4b30-a959-50382e3440cf/companies/1/push/dev-topic-luz-audit?ordering-key=037ef4de-cd23-4ec9-b344-30b5068bc033](http://luz-message-broker:8080/luz_message_broker/api/message/23bc4385-8eb2-4b30-a959-50382e3440cf/companies/1/push/dev-topic-luz-audit?ordering-key=037ef4de-cd23-4ec9-b344-30b5068bc033) status-code=200 time-consuming=65

**LUZ-DOCS**

`**APIs flow: luz-docs-view-controller → luz-docs → luz-jsonstore`

1 — Folder Search (55ms)

**luz_docs_view_controller → luz-docs**

```
POST http://luz-docs:8080/luz_docs/api/037ef4de-cd23-4ec9-b344-30b5068bc033/folders/search
```

**Query 1a — Folder List** → luz-jsonstore (11ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/folders?sort="updatedDate":"desc"
```

```
{
  "$and": [
    { "$or": [{ "parentFolderIds": { "$exists": false } }, { "parentFolderIds": { "$size": 0 } }] },
    { "$or": [
        { "$or": [{ "securityClassCodes": { "$exists": true, "$in": [] } }, { "inheritedSecurityClassCodes": { "$exists": true, "$in": [] } }] },
        { "$and": [
            { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": { "$eq": null } }] },
            { "$or": [{ "inheritedSecurityClassCodes": { "$exists": false } }, { "inheritedSecurityClassCodes": { "$size": 0 } }, { "inheritedSecurityClassCodes": { "$eq": null } }] }
        ]}
    ]},
    { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] },
    { "$or": [{ "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" }] }
  ]
}
```

**Query 1b — Folder Total Count** → luz-jsonstore (14ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/folders/aggregate?collation=...
```

```
[
  { "$match": { "$and": [
    { "$or": [{ "parentFolderIds": { "$exists": false } }, { "parentFolderIds": { "$size": 0 } }] },
    { "$or": [
        { "$or": [{ "securityClassCodes": { "$exists": true, "$in": [] } }, { "inheritedSecurityClassCodes": { "$exists": true, "$in": [] } }] },
        { "$and": [
            { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": { "$eq": null } }] },
            { "$or": [{ "inheritedSecurityClassCodes": { "$exists": false } }, { "inheritedSecurityClassCodes": { "$size": 0 } }, { "inheritedSecurityClassCodes": { "$eq": null } }] }
        ]}
    ]},
    { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] },
    { "$or": [{ "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" }] }
  ] } },
  { "$count": "totalRecordCount" }
]
```

2 — Document Search: Facets — Sender/Origin Stats (897ms)

**luz_docs_view_controller → luz-docs**

```
POST http://luz-docs:8080/luz_docs/api/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/search
     ?skip-security-classes=false&include-folder-name=false
```

**Query Payload:**

```
{
  "facets": {
    "test": {
      "terms": {
        "fields": ["senderTenantId", "senderCompanyId", "origin"],
        "getMax": ["documentReferenceDate"],
        "from": 0,
        "size": 2147483647
      }
    }
  }
}
```

**Query — Facet Aggregate** → luz-jsonstore (846ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/aggregate?collation=...
```

```
[
  { "$match": { "_isBeingCreated": { "$ne": true } } },
  { "$match": { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] } },
  { "$match": { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": null }] } },
  { "$lookup": { "from": "folders", "let": { "folderIds": "$folderIds" }, "pipeline": [ {"$match":{"$expr":{"$in":[{"$toString":"$_id"},"$$folderIds"]}}},{"$project":{"_id":{"$toString":"$_id"},"name":"$name","securityClassCodes":{"$concatArrays":[{"$ifNull":["$securityClassCodes",[]]},{"$ifNull":["$inheritedSecurityClassCodes",[]]}]}}} ], "as": "_folders" } },
  { "$match": { "$or": [{ "_folders.securityClassCodes": [] }, { "_folders.securityClassCodes": { "$exists": false } }] } },
  { "$addFields": { "_folders": { "$filter": { "input": "$_folders", "cond": { "$eq": [{ "$size": { "$ifNull": ["$$this.securityClassCodes", []] } }, 0] } } } } },
  { "$match": { "$and": [{ "senderTenantId": { "$exists": true } }, { "senderCompanyId": { "$exists": true } }, { "origin": { "$exists": true } }, { "_deletionStatus": "false" }] } },
  { "$group": { "_id": { "senderTenantId": "$senderTenantId", "senderCompanyId": "$senderCompanyId", "origin": "$origin" }, "documentReferenceDate": { "$max": "$documentReferenceDate" }, "count": { "$sum": 1 } } },
  { "$sort": { "_id.senderTenantId": 1 } },
  { "$skip": 0 },
  { "$limit": 2147483647 }
]
```

3 — Document Search: Paged List + Total Count (17,373ms)

**luz_docs_view_controller → luz-docs**

```
POST http://luz-docs:8080/luz_docs/api/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/search
     ?include-deleted-documents=false&skip-security-classes=false&include-folder-name=true&exclude-total-count=false
```

**Query Payload:**

```
{
  "query": {
    "and": [
      { "or": [
          { "term": { "isStored": true } },
          { "exists": { "field": "folderIds", "type": "array" } },
          { "and": [{ "not": { "exists": { "field": "folderIds", "type": "array" } } }, { "term": { "origin": "User uploaded" } }] }
      ]},
      { "not": { "terms": { "letterInfo.mediaType": [
          "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
          "application/vnd.ch.klara.epost.smartletter.template.v1+json",
          "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"
      ]}}}
    ]
  },
  "excludes": ["_files.referenceTsq", "_files.thumbnail512", "_files.thumbnail256", "uploadedHistoryEntry", "storageHistoryEntries", "printHistoryEntries", "downloadHistoryEntries", "exportHistoryEntries", "deletingHistoryEntries", "undoDeletingHistoryEntries", "tagModificationHistoryEntries", "securityClassModificationHistoryEntries", "documentTypeModificationHistoryEntries", "restoringHistoryEntries"],
  "from": 0,
  "size": 48,
  "sort": { "_updatedDate": "DESC" }
}
```

**Query 3a — Document Search (paged)** → luz-jsonstore (10,736ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/aggregate
```

```
[
  // Filter: (isStored=true OR folderIds[0] exists OR (no folderIds AND origin=User uploaded))
  //         AND exclude smartletter mediaTypes
  //         AND no securityClassCodes AND not being created AND not personal AND not deleted
  { "$match": {
      "$and": [
        { "$and": [
            { "$or": [
                { "isStored": true },
                { "folderIds.0": { "$exists": true } },
                { "$and": [
                    { "$or": [{ "folderIds": { "$exists": false } }, { "folderIds": { "$size": 0 } }] },
                    { "origin": "User uploaded" }
                ]}
            ]},
            { "$and": [
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.draft\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.template\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.receipt\\.v1\\+json$", "$options": "i" } } }
            ]}
        ]},
        { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": null }] },
        { "_isBeingCreated": { "$ne": true } },
        { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] },
        { "$or": [{ "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" }] }
      ]
  }},
  { "$lookup": {
      "from": "folders",
      "let": { "folderIds": "$folderIds" },
      "pipeline": [
        { "$match": { "$expr": { "$in": [{ "$toString": "$_id" }, "$$folderIds"] } } },
        { "$project": { "_id": { "$toString": "$_id" }, "name": "$name", "securityClassCodes": { "$concatArrays": [{ "$ifNull": ["$securityClassCodes", []] }, { "$ifNull": ["$inheritedSecurityClassCodes", []] }] } } }
      ],
      "as": "_folders"
  }},
  { "$match": { "$or": [{ "_folders.securityClassCodes": [] }, { "_folders.securityClassCodes": { "$exists": false } }] } },
  { "$addFields": { "_folders": { "$filter": { "input": "$_folders", "cond": { "$eq": [{ "$size": { "$ifNull": ["$$this.securityClassCodes", []] } }, 0] } } } } },
  { "$sort": { "_updatedDate": -1 } },
  { "$skip": 0 },
  { "$limit": 48 },
  { "$project": {
      "_files.referenceTsq": 0, "_files.thumbnail512": 0, "_files.thumbnail256": 0,
      "uploadedHistoryEntry": 0, "storageHistoryEntries": 0, "printHistoryEntries": 0,
      "downloadHistoryEntries": 0, "exportHistoryEntries": 0, "deletingHistoryEntries": 0,
      "undoDeletingHistoryEntries": 0, "tagModificationHistoryEntries": 0,
      "securityClassModificationHistoryEntries": 0, "documentTypeModificationHistoryEntries": 0,
      "restoringHistoryEntries": 0
  }}
]
```

**Query 3b — Document Total Count** → luz-jsonstore (6,444ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/aggregate?collation=...
```

```
[
  // Same filter as 3a: isStored OR folderIds exists OR User uploaded,
  { "$match": {
      "$and": [
        { "$and": [
            { "$or": [
                { "isStored": true },
                { "folderIds.0": { "$exists": true } },
                { "$and": [
                    { "$or": [{ "folderIds": { "$exists": false } }, { "folderIds": { "$size": 0 } }] },
                    { "origin": "User uploaded" }
                ]}
            ]},
            { "$and": [
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.draft\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.template\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.receipt\\.v1\\+json$", "$options": "i" } } }
            ]}
        ]},
        { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": null }] },
        { "_isBeingCreated": { "$ne": true } },
        { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] },
        { "$or": [{ "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" }] }
      ]
  }},
  { "$unwind": { "path": "$folderIds", "preserveNullAndEmptyArrays": true } },
  { "$addFields": { "folderId": { "$toObjectId": "$folderIds" } } },
  { "$lookup": { "from": "folders", "localField": "folderId", "foreignField": "_id", "as": "_folders" } },
  { "$addFields": { "_folders.securityClassCodes": { "$reduce": {
      "input": { "$concatArrays": [{ "$ifNull": ["$_folders.securityClassCodes", []] }, { "$ifNull": ["$_folders.inheritedSecurityClassCodes", []] }] },
      "initialValue": [], "in": { "$concatArrays": ["$$value", "$$this"] }
  }}}},
  { "$match": { "$or": [{ "_folders.securityClassCodes": [] }, { "_folders.securityClassCodes": { "$exists": false } }] } },
  { "$group": { "_id": "$_id" } },
  { "$count": "totalRecordCount" }
]
```

4 — Document Search: Facets — My Companies / Folder IDs (14,995ms)

**luz_docs_view_controller → luz-docs**

```
POST http://luz-docs:8080/luz_docs/api/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/search
     ?skip-security-classes=false&include-folder-name=false
```

**Query Payload:**

```
{
  "facets": {
    "My Companies": {
      "terms": {
        "field": "folderIds",
        "from": 0,
        "size": 2147483647
      }
    }
  }
}
```

**Query — Facet Aggregate (group by folderIds)** → luz-jsonstore (14,961ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/aggregate?collation=...
```

```
[
  // Filter: not being created, not personal documents, no document-level security class codes
  { "$match": { "_isBeingCreated": { "$ne": true } } },
  { "$match": { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] } },
  { "$match": { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": null }] } },
  { "$lookup": {
      "from": "folders",
      "let": { "folderIds": "$folderIds" },
      "pipeline": [
        { "$match": { "$expr": { "$in": [{ "$toString": "$_id" }, "$$folderIds"] } } },
        { "$project": { "_id": { "$toString": "$_id" }, "name": "$name", "securityClassCodes": { "$concatArrays": [{ "$ifNull": ["$securityClassCodes", []] }, { "$ifNull": ["$inheritedSecurityClassCodes", []] }] } } }
      ],
      "as": "_folders"
  }},
  { "$match": { "$or": [{ "_folders.securityClassCodes": [] }, { "_folders.securityClassCodes": { "$exists": false } }] } },
  { "$addFields": { "_folders": { "$filter": { "input": "$_folders", "cond": { "$eq": [{ "$size": { "$ifNull": ["$$this.securityClassCodes", []] } }, 0] } } } } },
  { "$match": { "$and": [{ "folderIds": { "$exists": true } }, { "_deletionStatus": "false" }] } },
  { "$group": { "_id": "$folderIds", "count": { "$sum": 1 } } },
  { "$sort": { "_id": 1 } },
  { "$skip": 0 },
  { "$limit": 2147483647 }
]
```

5 — Document Count: Unread / Inbox (6,161ms)

**luz_docs_view_controller → luz-docs**

```
POST http://luz-docs:8080/luz_docs/api/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/count
     ?skip-security-classes=false&include-folder-name=false
```

*(no DocumentResource payload — count endpoint)*

**Query — Unread Count** → luz-jsonstore (6,076ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/aggregate?collation=...
```

```
[
  // Filter: (unread = no readHistoryEntries AND createdDate >= 2021-11-01) OR (markedAsUnread=true)
  //         AND (no folderIds AND not stored AND not user-uploaded)
  //         AND exclude smartletter mediaTypes
  //         AND no securityClassCodes AND not being created AND not personal AND not deleted
  { "$match": {
      "$and": [
        { "$and": [
            { "$or": [
                { "$and": [
                    { "$or": [{ "readHistoryEntries": { "$exists": false } }, { "readHistoryEntries": "" }] },
                    { "$expr": { "$and": [{ "$gte": [{ "$dateFromString": { "dateString": "$_createdDate", "onError": null } }, { "$dateFromString": { "dateString": "2021-11-01", "onError": null } }] }] } }
                ]},
                { "$and": [{ "markedAsUnread": { "$exists": true, "$ne": "" } }, { "markedAsUnread": true }] }
            ]},
            { "$and": [
                { "$and": [
                    { "$or": [{ "folderIds": { "$exists": false } }, { "folderIds": { "$size": 0 } }] },
                    { "$or": [{ "isStored": false }, { "$or": [{ "isStored": { "$exists": false } }, { "isStored": "" }] }] },
                    { "$or": [{ "storedFolder": { "$exists": false } }, { "storedFolder": "" }] }
                ]},
                { "origin": { "$not": { "$regex": "^User uploaded$", "$options": "i" } } }
            ]},
            { "$and": [
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.draft\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.template\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.receipt\\.v1\\+json$", "$options": "i" } } }
            ]}
        ]},
        { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": null }] },
        { "_isBeingCreated": { "$ne": true } },
        { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] },
        { "$or": [{ "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" }] }
      ]
  }},
  { "$unwind": { "path": "$folderIds", "preserveNullAndEmptyArrays": true } },
  { "$addFields": { "folderId": { "$toObjectId": "$folderIds" } } },
  { "$lookup": { "from": "folders", "localField": "folderId", "foreignField": "_id", "as": "_folders" } },
  { "$addFields": { "_folders.securityClassCodes": { "$reduce": {
      "input": { "$concatArrays": [{ "$ifNull": ["$_folders.securityClassCodes", []] }, { "$ifNull": ["$_folders.inheritedSecurityClassCodes", []] }] },
      "initialValue": [], "in": { "$concatArrays": ["$$value", "$$this"] }
  }}}},
  { "$match": { "$or": [{ "_folders.securityClassCodes": [] }, { "_folders.securityClassCodes": { "$exists": false } }] } },
  { "$group": { "_id": "$_id" } },
  { "$count": "totalRecordCount" }
]
```

6 — Document Search: Security Class Check — All Docs (22,337ms)

**luz_docs_view_controller → luz-docs**

```
POST http://luz-docs:8080/luz_docs/api/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/search
     ?include-deleted-documents=false&skip-security-classes=false&include-folder-name=true&exclude-total-count=false
```

**Query Payload:**

```
{
  "query": {
    "and": [
      { "or": [{ "term": { "isStored": true } }, { "exists": { "field": "folderIds", "type": "array" } }] },
      { "not": { "terms": { "letterInfo.mediaType": [
          "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
          "application/vnd.ch.klara.epost.smartletter.template.v1+json",
          "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"
      ]}}}
    ]
  },
  "includes": ["securityClassCodes"],
  "sort": { "_updatedDate": "DESC" }
}
```

⚠️ No `from`/`size` — returns **all** matching documents, projected to `securityClassCodes` only.

**Query 6a — Search All (securityClassCodes projection)** → luz-jsonstore (7,160ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/aggregate
```

```
[
  // Filter: (isStored=true OR folderIds[0] exists)
  //         AND exclude smartletter mediaTypes
  //         AND no securityClassCodes AND not being created AND not personal AND not deleted
  { "$match": {
      "$and": [
        { "$and": [
            { "$or": [
                { "isStored": true },
                { "folderIds.0": { "$exists": true } }
            ]},
            { "$and": [
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.draft\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.template\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.receipt\\.v1\\+json$", "$options": "i" } } }
            ]}
        ]},
        { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": null }] },
        { "_isBeingCreated": { "$ne": true } },
        { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] },
        { "$or": [{ "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" }] }
      ]
  }},
  { "$lookup": {
      "from": "folders",
      "let": { "folderIds": "$folderIds" },
      "pipeline": [
        { "$match": { "$expr": { "$in": [{ "$toString": "$_id" }, "$$folderIds"] } } },
        { "$project": { "_id": { "$toString": "$_id" }, "name": "$name", "securityClassCodes": { "$concatArrays": [{ "$ifNull": ["$securityClassCodes", []] }, { "$ifNull": ["$inheritedSecurityClassCodes", []] }] } } }
      ],
      "as": "_folders"
  }},
  { "$match": { "$or": [{ "_folders.securityClassCodes": [] }, { "_folders.securityClassCodes": { "$exists": false } }] } },
  { "$addFields": { "_folders": { "$filter": { "input": "$_folders", "cond": { "$eq": [{ "$size": { "$ifNull": ["$$this.securityClassCodes", []] } }, 0] } } } } },
  { "$sort": { "_updatedDate": -1 } },
  { "$project": { "securityClassCodes": 1 } }
]
```

**Query 6b — Total Count** → luz-jsonstore (4,265ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/aggregate?collation=...
```

```
[
  // Filter: (isStored=true OR folderIds[0] exists)
  //         AND exclude smartletter mediaTypes
  //         AND no securityClassCodes AND not being created AND not personal AND not deleted
  { "$match": {
      "$and": [
        { "$and": [
            { "$or": [
                { "isStored": true },
                { "folderIds.0": { "$exists": true } }
            ]},
            { "$and": [
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.draft\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.template\\.v1\\+json$", "$options": "i" } } },
                { "letterInfo.mediaType": { "$not": { "$regex": "^application/vnd\\.ch\\.klara\\.epost\\.smartletter\\.receipt\\.v1\\+json$", "$options": "i" } } }
            ]}
        ]},
        { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": null }] },
        { "_isBeingCreated": { "$ne": true } },
        { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] },
        { "$or": [{ "_deletionStatus": { "$exists": false } }, { "_deletionStatus": "false" }] }
      ]
  }},
  { "$unwind": { "path": "$folderIds", "preserveNullAndEmptyArrays": true } },
  { "$addFields": { "folderId": { "$toObjectId": "$folderIds" } } },
  { "$lookup": { "from": "folders", "localField": "folderId", "foreignField": "_id", "as": "_folders" } },
  { "$addFields": { "_folders.securityClassCodes": { "$reduce": {
      "input": { "$concatArrays": [{ "$ifNull": ["$_folders.securityClassCodes", []] }, { "$ifNull": ["$_folders.inheritedSecurityClassCodes", []] }] },
      "initialValue": [], "in": { "$concatArrays": ["$$value", "$$this"] }
  }}}},
  { "$match": { "$or": [{ "_folders.securityClassCodes": [] }, { "_folders.securityClassCodes": { "$exists": false } }] } },
  { "$group": { "_id": "$_id" } },
  { "$count": "totalRecordCount" }
]
```

7 — Document Search: Facets — Tags / Item Groups (25,845ms) ⚠️ Slowest

**luz_docs_view_controller → luz-docs**

```
POST http://luz-docs:8080/luz_docs/api/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/search
     ?include-deleted-documents=false&skip-security-classes=false&include-folder-name=true&exclude-total-count=false
```

**Query Payload:**

```
{
  "facets": {
    "itemGroup": {
      "terms": {
        "field": "tags",
        "type": "array",
        "sort": { "key": "asc" }
      }
    }
  }
}
```

**Query — Facet Aggregate (group by tags)** → luz-jsonstore (25,798ms)

```
POST /luz_jsonstore/api/mdb/037ef4de-cd23-4ec9-b344-30b5068bc033/documents/aggregate?collation=...
```

```
[
  // Filter: not being created, not personal documents, no document-level security class codes
  { "$match": { "_isBeingCreated": { "$ne": true } } },
  { "$match": { "$or": [{ "personal": { "$exists": false } }, { "personal": { "$ne": true } }] } },
  { "$match": { "$or": [{ "securityClassCodes": { "$exists": false } }, { "securityClassCodes": { "$size": 0 } }, { "securityClassCodes": null }] } },
  { "$lookup": {
      "from": "folders",
      "let": { "folderIds": "$folderIds" },
      "pipeline": [
        { "$match": { "$expr": { "$in": [{ "$toString": "$_id" }, "$$folderIds"] } } },
        { "$project": { "_id": { "$toString": "$_id" }, "name": "$name", "securityClassCodes": { "$concatArrays": [{ "$ifNull": ["$securityClassCodes", []] }, { "$ifNull": ["$inheritedSecurityClassCodes", []] }] } } }
      ],
      "as": "_folders"
  }},
  { "$match": { "$or": [{ "_folders.securityClassCodes": [] }, { "_folders.securityClassCodes": { "$exists": false } }] } },
  { "$addFields": { "_folders": { "$filter": { "input": "$_folders", "cond": { "$eq": [{ "$size": { "$ifNull": ["$$this.securityClassCodes", []] } }, 0] } } } } },
  { "$match": { "$or": [
      { "$and": [{ "tags": { "$size": 0 } }, { "tags": { "$exists": true } }, { "_deletionStatus": "false" }] },
      { "$and": [{ "tags.0": { "$exists": true } }, { "_deletionStatus": "false" }] }
  ]}},
  { "$unwind": { "path": "$tags", "preserveNullAndEmptyArrays": true } },
  { "$group": { "_id": "$tags", "count": { "$sum": 1 } } },
  { "$sort": { "_id": 1 } }
]
```
