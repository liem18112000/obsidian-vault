---
ai_hash: 73ea8e2668efe26f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities:
- LUZ-158230
- ePost ZIP import
- test scope decisions
- QA owner
- Post Health
- HEALTH documents
- ZIPs
- eArchive
- engine defaults
- test plan
- Validation failure mode
- partial import + per-document error report
- Valid documents
- invalid documents
- metadata.json
- missing fields
- unknown SNOMED codes
- sender
- atomic all-or-nothing
- Duplicate handling
- de-dup and skip by full file path
- incoming document
- full path
- job-history success records
- Release gating
- LUZ-158243
- LUZ-159672
- SNOMED label language coverage
- recipient consent/opt-in
- axonivy-prod-luz_docs_import
- luz_docs
- refine business round
- run-e778a050
- Testing-agent refine flags low confidence when spec PDFs are recorded-only
- Scope additions
- define_plan
- scenario generation
- Full-ZIP re-import idempotency
- transfer.zip
- no duplicate documents
- no errors
- stable state
- batch-level no-op
- Performance test
- API layer
- import API endpoint
- full end-to-end system performance
- Documented size thresholds
- oversized-ZIP
- oversized-file
- ZIP > 2 GB
- HTTP 400
- whole-request rejection
- Individual file > 200 MB
- file rejected
- Boundary test design
- each threshold
- just-below (accept)
- at/just-above (reject)
source: testing-agent run-e778a050, 2026-09-16
status: seedling
tags:
- luz-158230
- testing-agent
- epost
- zip-import
- qa
title: LUZ-158230 ePost ZIP import - test scope decisions
type: lesson
---

# LUZ-158230 ePost ZIP import - test scope decisions

Test-scoping decisions confirmed with the QA owner for LUZ-158230 (ePost ZIP document import: Post Health pushes ZIPs of HEALTH documents into the recipient eArchive). These override several engine defaults and drive the test plan:

- **Validation failure mode = partial import + per-document error report.** Valid documents are imported; invalid ones (bad/missing metadata.json, missing fields, unknown SNOMED codes) are skipped and reported back to the sender. NOT atomic all-or-nothing.
- **Duplicate handling = de-dup and skip by full file path.** Compare each incoming document by its full path against the job-history success records (a stored array of successfully-imported full-path files). If the path was already imported successfully, skip it as a duplicate. (Not versioning, not blind-duplicate.)
- **Release gating = independent.** LUZ-158243 and LUZ-159672 are NOT hard prerequisites; track them separately.
- **Out of scope for this ticket:** SNOMED label language coverage (de/fr/it/en) and recipient consent/opt-in.

Implemented in [[axonivy-prod-luz_docs_import]] (+ luz_docs). These answers came from the refine business round on run-e778a050.

## Related

- [[Testing-agent refine flags low confidence when spec PDFs are recorded-only]]

## Scope additions (carried into define_plan, not the refine brief)

The refine loop closes after completion and its free-text answer does NOT rewrite the structured In-scope brief. These two additions were therefore carried forward into define_plan / scenario generation instead:

- **Full-ZIP re-import idempotency** — re-importing the exact same transfer.zip produces no duplicate documents and no errors (whole-batch no-op via the full-path job-history dedup). Test explicitly: import ZIP, import the identical ZIP again, assert no new docs / no errors / stable state. This is the batch-level counterpart to the per-document dedup-skip.
- **Performance test, API layer only** — load/throughput/latency test scoped to the import API endpoint, NOT full end-to-end system performance.

## Documented size thresholds (from QA owner, LUZ-158230)

Boundary limits for the oversized-ZIP / oversized-file cases:

- **ZIP > 2 GB -> HTTP 400** (whole-request rejection; the entire transfer.zip is refused).
- **Individual file > 200 MB -> that file is marked rejected** (per-document rejection, consistent with the partial-import + per-document error-report model; the rest of the ZIP can still import).

Boundary test design: for each threshold test just-below (accept) and at/just-above (reject).

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]]
- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]

**Relations:**
- LUZ-158230 — *is_about* — ePost ZIP import
- LUZ-158230 — *has_scope_decisions* — test scope decisions
- test scope decisions — *confirmed_with* — QA owner
- ePost ZIP import — *involves* — Post Health
- Post Health — *pushes* — HEALTH documents
- HEALTH documents — *are_in* — ZIPs
- ZIPs — *are_pushed_into* — eArchive
- test scope decisions — *override* — engine defaults
- test scope decisions — *drive* — test plan
- test scope decisions — *define* — Validation failure mode
- Validation failure mode — *is* — partial import + per-document error report
- partial import + per-document error report — *imports* — Valid documents
- partial import + per-document error report — *skips* — invalid documents
- invalid documents — *are_reported_to* — sender
- invalid documents — *due_to* — metadata.json
- invalid documents — *due_to* — missing fields
- invalid documents — *due_to* — unknown SNOMED codes
- Validation failure mode — *is_not* — atomic all-or-nothing
- test scope decisions — *define* — Duplicate handling
- Duplicate handling — *is* — de-dup and skip by full file path
- de-dup and skip by full file path — *compares* — incoming document
- incoming document — *by* — full path
- de-dup and skip by full file path — *against* — job-history success records
- test scope decisions — *define* — Release gating
- Release gating — *states* — LUZ-158243 is_not_prerequisite
- Release gating — *states* — LUZ-159672 is_not_prerequisite
- LUZ-158243 — *should_be* — tracked separately
- LUZ-159672 — *should_be* — tracked separately
- SNOMED label language coverage — *is_out_of_scope_for* — LUZ-158230
- recipient consent/opt-in — *is_out_of_scope_for* — LUZ-158230
- test scope decisions — *implemented_in* — axonivy-prod-luz_docs_import
- test scope decisions — *implemented_in* — luz_docs
- test scope decisions — *originated_from* — refine business round
- refine business round — *identified_by* — run-e778a050
- LUZ-158230 — *related_to* — Testing-agent refine flags low confidence when spec PDFs are recorded-only
- Scope additions — *carried_into* — define_plan
- Scope additions — *carried_into* — scenario generation
- Scope additions — *include* — Full-ZIP re-import idempotency
- Full-ZIP re-import idempotency — *tests* — transfer.zip
- transfer.zip — *re-import produces* — no duplicate documents
- transfer.zip — *re-import produces* — no errors
- transfer.zip — *re-import produces* — stable state
- Full-ZIP re-import idempotency — *is_a* — batch-level no-op
- Scope additions — *include* — Performance test
- Performance test — *scoped_to* — API layer
- API layer — *is* — import API endpoint
- Performance test — *is_not* — full end-to-end system performance
- Documented size thresholds — *from* — QA owner
- Documented size thresholds — *for* — LUZ-158230
- Documented size thresholds — *include* — oversized-ZIP
- oversized-ZIP — *has_threshold* — ZIP > 2 GB
- ZIP > 2 GB — *results_in* — HTTP 400
- HTTP 400 — *is_a* — whole-request rejection
- Documented size thresholds — *include* — oversized-file
- oversized-file — *has_threshold* — Individual file > 200 MB
- Individual file > 200 MB — *results_in* — file rejected
- file rejected — *is_consistent_with* — partial import + per-document error report
- Boundary test design — *for* — each threshold
- Boundary test design — *tests* — just-below (accept)
- Boundary test design — *tests* — at/just-above (reject)

%% ai-graph-end %%