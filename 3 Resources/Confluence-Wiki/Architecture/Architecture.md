---
ai_hash: 1dd7d2b66f46d815
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.52
entities: []
relevance: 0.769
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/49471946952/Architecture
space: FUT
status: reference
tags:
- confluence
- architecture
- space/fut
title: Architecture
topic: architecture
type: source
updated: 2026-06-25
---

# Architecture

> [!info] Imported from Confluence
> Space **FUT** · updated 2026-06-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/49471946952/Architecture)
> Relevance 0.769 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="8dbca138-4287-4dee-8ae3-ce5a0fbeb964" macro-name="toc">

</div>

# Architecture Overview


![[49471946952-image-20260624-101402.png]]



### **Use cases**

1.  User accesses Polaris GUI to search documents (skills, tools, agents, technical documents, etc.)

2.  Developer uses IDE and connects to Polaris MCP to call tool document search (skills, tools, agents, technical documents, etc.)

3.  Developer uses IDE to analyze workload logs on GCP

4.  Auto-trigger PR code review when new PR is created on Bitbucket

5.  User accesses Polaris GUI to contribute skills and agents

### **Open questions**

1.  What tools are inside the Runtime container? Are these built into the agent code and not exposed to the MCP client outside the Polaris platform?

*The built-in tool can read/write files or execute CRUD operations on the Knowledge container.*

2.  Does the Directory container look up the agent tool schema from built-in agent tools when connecting to the Runtime container?

3.  `Directory → Runtime` (*looks up*): **TBD**

    - what is “tool” in Runtime?

    - what information of agent loops, coding agent, tools can be cataloged?

    - how about agent run history?

4.  <span class="inline-comment-marker" ref="9dc60383-4f5a-4c2f-8c19-125a834b6d72">When or why does the Polaris API container access the Models container directly? Does the Polaris API container need an LLM to decide which agent or tool to call before routing?</span>

5.  <span class="inline-comment-marker" ref="060022e1-3de5-44d2-9068-d6bd83bdc247">How does the MCP container enforce policy? Through the Polaris API container or directly via the Security container?</span>

------------------------------------------------------------------------

# Use cases explanation

## 1. Big picture

Polaris is structured into **three concentric tiers** inside one bounded context:

- **Front** — entry points: `GUI`, `MCP`, `Polaris API` (the router).

- **Back** — the brain: `Directory` (catalog), `Runtime` (agents/tools execution), `Models` (LLM access), `Knowledge` (codebase + docs + memory bank).

- **Base** — cross-cutting concerns: `Notification`, `Observability`, `Security`.

External actors sit on the perimeter: `Polaris User`, `Client-side Agent (Cursor/Claude Code/Copilot)`, `CI/CD`, `Bitbucket` on the left; `Jira+Confluence`, `M365/SharePoint`, `GCP` on the right; `Chat (Slack/Teams)` and `Google Identity` at the bottom.

------------------------------------------------------------------------

## 2. Arrows explained, with concrete examples

### A. Inbound traffic into Polaris (left edge → Front)

<div>

|  |  |  |
|----|----|----|
| Arrow | Meaning | Concrete example |
| `Polaris User → GUI` (*uses*) | Humans (engineers, PMs, admins) open a browser. | A team lead opens the Polaris web app to browse the Directory of agents, edit an "Onboarding Buddy" skill, or inspect an agent run history. |
| `Client-side Agent → MCP` (*uses via MCP*) | IDE-resident agents speak the Model Context Protocol to Polaris. | A developer in Cursor types `@polaris find the JIRA ticket that owns this file and summarize related Confluence pages`. Cursor calls Polaris MCP, which exposes Polaris tools and proxies sanctioned external MCPs. |
| `CI/CD Pipeline → Polaris API` (*release/test trigger*) | Pipelines invoke Polaris programmatically. | After a build, Bamboo/Jenkins POSTs `/runs` to trigger the "Release Notes" agent which composes notes from merged PRs + Jira keys. |
| `Bitbucket → Polaris API` (*PRs, webhooks*) | SCM events drive agents. | A new PR fires a webhook → API spawns the "PR Reviewer" agent in Runtime. |
| `Bitbucket → Knowledge` (*ingests source code*) | Direct ingestion path for code intelligence. | Nightly job clones repos, computes embeddings, refreshes the code index used for RAG. |
| `GCP → Polaris API` (*alerts trigger*) | Cloud monitoring closes the loop. | A Cloud Monitoring alert hits `/incidents/triage`; Polaris launches the "Incident Triager" agent that pulls recent deploys and Confluence runbooks. |

</div>

### B. Front layer internal flow

- `GUI → Polaris API` (*consumes*): the SPA only talks to one HTTP surface, never directly to Back tier.

- `MCP → Polaris API` (*consumes*): MCP is a thin protocol adapter; all logic still flows through the API router.

- The diagram shows arrows **both ways** between `MCP ↔ Polaris API`. The downward arrow is "consumes". The return is the API exposing Polaris-native tools that MCP surfaces to clients (e.g., `polaris.search_knowledge`, `polaris.run_agent`).

> **Architectural note:** the API box explicitly says *"router… should not implement business logic. Security policy enforcement."* This is the classic **Backend-for-Frontend / API Gateway** pattern — keep it dumb, push semantics into Runtime/Directory/Knowledge.

### C. Polaris API → Back tier

<div>

|  |  |  |
|----|----|----|
| Arrow | Meaning | Example |
| `API → Directory` (*lookup*) | Resolve symbolic names to definitions. | "Run agent `pr-reviewer@v3`" → Directory returns its skill bundle, model preference, allowed tools. |
| `API → Runtime` (*uses tools / uses MCPs*) | Execute work. | The "Architecture Q&A" agent loops: think → call tool → think → answer. |
| `API → Knowledge` (*access*) | Direct read/write for documents and memory. | GUI page "search our codebase + Confluence" issues a hybrid search via API → Knowledge. |
| `API → Models` (*access*) | Lightweight passthrough. | A "summarize this paragraph" call from the GUI doesn't need an agent loop — API streams straight to Models. |

</div>

### D. Inside the Back tier

- `Directory → MCP`(looks up): Tool schemas live in MCP. Directory looks up them.

- `Directory → Runtime` (*looks up*): **TBD**

  - what is “tool” in Runtime?

  - what information of agent loops, coding agent, tools can be cataloged?

  - how about agent run history?

- `Directory → Knowledge` (*looks up*): Definitions of agents/skills/rules physically live in Knowledge; Directory indexes/projects them.

- `Directory → Models` (*looks up*): registry of model cards / availability.

- `Runtime → Knowledge` (*CRUD, looks up*): Agents read context (RAG over code + docs) and **write back to the memory bank** — long-term agent memory, run artifacts, learned facts.

- `Runtime → Models` (*looks up*): Agent loop calls Gemini/Claude/in-house LLMs.

> **Why split Directory and Knowledge?** Directory is the **control plane** ("what exists, what's allowed"); Knowledge is the **data plane** ("the actual content"). Same pattern as Kubernetes API server vs etcd, or Spring's `BeanFactory` vs the bean instances.

### E. Outbound to enterprise SaaS (right edge)

- `MCP → Jira + Confluence` (*board/docs/tickets*) and `MCP → M365/SharePoint`: external MCP servers are reached **through Polaris MCP**, not directly by the IDE. This is deliberate — Polaris MCP is where **security policy enforcement** happens (label-based DLP, "no PII out of LUZ", per-user tool allowlists).

  - Example: a Cursor user asks "list my open Jira tickets" → Polaris MCP brokers the Atlassian MCP call, attaches the user's identity, redacts forbidden custom fields.

- `Models → GCP`: LLMs hosted on Vertex AI / Gemini.

### F. Base tier (cross-cutting)

- `Runtime → Notification` (*sends notifications*) → `Notification → Chat (Slack/Teams)` (*summaries · notif.*): when the "Standup Bot" agent finishes, it posts a digest to a Slack channel.

- `Runtime / Knowledge / Models → Observability` (*logs, logs and traces*): every LLM call, tool invocation, and KB read is traced. Useful for cost attribution ("which team burned the Gemini quota?") and post-incident review ("which agent leaked that prompt?").

- `Polaris API → Security` (*uses*) → `Security → Google Identity` (*auth*): API delegates authN/Z. Security holds the **policy decision point (PDP)**; API and MCP are the **policy enforcement points (PEPs)** — note both boxes mention "Security policy enforcement". This is OPA/Cedar-style externalized authorization.

------------------------------------------------------------------------

## 3. End-to-end use case — "PR Reviewer agent"

Following the arrows top to bottom:

1.  Developer pushes a PR → **Bitbucket** webhook → **Polaris API**.

2.  API authenticates the call (→ **Security** → **Google Identity** for service-account auth).

3.  API asks **Directory** "give me agent `pr-reviewer`".

4.  Directory looks up the definition in **Knowledge**.

5.  API hands execution to **Runtime**.

6.  Runtime queries **Knowledge** for the diff context, related tickets, and prior review memory.

7.  Runtime calls **Models** (Gemini on **GCP**) in a loop.

8.  Runtime, via **MCP**, fetches the linked Jira ticket and Confluence design doc.

9.  Runtime writes the review back through **MCP** as a Bitbucket comment, sends a digest via **Notification → Slack**, and emits traces to **Observability**.

------------------------------------------------------------------------

## 4. Architectural observations worth flagging

- **Single ingress, two protocols (HTTP API + MCP).** Good — but make sure both PEPs share one policy bundle, otherwise you'll get drift.

- **Directory as a stateless lookup layer.** Strong choice for evolvability; just budget for a cache (Caffeine/Redis) — every Runtime call hits it.

- **Knowledge as both RAG store and memory bank.** Consider segregating logically (e.g., `kb.docs`, `kb.code`, `kb.agent-memory`) — different retention, different access policies.

- **Models behind its own container.** Lets you swap GCP for self-hosted vLLM later without touching Runtime — clean hexagonal boundary.

- **Observability is downstream of everything.** Make sure **Security** also emits to it (the diagram doesn't show that arrow explicitly), otherwise you can't audit policy decisions.

%% ai-graph-start %%

**Related notes:**
- [[Polaris MCP tool catalog and usage pattern]]
- [[AI-Powered Development Environment Architecture]]
- [[Polaris 0.2.0 serves agentsskillsrules over an MCP tunnel]]
- [[Vinnstack Polaris integration is three passive touchpoints]]
- [[Polaris MCP is search-only 5 tools, no list-all, driven over HTTP JSON-RPC not the CLI]]

%% ai-graph-end %%