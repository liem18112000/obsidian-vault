---
ai_hash: e155cd3de9c70848
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49608786001'
confluence_path: Team Kepler > Developer note
created: 2026-07-23
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- earchive
- performance
title: '[eArchive] Performance measurement & scalability assessment at 800000 documents'
type: source
updated: 2026-07-23
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49608786001/eArchive+Performance+measurement+scalability+assessment+at+800000+documents
---

# [eArchive] Performance measurement & scalability assessment at 800000 documents

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49608786001/eArchive+Performance+measurement+scalability+assessment+at+800000+documents) · updated 2026-07-23*

## Testing Specifications

- URL:

  - [https://performance.klara.tech/](https://performance.klara.tech/)

- Company / Profile:

  - LiemCompany

- Env **performance**

  - GKE `klara-performance`

  - Namespace `performance`

- Tenant `45b05710-b9d4-4d3e-935e-83c4525369fa`

  - Number of all document: 800000

  - Number of all folder: 10 (nested max level is 3)

> [!note]- Server Specs
>
> *Configured requests/limits (static, from the pod spec — not measured)*
>
>
> |  |  |  |  |
> |----|----|----|----|
> | **Service** | **Container** | **CPU request → limit** | **Memory request → limit** |
> | luz-docs | `luz-docs` | 1 → 15 cores | 10Gi → 10Gi |
> | luz-docs-view-controller | `luz-docs-view-controller` | 50m → 12 cores | 4Gi → 5Gi |
> | luz-jsonstore | `luz-jsonstore` | 500m → 12 cores | 512Mi → 10Gi |
> | mongo (rs0 shard) | `mongod` | 100m → 6 cores | 512Mi → 8Gi |
>
>
> *Observed live usage during the trial (all replicas pooled, min-max)*
>
>
> |                          |                |                      |                |
> |--------------------------|----------------|----------------------|----------------|
> | **Service**              | **Replicas**   | **CPU (millicores)** | **Memory**     |
> | luz-docs                 | 2 (`-0`, `-1`) | 31 – 874 m           | 1017 – 1912 Mi |
> | luz-docs-view-controller | 8              | 17 – 189 m           | 897 – 931 Mi   |
> | luz-jsonstore            | 3              | 91 – 240 m           | 1308 – 1457 Mi |
> | mongo (rs0 shard)        | 3              | 51 – 1151 m          | 1750 – 2976 Mi |
>
>
> ***Per-pod breakdown** (the interesting spread is between replicas, not just over time):*
>
>
> |  |  |  |  |
> |----|----|----|----|
> | **Pod** | **CPU min-max** | **Mem min-max** | **Note** |
> | `luz-docs-0` | 64 – 874 m | 1527 – 1912 Mi | took the real traffic (19h old pod) |
> | `luz-docs-1` | 31 – 178 m | 1017 – 1206 Mi | mostly idle — pod is 33 min old, likely didn't get load-balanced much traffic yet |
> | `luz-docs-view-controller-*748gg` | 28 – 80 m | 904 – 922 Mi | representative — all 8 replicas sit in a similar 17-189m/897-931Mi band, none stands out |
> | `luz-jsonstore-*6d6lm` | 99 – 160 m | 1308 – 1334 Mi | representative |
> | `luz-jsonstore-*fbxhz` | 125 – 240 m | 1430 – 1457 Mi | busiest jsonstore replica |
> | `luz-mongodb-cluster-rs0-0` | 51 – 130 m | 2159 – 2163 Mi |  |
> | `luz-mongodb-cluster-rs0-1` | **275 – 1151 m** | 2962 – 2976 Mi | clearly the hot replica — almost certainly the replica set **primary** (handles the writes + the slow `count`/`aggregate` scans) |
> | `luz-mongodb-cluster-rs0-2` | 52 – 148 m | 1750 – 1760 Mi |  |
>
>
> *Thread count (one representative pod per service)*
>
>
> |  |  |  |  |
> |----|----|----|----|
> | **Service** | **Representative pod** | **Threads min-max** | **Trend during trial** |
> | luz-docs | `luz-docs-0` (java pid) | 234 – 246 | flat |
> | luz-docs-view-controller | `luz-docs-view-controller-*748gg` (java pid) | 152 – 153 | flat |
> | luz-jsonstore | `luz-jsonstore-*6d6lm` (java pid) | **2538 – 2800** | **climbed steadily** (+262, ~10%) over the trial |
> | mongo (rs0-0) | `luz-mongodb-cluster-rs0-0` (pid 1, `mongod`) | 117 – 122 | flat |
>
>

## Test Conclusion

In Main page, User can free to interact with the page after 2 action done - 2 SECOND at most

- main pages: 47 document shown

- main pages: all folders shown

In folder page, User can free to interact with the page after 2 action done - 0.5 SECOND at most

- Folder choose: first item appeared

- Folder choose: all folders shown

In document detail page, User can free to interact with the page 0.5 SECOND at most

However, there are two point need to notices:

1.  The full-text search very slow (more than 2 minutes) or even timeout causing a blank page

2.  The document count for each folder (in main page and in a folder) have small chance to render slow (more than 30 seconds) or in rare case timeout causing the value show Zero files

## Test Cases

### Enter main pages:

- Access:

  - Log in with username/password: [liem18112000@gmail.com](mailto:liem18112000@gmail.com)/Liem025992382@1

  - If a popup appears, select company/profile: "LiemCompany"

  - On the home page's left sidebar (scroll down if needed), click the folder icon labeled "eArchive"

- Records:

  - Load time for 47 documents to appear (wait up to 5 minutes)

  - Load time for all folders to appear (wait up to 5 minutes)

  - Load time for each small number inside the folder icon "K Files". Record the time from page load start until the number K replaces the skeleton loader (spinner). Also record the value of K. If over 999, it shows "999+ Files".

  - Load time for the total document count "Documents (N)" (wait up to 5 minutes). Record the time from page load start until the number N replaces the skeleton loader (spinner). Also record the value of N.

  - Load time for the total folder count "Custom (M)" (wait up to 5 minutes). Record the time from page load start until the number M replaces the skeleton loader (spinner). Also record the value of M

### Scroll down to load more

- Access:

  - Same page as "Enter main pages"

  - Scroll down to the bottom loading bar

- Records:

  - Load time for next X documents

  - X values indicate how many documents load more

### Folder choose

- Access:

  - Same page as "Enter main pages"

  - Click each folder (except "LiemCompany" or "Trash")—choose up to 3 folders.

  - Skip the test if no folders.

- Records:

  - Load time for up to 47 documents (less if folder has fewer)

  - Load time for all folders (wait up to 5 mins)

  - Load time for each folder's "K Files" number: record time from page load start until number appears (spinner shows during load). Record K value; if over 999, display "999+ Files".

  - Load time for total document count "Documents (N)": record time from page load start until number appears (spinner during load). Record N value.

  - Load time for total folder count "Custom (M)": record time from page load start until number appears (spinner during load). Record M value.

### View details file

- Access:

  - Same page as "Enter earchive pages"

  - Wait for items to load (first time)

- Records:

  - Popup page load time

## Test Result (5 trials)

> [!note]- End to End test
>
>
> <table>
> <tbody>
> <tr>
> <th><p>**case**</p></th>
> <th><p>**metric**</p></th>
> <th><p>**Load skeleton**</p></th>
> <th><p>**run 1**</p></th>
> <th><p>**run 2**</p></th>
> <th><p>**run 3**</p></th>
> <th><p>**run 4**</p></th>
> <th><p>**run 5**</p></th>
> <th><p>**min**</p></th>
> <th><p>**avg**</p></th>
> <th><p>**max**</p></th>
> </tr>
> &#10;<tr>
> <td rowspan="5"><p>Enter main pages</p></td>
> <td><p>Load 47 documents</p></td>
> <td><p>No</p></td>
> <td><p>1736 ms</p></td>
> <td><p>1244 ms</p></td>
> <td><p>1090 ms</p></td>
> <td><p>1600 ms</p></td>
> <td><p>981 ms</p></td>
> <td><p>**981 ms**</p></td>
> <td><p>**1330 ms**</p></td>
> <td><p>**1736 ms**</p></td>
> </tr>
> <tr>
> <td><p>All folders shown</p></td>
> <td><p>No</p></td>
> <td><p>2176 ms</p></td>
> <td><p>1513 ms</p></td>
> <td><p>1416 ms</p></td>
> <td><p>1956 ms</p></td>
> <td><p>1280 ms</p></td>
> <td><p>**1280 ms**</p></td>
> <td><p>**1668 ms**</p></td>
> <td><p>**2176 ms**</p></td>
> </tr>
> <tr>
> <td><p>Total document count</p></td>
> <td><p>Yes</p></td>
> <td><p>55668 ms</p></td>
> <td><p>69477 ms</p></td>
> <td><p>97840 ms</p></td>
> <td><p>71486 ms</p></td>
> <td><p>59794 ms</p></td>
> <td><p>**55668 ms**</p></td>
> <td><p>**70853 ms**</p></td>
> <td><p>**97840 ms**</p></td>
> </tr>
> <tr>
> <td><p>Total folder count</p></td>
> <td><p>Yes</p></td>
> <td><p>1736 ms</p></td>
> <td><p>1244 ms</p></td>
> <td><p>1090 ms</p></td>
> <td><p>1600 ms</p></td>
> <td><p>981 ms</p></td>
> <td><p>**981 ms**</p></td>
> <td><p>**1330 ms**</p></td>
> <td><p>**1736 ms**</p></td>
> </tr>
> <tr>
> <td><p>All folder-document count</p></td>
> <td><p>Yes</p></td>
> <td><p>56075 ms</p></td>
> <td><p>3717 ms</p></td>
> <td><p>62571 ms</p></td>
> <td><p>4668 ms</p></td>
> <td><p>3449 ms</p></td>
> <td><p>**3449 ms**</p></td>
> <td><p>**26096 ms**</p></td>
> <td><p>**62571 ms**</p></td>
> </tr>
> <tr>
> <td><p>Scroll down to load more</p></td>
> <td><p>Scroll first new item</p></td>
> <td><p>Yes</p></td>
> <td><p>2139 ms</p></td>
> <td><p>1403 ms</p></td>
> <td><p>1525 ms</p></td>
> <td><p>2113 ms</p></td>
> <td><p>1366 ms</p></td>
> <td><p>**1366 ms**</p></td>
> <td><p>**1709 ms**</p></td>
> <td><p>**2139 ms**</p></td>
> </tr>
> <tr>
> <td><p>View details file</p></td>
> <td><p>View details popup</p></td>
> <td><p>No</p></td>
> <td><p>239 ms</p></td>
> <td><p>173 ms</p></td>
> <td><p>343 ms</p></td>
> <td><p>556 ms</p></td>
> <td><p>212 ms</p></td>
> <td><p>**173 ms**</p></td>
> <td><p>**305 ms**</p></td>
> <td><p>**556 ms**</p></td>
> </tr>
> <tr>
> <td rowspan="3"><p>Folder choose</p></td>
> <td><p>In folder #1: first item appeared</p></td>
> <td><p>No</p></td>
> <td><p>193 ms</p></td>
> <td><p>200 ms</p></td>
> <td><p>344 ms</p></td>
> <td><p>304 ms</p></td>
> <td><p>378 ms</p></td>
> <td><p>**193 ms**</p></td>
> <td><p>**284 ms**</p></td>
> <td><p>**378 ms**</p></td>
> </tr>
> <tr>
> <td><p>In folder: all folders shown</p></td>
> <td><p>Yes</p></td>
> <td><p>236 ms</p></td>
> <td><p>245 ms</p></td>
> <td><p>408 ms</p></td>
> <td><p>395 ms</p></td>
> <td><p>487 ms</p></td>
> <td><p>**236 ms**</p></td>
> <td><p>**354 ms**</p></td>
> <td><p>**487 ms**</p></td>
> </tr>
> <tr>
> <td><p>In folder: all folder-document count</p></td>
> <td><p>Yes</p></td>
> <td><p>61262 ms</p></td>
> <td><p>61373 ms</p></td>
> <td><p>61404 ms</p></td>
> <td><p>62307 ms</p></td>
> <td><p>61495 ms</p></td>
> <td><p>**61262 ms**</p></td>
> <td><p>**61568 ms**</p></td>
> <td><p>**62307 ms**</p></td>
> </tr>
> </tbody>
> </table>
>
>

> [!note]- API performance checks
>
>
> |  |  |  |  |  |  |  |  |  |
> |----|----|----|----|----|----|----|----|----|
> | **service / operation** | **run 1** | **run 2** | **run 3** | **run 4** | **run 5** | **min avg** | **avg of avg** | **max** |
> | `luz-docs` `GET documents/{id}` | 68/225 ms (71) | — | — | — | — | **68 ms** | **68 ms** | **225 ms** |
> | `luz-docs` `GET documents/{id}/files/thumbnail128` | 29/46 ms (71) | — | — | — | — | **29 ms** | **29 ms** | **46 ms** |
> | `luz-docs` `POST documents/search` | 78671/168351 ms (17) | 117633/254075 ms (11) | 104235/187493 ms (11) | 99840/179322 ms (11) | 102812/245338 ms (15) | **78671 ms** | **100638 ms** | **254075 ms** |
> | `luz-docs` `POST documents/count` | 57178/98954 ms (17) | 73328/99944 ms (8) | 61867/95693 ms (13) | 68697/90318 ms (13) | 67023/90276 ms (11) | **57178 ms** | **65619 ms** | **99944 ms** |
> | `luz-docs` `POST folders/search` | 145/506 ms (7) | 115/157 ms (2) | 133/224 ms (5) | 111/263 ms (5) | 59/109 ms (6) | **59 ms** | **113 ms** | **506 ms** |
> | `luz-docs` `GET folders/{id}` | 34/36 ms (2) | — | — | — | — | **34 ms** | **34 ms** | **36 ms** |
> | `luz-docs` `PATCH folders/{id}` | 185/185 ms (1) | — | — | — | — | **185 ms** | **185 ms** | **185 ms** |
> | `luz-jsonstore` `POST materializeCascade` | 23/195 ms (372) | 18/228 ms (42) | 22/152 ms (58) | 8/94 ms (58) | 6/10 ms (64) | **6 ms** | **15 ms** | **228 ms** |
> | `luz-jsonstore` `POST documents/count` | 36246/185780 ms (292) | 49399/157450 ms (113) | 44875/156211 ms (183) | 50356/184748 ms (182) | 49986/150767 ms (161) | **36246 ms** | **46172 ms** | **185780 ms** |
> | `luz-jsonstore` `POST luz_docs_migration_campaign` | 53/906 ms (85) | 8/16 ms (19) | 12/27 ms (28) | 8/16 ms (28) | 7/17 ms (31) | **7 ms** | **18 ms** | **906 ms** |
> | `luz-jsonstore` `GET documents/{id}` | 43/197 ms (71) | — | — | — | — | **43 ms** | **43 ms** | **197 ms** |
> | `luz-jsonstore` `POST documents` | 66069/167338 ms (15) | 114935/254051 ms (10) | 100647/187463 ms (10) | 93129/179294 ms (10) | 100814/245309 ms (13) | **66069 ms** | **95119 ms** | **254051 ms** |
> | `luz-jsonstore` `POST folders` | 77/467 ms (7) | 11/13 ms (2) | 16/30 ms (5) | 9/13 ms (5) | 9/17 ms (6) | **9 ms** | **24 ms** | **467 ms** |
> | `luz-jsonstore` `POST folders/aggregate` | 33/147 ms (7) | 71/119 ms (2) | 84/180 ms (5) | 69/225 ms (5) | 16/65 ms (6) | **16 ms** | **55 ms** | **225 ms** |
> | `luz-jsonstore` `POST enrichmentstatus` | 461/881 ms (3) | 6/6 ms (2) | 11/11 ms (2) | 7/7 ms (2) | 5/6 ms (4) | **5 ms** | **98 ms** | **881 ms** |
> | `luz-jsonstore` `GET folders/{id}` | 15/19 ms (3) | — | — | — | — | **15 ms** | **15 ms** | **19 ms** |
> | `luz-jsonstore` `POST documents/aggregate` | 168214/168317 ms (2) | 143745/143745 ms (1) | 138911/138911 ms (1) | 165790/165790 ms (1) | 114949/137159 ms (2) | **114949 ms** | **146322 ms** | **168317 ms** |
> | `luz-jsonstore` `POST profile` | 1282/1282 ms (1) | 8/8 ms (1) | 8/8 ms (1) | 9/9 ms (1) | 6/6 ms (1) | **6 ms** | **263 ms** | **1282 ms** |
> | `luz-jsonstore` `POST pin-folder` | 8/8 ms (1) | — | — | — | 7/7 ms (1) | **7 ms** | **8 ms** | **8 ms** |
> | `luz-jsonstore` `PATCH folders/{id}` | 155/155 ms (1) | — | — | — | — | **155 ms** | **155 ms** | **155 ms** |
>
>

%% ai-graph-start %%

**Related notes:**
- [[eArchive Performance measurement & scalability assessment at 2.2M documents]]
- [[eArchive request flow and log correlation (perf)]]
- [[Count Fan-out (K) Benchmark on Performance Env]]
- [[eArchive 800k bottleneck is view-controller not K]]
- [[luz-docs documentscount is ~130s on an 800k tenant — the 16-shard fan-out, not counting, is the bottleneck]]

%% ai-graph-end %%