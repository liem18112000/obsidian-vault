---
ai_hash: 0edf9dd61d08d5c5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49357979662'
confluence_path: Team Kepler > Developer note
created: 2026-04-24
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- security
title: Pre-compute Security Class Code - Eliminate Lookup Query
type: source
updated: 2026-04-24
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49357979662/Pre-compute+Security+Class+Code+-+Eliminate+Lookup+Query
---

# Pre-compute Security Class Code - Eliminate Lookup Query

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49357979662/Pre-compute+Security+Class+Code+-+Eliminate+Lookup+Query) · updated 2026-04-24*

## Problem

The database has to check folder-level permissions on every document every time.

## Proposal

Store **"which security classes guard this document?"** directly on each document,

Updated it whenever

- Folders change security class or relocate

- Document change security class or relocate

The search query then checks one field instead of running a join across thousands of folders.

The end result:

- **Search drops to under 1 second** on the same tenant that triggered the original incidents.

- It also removes the remaining failure mode; the slowest operation in the pipeline disappears.

## Why

|  |  |
|----|----|
| Today | After |
| Folder-permission check runs per-document, per-request | Answered from an indexed field, no folder lookups |
| Any future feature that scans documents with security filtering pays the same cost | All future scans benefit |
| Overall impact on all feature related to list document and count | The payoff here scales to the whole product — any feature that lists documents (search, folder views, stats dashboards, export, background indexing) gets the same speedup. |

![[image-20260424-030033.png]]

## How

1.  A one-off migration fills that field across each tenant's existing documents.

    - Runs once per tenant asynchronously

    - Automatically triggered on any tenant-based operations

2.  When a folder's permission settings change

    - an async background task updates affected documents.

    - Invisible to users.

3.  The feature is enabled tenant-by-tenant via a config flag.

No API changes. No UI changes. No user-visible migration window.

## Risks and mitigation

|  |  |
|----|----|
| Risk | Mitigation |
| A bug in the new compute logic makes some documents silently invisible to the user | We will run this on DEV to see any unexpected behaviors. When it is stable, we deploy |
| Rollout stalls mid-tenant. | Feature flag off mean instant rollback. |
| Background update task crashes before finishing. | We handle retry at mode success at least one |
| Race Condition for parallel update | Consider the chance of happen and fix |
| Domino effect on changing a folder has many folders and documents | Need to discuss to get the solution |

## What happens after this

1.  We delete the slow code path and the old database index it depended on.

2.  The `/documents/search` system becomes smaller, faster, and easier to reason about than it was before the incident.

%% ai-graph-start %%

**Related notes:**
- [[Technical Details - Pre-compute Security Class Code]]
- [[eArchive Performance — Executive Overview]]
- [[Folder recovery with re-parenting leaves inheritedSecurityClassCode stale]]
- [[eArchive Performance — Detail Overview]]
- [[Research on Delete Access class]]

%% ai-graph-end %%