---
ai_hash: 4f675585a82b7643
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: PROD investigation 2026-09-09
status: seedling
tags:
- gcp
- cloud-logging
- url
- observability
- howto
title: Deep-link a Google Cloud Logging query into the console via URL
type: howto
---

# Deep-link a Google Cloud Logging query into the console via URL

You can hand-craft a Cloud Logging **Logs Explorer** URL that opens pre-filtered to an exact query + time range — useful for putting verifiable "open in console" links into an incident report.

Format (path uses `;`-delimited matrix params, then a normal `?project=` query param):

```
https://console.cloud.google.com/logs/query;query=<ENC>;timeRange=<START>%2F<END>?project=<PROJECT>
```

- `<ENC>` = the full-text filter, **percent-encoded** (encode everything incl. `"` `=` `:` and newlines as `%0A`). The console filter language matches the gcloud `--log-filter`; `field:"x"` is substring, `field="x"` exact.
- `timeRange` = `START/END` ISO-8601, with the `/` encoded as `%2F` (e.g. `2026-09-08T06:20:00Z%2F2026-09-08T06:55:00Z`).
- Viewer needs `logging.viewer` on the project.

Generate reliably with Python `urllib.parse.quote(q, safe="")`. Reader needs no gcloud install — just the browser. Pair each link with the equivalent `gcloud logging read` command for CLI users.

## Related
[[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]

## Related

- [[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]

%% ai-graph-start %%

**Related notes:**
- [[Resolve Cloud Logging share links via redirect Location header]]
- [[Cloud Logging share link endTime can truncate a job's logs mid-run]]
- [[gcloud logging read --freshness is unreliable with broad OR filters; use explicit timestamp bound]]
- [[PowerShell 5.1 eats inner double-quotes passed to native exes like gcloud]]
- [[gcloud logging read --order=asc silently ignores --freshness]]

%% ai-graph-end %%