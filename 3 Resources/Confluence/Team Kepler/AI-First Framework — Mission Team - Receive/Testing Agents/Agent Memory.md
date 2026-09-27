---
title: "Agent Memory"
created: 2026-09-07
updated: 2026-09-07
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49731338323/Agent+Memory
confluence_id: "49731338323"
confluence_path: "Team Kepler > AI-First Framework — Mission Team: Receive > Testing Agents"
tags: [confluence, ai-agents]
---

# Agent Memory

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49731338323/Agent+Memory) · updated 2026-09-07*

## Overview

Today the whole memory is **GCS blobs** and retrieval is **case-insensitive substring matching** over a node's `id`/`title`/`type` (`match_index_nodes`).

That is an **event/artifact log with a keyword index**, not a memory — semantically-related knowledge is invisible unless the exact token overlaps.

Split it in two tier:

- **GCS stays the system of record** —

  - the raw, append-only event/artifact tier: crawl notes, rendered markdown, run-logs, refine Q&A/understanding, the capture-queue.

  - Cheap, auditable, human-readable, source of truth.

  - **Unchanged.**

- **Cloud SQL +** `pgvector` **becomes the system of recall** —

  - a queryable materialized view: one `memory_node` table carrying a Vertex **embedding**, a **tsvector**, and **structured metadata**, plus a `memory_edge` table that replaces `knowledge-index.json`.

  - Retrieval becomes **hybrid**: vector similarity `∪` full-text `∪` SQL filters — and the existing **hub-penalty + grounding** de-bias moves onto the SQL/re-rank path so semantic recall can't re-poison unrelated runs.

Reuse the Cloud SQL instance already wired for the task store. Embeddings run **off the request path**. Feature-flagged, backfillable, and reversible: GCS remains the truth, so a Postgres outage degrades recall to today's graph-JSON behaviour — it never breaks the pipeline.

![[image-20260907-061544.png]]

## The split — system of record vs system of recall

- **GCS = write model + audit.**

  - Every artifact still lands in GCS exactly as today.

  - It is the durable truth and the human-readable trail. Nothing is deleted or migrated out.

- **Postgres = read model + semantic index.**

  - A compact, embedded projection of each node: the text that matters, its vector, its full-text tsvector, and its metadata — plus a `content_uri` pointer **back to the GCS blob** for the full bytes.

  - Postgres stays lean; GCS holds the mass.

- **Rebuildable by construction.**

  - Because Postgres is a *projection* of GCS, it can be dropped and re-derived at any time by a backfill pass.

  - This is what makes the migration safe and reversible.

This is the same event-sourcing shape the pipeline already leans on: GCS is the log; the DB is the view.

![[image-20260907-062527.png]]

## Why Cloud SQL + `pgvector`

You already run **Cloud SQL for PostgreSQL** for the A2A task store, dialed through the **Cloud SQL Python Connector** (socketless, IAM-auth + TLS — `common/taskstore.py`, \[\[task-store-cloud-sql\]\]). Adding the `vector` extension to that instance is the lowest-friction path to a real memory:

**pgvector specifics.** `CREATE EXTENSION vector;` (Cloud SQL PG 15+). Column `embedding vector(768)`; cosine distance operator `<=>`; ANN index `USING hnsw (embedding vector_cosine_ops)` (HNSW for recall/latency; IVFFlat as the low-memory alternative). Full-text via `tsvector` + GIN. All in one transaction, one instance.

|  |  |  |
|----|----|----|
| **Option** | **Verdict** | **Why** |
| **Cloud SQL +** `pgvector` ✅ | **Chosen** | One DB holds vectors **and** relational metadata **and** full-text. Reuses the running instance, the Connector, the SQLAlchemy async engine, the Terraform. Hybrid search + the B4/B5 de-bias become plain SQL. No new service. |
| AlloyDB + ScaNN | Scale-up path | Postgres-compatible with the ScaNN vector index + in-DB embedding (`google_ml_integration` calls Vertex from SQL). Better vector recall/latency at large scale — but a new, heavier, costlier service. Documented as the migration target if volume demands it. |
| Vertex AI Vector Search | Rejected for v1 | Managed ANN, but a **pure** vector index: you still need Postgres/GCS for metadata, transactions, veto/scope filters, and the edges. Splits vectors from metadata, adds index-rebuild latency and another service — wrong shape for this metadata-rich, vetoable memory. |
| Firestore vector search | Rejected | Serverless vectors, but weak on relational filters/joins and the edge graph; would fragment the model. |

## Data model

Two tables (a thin projection — the heavy content stays in GCS behind `content_uri`):

**Mapping from today's models** (no data-contract churn):

- `Note` / `Insight` → one `memory_node` row (+ their `links` / `source_refs` → `memory_edge` rows), exactly what `Graph.add_note` / `Graph.add_insight` build today — but as rows, not a rewritten JSON blob.

- The blob CAS (`update_index`) is **replaced by row upserts** (`INSERT … ON CONFLICT (id) DO UPDATE`), which are natively concurrency-safe — the CAS retry loop disappears for the index.

- `embedding` is **nullable and backfilled async** so a write never blocks on Vertex.

**Multilingual note.** LUZ content is Swiss/German as well as English. Use Vertex `text-multilingual-embedding-002` (768 dims) or `gemini-embedding-001` rather than the English-centric `text-embedding-005`, so a German dunning page and an English test note land near each other.

## Embeddings

- **Model:** Vertex AI `text-multilingual-embedding-002` (768 dims) — Swiss/German + English corpus. `gemini-embedding-001` (configurable output dims) is the upgrade if quality demands it.

- **Where:** in the drain worker only, thread-offloaded and bounded (never in a request handler) — see §5.

- **Embeddable text:** `title + "\n" + synopsis` for notes; `statement` for insights/lessons. Keep it short and stable so re-embeds are rare; store a content hash in `meta` to skip re-embedding unchanged text.

- **Query embeddings:** one call per `search-memory` / recall, cached per query string within a turn.

- **Cost/latency:** embeddings are cheap and batched; the request path pays **zero** Vertex latency because it only enqueues. (AlloyDB's in-DB `embedding()` would remove even the app-side call — a scale-up lever.)

```
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE memory_node (
  id           text PRIMARY KEY,              -- canonical id, e.g. 'jira:LUZ-158390', 'insight:...'
  type         text NOT NULL,                 -- jira-issue | confluence-page | codegraph | insight | ...
  kind         text,                          -- for insights: decision|assumption|lesson|correction|gotcha
  title        text,
  synopsis     text,                          -- the embeddable text (title + synopsis + statement)
  source_url   text,
  content_uri  text,                          -- gs:// pointer back to the full note .md in GCS
  run_id       text,                          -- provenance (B0 pack scoping)
  context_id   text,
  scope        text NOT NULL DEFAULT 'context',   -- context | shared
  status       text NOT NULL DEFAULT 'active',    -- active | vetoed | superseded
  confidence   text NOT NULL DEFAULT 'high',       -- high (human) | med | low (agent-derived)
  created_at   timestamptz NOT NULL DEFAULT now(),
  embedding    vector(768),                   -- Vertex text embedding; NULL until the async backfill fills it
  tsv          tsvector,                      -- full-text over title+synopsis (lexical recall; keeps substring parity)
  meta         jsonb NOT NULL DEFAULT '{}'    -- labels, components, depth, supersedes, seed_refs, ...
);

CREATE INDEX memory_node_embedding_hnsw ON memory_node
  USING hnsw (embedding vector_cosine_ops);
CREATE INDEX memory_node_tsv_gin ON memory_node USING gin (tsv);
CREATE INDEX memory_node_filter ON memory_node (type, scope, status);
CREATE INDEX memory_node_run ON memory_node (run_id);

CREATE TABLE memory_edge (                    -- replaces knowledge-index.json edges
  source_id  text NOT NULL,
  target     text NOT NULL,
  type       text,
  origin     text,                            -- description|comment|issuelink|child|insight-kind|...
  in_scope   boolean DEFAULT false,
  PRIMARY KEY (source_id, target)
);
CREATE INDEX memory_edge_target ON memory_edge (target);
```

### Write path — GCS unchanged, Postgres projected async

Keep GCS as the synchronous source of truth; project into Postgres **off the request path**.

1.  **Step handler writes GCS exactly as today** (`upsert_note` / `upsert_insight` / run-log). No behaviour change; GCS is authoritative.

2.  **Enqueue an** `IndexJob` (node id + type + a compact payload) — O(1), reusing the existing durable `mutate_json` CAS queue that the self-learning capture-queue already rides on. The handler returns immediately.

3.  **A drain worker** (the same head-of-next-request + background drain pattern as pulls jobs and, per node:

    - **Metadata upsert** (cheap, no LLM): `INSERT … ON CONFLICT DO UPDATE` the row + edges, `tsv` computed in SQL. This makes filters/full-text consistent quickly even before the vector exists.

    - **Embedding backfill**: one **thread-offloaded, bounded** Vertex embedding call (`asyncio.to_thread`, same discipline as `refine.next_questions`) → `UPDATE … SET embedding = $1`.

### **Why async, not dual-write-inline.**

Cloud Run throttles CPU on the instance once the response is sent unless CPU-always-allocated is set, and **serial blocking Vertex calls have already killed an instance here with** `ERROR_TIMEOUT` .

- Embedding is the slow part, so it must live in the drain, not the handler.

- Metadata is cheap and can go either inline or in the drain.

### **Failure model.**

1.  Postgres is best-effort and **never source of truth**.

2.  If the DB is down or the embed fails: the job stays queued (at-least-once), GCS is intact, and recall falls back to the GCS graph.

3.  Upserts are idempotent (`ON CONFLICT` + content-keyed ids), so re-processing is safe.

### Read path — hybrid retrieval that keeps the de-bias

Every current retrieval call gets a Postgres implementation behind the same function signature, so the executors don't change shape — only the backend does (flag-gated, §8).

|  |  |
|----|----|
| Today (GCS graph) | Becomes (Postgres) |
| `match_index_nodes(graph, q)` — substring | `tsv @@ plainto_tsquery(q)` **∪** `embedding <=> embed(q)` top-K (hybrid) |
| `rank_promotions(...)` — B4 IDF hub-penalty | hybrid candidates **re-ranked** by the same IDF hub-penalty (computed from `df` via SQL `COUNT`), or fused with RRF |
| `graph_grounded(cand, anchors)` — B5 edge gate | `EXISTS (SELECT 1 FROM memory_edge WHERE …)` — the identical structural gate as a SQL predicate |
| `recall_lessons(seed_refs)` — structural only | **semantic + structural**: vector-nearest lessons **AND** `source_refs ∩ seed_refs`, filtered `status='active'`, `scope IN (…)`, re-ranked by confidence + grounding |

### **Hybrid query sketch** (semantic ∪ lexical, filtered, then de-biased):

```
WITH vec AS (
  SELECT id, 1 - (embedding <=> $q_embed) AS vscore
  FROM memory_node
  WHERE status = 'active' AND scope = ANY($scopes) AND type = ANY($types)
  ORDER BY embedding <=> $q_embed
  LIMIT 40
),
lex AS (
  SELECT id, ts_rank(tsv, plainto_tsquery($q_text)) AS lscore
  FROM memory_node
  WHERE status = 'active' AND tsv @@ plainto_tsquery($q_text)
  LIMIT 40
)
SELECT id FROM vec FULL OUTER JOIN lex USING (id)
ORDER BY /* RRF or weighted vscore+lscore */ ... LIMIT 10;
```

Then apply, in app or SQL, the **B4 hub-penalty** (suppress tokens with `df/N > hub_ratio` once the corpus is saturated) and the **B5 grounding gate** (`memory_edge` join to seed anchors). **The de-bias is not dropped — it is ported.** This is the crux: semantic recall *widens* what can surface, and B4/B5 are exactly what stop that widening from re-poisoning unrelated runs.
