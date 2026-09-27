---
title: "eArchive Performance — Detail Overview"
created: 2026-06-15
updated: 2026-06-15
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49504649413/eArchive+Performance+Detail+Overview
confluence_id: "49504649413"
confluence_path: "Team Kepler > Developer note > eArchive Performance — Executive Overview"
tags: [confluence, earchive, performance]
---

# eArchive Performance — Detail Overview

*Confluence source · Team Kepler › Developer note › eArchive Performance — Executive Overview · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49504649413/eArchive+Performance+Detail+Overview) · updated 2026-06-15*

## The Problem

The eArchive feature suffers from **severe performance degradation** for tenants with large document volumes. A production tenant with **128,000+ documents** experienced:

- **~45 seconds** to open the archive

- **~2.5 minutes** to find and open a document (~3 min total per access)

Root cause analysis identified **8 distinct bottlenecks** across all system layers. The core issue: **inefficient MongoDB queries that scan all 128K documents on every request** (full COLLSCAN), even when only 48 documents are displayed. [Performance Analysis and Proposed Solutions](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49317347331/Performance+Analysis+and+Proposed+Solutions)

## Root Causes Identified

1.  **No index usage** — every query against the `documents` collection does a full collection scan

2.  **Expensive** `$lookup` to folders collection — search/count APIs join with the folder collection for permission checks on every request

3.  **Unnecessary count query** — the UI asks "how many documents match?" on every search, but **never displays that number** to the user (only uses it for a yes/no check) [\[LUZ-153656\] Document search performance — the UI count is never displayed](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49395728416/LUZ-153656+Document+search+performance+the+UI+count+is+never+displayed)

4.  `$expr/$toString` anti-pattern on detail page — casting `_id` to string inside `$match` defeats index usage, turning a 5ms lookup into **16 seconds**

## Solution Strategy (Phased)

### **Phase 1 — Denormalization (Backend, mostly complete ✅)**

Denormalize folder/security-class metadata directly into documents to eliminate the expensive `$lookup` to the folder collection. Key stories — all **Resolved**:

- [LUZ-153642](https://axonivy.atlassian.net/browse/LUZ-153642) — Enhanced search API query (no folder lookup)

- [LUZ-153653](https://axonivy.atlassian.net/browse/LUZ-153653) — Enhanced count API query (no folder lookup)

- [LUZ-153654](https://axonivy.atlassian.net/browse/LUZ-153654) — Enhanced facet query (no folder lookup)

- [LUZ-154586](https://axonivy.atlassian.net/browse/LUZ-154586) — Adapted get-document-by-id to materialized path

- [LUZ-154562](https://axonivy.atlassian.net/browse/LUZ-154562) — Replaced aggregate with `find` for search on materialized path

- [LUZ-153646](https://axonivy.atlassian.net/browse/LUZ-153646) — Cache for materialize status to route queries

- [LUZ-153934](https://axonivy.atlassian.net/browse/LUZ-153934) — Refactored search query to exclude count, remove `$facet`

- [LUZ-153438](https://axonivy.atlassian.net/browse/LUZ-153438) — Parallelized search and count DB queries

### **Phase 2 — Materialization Migration (complete ✅)**

- [LUZ-153649](https://axonivy.atlassian.net/browse/LUZ-153649) — First-time materialize migration (Resolved)

- [LUZ-154067](https://axonivy.atlassian.net/browse/LUZ-154067) — Materialize migration spill-over (Resolved)

- [LUZ-154080](https://axonivy.atlassian.net/browse/LUZ-154080) — Compute all materialize fields (Resolved)

### **Phase 3 — MongoDB Indexes (partially done)**

- [LUZ-152937](https://axonivy.atlassian.net/browse/LUZ-152937) — Add indexes to stop full collection scans (Resolved)

- [LUZ-153424](https://axonivy.atlassian.net/browse/LUZ-153424) — Review and stabilize indexes, Part 2 (To Do)

- [LUZ-153864](https://axonivy.atlassian.net/browse/LUZ-153864) — Add MongoDB indexes follow-up (To Do)

## Outstanding Issue — Frontend Integration ⚠️

A critical frontend issue in `luz_epost_business_web` (owned by Team Miracle) is **largely cancelling out the backend gains**:

- The frontend calls both search and count APIs in parallel but **blocks the search response on the count** — so effective response time = the slow count, on every search

- This happens **multiple times per page load** across three interactors

- [LUZ-155437](https://axonivy.atlassian.net/browse/LUZ-155437) — **In Progress**: Stop blocking on count API

- [eArchive performance — luz-epost-business-web calls the count API on every search](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49492099078/eArchive+performance+luz-epost-business-web+calls+the+count+API+on+every+search)

## Other Pending Items

- [LUZ-153927](https://axonivy.atlassian.net/browse/LUZ-153927) — Apply caching for query results (To Do)

- [LUZ-154614](https://axonivy.atlassian.net/browse/LUZ-154614) — UI skeleton/lazy loading for document grid (To Do)

- [LUZ-153949](https://axonivy.atlassian.net/browse/LUZ-153949) — Adapt count API on frontend side (Done)

## Target Outcome

The ultimate goal is **whole eArchive process under 10 seconds**, even for tenants with 128K+ documents. The backend work is largely landed; the remaining bottleneck is the frontend integration fix ( [LUZ-155437](https://axonivy.atlassian.net/browse/LUZ-155437) ) and the pending indexing/caching work.
