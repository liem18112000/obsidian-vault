---
ai_hash: 7dfef7af33db7387
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
tags:
- gcp
- vertex-ai
- monitoring
- cost
- klara-nonprod
- testing-agent
---

# Querying Vertex AI Claude token usage (klara-nonprod)

**Goal:** how much a model (e.g. `claude-sonnet-5`) consumed on Vertex AI.

**Key gotchas:**
- `gcloud monitoring` has **no** `metrics-descriptors` / time-series read subcommand — hit the Monitoring API v3 directly with `curl` + `gcloud auth print-access-token`.
- **No BigQuery billing export** exists in klara-nonprod (`bq ls` shows only `analytics_123456789`, `dev_pubsub_log`), so exact cost can't be confirmed — only token counts are hard data.
- Vertex is **partner-billed**; first-party Sonnet-5 rates ($3/$15/M, cache 1.25×/0.1×) are an approximation, not the invoice.

**Metric:** `aiplatform.googleapis.com/publisher/online_serving/token_count`
(DELTA/INT64). Labels: `metric.label.type` = input | output | cache_read_input | cache_write_input | cache_write_1h_input; `metric.label.request_type`; resource label `model_user_id` = model id (`claude-sonnet-5`, `claude-haiku-4-5`, `gemini-*`, embeddings).

**Query (30d, grouped):**
```
TOKEN=$(gcloud auth print-access-token)
curl -s -G -H "Authorization: Bearer $TOKEN" \
  "https://monitoring.googleapis.com/v3/projects/klara-nonprod/timeSeries" \
  --data-urlencode 'filter=metric.type="aiplatform.googleapis.com/publisher/online_serving/token_count"' \
  --data-urlencode "interval.startTime=$(date -u -d '30 days ago' +%Y-%m-%dT%H:%M:%SZ)" \
  --data-urlencode "interval.endTime=$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
  --data-urlencode 'aggregation.alignmentPeriod=2592000s' \
  --data-urlencode 'aggregation.perSeriesAligner=ALIGN_SUM' \
  --data-urlencode 'aggregation.crossSeriesReducer=REDUCE_SUM' \
  --data-urlencode 'aggregation.groupByFields=resource.label."model_user_id"' \
  --data-urlencode 'aggregation.groupByFields=metric.label."type"'
```

**Windows/git-bash trap:** curl writes to git-bash `/tmp`, but Windows `python` resolves `/tmp` differently — write the JSON to the scratchpad dir and read it by absolute Windows path (heredoc `python - <<PY` also steals stdin, so read from file, not a `cat | python` pipe).

**GCP UI (no curl):** Cloud Monitoring → Metrics Explorer → resource `Vertex AI Publisher Model`, metric `Token count`, group by `model_user_id` + `type`, aligner=sum. PromQL tab: `sum by (model_user_id, type) (rate(aiplatform_googleapis_com:publisher_online_serving_token_count[30d]))`. For **cost** (not tokens): Billing → Reports, service=Vertex AI, group by SKU. klara-nonprod billing account = `01A7BD-723676-99D33D`; direct link `https://console.cloud.google.com/billing/01A7BD-723676-99D33D/reports?project=klara-nonprod` (filter Service=Vertex AI + group by SKU manually — billing-reports pageState schema is undocumented/breaks across console versions, unlike Monitoring's which deep-links fine). Billing data lags hours-to-a-day.

**Test agent tie-in:** `claude-sonnet-5` = testing-agent default tier (v1+v2 share klara-nonprod); `claude-haiku-4-5` = fast tier. Gemini + embedding usage is other workloads (Leo CDP / KGA memory), not the test agent.

Result 2026-09-23: sonnet-5 = 32.1M tokens/30d (~$204 est).

%% ai-graph-start %%

**Related notes:**
- [[Claude Sonnet 5 confirmed working on Vertex AI for klara-nonprod]]
- [[Vertex AI Model Garden enablement and quota are separate, per-model steps]]
- [[List Anthropic models on Vertex via the publisherModels REST endpoint]]
- [[Claude models are available on GCP Vertex AI Model Garden]]
- [[Claude on Vertex AI availability is per-project per-region (klara-nonprod)]]

%% ai-graph-end %%