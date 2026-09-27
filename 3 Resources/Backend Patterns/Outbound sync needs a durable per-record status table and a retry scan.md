---
title: "Outbound sync needs a durable per-record status table and a retry scan"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Technical design of Klara Hubspot (LUZ)"
tags: [etl, integration, retry, sync, hubspot, reliability, confluence-distilled]
---

# Outbound sync needs a durable per-record status table and a retry scan

Syncing data to a third-party SaaS fails for reasons you do not control — their API is down, the network blips, a rate limit hits. If the sync is fire-and-forget, those records are silently never synced and nobody finds out until someone notices the CRM is missing a month of companies.

The pattern that fixes it: **a tracking table owned by your sync service, plus a scheduled scan that re-processes failures.**

The design, for a Klara → HubSpot integration:

1. **Detect changes** (company and user data).
2. **Extract → transform → load** into the third party's format via its API.
3. **The sync service owns its own database**, storing **per-record synced status**.
4. A **scheduled trigger scans for failed jobs since the last run** and re-processes them.

> *"If some data can't sync due to internet connection or service unavailable, we can re-process it later."*

**Why the tracking table is the load-bearing part.** Retrying in-process only survives transient blips within one run. A durable per-record status survives a crash, a deploy, and a multi-hour outage at the other end — and it answers the question that actually gets asked: *"is everything synced?"* You can count unsynced records, which is a monitorable number.

**The ETL-with-cron choice was explicit, and honest about why.** The team considered both **ETL/cron** and **event-driven**, and picked ETL *"aligning with our current design as well as the urgent plan of this requirement"* — that is, it fits the existing `luz_analytics_etl` service and ships sooner. Naming schedule pressure as a reason is better practice than retrofitting a technical justification.

**The structural choice worth copying:** define an **ETL interface** and implement it per entity (Company, User). New entities become new implementations rather than new services.

> [!warning] Decide what a *permanently* failed record does
> A retry scan that keeps re-trying forever will hammer the third party with a record it will never accept — a validation rejection, a deleted counterpart. Track attempt counts, stop after N, and move the record to a state a human can see. "Retry later" without a terminal state is an infinite loop with a delay.

> [!tip] Change detection is the other half
> ETL-on-a-schedule needs to know what changed since the last run. Whatever the cursor is — an updated-at watermark, a change table — it must be durable and advanced **only after a successful load**, or a crash mid-run skips records permanently.

Related: [[Multi-channel delivery filter for eligibility, then send in priority order|Multi-channel delivery: filter for eligibility, then send in priority order]] · [[Claim work across pods with an expiring lease column on the row]].

Source: [[Technical design of Klara - Hubspot]] (LUZ, Confluence).

## Related

- [[Claim work across pods with an expiring lease column on the row]]
