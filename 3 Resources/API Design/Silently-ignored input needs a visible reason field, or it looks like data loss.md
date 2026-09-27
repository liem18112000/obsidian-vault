---
ai_hash: 8cb7f8eff054dfd8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: ePost ZIP Import Test Fixture Matrix (2026-08-14)'
status: seedling
tags:
- api-design
- error-handling
- validation
- epost
- zip-import
- security
title: Silently-ignored input needs a visible reason field, or it looks like data
  loss
type: lesson
---

# Silently-ignored input needs a visible reason field, or it looks like data loss

The ePost ZIP import accepts a per-document `.metadata.json` sidecar, and it takes a deliberate stance on bad ones: **the document is still imported; only the metadata is dropped.** Unparseable JSON, an oversized sidecar (`> BUFFER_SIZE` = 100 KiB), or a disallowed top-level field never fails the import.

That is the right call — losing a health document because its metadata had a trailing comma would be absurd. But "silently ignored" is indistinguishable from "silently lost" to whoever uploaded it.

The mechanism that makes it safe is an **`ignoredReason`** recorded on the document (e.g. *"not valid JSON"*), plus a strict allow-list (`HealthDocImporter.ALLOWED_FIELDS`: `documentReferenceDate`, `documentTypes`, `senderName`, `senderTenantId`, `senderCompanyId`, `healthData`, `documentTitle`) so injected keys like `id`, `_id`, `tenantId`, `status` cannot reach internal properties.

Two design rules worth keeping:

- **Every silent degradation needs a visible artifact.** If the system chose to continue without part of the input, the record must say so and why. Otherwise the only evidence is absence, and absence gets reported as a bug months later.
- **Ignore-unknown-fields and allow-list-known-fields look identical on the happy path and differ entirely under attack.** The sidecar is attacker-controlled; an allow-list is what stops `_id` or `tenantId` from being set by the uploader.

Note the distinction the fixtures draw: an *unparseable* sidecar is ignored (document imports), while an *orphan* sidecar — one with no matching document — is **rejected** into `rejectedFiles` with `DETAIL_ORPHAN_METADATA`. Ignored means "I continued without it"; rejected means "this input was wrong". Keeping those in different buckets is what makes the job report readable.

## Related

- [[Order a test matrix by feedback latency - smoke, sync rejections, volume last]]

## Related

- [[Order a test matrix by feedback latency - smoke]]
- [[sync rejections]]
- [[volume last]]

%% ai-graph-start %%

**Related notes:**
- [[Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection]]
- [[ePost ZIP Import Test Fixture Matrix]]
- [[Order a test matrix by feedback latency - smoke, sync rejections, volume last]]
- [[ePost Zip-Import - dev test-suite results - 18-08-2026]]
- [[ePost ZIP-import — dev test-suite results - 13-08-2026]]

%% ai-graph-end %%