---
title: "Sub Agentic Loop 1.2 - GCP Service Exploration"
created: 2026-09-11
updated: 2026-09-11
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49745166427/Sub+Agentic+Loop+1.2+-+GCP+Service+Exploration
confluence_id: "49745166427"
confluence_path: "Team Kepler > AI-First Framework — Mission Team: Receive > Testing Agents > Agent Loop 1 - Knowledge Gathering - v2 > Evaluating Knowledge-Gathering Agent - V2"
tags: [confluence, ai-agents]
---

# Sub Agentic Loop 1.2 - GCP Service Exploration

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 1 - Knowledge Gathering - v2 › Evaluating Knowledge-Gathering Agent - V2 · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49745166427/Sub+Agentic+Loop+1.2+-+GCP+Service+Exploration) · updated 2026-09-11*

## **Purpose**

Extend the Knowledge-Gathering Agent's **self-exploration loop** (`RESEARCH-self-exploring-knowledge-gather.md`) with a **runtime/operational** source: the live Google Cloud estate. Today the loop reaches four *document* tiers (memory → Atlassian search → external web → external LLM). It has never looked at **what is actually deployed and running**. This plan adds a **GCP-explore sub-agent** that grounds the test basis in production reality by doing three things, in order:

|  |  |  |
|----|----|----|
| New tier | Responsibility | Rides which existing seam |
| **Tier 5 — Discover** | Enumerate the *prominent* services/deployments across **every env** (`dev`, `dev-staging`, `performance`, `test`, `prod`) for **GKE**, **Cloud Run**, and **managed services** (Cloud SQL, Pub/Sub, …). | New **seed producer** in `expansion_round` → promotes `gcpsvc:` node ids |
| **Tier 6 — Inspect logs** | For each discovered service, read Cloud Logging over an **expanding window** — 7 → 14 → 21 → 28 days — widening only until *enough* signal is captured. | New `NodeFetcher` for the `gcpsvc:` kind (the "fetch" seam) |
| **Tier 7 — Relate** | Infer how the services **communicate** (who calls whom, over what) and emit those as graph edges. | The fetcher emits `LinkRecord`**s** → in-scope → the existing crawl **walks the service graph** |

![[image-20260911-090249.png]]

## The one gap and the one principle

### **Where we are**

The self-exploration loop turns a thin Jira seed into a grounded pack by fanning out across *documents*: prior memory, Jira/Confluence search, web pages, and LLM-suggested leads (`expansion_round` → promote seeds → `crawl` → fetch → follow links → converge). Every node kind it knows (`jira`, `confluence`, `bitbucket`, `codegraph`, `external-web`) is a **document**.

### **The gap**

A test basis built only from documents describes the system **as designed**, never **as running**. The pack cannot answer: *Which services actually implement this ticket? In which envs are they deployed? What errors do they throw today? Which downstream services do they call?* That operational truth is the difference between a plausible test plan and one that exercises the real failure modes (the memory notes already name this — codegraph grounding gives *structure*, not *execution depth*).

### **The principle**

Mirror the loop's existing rule — *"don't stop at the seed"* — into the infrastructure plane:

> **From the ticket's terms, discover the services that actually run it across every environment; read each one's recent logs, widening the window only until the signal is enough, not to a fixed depth; and follow the communication edges the logs reveal to the neighbouring services — the same way the crawl follows a Jira issue-link. Every service and every edge must cite a real GCP resource or log field. The sub-agent prioritizes and distills; it never invents a service or an edge.**

That is Retrieval-Augmented exploration extended to the **live estate**: the GCP API is the authoritative source (like Jira), the deterministic crawl stays the executor, and the LLM is a *planner/distiller*, never a source of truth — exactly the split already used for the `hypothesize` / `leads` planners.

> [!note]- Concepts & terms (glossary)
>
>
> |  |  |  |
> |----|----|----|
> | Term | Definition | Why it matters here |
> | **Env matrix** | The five deployment environments — `dev`, `dev-staging`, `performance`, `test`, `prod` — each resolved to a concrete `(project, region, namespace)` tuple by a single config map. | "Across all envs" must be *data*, not branching logic. One dict maps env → where to look; adding an env is a one-line edit. |
> | **Prominent service** | A deployed workload that is relevant to the ticket *and* actually live (has traffic / recent revisions / recent logs), ranked above the long tail of dormant services. | "Prominent" bounds Tier 5: we promote the top-N ranked services, not every asset in the org. |
> | `gcpsvc:` **node** | A first-class knowledge-graph node for one deployed service in one env: `gcpsvc:<env>/<platform>/<name>` (e.g. `gcpsvc:prod/run/luz-thumbnail`, `gcpsvc:dev/gke/luz-docs`, `gcpsvc:test/managed/cloudsql-taskstore`). | Makes services fetchable + follow-able by the *unchanged* crawl. The canonical id is a stable dedup key, same role as `jira:LUZ-1`. |
> | **Expanding log window** | Read logs for the last 7 days; if signal is insufficient, widen to 14, 21, 28; stop at the first window that yields *enough* (or at the 28-day cap). | The adaptive-breadth analog of the crawl's adaptive depth. A fixed lookback under- or over-reads; the window is the marginal-yield stop applied to *time*. |
> | **"Enough" gate** | The stop condition for the window: ≥ *k* distinct error/log signatures **or** ≥ *m* distinct outbound dependency edges **or** ≥ *n* total in-scope entries — whichever first. | Bounds cost and latency. Same shape as the crawl's convergence stop; without it, every service reads 28 days of logs every time. |
> | **Communication edge** | An inferred `A → B` relationship: service A calls service B (HTTP host, gRPC target, DB instance, Pub/Sub topic, audience claim, trace span). | Tier 7. Emitted as `LinkRecord`s so the frontier walks A → B → C without any new traversal code. |
> | **Grounding gate (infra)** | An edge or service enters the pack only if backed by a real GCP field (a resolved resource, a log line, a config value). LLM-suggested edges that can't be corroborated are dropped or demoted to "unconfirmed". | The single highest-risk rule. Skipping it injects a hallucinated topology into the test basis. Direct port of the loop's Tier-3b lead-grounding gate. |
> | **Read-only reach** | Every GCP call is a *viewer* operation (list assets, read logs, describe services). No mutation, ever. | Security boundary. The agent's SA gets only `*.viewer` roles; a gather can never change the estate it inspects. |
> | **Cloud Asset Inventory (CAI)** | `cloudasset.searchAllResources` — one API that lists resources of many types across all projects in a folder/org. | The cheapest single call to enumerate "everything deployed across all envs". Per-platform list APIs are the fallback when CAI isn't available. |
>
>

## Where we are — the loop's anatomy & the exact seams

### The three extension points (from the current code)

1.  **Seed-producer seam —** `expansion_round()` (`gather/explore/expand.py`). Runs one pre-crawl fan-out and returns `(new_seeds, md_blocks)`. Every tier is just a function that returns canonical node ids to promote and a markdown block for the reply. **Tier 5 is a new producer here.**

2.  **Fetcher seam — the** `NodeFetcher` **registry** (`gather/crawl/fetch/base.py`). One subclass per node `kind`, auto-registered by its `kind` prefix; `fetch_node` dispatches on the id prefix with zero changes. Adding a source = drop in a subclass + import it. **Tier 6 is a new** `GcpServiceFetcher(kind="gcpsvc")`**.**

3.  **Link-follow seam —** `crawl()` **+** `_fetchable()` (`gather/crawl/crawl.py`). A node's in-scope `LinkRecord`s are pushed to the frontier and fetched, bounded by `depth / max_nodes / max_seconds`. **Tier 7 needs only that** `gcpsvc:` **edges are** `in_scope` **and** `gcpsvc` **is** `_fetchable` **— the crawl then walks the service graph for free.**

### The planner pattern to copy

The two existing planners (`hypothesize`, `leads`) are ADK `LlmAgent`s with a Pydantic `output_schema`, run by `GatherAgent._run_planner` **before** `expansion_round`; each degrades to a no-op when Vertex is unconfigured (`_run_planner` swallows the failure). **The GCP-explore sub-agent is a third planner of exactly this shape** — it plans *which* envs/services matter and *distills* what the deterministic reach returns; the reach itself is plain, offloaded GCP calls.

### What must NOT change (invariants inherited from history)

- **The deterministic crawl stays** — new tiers feed it seeds and edges; they never replace `crawl.py`.

- **The human gate stays** — `refine → approve` still decide what becomes the test basis. Every service node and edge carries provenance so a human can prune.

- **One LLM budget discipline** — the sub-agent is *one* planner call (like `hypothesize`), not a per-service call. Memory: *serial blocking model/API calls in an async handler blew Cloud Run's liveness/request timeout*. All GCP I/O is offloaded to threads and bounded.

- **No silent truncation** — every cap hit (max services, window at 28 d, dropped edges) is logged and surfaced in the reply, per the standing rule.

## Target architecture — the GCP-explore sub-agent

The sub-agent is invoked once (Tier 5 planning + ranking); Tiers 6 and 7 are pure fetcher work that the crawl drives per node. The service graph converges on the same budget knobs the document crawl uses.

## Tier 5 — Discover services across every env

**Goal.** From the ticket terms, list the *prominent* deployed services across `dev / dev-staging / performance / test / prod` for **GKE**, **Cloud Run**, and **managed services**, and promote the top-N to `gcpsvc:` seeds.

### The env matrix (config, not code)

One map is the whole "all envs" surface. Grounded in the org defaults the `google-skill-*` skills encode (`klara-nonprod` project, `europe-west6`, namespace-per-env; `prod` in its own project):

```
# knowledge_gathering/gather/explore/gcp/envs.py
ENV_MATRIX = {
    "dev":         Env(project="klara-nonprod", region="europe-west6", namespace="dev"),
    "dev-staging": Env(project="klara-nonprod", region="europe-west6", namespace="staging"),
    "performance": Env(project="klara-nonprod", region="europe-west6", namespace="performance"),
    "test":        Env(project="klara-nonprod", region="europe-west6", namespace="test"),
    "prod":        Env(project="klara-prod",    region="europe-west6", namespace="prod"),
}
```

Envs, projects, and namespaces are **read from config** (env var `KGA_GCP_ENV_MATRIX` as JSON, falling back to this default). Adding `sandbox` is a one-line dict entry, never a new branch.

### The reach — one API, per-platform fallback

**Primary: Cloud Asset Inventory.** One `searchAllResources(scope=folder/…, assetTypes=[…])` call per env-project enumerates all three platform families:

|  |  |
|----|----|
| Platform | Asset type(s) |
| Cloud Run | `run.googleapis.com/Service` |
| GKE | `container.googleapis.com/Cluster` + workloads via `k8s.io/Deployment`, `k8s.io/StatefulSet` |
| Managed | `sqladmin.googleapis.com/Instance`, `pubsub.googleapis.com/Topic`, `redis.googleapis.com/Instance`, `cloudtasks.googleapis.com/Queue`, … (config-driven allow-list) |

**Fallback (CAI not enabled / no org-level scope):** per-platform list clients — `run_v2.ServicesClient.list_services`, `container_v1.list_clusters`, `sqladmin`/`pubsub` list — iterated over the env matrix. Same output shape. Each platform failing independently degrades to the others (the tier-2 pattern: *one source failing must not kill the rest*).

Every discovered asset carries its **full resource path** as provenance (`//run.googleapis.com/projects/klara-prod/locations/europe-west6/services/luz-thumbnail`).

### Ranking — what "prominent" means

Discovery can return hundreds of assets. The sub-agent (or its heuristic fallback) scores each and keeps the top-N (`KGA_GCP_MAX_SERVICES`, default 8):

```
score(service) =  w1 · term_match(name, labels ; ticket_terms)     # relevance
                + w2 · liveness(recent_revisions | recent_logs)     # actually running
                + w3 · env_weight(prod > test > perf > staging > dev)  # closer to reality
```

- `term_match` reuses the loop's existing salient-token matcher (`salient_tokens`) — no new NLP.

- `liveness` is a cheap signal already available from the asset (update time / revision count); a service with zero recent activity is dormant, not prominent.

- The **LLM's only job** here is to *re-rank and cluster* the candidate list against the ticket intent (e.g. "these three thumbnail services are the same logical service across envs"). It **cannot add** a service that discovery didn't return — the grounding gate. With no Vertex, the numeric score alone ranks.

### Output

Top-N services → `gcpsvc:<env>/<platform>/<name>` ids appended to `extra_seeds` (exactly like `atlassian_search_seeds`), plus a markdown block listing what was found per env and what was capped.

![[image-20260911-092404.png]]

## Tier 6 — Inspect logs with an expanding window

**Goal.** For each `gcpsvc:` node the crawl fetches, read Cloud Logging over an **expanding time window** and distill it into a Note.

### The `GcpServiceFetcher`

A `NodeFetcher(kind="gcpsvc")`. `fetch(client, ident, nid, scope)` parses `ident = "<env>/<platform>/<name>"`, resolves the env tuple, and reads logs with a platform-appropriate filter (reusing the exact filters the org skills already use):

- Cloud Run → `resource.type=cloud_run_revision AND resource.labels.service_name=<name>`

- GKE → `resource.type=k8s_container AND resource.labels.container_name=<name> AND …namespace_name=<ns>`

- Managed → resource-type-specific (`cloudsql_database`, `pubsub_topic`, …)

### The expanding window (the heart of Tier 6)

```
WINDOWS = (7, 14, 21, 28)   # days; KGA_GCP_WINDOWS overrides
for days in WINDOWS:
    entries = await asyncio.to_thread(read_logs, env, name, platform, days, cap=MAX_ENTRIES)
    signal = summarize_signal(entries)          # error sigs, routes, dep edges, count
    if enough(signal):        # ≥k error sigs OR ≥m dep edges OR ≥n entries
        break
    log.info("gcpsvc %s: %dd window insufficient (%s) — widening", nid, days, signal.brief())
# else: reached 28d cap — record the shortfall as a gap, never silently
```

- **Read-only, bounded, off the event loop.** `read_logs` is the sync Cloud Logging client wrapped in `asyncio.to_thread` (memory: sync GCS/Vertex on the loop = Cloud Run liveness death). Entry count is capped (`MAX_ENTRIES`, default 2000, matching the skills' `LIMIT`); severity defaults to `WARNING`+ to keep volume down, widening to all severities only if the window is starved.

- **"Enough" is marginal-yield on time**, not a fixed lookback — the same convergence principle the crawl applies to depth, applied to the window.

- **Concurrency** is inherited: the crawl already fetches each depth level concurrently under a semaphore, so N services' windows expand in parallel, not serially.

### Distill + redact

The window's entries become the Note `synopsis`:

- **Purpose** — top request routes / handler names / operation types.

- **Health** — the distinct error/exception signatures + their counts (dedup by normalized message, not raw lines — the memory bank must not fill with 2000 near-identical stack traces).

- **Dependencies** — the outbound targets seen (feeds Tier 7).

**Redaction is mandatory** (security, non-negotiable): log bodies routinely carry tokens, PII, and connection strings. The distiller strips anything matching secret/PII patterns and keeps *signatures and counts*, never raw payloads. Nothing sensitive enters the persisted Note.

## Tier 7 — Relate: how the services communicate

**Goal.** Turn each service's logs+config into `A → B` communication edges, and let the crawl walk them.

### Where edges come from (each cites a real field — the grounding gate)

|  |  |  |
|----|----|----|
| Signal source | Edge evidence | `origin` |
| **Log fields** | outbound HTTP host, gRPC `:authority`, DB host/instance, Pub/Sub topic in the payload metadata, `httpRequest.referer`, JWT `aud` | `gcp-log` |
| **Config** | Cloud Run env vars holding a sibling service URL, GKE `Service` DNS (`svc.namespace.svc.cluster.local`), VPC connector targets | `gcp-config` |
| **Trace** | Cloud Trace spans that share a trace-id across two services in one request (the strongest, cheapest topology signal) | `gcp-trace` |

Each becomes `LinkRecord(source_id="gcpsvc:prod/run/A", url=<resource>, type="gcp-edge", canonical_url="gcpsvc:prod/run/B", origin="gcp-log", in_scope=True)`. An edge with no resolvable target resource is **dropped or demoted to a recorded-only "unconfirmed" edge** — never promoted. The LLM may *label* an edge ("A publishes billing events to B") but may not *create* one without a field behind it.

### The crawl walks the graph for free

Because `gcpsvc` edges are `in_scope` and `gcpsvc` is added to `_fetchable`, the existing loop pushes B onto the frontier, fetches B's logs (Tier 6 again), discovers B's edges (Tier 7 again), and converges when `depth` / `max_nodes` / `max_seconds` are hit — **identical machinery to following a Jira issue-link**. This is the whole reason to model services as nodes and communication as links: Tier 7 needs almost no new traversal code, only edge-extraction.

### Result

The pack gains a **live service-communication subgraph** rooted at the services that implement the ticket: who they are, per env; what they log; and how they talk to each other — each fact traceable to a GCP resource or log line. That is the operational grounding the document tiers can't provide.
