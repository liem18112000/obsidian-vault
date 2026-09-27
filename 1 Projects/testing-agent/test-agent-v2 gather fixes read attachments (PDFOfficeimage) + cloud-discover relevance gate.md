---
title: "test-agent-v2 gather fixes: read attachments (PDF/Office/image) + cloud-discover relevance gate"
created: 2026-09-15
type: lesson
status: seedling
source: "test-agent-v2 code changes 2026-09-15"
tags: [testing-agent, kga, gather, attachments, pdf, ocr, cloud-discover, precision, v2]
---

# test-agent-v2 gather fixes: read attachments (PDF/Office/image) + cloud-discover relevance gate

Two more test-agent-v2 gather fixes (branch feature/test-agent/v2-adk, 2026-09-15), on top of the implement-timeout + explore-gate work.

## 1. Jira/Confluence attachments are now READ (the root cause of the whole LUZ-158230 confidence problem)
Attachments (the spec PDFs) were extracted as ATTACHMENT links but never fetched — three gates blocked them: `_fetchable()` (crawl.py) whitelisted only jira/confluence/bitbucket/codegraph; `Scope.follow_types` excluded ATTACHMENT; and `classify_url` left the canonical a raw `https://` URL so `fetch_node` routed it to the auth-less web fetcher, which rejects `application/pdf`. Fix:
- `classify_url` canonicalizes attachment URLs (Jira `/rest/api/3/attachment/`, Confluence `/wiki/download/`, `/download/attachments/`) to `attachment:<url>` so they route to a new **AttachmentFetcher** (kind="attachment").
- `AttachmentFetcher` downloads with the **authenticated** Atlassian client (`BaseClient.download_bytes`, which follows the redirect to the pre-signed media URL — httpx drops Basic-auth on the cross-origin hop, which is correct) and extracts text off the event loop.
- `common/extract/attachment.py` extracts: **PDF** (pypdf), **MS Word .docx** (python-docx), **MS Excel .xlsx** (openpyxl), **CSV/text/json/xml** (decode), and **images** (Claude-on-Vertex vision via `vertex.describe_image`, degrading to a recorded placeholder when Vertex is unconfigured/offline). `is_extractable_attachment` filters non-readable blobs (zip/video/audio/legacy .doc/.xls) at the source in `assemble.py`, so they never become nodes. `.zip` (e.g. transfer.zip) is intentionally NOT read — it is an archive, not a document.
- ATTACHMENT added to default `Scope.follow_types` and to `_fetchable` — attachments are on-topic + authed, so followed by DEFAULT (not gated behind explore). New deps: pypdf, python-docx, openpyxl, pillow (Docker installs from pyproject).

## 2. Cloud-service discovery noise → relevance gate
The X2 cloud discover promoted the first N live services when NO ticket token matched any service name — the ranking collapsed to liveness+env-weight, filling the cap with arbitrary services (the dev-luz-salary-* noise). Fix in `cloud_discover.py`: a **relevance gate** keeps only services whose haystack (name + resource_path + labels) contains ≥1 salient ticket token OR matches an LLM priority hint, BEFORE ranking/capping. No relevant service → promote NOTHING + a transparent "scanned N, none matched" note. `_term_match` now uses the full haystack too. (Cloud discovery still only runs in explore mode.)

All 510 tests pass, ruff clean on changed files. UNCOMMITTED at time of note. Related: [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]], [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]], [[Testing-Agent refine confidence is capped by un-ingested spec PDFs]].

## Related

- [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]]
- [[Testing-Agent refine confidence is capped by un-ingested spec PDFs]]

---
**DEPLOYED 2026-09-15**: image `32ca0f4-attachments` live on klara-nonprod — all 5 v2 services bumped in-place (Cloud Build 70b994ab; terraform 0 added / 5 changed / 0 destroyed). Commits d3d0a9a (implement timeout + explore gate) + 32ca0f4 (attachments + cloud relevance) both live. Note: client may need /mcp-reconnect to see the new gather_knowledge `explore` param; a plain gather works regardless.
