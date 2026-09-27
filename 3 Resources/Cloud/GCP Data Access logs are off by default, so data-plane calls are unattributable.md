---
ai_hash: 7167d0c123d1af8d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: INC-2026-09-24 Gemini consumption (LUZ)'
status: seedling
tags:
- gcp
- audit-logs
- observability
- security
- incident-response
- confluence-distilled
title: GCP Data Access logs are off by default, so data-plane calls are unattributable
type: gotcha
---

# GCP Data Access logs are off by default, so data-plane calls are unattributable

Google Cloud Audit Logs come in two kinds, and the split decides what you can investigate months later:

- **Admin Activity** — always on, **cannot be disabled**, free. Records control-plane actions: enabling or disabling an API, creating or deleting a service account key, changing IAM.
- **Data Access** — **off by default**, must be explicitly enabled, and you pay for the volume. Records data-plane calls: the actual reads, writes, and model invocations.

The consequence, seen in a real investigation: when a project burned 118.7 M Vertex AI tokens across **28,034 model invocations**, every administrative action around the incident was attributable —

> `firebase-adminsdk-fon1l@…` enabled `cloudbilling.googleapis.com` from 24.91.133.0
> `helios.aavn@gmail.com` disabled `aiplatform.googleapis.com`
> `helios.aavn@gmail.com` deleted service account key `d7f2f62284…`

— each with its principal, timestamp, and source address, because those are Admin Activity events. The 28,034 model calls themselves were **not recorded at all**. They survive only as aggregate counts in Cloud Monitoring, which knows the *model name* but not the *caller*.

> [!warning] You cannot turn this on retroactively
> Enabling Data Access logging today does nothing for yesterday. The decision to log is made **before** the incident, which is precisely when the cost looks unjustified. That is the trade: pay for log volume continuously, or accept that you will never be able to attribute data-plane activity after the fact.

**A workable middle ground** rather than all-or-nothing:

- Enable Data Access logging on the **services where attribution matters** — Vertex AI and other metered AI APIs, secret access, storage holding personal data — not on everything.
- Within those, `ADMIN_READ` / `DATA_READ` / `DATA_WRITE` are separately selectable; writes are usually the ones you must be able to attribute.
- Set **budget alerts** regardless. In this incident, a full day of spend surfaced only because a person happened to look at billing. An alert is far cheaper than logging and catches the same class of problem earlier.

> [!tip] The generalisable rule
> In any system, ask which plane an action belongs to. *Control-plane* actions (configuration, permissions, provisioning) are usually logged by default. *Data-plane* actions (the actual work) usually are not, because the volume is orders of magnitude higher. The gap between the two is where post-incident attribution fails.

Even perfect logging is not sufficient on its own — see [[Shared and personal accounts make attribution impossible by construction]].

Source: [[INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user]] (LUZ, Confluence).

## Related

- [[Shared and personal accounts make attribution impossible by construction]]

%% ai-graph-start %%

**Related notes:**
- [[Shared and personal accounts make attribution impossible by construction]]
- [[INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user]]
- [[Token input-output ratio fingerprints whether an LLM caller is an agent or a feature]]

%% ai-graph-end %%