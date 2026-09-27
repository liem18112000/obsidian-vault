---
title: "customer360-api events reader: per-source vs single-bucket mode"
created: 2026-09-16
type: reference
status: seedling
source: "session 2026-09-16"
tags: [leo-cdp, customer360-api, s3, events, data-model]
---

# customer360-api events reader: per-source vs single-bucket mode

The customer360-api behavioral-events reader (`core/repositories/event_query_repository.py`) reads S3 in TWO modes, chosen by whether `EVENT_S3_BUCKET` is set:

```python
bucket = self.settings.event_s3_bucket or f"data-tracking-{source_id}"
if self.settings.event_s3_bucket:   # SINGLE-BUCKET (Hive) mode
    prefix = f"{event_s3_prefix}/tenant_id={tenant_id}/source_id={source_id}/event_date={date}/"
else:                                # PER-SOURCE mode (default)
    prefix = f"{event_s3_prefix}/{date}-"
```

## Per-source mode (EVENT_S3_BUCKET empty) = the working default
Matches the tracking WRITER (`data-tracking-api/core/storage.py:60`): bucket `data-tracking-<data_source_id>`, key `events/<YYYY-MM-DD-HH>/<batch>.jsonl.gz`. Reader lists `events/<date>-` in each per-source bucket, reads `.jsonl(.gz)`, then post-filters rows by `event_time` + facets. `event_s3_prefix` defaults to `events`. This is what actually contains data.

## Single-bucket mode (EVENT_S3_BUCKET set) = requires a writer migration
Reader expects ONE bucket with Hive partitions `events/tenant_id=/source_id=/event_date=/`. The current writer does NOT produce that layout, so setting a bucket yields EMPTY analytics until the writer is migrated (split each batch by tenant+event_date) and history is backfilled.

## Consequence
For the reader to work today, leave `EVENT_S3_BUCKET` empty. But an empty value is exactly what the ssh deploy transport dropped -> see [[ssh drops empty positional args; pass a base64 newline-joined argv + mapfile]]. Reaching per-source buckets still needs endpoint/region/creds/path-style wired (vStorage hcm04.vstorage.vngcloud.vn, region us-east-1, path-style).

## Related

- [[ssh drops empty positional args; pass a base64 newline-joined argv + mapfile]]
