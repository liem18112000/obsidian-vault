---
title: "Follow Up Points After Client Meeting"
created: 2026-04-16
updated: 2026-04-16
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49331011637/Follow+Up+Points+After+Client+Meeting
confluence_id: "49331011637"
confluence_path: "Team Kepler > Risk & Issues > Issues > Kunde Quickschild GmbH_eArchiv"
tags: [confluence]
---

# Follow Up Points After Client Meeting

*Confluence source · Team Kepler › Risk & Issues › Issues › Kunde Quickschild GmbH_eArchiv · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49331011637/Follow+Up+Points+After+Client+Meeting) · updated 2026-04-16*

## Follow up points

- Calculate the proportion of tenant who have more than 50k document as the issue becomes **critical above ~50K documents**. =\> Need Invisible support to get insight

- Focus on: **P1, P3, P4, P7:**

  - For P1, we need a internal discussion first =\> May need a performance test for the search API to get the current status. The result will decide how we deal with these problem

  - For P2, we need to apply the suggested solution with index =\> Need Invisible support to apply for the tenant in Mongo Database

  - For P4, we need the support and review from the team who own the `luz_docs_view_controller`

  - For P7, we need a internal discussion first =\> Decide the cache flow to optimize the service calls

Note: [Alvin Villanueva](https://axonivy.atlassian.net/wiki/people/712020:a8f84630-b701-4aff-b3f4-82349928cfc7?ref=confluence) Please add more section for follow up if we missing

## Problem Summary

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p>**ID**</p></th>
<th><p>**Root Cause**</p></th>
<th><p>**Service / Location**</p></th>
<th><p>**Mapped Solution**</p></th>
</tr>
&#10;<tr>
<td><p>**P1**</p></td>
<td><p>Every search counts ALL 128K documents (two separate DB queries instead of one). Doubles DB round-trips; full-collection count dominates query time</p></td>
<td><p>`luz_docs` → `luz_jsonstore` → MongoDB<br />
(`DocumentSearchService.java:56-70`)</p></td>
<td><p>Combine search + count into a single `$facet` pipeline (or estimated count for large sets)</p></td>
</tr>
<tr>
<td><p>**P3**</p></td>
<td><p>No database indexes — MongoDB scans entire collection on every query. Full collection scan on every filter/sort</p></td>
<td><p>MongoDB (via `luz_jsonstore`)</p></td>
<td><p>Create compound + single-field indexes on `deletionStatus`, `folderIds`, `securityClassCodes`, `createdDate`, etc.; add text index for keyword search</p></td>
</tr>
<tr>
<td><p>**P4**</p></td>
<td><p>50+ individual REST calls to fetch sender branding (one per sender). N+1 pattern; dominates archive open time</p></td>
<td><p>`luz_docs_view_controller` → `luztenant-service`<br />
(`LetterBuilderService.java:481-506`)</p></td>
<td><p>Cache sender configurations in existing `luz-cache` (key `sender_config_{id}`, TTL 5min)</p></td>
</tr>
<tr>
<td><p>**P7**</p></td>
<td><p>Caching system (`luz-cache` + Redis) exists but is not used for search. Repeated/identical queries re-hit Mongo. Sender branding is highly cacheable but re-fetched every open</p></td>
<td><p>`luz_docs`, `luz_docs_view_controller`<br />
(`DualCache.java`, `CacheService.java`)</p></td>
<td><p>**L3.** Extend `DualCache` to cover search counts, folder structures, badge counts</p></td>
</tr>
</tbody>
</table>
