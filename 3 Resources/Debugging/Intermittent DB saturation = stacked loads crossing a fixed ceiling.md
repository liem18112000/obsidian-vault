---
title: "Intermittent DB saturation = stacked loads crossing a fixed ceiling"
created: 2026-09-09
type: model
status: seedling
source: "PROD investigation 2026-09-09"
tags: [debugging, capacity, mongodb, reliability, mental-model]
---

# Intermittent DB saturation = stacked loads crossing a fixed ceiling

A datastore that dies **at roughly the same time, only on some days, with no request spike** is usually not "one bad query" — it is several independent loads **stacking** onto a fixed capacity ceiling (WiredTiger cache + connection slots + CPU). On calm days the stack sits under the ceiling; on a heavy day one band grows and pushes the total over — and because the *ceiling* is crossed, EVERY operation on that node degrades at once (all op types slow together). This model explains three otherwise-confusing facts: no visible request spike, intermittency, and everything-slow-not-one-thing.

Practical consequences:
- Look for the **variable band** (the load that differs crash vs calm) — that is the trigger. In the Luz case it was the enrichment re-run campaign.
- **Aggravators still matter**: removing any band (a chatty backup, cache-miss amplification that multiplies reads, a shared-DB write hot spot, missing `maxTimeMS` so slow ops hold resources, retry storms) can lower the stack back under the line. So multi-factor fixes are legitimate even when one band is "the cause."
- Draw it as two stacked bars (calm vs crash) against a dashed ceiling line — the breach segment above the line is the outage. Comparison + mechanism in one figure.

## Related
[[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
[[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]

## Related

- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
- [[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]
