---
ai_hash: e0a34518e54716b3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.37
entities: []
relevance: 0.721
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/49696374801/IR+-+System+Design
space: AI
status: reference
tags:
- confluence
- architecture
- space/ai
title: IR - System Design
topic: architecture
type: source
updated: 2026-09-15
---

# IR - System Design

> [!info] Imported from Confluence
> Space **AI** · updated 2026-09-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/49696374801/IR+-+System+Design)
> Relevance 0.721 · topic `architecture`

# About this Document

This document describes the system design of the new search solution for ePost.

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="c48b281e-0b07-43a2-9a53-37d2d68b4955" macro-name="toc">

</div>

# Glossary

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>Chunk</p></td>
<td><p>A segment of a document that is embedded, indexed, and retrieved as its own unit.</p></td>
</tr>
<tr>
<td><p>Embedding vector</p></td>
<td><p>A dense, fixed-length numeric representation of a query or document produced by a neural encoder.</p>
<p>Semantically similar texts map to nearby points in the vector space, so dense retrieval can rank documents by vector similarity (e.g., cosine) rather than shared terms, closing the vocabulary gap that keyword search leaves open. Often combined with keyword retrieval in hybrid search.</p></td>
</tr>
<tr>
<td><p>Entity</p></td>
<td><p>A uniquely identifiable real-world object or concept (person, organization, place, product) referenced in a query or document.</p>
<p>Entity linking maps surface mentions to a canonical ID, so "NYC" and "New York City" resolve to the same entity while "Apple" the company and "apple" the fruit are kept apart. Entities support disambiguation, knowledge-graph lookups, and faceted filtering.</p></td>
</tr>
<tr>
<td><p>HNSW graph</p></td>
<td><p>Hierarchical Navigable Small World graph. Provides efficient approximate nearest neighbor search for high dimensional vectors.</p></td>
</tr>
<tr>
<td><p>Keyword</p></td>
<td><p>A term extracted from a query or document used for lexical matching.</p>
<p>Keyword-based retrieval (e.g., BM25) scores documents by how often and how distinctively a query's terms appear in them, which makes it precise but vulnerable to vocabulary mismatch: a query for "car" won't retrieve a document that only says "automobile".</p></td>
</tr>
</tbody>
</table>

</div>

# Architectural Concepts

## Per-tenant indices

Per-tenant indices with per-tenant keys gives strong isolation, crypto-shredding for GDPR deletion (destroy the key, the data is gone), and small per-tenant HNSW graphs, which Lucene handles well.

## Versioning the entire pipeline

Versioning the entire pipeline definition (chunking, metadata prompts, embedding model, etc) is best practice. It gives reproducibility, makes "what produced this vector?" answerable, and enables offline A/B evaluation of strategies.

New embedding model (versions) require a full rebuild: v1 and v2 vectors live in different spaces and cannot coexist in one similarity computation.

The v2 index is built in the background while v1 keeps serving. New and updated documents gets written to both indices. When the backfill completes, the tenant's pointer atomically flips to v2 and v1 can be deleted. The critical part is the double computation and write work during the transition phase. The ability to reuse previous results and to recompute only the needed elements can cut migration cost enormously.


![[49696374801-shared-schema-registry.png]]



## Read-Write separation

The three objectives — low query latency, high update freshness, and low operational cost — form a triangle where improving any one dimension tends to worsen the others. Today solutions do

- Separate ingestion and serving

- Have authoritative state in shared storage, cache data locally


![[49696374801-read-write-separation.png]]



### Read path (serving)

The serving layer of the search solution is responsible for handling user queries and retrieving information from the search index. This layer has two main responsibilities

- Routing – Directing to the appropriate search index version (also allows smooth blue-\>green transitions)

- Search – Executing queries, understanding to the index schema and search strategy

### Write path (ingestion)

The ingestion layer handles the process of updating the search index in response to changes in the underlying data. Optionally replicating to reading index in real-time.

### Offline Processing

Offline processing enables reconstruction of full indices without impact to current search.

## Document (pre-) Processing

Today the Analyze API processes a document and prepares all information required to index the document for search. The existing separation of concerns remains and Analyze API will perform the following work

1.  OCR. Compute text layer for scanned documents, if needed

2.  Splitting a document into smaller logical chunks

3.  Enriching the document and the chunks (e.g. title, summary, context, keywords, entities)

4.  Preparing literal content and calculating embedding vectors


![[49696374801-rag-document.png]]



## Evaluation of IR Performance

Information retrieval (IR) performance evaluation measures how well a search engine, database, or indexing system returns relevant results to satisfy user queries.

Offline Evaluation: Tests a system against a static, pre-collected test dataset with human relevance judgments. It is repeatable, fast, and ideal for early-stage development.

# Technology

## Apache Lucene

A free and open-source (Apache Software License) search engine software library, widely used as a standard foundation for production search applications like (Elasticsearch, MongoDB Atlas, OpenSearch) and a silent powerhouse behind high-speed search at Google, Netflix, Uber, Amazon, and Wikipedia.

Apache Lucene is a disk-first, immutable, merge-driven system that behaves more like a constantly expanding library than a traditional database. It is disk-optimized, not memory-dependent and therefore a perfect match for low resource environments.

### The Lucene Segment Model

Lucene implements logarithmic merging as its segment model. The lifecycle is as follows:

1.  Documents accumulate in a RAM buffer.

2.  Refresh: the buffer is flushed to a new segment in the OS page cache, making it searchable without an fsync.

3.  Flush: Lucene performs an fsync, clearing the translog.

<!-- -->

4.  Merge: background threads coalesce small segments via tiered merge policy.

### Lucene's near-real-time segment index replication

This write-once design enables Lucene to efficiently replicate a search index from one place ("primary") to another ("replica"): in order to sync recent index changes from primary to replica you only need to look at the file names in the index directory, and not their contents. Any new files must be copied, and any files previously copied do not need to be copied again because they are never changed.

## Alternatives to consider

With embedded Lucene, we get maximum control: any storage layout, true physical isolation, tiny per-tenant HNSW graphs, per-tenant strategy versions (tenant A on v1, tenant B on v2), in-process search with no network hop, and trivially cheap tenant deletion.

The cost is that you're building a distributed database's control plane yourself: tenant→node placement and rebalancing, replication/failover, an index-open LRU pool, query routing, observability, and the entire blue/green migration orchestration. None of this is exotic, but it's 60–70% of your engineering budget and it's undifferentiated. The scaling shape is: excellent up to roughly low thousands of active tenants per node pool, then increasingly dominated by heap, file handles, and placement logic.

### OpenSearch

<a href="https://opensearch.org" class="external-link" rel="nofollow">https://opensearch.org</a>

Elasticsearch & Kibana fork, by Amazon Web Services (Apache License v2).

There's an open-source opensearch-storage-encryption plugin that implements transparent encryption at the Lucene Directory level with per-index keys via AWS KMS, using a block cache and read-ahead to limit the performance penalty.

OpenSearch gives the most complete migration toolkit for index version migration: index aliases make per-tenant blue/green a one-call atomic swap, `_reindex` does the backfill, ISM handles lifecycle, snapshots are built in, and hybrid lexical+vector search with score normalization/RRF is native.

The structural limit is index-per-tenant cardinality. Every index is a set of shards, every shard a full Lucene index with fixed heap and cluster-state cost. Practical comfort zone is low thousands of indices per cluster; tens of thousands of tenants means shared indices with tenant-field filtering (weakening isolation and per-tenant keys) or multiple clusters (operational sprawl). Also: even with one shard per tenant, per-tenant is *logical* isolation on shared nodes — a noisy tenant can degrade neighbors, and you don't control the storage layout the way you do embedded.

### Vespa

<a href="https://vespa.ai/" class="external-link" rel="nofollow">https://vespa.ai/</a>

Apache License v2

Vespa's differentiator is streaming mode (it powers Yahoo Mail search).

You make the tenant/user id part of the document id, and Vespa co-locates each tenant's data on the same chunk of disk on a small set of nodes; searches scan only that tenant's data with no index structures maintained at all.

It's suited when per-tenant subsets are small relative to the corpus, and large tenants are automatically sharded across nodes and searched in parallel. This scales to millions of tenants: one to two orders of magnitude past index-per-tenant approaches — at a fraction of the memory cost.

Vespa also has the strongest answer to the reindexing-strategy problem: embedders run inside the engine (feed-time and query-time), documents are stored as source, and Vespa can reprocess the stored corpus through the indexing pipeline when the schema changes — so an embedding-model upgrade is a schema change plus a triggered reindex from data Vespa already holds, rather than an external orchestration project. Multi-phase ranking (cheap first phase, cross-encoder-style second phase) is native. Note that streaming mode has trade-offs: no stemming, and no corpus term statistics, so BM25-family features need an externally supplied significance model.

No per-tenant encryption keys. Encryption at rest is node/volume-level with one key domain. Tenant isolation is logical (document-id grouping), and crypto-shredding per tenant isn't available — deletion is delete-by-group, which must also propagate through backups. If per-tenant keys are a contractual requirement, Vespa fails it today. It's also the steepest learning curve of the three, with the smallest community.

%% ai-graph-start %%

**Related notes:**
- [[Lucene's write-once segments turn replication into a filename diff]]
- [[Changing the embedding model forces a full index rebuild]]
- [[Agent Memory]]
- [[Embedding a search library means building its control plane yourself]]
- [[Full‑Text Document Search — Performance Analysis & Proposals]]

%% ai-graph-end %%