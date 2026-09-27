---
title: "2026-01-09 Meeting notes"
created: 2026-01-09
updated: 2026-01-09
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49028202497/2026-01-09+Meeting+notes
confluence_id: "49028202497"
confluence_path: "Team Kepler > Developer note > luz-store transfer"
tags: [confluence, luz-store]
---

# 2026-01-09 Meeting notes

*Confluence source · Team Kepler › Developer note › luz-store transfer · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49028202497/2026-01-09+Meeting+notes) · updated 2026-01-09*

### \uD83D\uDDD3 Date

09 Jan 2026

### \uD83D\uDC65 Participants

Team Kepler:

- [Alvin Villanueva](https://axonivy.atlassian.net/wiki/people/712020:a8f84630-b701-4aff-b3f4-82349928cfc7?ref=confluence) - PO

- [[MT Receive] Lam Nguyen](https://axonivy.atlassian.net/wiki/people/5efc0f621225f80bb1711f8f?ref=confluence) - SM

- [[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)

- [[Kepler] Lai Hong Thien](https://axonivy.atlassian.net/wiki/people/712020:bcf11719-519d-4418-9b47-693ecc724076?ref=confluence)

- [[Kepler] Thanh Nguyen](https://axonivy.atlassian.net/wiki/people/712020:8a1af637-d814-4c96-bd25-44dba2af04b5?ref=confluence)

- [[Kepler] baonguyentx](https://axonivy.atlassian.net/wiki/people/6424fcd05534b0bf744302cf?ref=confluence)

Team WoW:

- [Alexandra Reich](https://axonivy.atlassian.net/wiki/people/622d403749c900007024cd63?ref=confluence) - PO

- [thong.nguyen](https://axonivy.atlassian.net/wiki/people/557058:03c63c48-df0c-431f-ade0-75431c014051?ref=confluence) - SM

- [DuongNguyen](https://axonivy.atlassian.net/wiki/people/712020:0f8284b2-d184-4028-89b5-01a558073960?ref=confluence)

- [Trung Nguyen](https://axonivy.atlassian.net/wiki/people/5de470918743750d00b7c70a?ref=confluence)

- [Tuyen PhamVu](https://axonivy.atlassian.net/wiki/people/63a919d06ad11358a097f77a?ref=confluence)

- [[WOW] Hien Mai](https://axonivy.atlassian.net/wiki/people/5b6c22c523300129cc4472fd?ref=confluence)

- [[WOW] Tuấn Phạm Phương](https://axonivy.atlassian.net/wiki/people/557058:8e75a3fc-a02a-40b8-ac0c-7718d1cd7bb8?ref=confluence)

### \uD83E\uDD45 Goals

- **Continuity** — Maintain uninterrupted operation and support after the handover

- **Knowledge transfer** — Pass on understanding of code, architecture, and business logic

- **Documentation** — Provide complete technical and operational documentation

- **Risk mitigation** — Minimize failures, data loss, or service disruption

- **Independence** — Enable the receiving team to operate without relying on the original team

- **Compliance** — Ensure licensing, security, and regulatory requirements are met

- **Accountability** — Establish clear ownership and responsibility for the software

- **Quality assurance** — Verify the software meets agreed standards before handover

### \uD83C\uDFA8 Resource Provided from team WoW

Documentation Space:

- <https://axonivy.atlassian.net/wiki/spaces/KS>

Summarized document:

- [https://axonivy.atlassian.net/wiki/x/BADiaQs](https://axonivy.atlassian.net/wiki/x/BADiaQs)

### \uD83D\uDDE3 Discussion topics

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p>**Domain**</p></th>
<th><p>**Question**</p></th>
<th><p>**Answer**</p></th>
<th><p>**Notes**</p></th>
</tr>
&#10;<tr>
<td rowspan="2"><p>High‑level domain and scope</p>
<p>URL: [Luzstore Business Overview](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/49021059076/Luzstore+Business+Overview)</p>
<p><br />
</p></td>
<td><p>Which systems depend on `luz_store` and which systems does `luz_store` depend on?</p></td>
<td></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td><p>Are there any legal or compliance constraints (tax law, data retention, invoicing regulations) that affect implementation?</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td rowspan="4"><p>Architecture and data model</p>
<p>URL: [Luzstore Business Overview](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/49021059076/Luzstore+Business+Overview)</p></td>
<td><p>Is there a system architecture diagram for `luz_store` (services, databases, integrations)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Where is the canonical source of truth (a single, authoritative data source that all systems and stakeholders reference for accurate, consistent information) for:</p>
<ul>
<li><p>Products?</p></li>
<li><p>Subscriptions?</p></li>
<li><p>Billings?</p></li>
<li><p>Partners/customers (is it KLARA, EPOST AG, or `luz_store`)?</p></li>
</ul></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Can we see the ERD / schema of the main tables:</p>
<ol>
<li><p>Product</p></li>
<li><p>Subscription</p></li>
<li><p>Billing</p></li>
<li><p>Partner/Customer</p></li>
<li><p>Any linking tables (e.g., product–widget, subscription–widget)?</p></li>
</ol></td>
<td></td>
<td><p>Describe all tables</p></td>
</tr>
<tr>
<td><p>Are there any data volume or performance constraints (e.g., typical number of subscriptions, billings per month)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td rowspan="3"><p>Product model</p>
<p>URL: [Luzstore Business Overview](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/49021059076/Luzstore+Business+Overview)</p></td>
<td><p>Where is the “admin UI” for products implemented (module, technology, URL)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Are there constraints or formats for:</p>
<ul>
<li><p>`productCode`</p></li>
<li><p>`marketingCode`</p></li>
<li><p>`widgetCode`</p></li>
</ul></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>How are the “New pricing model” fields (Master product, Combination, Maximum allowed users) used technically?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td rowspan="2"><p>Subscription behavior</p>
<p>URL: [Luzstore Business Overview](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/49021059076/Luzstore+Business+Overview)</p></td>
<td><p>For each price plan (Free, Monthly/Quarterly/Yearly, Volume/Single, PayPerUse), what are the exact business rules and UI flows?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>How are the subscription fields actually used in code:</p>
<ol>
<li><p>`manual price`</p></li>
<li><p>`start billing time`</p></li>
<li><p>`last billed time`</p></li>
<li><p>`testing duration`</p></li>
<li><p>`renewal date` / `renewal period`</p></li>
</ol></td>
<td></td>
<td><p>we should change to: [have any highlight fields that you usually use?]</p></td>
</tr>
<tr>
<td rowspan="4"><p>Billing logic & jobs</p>
<p>URL: [Luzstore Business Overview](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/49021059076/Luzstore+Business+Overview)</p></td>
<td><p>For Single/Volume: what happens if billing creation fails right after subscription?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>What does the **daily** billing CRON do vs the **monthly** one? How do we avoid double‑billing?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>In which scenarios is billing `price` negative, and how is that handled downstream?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>How are failed address syncs detected and retried (beyond manual scripts)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Missing / brief sections</p>
<p>URL: [Luzstore Business Overview](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/49021059076/Luzstore+Business+Overview)</p></td>
<td><p>Is there detailed documentation for:</p>
<ol>
<li><p>“Creating billing records process for monthly/yearly subscriptions (Briefly)”</p></li>
<li><p>“Invoice Run”</p></li>
<li><p>“Rule engine”</p></li>
<li><p>“KLARA pay”</p></li>
</ol></td>
<td></td>
<td></td>
</tr>
<tr>
<td rowspan="4"><p>Known Issues, Risks & Tech Debt</p>
<p>URL: [Luzstore Business Overview](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/49021059076/Luzstore+Business+Overview)</p></td>
<td><p>What known issues, limitations, and workarounds are documented?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>What technical debt items are accepted and why?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Are there “do not touch” areas or fragile components?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>What known issues, limitations, and workarounds are not documented yet?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td rowspan="7"><p>Cron-/Batch-Job Handling Concept</p>
<p>URL: [Cron-/Batch-Job Handling Concept](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20490589176/Cron-+Batch-Job+Handling+Concept)</p></td>
<td><p>Can you confirm that Kubernetes CronJob is the *only* recommended scheduler now, or are Linux cron / Klara Ivy triggers still used anywhere?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>How are jobs prevented from running multiple times in a multi‑JVM / multi‑pod environment (locking strategy, leader election, DB flags)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>The doc says jobs must not cross tenant/public data spaces. Which existing jobs already violate or risk violating this rule?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>What is the concrete reservation strategy mentioned (e.g., modulo on tenantId / jobId)? Is there any risk of two runners picking the same job?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>What are the retention rules for Job Execution Table (how long do we keep execution history)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>The doc recommends “It is important to note, that a job should do lightweight work.” and avoiding long JEE transactions. Which existing jobs are known to be heavy or violate this?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Are there known production incidents that led to this concept (so we can avoid repeating them)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td rowspan="8"><p>List of background jobs have been using in Klara</p>
<p>URL: [List of background jobs have been using in Klara](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513306663/List+of+background+jobs+have+been+using+in+Klara)</p></td>
<td><p>Are these all background jobs in luz-store</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Is this list 100% up to date for all environments (DEV/TEST/STAGE/PROD), or is it partial/historic?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>For each job marked “NOT released yet”, what is the current status (canceled, postponed, or actually released but not updated)?</p></td>
<td></td>
<td><p>are all jobs released on PROD ?</p></td>
</tr>
<tr>
<td><p>For each entry (e.g. `luzfin_finance`, `luz_sua`, `luz_asset`, `luz-doc-output-mgmt`, `luz-scancenter`):</p>
<ul>
<li><p>What is the **exact cron schedule and timezone** used in production?</p></li>
<li><p>Where is the job defined (Ivy background job, Kubernetes CronJob, external scheduler)?</p></li>
<li><p>In which **repository/module** is the runner class implemented (e.g. `com.axonivy.finance.bank.backgroundjob.BankBackgroundJobRunner`)?</p></li>
</ul></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>For jobs whose schedule is configurable in the admin page (in section 13, 14, 15, 18, 35)</p>
<ol>
<li><p>Where exactly is this configuration UI?</p></li>
<li><p>What safe/unsafe ranges should we respect when changing the schedule?</p></li>
</ol></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>What happens if a job overlaps with itself (no locking, DB lock, explicit flag)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Which jobs are critical for billing or customer‑visible behavior (e.g. `oneapi-billing-trigger-cronjob`, `billing-delivered-documents`)?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Which jobs are known to be flaky, noisy, or sensitive and need extra attention after transfer?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

### ✅ Action items

- Team Wow answer all questions

### ⤴ Decisions

### 🗃️ Related info
