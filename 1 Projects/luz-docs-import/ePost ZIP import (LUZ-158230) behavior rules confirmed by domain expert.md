---
title: "ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert"
created: 2026-09-16
type: lesson
status: seedling
source: "Testing-Agent run-188f96b8"
tags: [luz-docs-import, zip-import, LUZ-158230, testing-agent, domain-rules]
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
