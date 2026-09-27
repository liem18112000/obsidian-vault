---
ai_hash: aae456013631b2f6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities:
- ePost ZIP import
- LUZ-158230
- behavior rules
- domain expert
- ePost ZIP document import feature
- luz_docs_import
- Testing-Agent
- metadata.json
- default metadata
- recipient validation
- Notification
- eArchive arrival notification
- Multi-sender
- Post Health
- test plan
- multiple senders/tenants
- SNOMED labels
- sender data
- Dedup
- re-import
- ZIP-import success history
- Testing-Agent pipeline run-188f96b8
- Deployed Testing-Agent refine recommendations are speculative until validated
source: Testing-Agent run-188f96b8
status: seedling
tags:
- luz-docs-import
- zip-import
- LUZ-158230
- testing-agent
- domain-rules
title: ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert
type: lesson
---

# ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert

Grounded business/technical rules for the ePost ZIP document import feature (Jira **LUZ-158230**, service **luz_docs_import**), confirmed by the domain-expert user during a Testing-Agent refine round — several **correct** the deployed bot's speculative recommendations.

- **Missing metadata.json → import with DEFAULT metadata.** A binary/document with no `metadata.json` sidecar is *not* an error: it lands with default metadata. It is not skipped and does not fail the ZIP. (Malformed/present-but-invalid metadata is a separate, still-open case.)
- **No recipient validation in this process.** luz_docs_import does *not* check recipient existence, eArchive opt-in, or storage quota. No rejection/retry-queue for ineligible recipients.
- **Notification reuses the existing eArchive arrival notification** for a landed batch — no new health-specific channel.
- **Multi-sender.** Rollout gated to Post Health first (allowlist), but the test plan must validate multiple senders/tenants.
- **SNOMED labels stored/shown VERBATIM.** No SNOMED code→label resolution at import or read time; de/fr/it/en labels come from sender data. Translation sign-off is clinical governance, out of import-test scope.
- **Dedup on re-import.** Folders + nested content dedupe/merge into existing same-name recipient folders (idempotent). Documents dedupe via **ZIP-import success history** — files already imported by a prior *successful* import are skipped; otherwise re-import proceeds.

Source: Testing-Agent pipeline run-188f96b8, business+technical refine rounds.

## Related

- [[Deployed Testing-Agent refine recommendations are speculative until validated]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 import stores healthData as-is no validation, no SNOMED label resolution, schemaless Mongo]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[luz_docs_import scope no sender auth, individual tenants, partial-import policy]]

**Relations:**
- ePost ZIP import — *is identified by* — LUZ-158230
- ePost ZIP import — *has* — behavior rules
- behavior rules — *confirmed by* — domain expert
- ePost ZIP document import feature — *is tracked by* — LUZ-158230
- ePost ZIP document import feature — *uses service* — luz_docs_import
- behavior rules — *confirmed during* — Testing-Agent refine round
- Missing metadata.json — *leads to* — import with default metadata
- luz_docs_import — *does not perform* — recipient validation
- Notification — *reuses* — eArchive arrival notification
- Multi-sender — *is gated to* — Post Health
- test plan — *must validate* — multiple senders/tenants
- SNOMED labels — *are stored/shown* — VERBATIM
- SNOMED labels — *originate from* — sender data
- Dedup — *applies to* — re-import
- Documents — *dedupe via* — ZIP-import success history
- behavior rules — *sourced from* — Testing-Agent pipeline run-188f96b8
- ePost ZIP import — *is related to* — Deployed Testing-Agent refine recommendations are speculative until validated

%% ai-graph-end %%