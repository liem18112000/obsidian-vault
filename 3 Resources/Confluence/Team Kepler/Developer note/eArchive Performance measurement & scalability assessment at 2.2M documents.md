---
ai_hash: 3f56c38540c8a0ce
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49613176863'
confluence_path: Team Kepler > Developer note > [eArchive] Performance measurement
  & scalability assessment at 800000 documents
created: 2026-07-24
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- earchive
- performance
title: '[eArchive] Performance measurement &amp; scalability assessment at 2.2M documents'
type: source
updated: 2026-07-24
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49613176863/eArchive+Performance+measurement+amp+scalability+assessment+at+2.2M+documents
---

# [eArchive] Performance measurement &amp; scalability assessment at 2.2M documents

*Confluence source · Team Kepler › Developer note › [eArchive] Performance measurement & scalability assessment at 800000 documents · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49613176863/eArchive+Performance+measurement+amp+scalability+assessment+at+2.2M+documents) · updated 2026-07-24*

## [eArchive] Performance measurement & scalability assessment at 2.2M documents
This page consolidates the original assessment and the newer trace/report output for the same tenant and dataset so the results can be compared in one place.

> [!note]
>
>
> **Sensitive access information removed:** the original source contained direct login credentials. They are intentionally not reproduced on this page.
>
>

> [!info]
>
>
>
> **Test target**
> **URL:** [https://performance.klara.tech/](https://performance.klara.tech/)
> **Company / Profile:** LiemCompany
> **Environment:** performance
> **GKE cluster:** `klara-performance`
> **Namespace:** `performance`
> **Tenant:** `45b05710-b9d4-4d3e-935e-83c4525369fa`
> **Dataset size:** 2,206,000 documents
> **Folder count:** 10 folders, nested max level 3
>
>

## Executive summary

The tenant remains **usable for primary browsing flows** with the performance dataset now at 2,206,000 documents, but the system shows a clear split between **fast list/detail rendering** and **very slow count/search operations**.

- **Main/root page content rendering is generally fast:** first documents and folder rows appear in about 2.5 seconds

- **Folder navigation is fast:** entering a folder and opening a document detail popup are typically sub-second.

- **The major bottleneck is count/search behavior:** total document counts, folder document badges, and full-text search can take tens of seconds to minutes, and some calls fail with HTTP 500.

- **Operational evidence points to backend query pressure rather than front-end rendering limits:** front-end paint timings are acceptable, while API timings for `documents/count`, `documents/search`, and related aggregate paths are extreme and unstable.

> [!note]
>
>
>
> **Key risk:** users can start working with the page quickly, but delayed or failed counts can leave the UI showing loading skeletons for around 60 seconds or even incorrect zero-value folder badges.
>
>

## Test conclusion

**Main page:** users can generally interact with the page after the core content appears, with documents and folders loading quickly.

- Main page document list: up to 47 visible quickly

- Main page folder list: all folders shown quickly

**Folder page:** users can generally interact with the folder content in under 0.5 seconds once navigation completes.

- Folder selection: first item appears quickly

- Folder selection: all child folders shown quickly

**Document detail page:** the detail popup is responsive and generally appears in under 0.5 seconds in the original assessment, with the newer trace averaging slightly above that but still fast in practical use.

However, there are two important issues:

1.  **Full-text search is very slow** and can exceed 2 minutes or timeout, causing blank or failed states.

2.  **Document counts per folder and overall document totals are unstable and slow**, often taking more than 30 seconds and in some cases timing out, which can result in zero-value badges or unresolved counters.

## Testing specifications

### Environment and scope

- **System under test:** eArchive on the performance environment

- **Tenant:** `45b05710-b9d4-4d3e-935e-83c4525369fa`

- **Dataset:** 2,206,000 total documents

- **Folder model:** 10 folders, maximum nesting depth of 3

- **Test style:** end-to-end UI timing, browser-side trace capture, and backend API timing review

### Core test cases

1.  **Enter main page**

    - Measure time until documents appear

    - Measure time until all folders appear

    - Measure time until each folder badge resolves

    - Measure time until total document count resolves

    - Measure time until total folder count resolves

2.  **Scroll down to load more**

    - Measure time until next batch begins appearing

    - Measure how many additional items load

3.  **Choose folders**

    - Open up to 3 folders except root/company and trash

    - Measure child item rendering, child folders, and folder badge resolution

4.  **Open file details**

    - Measure popup/dialog load time

## Server specifications

*Configured requests/limits from pod specs; these are configuration values, not measured runtime usage.*

|  |  |  |  |
|----|----|----|----|
| Service | Container | CPU request → limit | Memory request → limit |
| luz-docs | `luz-docs` | 1 → 15 cores | 10Gi → 10Gi |
| luz-docs-view-controller | `luz-docs-view-controller` | 50m → 12 cores | 4Gi → 5Gi |
| luz-jsonstore | `luz-jsonstore` | 500m → 12 cores | 512Mi → 10Gi |
| mongo (rs0 shard) | `mongod` | 100m → 6 cores | 512Mi → 8Gi |

*Observed live usage during the original trial; pooled across replicas, min-max.*

|                          |          |                  |                |
|--------------------------|----------|------------------|----------------|
| Service                  | Replicas | CPU (millicores) | Memory         |
| luz-docs                 | 2        | 31 – 874 m       | 1017 – 1912 Mi |
| luz-docs-view-controller | 8        | 17 – 189 m       | 897 – 931 Mi   |
| luz-jsonstore            | 3        | 91 – 240 m       | 1308 – 1457 Mi |
| mongo (rs0 shard)        | 3        | 51 – 1151 m      | 1750 – 2976 Mi |

### Per-pod observations

|  |  |  |  |
|----|----|----|----|
| Pod | CPU min-max | Mem min-max | Note |
| `luz-docs-0` | 64 – 874 m | 1527 – 1912 Mi | Took the real traffic; older pod with clear active load. |
| `luz-docs-1` | 31 – 178 m | 1017 – 1206 Mi | Mostly idle; likely received much less load due to recent age. |
| `luz-docs-view-controller-*748gg` | 28 – 80 m | 904 – 922 Mi | Representative; all 8 replicas were in a similar range. |
| `luz-jsonstore-*6d6lm` | 99 – 160 m | 1308 – 1334 Mi | Representative replica. |
| `luz-jsonstore-*fbxhz` | 125 – 240 m | 1430 – 1457 Mi | Busiest jsonstore replica during the trial. |
| `luz-mongodb-cluster-rs0-0` | 51 – 130 m | 2159 – 2163 Mi | Steady secondary-like behavior. |
| `luz-mongodb-cluster-rs0-1` | **275 – 1151 m** | 2962 – 2976 Mi | Clearly the hot replica, very likely the replica set primary handling writes and expensive scans. |
| `luz-mongodb-cluster-rs0-2` | 52 – 148 m | 1750 – 1760 Mi | Lower steady usage. |

### Thread count observations

|  |  |  |  |
|----|----|----|----|
| Service | Representative pod | Threads min-max | Trend during trial |
| luz-docs | `luz-docs-0` | 234 – 246 | Flat |
| luz-docs-view-controller | `luz-docs-view-controller-*748gg` | 152 – 153 | Flat |
| luz-jsonstore | `luz-jsonstore-*6d6lm` | **2538 – 2800** | **Climbed steadily** by about 10% |
| mongo (rs0-0) | `luz-mongodb-cluster-rs0-0` | 117 – 122 | Flat |

## End-to-end results from the original 5-trial test

|  |  |  |  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|----|----|----|
| Case | Metric | Load skeleton | Run 1 | Run 2 | Run 3 | Run 4 | Run 5 | Min | Avg | Max |
| Enter main pages | Load 47 documents | No | 1736 ms | 1244 ms | 1090 ms | 1600 ms | 981 ms | **981 ms** | **1330 ms** | **1736 ms** |
| Enter main pages | All folders shown | No | 2176 ms | 1513 ms | 1416 ms | 1956 ms | 1280 ms | **1280 ms** | **1668 ms** | **2176 ms** |
| Enter main pages | Total document count | Yes | 55668 ms | 69477 ms | 97840 ms | 71486 ms | 59794 ms | **55668 ms** | **70853 ms** | **97840 ms** |
| Enter main pages | Total folder count | Yes | 1736 ms | 1244 ms | 1090 ms | 1600 ms | 981 ms | **981 ms** | **1330 ms** | **1736 ms** |
| Enter main pages | All folder-document count | Yes | 56075 ms | 3717 ms | 62571 ms | 4668 ms | 3449 ms | **3449 ms** | **26096 ms** | **62571 ms** |
| Scroll down to load more | Scroll first new item | Yes | 2139 ms | 1403 ms | 1525 ms | 2113 ms | 1366 ms | **1366 ms** | **1709 ms** | **2139 ms** |
| View details file | View details popup | No | 239 ms | 173 ms | 343 ms | 556 ms | 212 ms | **173 ms** | **305 ms** | **556 ms** |
| Folder choose | In folder \#1: first item appeared | No | 193 ms | 200 ms | 344 ms | 304 ms | 378 ms | **193 ms** | **284 ms** | **378 ms** |
| Folder choose | In folder: all folders shown | Yes | 236 ms | 245 ms | 408 ms | 395 ms | 487 ms | **236 ms** | **354 ms** | **487 ms** |
| Folder choose | In folder: all folder-document count | Yes | 61262 ms | 61373 ms | 61404 ms | 62307 ms | 61495 ms | **61262 ms** | **61568 ms** | **62307 ms** |

## Front-end trace summary from the newer 3-run report

> [!info]
>
>
>
> **Trace run window:** all three browser-based trace runs were executed on 24 Jul 2026. Each run ended with a timeout note indicating the page remained under load while waiting for slow counters to resolve.
>
>

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| Metric | Run 1 | Run 2 | Run 3 | Min | Avg | Max |
| Page load (nav click → page usable marker) | 8784 ms | 27714 ms | 4523 ms | **4523 ms** | **13674 ms** | **27714 ms** |
| First item appeared | 2749 ms | 1196 ms | 1780 ms | **1196 ms** | **1908 ms** | **2749 ms** |
| 47 items shown | 2749 ms | 1196 ms | 1780 ms | **1196 ms** | **1908 ms** | **2749 ms** |
| All folders shown | 3069 ms | 1611 ms | 2121 ms | **1611 ms** | **2267 ms** | **3069 ms** |
| Documents (N) resolved | Not resolved | Not resolved | Not resolved | **Not resolved** | **Not resolved** | **Not resolved** |
| Custom (M) resolved | 2749 ms | 1196 ms | 1780 ms | **1196 ms** | **1908 ms** | **2749 ms** |
| All folder-badge skeletons resolved | 63595 ms | 62399 ms | 63436 ms | **62399 ms** | **63143 ms** | **63595 ms** |
| Scroll: first new item | 3468 ms | 2127 ms | 2997 ms | **2127 ms** | **2864 ms** | **3468 ms** |
| View details popup | 713 ms | 370 ms | 446 ms | **370 ms** | **510 ms** | **713 ms** |
| Folder \#1: first item appeared | 457 ms | 352 ms | 317 ms | **317 ms** | **375 ms** | **457 ms** |
| Folder \#1: all folders shown | 549 ms | 435 ms | 381 ms | **381 ms** | **455 ms** | **549 ms** |
| Folder \#1: all folder-badge skeletons resolved | 61829 ms | 61142 ms | 61483 ms | **61142 ms** | **61485 ms** | **61829 ms** |

### Interpretation of the trace

- **Visible content is not the main problem.** Document rows and folder rows render quickly enough for user interaction.

- **Counters are the dominant UX problem.** The document total never resolved in the three newer runs, and folder badges stayed in skeleton/loading state for about 61–64 seconds.

- **Folder navigation remains healthy.** The first folder tested consistently showed items in about 0.3–0.5 seconds.

- **Load-more behavior is acceptable but not instant.** The next batch of 48 items appeared in about 2.1–3.5 seconds.

## Browser performance indicators from the trace

|  |  |  |  |  |
|----|----|----|----|----|
| Metric | Run 1 | Run 2 | Run 3 | Observation |
| First Contentful Paint | 2584 ms | 1100 ms | 1028 ms | Reasonable; initial rendering starts comparatively early. |
| Largest Contentful Paint | 5532 ms | 3208 ms | 3940 ms | Moderate; not ideal, but not the dominant failure mode. |
| Total Blocking Time | 1425 ms | 940 ms | 285 ms | Some main-thread blocking exists, especially in run 1 and run 2. |
| CLS | 0.028 | 0.068 | 0.022 | Layout stability is acceptable. |
| Long task count | 19 | 21 | 16 | Front-end work is present, but still secondary versus backend delays. |

The trace also showed repeated thumbnail image requests around 0.8–1.2 seconds in one run and multiple UI/XHR resources in the 0.35–0.60 second range. These contribute to perceived slowness, but they do not explain the extreme 60-second-plus waits for counters.

## API performance checks from the original 5-trial assessment

|  |  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|----|
| Service / operation | Run 1 | Run 2 | Run 3 | Run 4 | Run 5 | Min avg | Avg of avg | Max |
| `luz-docsGET documents/{id}` | 68/225 ms (71) |  |  |  |  | **68 ms** | **68 ms** | **225 ms** |
| `luz-docsGET documents/{id}/files/thumbnail128` | 29/46 ms (71) |  |  |  |  | **29 ms** | **29 ms** | **46 ms** |
| `luz-docsPOST documents/search` | 78671/168351 ms (17) | 117633/254075 ms (11) | 104235/187493 ms (11) | 99840/179322 ms (11) | 102812/245338 ms (15) | **78671 ms** | **100638 ms** | **254075 ms** |
| `luz-docsPOST documents/count` | 57178/98954 ms (17) | 73328/99944 ms (8) | 61867/95693 ms (13) | 68697/90318 ms (13) | 67023/90276 ms (11) | **57178 ms** | **65619 ms** | **99944 ms** |
| `luz-docsPOST folders/search` | 145/506 ms (7) | 115/157 ms (2) | 133/224 ms (5) | 111/263 ms (5) | 59/109 ms (6) | **59 ms** | **113 ms** | **506 ms** |
| `luz-docsGET folders/{id}` | 34/36 ms (2) |  |  |  |  | **34 ms** | **34 ms** | **36 ms** |
| `luz-docsPATCH folders/{id}` | 185/185 ms (1) |  |  |  |  | **185 ms** | **185 ms** | **185 ms** |
| `luz-jsonstorePOST materializeCascade` | 23/195 ms (372) | 18/228 ms (42) | 22/152 ms (58) | 8/94 ms (58) | 6/10 ms (64) | **6 ms** | **15 ms** | **228 ms** |
| `luz-jsonstorePOST documents/count` | 36246/185780 ms (292) | 49399/157450 ms (113) | 44875/156211 ms (183) | 50356/184748 ms (182) | 49986/150767 ms (161) | **36246 ms** | **46172 ms** | **185780 ms** |
| `luz-jsonstorePOST luz_docs_migration_campaign` | 53/906 ms (85) | 8/16 ms (19) | 12/27 ms (28) | 8/16 ms (28) | 7/17 ms (31) | **7 ms** | **18 ms** | **906 ms** |
| `luz-jsonstoreGET documents/{id}` | 43/197 ms (71) |  |  |  |  | **43 ms** | **43 ms** | **197 ms** |
| `luz-jsonstorePOST documents` | 66069/167338 ms (15) | 114935/254051 ms (10) | 100647/187463 ms (10) | 93129/179294 ms (10) | 100814/245309 ms (13) | **66069 ms** | **95119 ms** | **254051 ms** |
| `luz-jsonstorePOST folders` | 77/467 ms (7) | 11/13 ms (2) | 16/30 ms (5) | 9/13 ms (5) | 9/17 ms (6) | **9 ms** | **24 ms** | **467 ms** |
| `luz-jsonstorePOST folders/aggregate` | 33/147 ms (7) | 71/119 ms (2) | 84/180 ms (5) | 69/225 ms (5) | 16/65 ms (6) | **16 ms** | **55 ms** | **225 ms** |
| `luz-jsonstorePOST enrichmentstatus` | 461/881 ms (3) | 6/6 ms (2) | 11/11 ms (2) | 7/7 ms (2) | 5/6 ms (4) | **5 ms** | **98 ms** | **881 ms** |
| `luz-jsonstoreGET folders/{id}` | 15/19 ms (3) |  |  |  |  | **15 ms** | **15 ms** | **19 ms** |
| `luz-jsonstorePOST documents/aggregate` | 168214/168317 ms (2) | 143745/143745 ms (1) | 138911/138911 ms (1) | 165790/165790 ms (1) | 114949/137159 ms (2) | **114949 ms** | **146322 ms** | **168317 ms** |
| `luz-jsonstorePOST profile` | 1282/1282 ms (1) | 8/8 ms (1) | 8/8 ms (1) | 9/9 ms (1) | 6/6 ms (1) | **6 ms** | **263 ms** | **1282 ms** |
| `luz-jsonstorePOST pin-folder` | 8/8 ms (1) |  |  |  | 7/7 ms (1) | **7 ms** | **8 ms** | **8 ms** |
| `luz-jsonstorePATCH folders/{id}` | 155/155 ms (1) |  |  |  |  | **155 ms** | **155 ms** | **155 ms** |

## Backend API timing across the newer 3-run trace

*Per-run cell represents avg/max milliseconds and call count for that operation within the run.*

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| Service / operation | Run 1 | Run 2 | Run 3 | Min avg | Avg of avg | Max |
| `luz-jsonstoreGET documents/{id}` | 16/56 ms (47) |  |  | **16 ms** | **16 ms** | **56 ms** |
| `luz-jsonstorePOST materializeCascade` | 11/42 ms (40) |  | 30/57 ms (10) | **11 ms** | **21 ms** | **57 ms** |
| `luz-jsonstorePOST luz_docs_migration_campaign` | 60/249 ms (14) |  | 30/32 ms (2) | **30 ms** | **45 ms** | **249 ms** |
| `luz-jsonstorePOST documents/count` | 16/62 ms (11) | 817453/892819 ms (29) | 58/58 ms (1) | **16 ms** | **272509 ms** | **892819 ms** |
| `luz-jsonstorePOST folders` | 52/240 ms (6) | 14/14 ms (2) | 61/96 ms (2) | **14 ms** | **42 ms** | **240 ms** |
| `luz-jsonstorePOST folders/aggregate` | 40/164 ms (5) | 85/138 ms (2) | 228/278 ms (2) | **40 ms** | **118 ms** | **278 ms** |
| `luz-jsonstorePOST documents` | 222/369 ms (4) | 388/585 ms (3) | 750/1321 ms (3) | **222 ms** | **453 ms** | **1321 ms** |
| `luz-jsonstorePOST enrichmentstatus` | 173/275 ms (3) | 55751/55751 ms (1) | 120073/120073 ms (1) | **173 ms** | **58666 ms** | **120073 ms** |
| `luz-jsonstorePOST expirabledocuments` | 236/236 ms (1) |  |  | **236 ms** | **236 ms** | **236 ms** |
| `luz-jsonstorePOST profile` | 6/6 ms (1) | 40165/56704 ms (2) | 60155/120277 ms (2) | **6 ms** | **33442 ms** | **120277 ms** |
| `luz-docsPOST documents/count` | Collection failed | 732595/899011 ms (4), 500 status | 997401/1020294 ms (3), 500 status | **732595 ms** | **864998 ms** | **1020294 ms** |
| `luz-docsPOST documents/search` | Collection failed | 75495/300139 ms (4), mixed 200/500 | 210382/300140 ms (10), mixed 200/500 | **75495 ms** | **142939 ms** | **300140 ms** |
| `luz-docsPOST folders/search` | Collection failed | 136/187 ms (2) | 328/410 ms (2) | **136 ms** | **232 ms** | **410 ms** |

## Cross-source findings

### What is consistent between the original page and the newer report

- **Main list rendering is consistently fast.** Both datasets show document rows becoming visible in about 1–3 seconds.

- **Folder entry remains fast.** Sub-second folder rendering is consistent across both measurements.

- **Detail popup remains fast.** Original average was 305 ms; newer trace average was 510 ms.

- **Count-related calls remain the weakest path.** This is the strongest pattern across all evidence.

### What the newer report adds

- **More explicit evidence of unresolved totals:** in the three trace runs, the main `Documents (N)` counter did not resolve at all.

- **Timeout behavior is more visible:** all three trace runs ended with timeout notes while the page was still under load.

- **HTTP 500 responses were observed** on `luz-docs` `POST documents/count` and some `POST documents/search` calls.

- **Browser rendering metrics are acceptable relative to backend latency,** reinforcing that the bottleneck is not primarily front-end paint or layout work.

## Assessment

With the performance tenant now at 2,206,000 documents, the eArchive solution demonstrates **good interactive performance for content browsing** but **poor scalability for count and search-heavy operations**. The user can usually start working quickly because documents and folders appear early, but the overall experience is degraded by long-running counters, unresolved skeleton loaders, and intermittent backend failures.

The practical outcome is:

- `GOOD` browsing existing documents and navigating folders

- `GOOD` opening document details

- `AT RISK` infinite-scroll/load-more responsiveness under load

- `CRITICAL` full-text search responsiveness and reliability

- `CRITICAL` total counts and folder badge counts

## Recommended focus areas

1.  **Prioritize count-query optimization.** The strongest user-visible pain comes from folder badge counts and total document counters.

2.  **Investigate** `documents/search` **and** `documents/count` **execution plans** in both `luz-docs` and `luz-jsonstore` paths.

3.  **Review Mongo primary pressure**, especially on the hot replica observed in the original trial.

4.  **Examine why counters can return or display zero** when the backend path is slow or times out.

5.  **Add UI fallbacks for delayed counters** so core content remains trustworthy even when counts are unavailable.

6.  **Consider caching, pre-aggregation, or asynchronous badge calculation** for counts displayed on the main and folder pages.

> [!note]- Original access and execution flow used in testing
>
> Open the performance environment, authenticate, select the LiemCompany profile if prompted, and navigate through the left sidebar to eArchive. From there, measure main-page list rendering, folder rendering, document and folder totals, folder badge resolution, infinite scroll, folder navigation, and detail popup loading.
>

%% ai-graph-start %%

**Related notes:**
- [[eArchive Performance measurement & scalability assessment at 800000 documents]]
- [[eArchive Performance — Executive Overview]]
- [[eArchive Performance — Detail Overview]]
- [[eArchive request flow and log correlation (perf)]]
- [[eArchive 800k bottleneck is view-controller not K]]

%% ai-graph-end %%