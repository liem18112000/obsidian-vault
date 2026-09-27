---
title: "How to execute API to create sync event for post from tenant schemas to public table"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47057404155/How+to+execute+API+to+create+sync+event+for+post+from+tenant+schemas+to+public+table
space: "HACKA"
topic: programming
relevance: 0.832
depth: 3
updated: 2022-02-17
attachments: 0
tags:
  - confluence
  - programming
  - space/hacka
---

# How to execute API to create sync event for post from tenant schemas to public table

> [!info] Imported from Confluence
> Space **HACKA** · updated 2022-02-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47057404155/How+to+execute+API+to+create+sync+event+for+post+from+tenant+schemas+to+public+table)
> Relevance 0.832 · topic `programming`

1.  Curl command

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e3179d43-aeef-41be-9836-26aab4dc99d2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-H "runAsRole: $CH_KLARA_RUN_AS_TOKEN" \
-d "{\"dryRunMode\":false,\"clearOldEvent\":false,\"tenantIds\":[]}" \
http://localhost:8086/luz_marketing/api/post-sync-event/sync
```

</div>

</div>

Formatted request body

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="790e0715-ab0b-4957-976b-9c8c732e4708" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "dryRunMode": false,
  "clearOldEvent": false,
  "tenantIds": []
}
```

</div>

</div>

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p><strong>Body field</strong></p></td>
<td><p><strong>Description</strong></p></td>
</tr>
<tr>
<td><p>dryRunMode</p></td>
<td><p>Optional. If this flag is true, there are no changes applied to database.</p>
<p>Default is false.</p></td>
</tr>
<tr>
<td><p>clearOldEvent</p></td>
<td><p>Optional. If this flag is true, old data generated from previous execution will be deleted before inserting new data into database.</p>
<p>Default is false.</p></td>
</tr>
<tr>
<td><p>tenantIds</p></td>
<td><p>Optional. List of specific tenant id. If it is null or empty, all tenant will affected.</p>
<p>Default is null.</p></td>
</tr>
</tbody>
</table>

</div>

2\. Result

The response of this execution

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="efd9ed6b-c496-4298-9cf9-91f8a34b6837" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "startedExecutionAt": "2022-01-27 13:42:37.229+0700",
    "stoppedExecutionAt": "2022-01-27 13:42:48.017+0700",
    "errorMessage": null,
    "request": {
        "dryRunMode": true,
        "tenantIds": [],
        "clearOldEvent": false
    },
    "totalPostSyncEvents": 11,
    "top100PostSyncEvents": [
        {
            "startDate": "2022-09-13 10:20:00",
            "tenantId": "321538e1-95a3-4011-aa2e-fbf8daaa9685"
        },
        {
            "startDate": "2022-02-14 16:59:00",
            "tenantId": "7f523413-5078-472f-a09e-3569536540e5"
        },
        {
            "startDate": "2025-03-03 16:59:00",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        },
        {
            "startDate": "2021-09-13 16:59:00",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        },
        {
            "startDate": "2021-09-19 10:42:00",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        },
        {
            "startDate": "2021-09-10 03:34:21",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        },
        {
            "startDate": "2021-09-12 04:21:00",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        },
        {
            "startDate": "2025-03-03 16:59:00",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        },
        {
            "startDate": "2025-03-03 16:59:00",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        },
        {
            "startDate": "2021-09-30 03:03:00",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        },
        {
            "startDate": "2022-01-27 06:42:47",
            "tenantId": "f16ba04c-87f6-4966-8d7a-de17782ef304"
        }
    ]
}
```

</div>

</div>

If there are any non existed specific tenant

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4a5cbeb0-422d-4b7c-a74e-707217e75678" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "startedExecutionAt": "2022-02-15 12:23:08.578+0700",
    "stoppedExecutionAt": "2022-02-15 12:23:09.106+0700",
    "errorMessage": "Schema does not exist [s_321538e1_95a3_4011_aa2e_fbf8daaa968]",
    "request": {
        "dryRunMode": true,
        "tenantIds": [
            "321538e1-95a3-4011-aa2e-fbf8daaa968",
            "7f523413-5078-472f-a09e-3569536540e5"
        ],
        "clearOldEvent": false
    },
    "totalPostSyncEvents": 1,
    "top100PostSyncEvents": [
        {
            "startDate": "2022-02-14 16:59:00",
            "tenantId": "7f523413-5078-472f-a09e-3569536540e5"
        }
    ]
}
```

</div>

</div>
