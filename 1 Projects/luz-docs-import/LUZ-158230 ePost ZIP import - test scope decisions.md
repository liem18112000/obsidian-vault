---
title: "LUZ-158230 ePost ZIP import - test scope decisions"
created: 2026-09-16
type: lesson
status: seedling
source: "testing-agent run-e778a050, 2026-09-16"
tags: [luz-158230, testing-agent, epost, zip-import, qa]
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
