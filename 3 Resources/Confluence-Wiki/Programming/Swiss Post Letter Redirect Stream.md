---
ai_hash: 29504e8ed4ef89fd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.3
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/48302227474/Swiss+Post+Letter+Redirect+Stream
space: AI
status: reference
tags:
- confluence
- programming
- space/ai
title: Swiss Post Letter Redirect Stream
topic: programming
type: source
updated: 2025-01-30
---

# Swiss Post Letter Redirect Stream

> [!info] Imported from Confluence
> Space **AI** · updated 2025-01-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/48302227474/Swiss+Post+Letter+Redirect+Stream)
> Relevance 0.711 · topic `programming`

Redirect event stream is accessible via Confluent REST API.

<a href="https://docs.confluent.io/platform/current/kafka-rest/api.html#" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.confluent.io/platform/current/kafka-rest/api.html#</a>

# Support

Contact?

# Use Cases

## Read available partitions

Check the amount of partitions to be able to reset them all.

1.  Acquire access token

2.  Read partitions - This is independend from consumer

## Reset offsets

If committing is enabled and the Analyze Cluster gets started from scratch we need to reset the stream offsets to earliest position.

1.  Acquire access token

2.  Create consumer instance

3.  Subscribe to topic

4.  Consume from topic – This is needed to let the consumer join the group!

5.  Write offsets - Set offset to 0 for each partition

6.  Delete consumer instance

%% ai-graph-start %%

**Related notes:**
- [[MPI KAFKA Stream Letter]]

%% ai-graph-end %%