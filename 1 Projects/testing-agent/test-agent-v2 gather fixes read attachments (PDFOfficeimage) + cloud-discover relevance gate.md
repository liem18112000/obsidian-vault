---
ai_hash: 7798df0fca77d225
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- test-agent-v2
- gather fixes
- read attachments
- cloud-discover relevance gate
- feature/test-agent/v2-adk
- implement-timeout
- explore-gate
- Jira attachments
- Confluence attachments
- LUZ-158230
- _fetchable()
- crawl.py
- Jira
- Confluence
- Bitbucket
- Codegraph
- Scope.follow_types
- classify_url
- AttachmentFetcher
- authenticated Atlassian client
- BaseClient.download_bytes
- common/extract/attachment.py
- PDF extraction
- MS Word .docx extraction
- MS Excel .xlsx extraction
- CSV/text/json/xml extraction
- image extraction
- pypdf
- python-docx
- openpyxl
- Claude-on-Vertex vision
- vertex.describe_image
- Vertex
- is_extractable_attachment
- assemble.py
- ATTACHMENT
- Pillow
- Docker
- pyproject
- Cloud-service discovery
- relevance gate
- cloud_discover.py
- ticket token
- LLM priority hint
- _term_match
- explore mode
- test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in
  gate
- Testing-Agent refine confidence is capped by un-ingested spec PDFs
- LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- image 32ca0f4-attachments
- klara-nonprod
- Cloud Build 70b994ab
- terraform
- d3d0a9a
- 32ca0f4
- gather_knowledge explore param
- client
source: test-agent-v2 code changes 2026-09-15
status: seedling
tags:
- testing-agent
- kga
- gather
- attachments
- pdf
- ocr
- cloud-discover
- precision
- v2
title: 'test-agent-v2 gather fixes: read attachments (PDF/Office/image) + cloud-discover
  relevance gate'
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Testing-agent refine loses confidence when source-of-truth PDF attachments are undistilled gaps]]
- [[Testing-Agent refine confidence is capped by un-ingested spec PDFs]]
- [[testing-agent implement_plan generates scenarios per pack-node x 4 kinds, amplifying pack noise and ignoring non-functional-kind guidance]]
- [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]]
- [[Testing-agent refine flags low confidence when spec PDFs are recorded-only]]

**Relations:**
- test-agent-v2 — *HAS_WORK_ITEM* — gather fixes
- gather fixes — *IN_BRANCH* — feature/test-agent/v2-adk
- gather fixes — *INCLUDES_FIX* — read attachments
- gather fixes — *INCLUDES_FIX* — cloud-discover relevance gate
- gather fixes — *BUILDS_ON* — implement-timeout
- gather fixes — *BUILDS_ON* — explore-gate
- read attachments — *ADDRESSES_PROBLEM* — LUZ-158230
- read attachments — *APPLIES_TO* — Jira attachments
- read attachments — *APPLIES_TO* — Confluence attachments
- Jira attachments — *BLOCKED_BY* — _fetchable()
- Confluence attachments — *BLOCKED_BY* — _fetchable()
- _fetchable() — *DEFINED_IN* — crawl.py
- _fetchable() — *WHITELISTS* — Jira
- _fetchable() — *WHITELISTS* — Confluence
- _fetchable() — *WHITELISTS* — Bitbucket
- _fetchable() — *WHITELISTS* — Codegraph
- ATTACHMENT — *EXCLUDED_BY* — Scope.follow_types
- classify_url — *CANONICALIZES* — Jira attachments
- classify_url — *CANONICALIZES* — Confluence attachments
- classify_url — *ROUTES_TO* — AttachmentFetcher
- AttachmentFetcher — *HANDLES_KIND* — ATTACHMENT
- AttachmentFetcher — *USES* — authenticated Atlassian client
- authenticated Atlassian client — *USES_METHOD* — BaseClient.download_bytes
- common/extract/attachment.py — *PERFORMS* — PDF extraction
- common/extract/attachment.py — *PERFORMS* — MS Word .docx extraction
- common/extract/attachment.py — *PERFORMS* — MS Excel .xlsx extraction
- common/extract/attachment.py — *PERFORMS* — CSV/text/json/xml extraction
- common/extract/attachment.py — *PERFORMS* — image extraction
- PDF extraction — *USES_LIBRARY* — pypdf
- MS Word .docx extraction — *USES_LIBRARY* — python-docx
- MS Excel .xlsx extraction — *USES_LIBRARY* — openpyxl
- image extraction — *USES_SERVICE* — Claude-on-Vertex vision
- Claude-on-Vertex vision — *INVOKES_METHOD* — vertex.describe_image
- Claude-on-Vertex vision — *HOSTED_ON* — Vertex
- is_extractable_attachment — *FILTERS_BLOBS_IN* — assemble.py
- ATTACHMENT — *ADDED_TO_CONFIG* — Scope.follow_types
- ATTACHMENT — *ADDED_TO_FUNCTION* — _fetchable()
- read attachments — *ADDS_DEPENDENCY* — pypdf
- read attachments — *ADDS_DEPENDENCY* — python-docx
- read attachments — *ADDS_DEPENDENCY* — openpyxl
- read attachments — *ADDS_DEPENDENCY* — Pillow
- Pillow — *INSTALLED_VIA* — Docker
- Pillow — *INSTALLED_FROM* — pyproject
- cloud-discover relevance gate — *FIXES_PROBLEM* — Cloud-service discovery
- relevance gate — *DEFINED_IN* — cloud_discover.py
- relevance gate — *FILTERS_BY* — ticket token
- relevance gate — *FILTERS_BY* — LLM priority hint
- relevance gate — *OPERATES_ON_DATA* — haystack
- haystack — *INCLUDES* — name
- haystack — *INCLUDES* — resource_path
- haystack — *INCLUDES* — labels
- _term_match — *USES_DATA* — haystack
- Cloud-service discovery — *RUNS_IN_MODE* — explore mode
- test-agent-v2 — *RELATED_WORK* — test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate
- test-agent-v2 — *RELATED_WORK* — Testing-Agent refine confidence is capped by un-ingested spec PDFs
- test-agent-v2 — *RELATED_WORK* — LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- image 32ca0f4-attachments — *DEPLOYED_TO* — klara-nonprod
- image 32ca0f4-attachments — *DEPLOYED_ON* — 2026-09-15
- image 32ca0f4-attachments — *BUILT_BY* — Cloud Build 70b994ab
- image 32ca0f4-attachments — *INCLUDES_COMMIT* — d3d0a9a
- image 32ca0f4-attachments — *INCLUDES_COMMIT* — 32ca0f4
- d3d0a9a — *IMPLEMENTS* — implement-timeout
- d3d0a9a — *IMPLEMENTS* — explore-gate
- 32ca0f4 — *IMPLEMENTS* — read attachments
- 32ca0f4 — *IMPLEMENTS* — cloud-discover relevance gate
- gather_knowledge explore param — *AFFECTS* — client

%% ai-graph-end %%