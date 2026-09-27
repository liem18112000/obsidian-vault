---
title: "SAP export defaults come from the /default-sap config service so accounting codes change without a deploy"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: SAP in luz_finance — What, Why, How, When (2026-07-27)"
tags: [sap, luz-finance, configuration, accounting, fail-closed, kepler]
---

# SAP export defaults come from the /default-sap config service so accounting codes change without a deploy

The values SAP validates an import against — **VAT codes, posting keys, contact qualifiers, account numbers, and the file header/content templates** — are not constants in `luz_finance`. They are fetched at export time from `FINANCE_PUBLIC` → **`/default-sap`**, returning a `SAPReport` template bag that is merged into the generated CSVs.

The reason is ownership: those codes belong to **SAP's configuration**, which the finance team changes on their own schedule. Hard-coding them would mean a code change and a deploy every time accounting reconfigured a VAT code — and, worse, a silent mismatch producing files SAP rejects or, more dangerously, *accepts and posts wrongly*.

The guard worth copying: if `masterHeader` or `bookingHeader` come back empty, `DefaultSapContainer.getDefaultSAPValues()` throws `ClientException` and **nothing is exported**. Failing closed is the right call for generated accounting data — a partial or template-less file that reaches SAP is far more expensive than a failed download.

Generalisable: **configuration that belongs to a downstream system should be read from that system's side of the boundary, and its absence should abort rather than default.** A default VAT code is a guess about someone else's books.

## Related

- [[SAP in luz_finance is a manual CSV export, not a live integration]]

## Related

- [[SAP in luz_finance is a manual CSV export, not a live integration]]
