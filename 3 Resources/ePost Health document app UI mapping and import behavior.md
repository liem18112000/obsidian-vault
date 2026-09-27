---
ai_hash: 36e4c1f5c01afdb5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
tags:
- klara
- epost
- health
- ui
- luz-docs-import
- spec
---

# ePost Health document — app UI mapping & import behavior

How a HEALTH document's metadata surfaces in the ePost app ("Edit digital letter data" screen), per the dev handoff §3:
- **Title** ← `documentTitle` · **Document date** ← `documentReferenceDate` · **Author** ← `healthData.author`.
- **Document type** ← `healthData.documentCategory` · **Facility** ← `healthData.facility` · **Practice setting** ← `healthData.practiceSetting` — each a **dropdown backed by a value set**.
- Coded fields resolve via the **EPDV-EDI value sets (FHIR translations)** and display in the user's language (DE/FR/IT/EN). Deviation from the mock: each coded field gets its own dropdown (not one description blob), since codes arrive in the metadata.
- Health docs appear in a **branded folder** (today by senderTenantId; extension: also by HEALTH document type).
- **Health-area placeholder:** as the eHealth SDK retires (end 2026), tapping the app's health area shows a placeholder screen (feature switch `epost:eHealth:onePager`), not the SDK.

Per-file **import behavior** (import spec §6 — one faulty doc never fails the whole ZIP):
- valid metadata → imported; no metadata file → imported without health metadata; disallowed top-level element → imported (element ignored); metadata not parseable → imported (whole metadata ignored); metadata without a matching document → **rejected/failed**; document already exists (same name+size) → **skipped** (kept unchanged).
- Limits: ZIP < 2 GB · metadata ≤ 100 KB/file · password-protected ZIPs rejected · UTF-8 names.

Related: [[ePost Health documents ZIP import business context]] · [[luz-docs-import ZIP import call chain]].

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[HEALTH document type carries verbatim SNOMED healthData]]
- [[ePost Health documents ZIP import business context]]
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[LUZ-158230 import stores healthData as-is no validation, no SNOMED label resolution, schemaless Mongo]]

%% ai-graph-end %%