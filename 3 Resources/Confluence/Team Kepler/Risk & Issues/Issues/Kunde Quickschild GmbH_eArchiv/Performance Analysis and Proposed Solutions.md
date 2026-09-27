---
ai_hash: 82d140c4e45cdb06
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49317347331'
confluence_path: Team Kepler > Risk & Issues > Issues > Kunde Quickschild GmbH_eArchiv
created: 2026-04-11
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- performance
title: Performance Analysis and Proposed Solutions
type: source
updated: 2026-05-04
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49317347331/Performance+Analysis+and+Proposed+Solutions
---

# Performance Analysis and Proposed Solutions

*Confluence source · Team Kepler › Risk & Issues › Issues › Kunde Quickschild GmbH_eArchiv · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49317347331/Performance+Analysis+and+Proposed+Solutions) · updated 2026-05-04*

## Executive Summary

- The eArchive feature suffers from severe performance degradation for tenants with large document volumes.

- A tenant with **128,000+ documents** currently experiences **~3 minutes per document access** (45 seconds to open the archive, 2.5 minutes to find and open a document).

- Root cause analysis has identified **8 distinct bottlenecks** across all system layers.

- The majority of the delay comes from **inefficient database queries that scan all 128K documents on every single request**, even when only 48 are displayed.

- **The Ultimate result is whole process (Suggested by AI) is under 10 seconds**

## Why It's Slow

- The document request passes through each layer, and performance problems compound at every level.

- Every time a user opens the archive or searches, the system **counts all 128,000 documents one by one** instead of using a pre-computed number - and it does this **twice** (once for the count, once for the results).

- Meanwhile, an existing caching system sits idle, unused for search operations.

## Who Is Affected

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><ul>
<li><p>The problem **scales with document volume**.</p></li>
<li><p>Tenants below ~10K documents see acceptable performance.</p></li>
<li><p>The issue becomes **critical above ~50K documents**.</p></li>
</ul></td>
<td>![[image-20260411-081316.png]]</td>
</tr>
</tbody>
</table>

## Architecture Overview

![[image-20260414-092547.png]]

### Backend Bottlenecks (luz_docs_view_controller, luz_docs, luz_jsonstore)

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th><p>**ID**</p></th>
<th><p>**Severity**</p></th>
<th><p>**Issue**</p></th>
<th><p>**Root Cause**</p></th>
<th><p>**Service / Location**</p></th>
<th><p>**Mapped Solution**</p></th>
<th><p>**Time Saved**</p></th>
</tr>
&#10;<tr>
<td><p>**P1**</p></td>
<td><p>**CRITICAL**</p></td>
<td><p>Finding Document / Open Archive</p></td>
<td><p>Every search counts ALL 128K documents (two separate DB queries instead of one). Doubles DB round-trips; full-collection count dominates query time</p></td>
<td><p>`luz_docs` → `luz_jsonstore` → MongoDB<br />
(`DocumentSearchService.java:56-70`)</p></td>
<td><p>**S1.** Combine search + count into a single `$facet` pipeline (or estimated count for large sets)<br />
[LUZ-152936](https://axonivy.atlassian.net/browse/LUZ-152936)<br />
[LUZ-153425](https://axonivy.atlassian.net/browse/LUZ-153425)</p></td>
<td><p>**20-30s**</p></td>
</tr>
<tr>
<td><p>**P3**</p></td>
<td><p>**HIGH**</p></td>
<td><p>Finding Document</p></td>
<td><p>No database indexes — MongoDB scans entire collection on every query. Full collection scan on every filter/sort</p></td>
<td><p>MongoDB (via `luz_jsonstore`)</p></td>
<td><p>**S2.** Create compound + single-field indexes on `deletionStatus`, `folderIds`, `securityClassCodes`, `createdDate`, etc.; add text index for keyword search<br />
[LUZ-152937](https://axonivy.atlassian.net/browse/LUZ-152937)<br />
[LUZ-153424](https://axonivy.atlassian.net/browse/LUZ-153424)</p></td>
<td><p>**5-10s**</p></td>
</tr>
<tr>
<td><p>**P4**</p></td>
<td><p>**HIGH**</p></td>
<td><p>Open Archive</p></td>
<td><p>50+ individual REST calls to fetch sender branding (one per sender). N+1 pattern; dominates archive open time</p></td>
<td><p>`luz_docs_view_controller` → `luztenant-service`<br />
(`LetterBuilderService.java:481-506`)</p></td>
<td><p>**S3.** Cache sender configurations in existing `luz-cache` (key `sender_config_{id}`, TTL 5min)<br />
[LUZ-152939](https://axonivy.atlassian.net/browse/LUZ-152939)</p></td>
<td><p>**5-10s**</p></td>
</tr>
<tr>
<td><p>**P5**</p></td>
<td><p>**MEDIUM**</p></td>
<td><p>Finding Document / Open Archive</p></td>
<td><p>No connection reuse between services (`Connection: close` header). TCP/TLS handshake cost on every search request. Each of the 50+ calls pays full handshake cost (compounds with P4)</p></td>
<td><p>`luz_docs` → `luz_jsonstore`<br />
(`JsonStoreMongoClient.java:25`)</p></td>
<td><p>**S5.** Remove `@ClientHeaderParam("Connection","close")` to enable keep-alive<br />
[LUZ-153437](https://axonivy.atlassian.net/browse/LUZ-153437)</p></td>
<td><p>**1-2s**</p></td>
</tr>
<tr>
<td><p>**P6**</p></td>
<td><p>**MEDIUM**</p></td>
<td><p>Finding Document</p></td>
<td><p>Search and count run sequentially instead of in parallel. Wall-clock = search time + count time</p></td>
<td><p>`luz_docs`<br />
(`DocumentSearchService.java:61,66`)</p></td>
<td><p>**S4.** Run both queries via `CompletableFuture` in parallel (if S1 not applied)<br />
[LUZ-153438](https://axonivy.atlassian.net/browse/LUZ-153438)</p></td>
<td><p>**3-5s**</p></td>
</tr>
<tr>
<td><p>**P7**</p></td>
<td><p>**MEDIUM**</p></td>
<td><p>Finding Document / Open Archive</p></td>
<td><p>Caching system (`luz-cache` + Redis) exists but is not used for search. Repeated/identical queries re-hit Mongo. Sender branding is highly cacheable but re-fetched every open</p></td>
<td><p>`luz_docs`, `luz_docs_view_controller`<br />
(`DualCache.java`, `CacheService.java`)</p></td>
<td><p>**L3.** Extend `DualCache` to cover search counts, folder structures, badge counts<br />
[LUZ-152941](https://axonivy.atlassian.net/browse/LUZ-152941)</p></td>
<td><p>**20-30s**</p></td>
</tr>
<tr>
<td><p>**P8**</p></td>
<td><p>**MEDIUM**</p></td>
<td><p>Finding Document / Open Archive</p></td>
<td><p>Token validation on every request adds latency. Fixed latency added to every search call. Multiplied across the 50+ branding calls</p></td>
<td><p>`luz_docs_view_controller` → `jwt-service`</p></td>
<td><p>Cache JWT validation results with short TTL; reuse existing `TenantTokenCache` pattern<br />
[LUZ-153439](https://axonivy.atlassian.net/browse/LUZ-153439)</p></td>
<td><p>**<1 s**</p></td>
</tr>
</tbody>
</table>

### Web-Layer Bottlenecks (luz_epost_business_web)

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th><p>**ID**</p></th>
<th><p>**Severity**</p></th>
<th><p>**Issue**</p></th>
<th><p>**Root Cause**</p></th>
<th><p>**Location**</p></th>
<th><p>**Mapped Solution**</p></th>
<th><p>**Time Saved**</p></th>
</tr>
&#10;<tr>
<td rowspan="8"><p>**P2**</p></td>
<td rowspan="8"><p>**CRITICAL**</p></td>
<td rowspan="8"><p>Finding Document / Open Archive</p></td>
<td><p>Web layer makes a redundant count API call (already returned by the search). Extra network round-trip per search; count is already available</p></td>
<td><p>`luz_epost_business_web`<br />
(`StorageLetterboxContentHandler.java:185`)</p></td>
<td><p>**S0.** Use `initialLetters.getTotal()`; drop the count call in `refreshFolderStates()`</p></td>
<td rowspan="8"><p>**5-15s for each point**</p></td>
</tr>
<tr>
<td><p>Duplicate search on postback — DataScroller re-fetches the same 48 docs</p></td>
<td><p>`LetterboxContentHandler.java:104-107`</p></td>
<td><p>**S0.** Respect the `initialLetters` cache on postback</p></td>
</tr>
<tr>
<td><p>35× `new LetterRestClient()` across 23 files — no connection pooling, fresh JSON deserializers per instance</p></td>
<td><p>`StorageLetterBoxContentHandlerFactory` (7×), `LetterDetailHandler` (4×), `DocumentListHandler` (3×), `LetterboxContentHandler` (2×), 18 others</p></td>
<td><p>**S0.** Replace with a CDI-managed singleton `LetterRestClient`</p></td>
</tr>
<tr>
<td><p>Document open performs 3–4 sequential REST calls (metadata → content → thumbnail → read-history)</p></td>
<td><p>`LetterDetailHandler`</p></td>
<td><p>**S4.** Parallelize independent calls with `CompletableFuture`</p></td>
</tr>
<tr>
<td><p>`allLetters` grows unboundedly as the user scrolls (never shrinks)</p></td>
<td><p>`LetterboxContentHandler.java:116`</p></td>
<td><p>**S0.** Slidin. g-window buffer (keep last N chunks)</p></td>
</tr>
<tr>
<td><p>Badge count always times out (readTimeout=1000ms vs 30s+ backend) — LUZ-128397 workaround</p></td>
<td><p>`LetterRestClient.java:216-217`</p></td>
<td><p>**S0.** Pre-compute badge count server-side and cache; skip client call for large tenants</p></td>
</tr>
<tr>
<td><p>Per-chunk security-class lookup on every 48-doc load</p></td>
<td><p>`LetterboxContentHandler.java:110-113`</p></td>
<td><p>**S0.** Batch or pre-fetch security classes once per session</p></td>
</tr>
<tr>
<td><p>`refreshFolderStates()` fires 3 API calls, invoked from 4 JSF process files on every folder navigation</p></td>
<td><p>`StorageDetailContentProcess.p.json:551,598,830`, `LetterStorageDetailProcess.p.json:123`</p></td>
<td><p>**S0.** Cache folder state; invalidate on write only</p></td>
</tr>
</tbody>
</table>

![[image-20260411-070750.png]]

![[image-20260411-061752.png]]

|  |  |  |  |
|----|----|----|----|
| **Phase** | **Fixes** | **Technical Risk** | **Business Risk of Delay** |
| **Phase 1 — Quick wins** | S0 (redundant web calls), S1 (\$facet), S2 (indexes), S5 (Connection:close), S6 (pool + shared client) | **Low** - config changes + isolated code fixes | **High** - every day costs users 30-150 min |
| **Phase 2 — Optimization** | S3 (cache sender), S4 (parallelize) | **Low** - uses existing luz-cache infrastructure | **Medium** - noticeable but livable at 15s |
| **Phase 3 — Structural** | L2 (denormalize security), L3 (DualCache on hot path), L1 (Elasticsearch) | **Medium** - Elasticsearch is new, but cache part uses existing infra | **Low** - acceptable at 1s, but limits growth |

## Recommendation

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><h2 id="PerformanceAnalysisandProposedSolutions-ImmediateAction" data-local-id="756127e6985d">Immediate Action</h2>
<ol>
<li><p>**Start Phase 1 immediately** - highest ROI, lowest risk, delivers 80% of improvement</p></li>
<li><p>**Assign 1-2 developers** for 2 weeks</p></li>
<li><p>**Measure baseline** before and after each fix for validation</p></li>
</ol></td>
<td><h2 id="PerformanceAnalysisandProposedSolutions-NextQuarter" data-local-id="7e1d18d44718">Next Quarter</h2>
<ol start="4">
<li><p>**Plan Phase 2** for the following sprint - lower risk than expected since it leverages existing infrastructure</p></li>
<li><p>**Evaluate Phase 3** based on customer growth projections - the caching part can be pulled forward since infra already exists; Elasticsearch is only needed if volumes will exceed 500K</p></li>
</ol></td>
</tr>
</tbody>
</table>

%% ai-graph-start %%

**Related notes:**
- [[Follow Up Points After Client Meeting]]
- [[eArchive Performance — Detail Overview]]
- [[eArchive Performance — Executive Overview]]
- [[Performance Issue Slow Document Listing Query in MongoDB - eArchive page]]
- [[eArchive request flow and log correlation (perf)]]

%% ai-graph-end %%