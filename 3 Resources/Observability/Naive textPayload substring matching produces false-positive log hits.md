---
ai_hash: 7bed6c6923b4006e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: PROD investigation 2026-08-03
status: seedling
tags:
- logs
- observability
- gotcha
- triage
title: Naive textPayload substring matching produces false-positive log hits
type: lesson
---

# Naive textPayload substring matching produces false-positive log hits

When triaging by grepping log `textPayload` for a token like `503`, expect **false positives**: the digits appear in latency values (e.g. `time-consuming=503` ms), IDs, and amounts — not just HTTP status. A raw substring match conflates all of them and inflates the apparent failure count.

## Rule
Match on the **specific, structured** form, not the bare number:
- prefer `status code 503` / `status-code=503` (or the WARN/ERROR line that actually carries the HTTP status) over `503`;
- confirm severity/logger too (the real invoice-PDF fails were `status code 503` at WARN, whereas most `503` substring hits were latency/ID noise);
- always eyeball a few full payloads before trusting a count, and re-count with the tightened filter.

## Related
[[gcloud logging read --order=asc silently ignores --freshness|gcloud logging read: --order=asc silently ignores --freshness]] [[Re-wrapping a 5xx as 4xx defeats status-based retry]]

## Related

- [[gcloud logging read --order=asc silently ignores --freshness|gcloud logging read: --order=asc silently ignores --freshness]]
- [[Re-wrapping a 5xx as 4xx defeats status-based retry]]

%% ai-graph-start %%

**Related notes:**
- [[Re-wrapping a 5xx as 4xx defeats status-based retry]]
- [[gcloud logging read --order=asc silently ignores --freshness]]
- [[gcloud logging read --freshness is unreliable with broad OR filters; use explicit timestamp bound]]
- [[Error volume and error severity are independent, so triage by impact not by count]]
- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]

%% ai-graph-end %%