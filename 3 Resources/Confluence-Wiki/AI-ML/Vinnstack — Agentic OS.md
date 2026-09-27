---
title: "Vinnstack — Agentic OS"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49514119198/Vinnstack+Agentic+OS
space: "TK"
topic: ai_ml
relevance: 0.731
depth: 2.51
updated: 2026-06-25
attachments: 0
tags:
  - confluence
  - ai-ml
  - space/tk
---

# Vinnstack — Agentic OS

> [!info] Imported from Confluence
> Space **TK** · updated 2026-06-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49514119198/Vinnstack+Agentic+OS)
> Relevance 0.731 · topic `ai_ml`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

↑ Part of the [AI-First Framework — Mission Team: Receive](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49459101783/AI-First+Framework+Mission+Team+Receive). **Vinnstack is the cockpit for the framework's Refinement half** — it runs the interrogations, the PRD, and Story creation (on Claude). The autonomous **build** half (generate tests → write code → open PR) is the separate [multi-agent build framework](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259708519/Multi-Agentic+Architecture+Apply+in+AI+Driven+Testing) (Gemini / Vertex). Deep-dives: [Architecture & Runtime](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49516544006/Vinnstack+Architecture+amp+Runtime) · [Prompt Governance & Code Grounding](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49515102402/Vinnstack+Prompt+Governance+amp+Code+Grounding) · [Integrations & Authentication](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49516314638/Vinnstack+Integrations+amp+Authentication) · [Graphify Security](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49513365644/Graphify+Security+Audit+Caveats+Installation).

</div>

</div>

<div hasbody="true" macro-id="9d35fafa-cd32-4d33-b7d1-66d7711d6849" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**Vinnstack** is a locally-hosted **Agentic Operation System** — a mission-control surface that wraps the **Claude Code** CLI and turns it into a persistent, team-aware operating layer. It does not replace Claude Code; it gives it a home: a UI, durable memory, domain context (Jira, code graphs, the vault), and structured workflows that take a Jira Epic all the way to reviewed code. It runs entirely on each user's own machine.

</div>

</div>

## 1. What Vinnstack is

Vinnstack is a Next.js 14 application (Tailwind CSS + Framer Motion) that runs locally on **http://localhost:3001**. Every "real" action it performs is executed by spawning the actual `claude` CLI as a child process — so Vinnstack is a *control surface*, not a re-implementation of the model. The configured model is `claude-opus-4-8` (Opus 4.8), overridable via `ANTHROPIC_MODEL`.

It is styled in the ePost / Post brand (Post-yellow accent on ink): a persistent left sidebar (AI Agents + Workspaces) plus a dynamic main workspace. It's designed to be installed per machine and driven by each operator's own logins.

## 2. The core idea — how Vinnstack complements Claude Code

Claude Code is a stateless, terminal-first coding agent: powerful, but ephemeral and context-free between sessions. Vinnstack adds the four things a terminal alone cannot:

<div>

|  |  |
|----|----|
| Gap in raw Claude Code | What Vinnstack adds |
| **No memory between sessions** | Every chat, goal, journal entry and decision is auto-persisted to an Obsidian vault and re-hydrated on the next session. A curated long-term memory note is injected into each run. |
| **No domain context** | Vinnstack feeds Claude real context: code knowledge graphs for 18 repos (Graphify), the Obsidian vault, and a three-source skill library (generic + learned + company-wide). |
| **No visibility into multi-agent work** | The Ultracode view renders Claude's dynamic sub-agent swarms as a live constellation map with cost, verdicts and replayable history. |
| **No structured product workflow** | The Interrogation Room drives a governed Epic → Business/Technical interrogation → PRD → Story pipeline, with a human gate at each step — extended on the roadmap to an autonomous code → PR loop. |

</div>

<div hasbody="true" macro-id="d737b6f1-6b46-4b2e-9a7d-2698be6d423b" macro-name="tip">

<span class="aui-icon aui-icon-small aui-iconfont-approve confluence-information-macro-icon"> </span>

<div>

**In one line:** Claude Code is the engine; Vinnstack is the cockpit, the memory, and the flight plan.

</div>

</div>

## 3. The shell & navigation

A sticky 268px left sidebar holds the navigation; the main pane swaps between mutually-exclusive sections.

- **AI Agents** — the agent roster (currently one card: *Claude Code*, "Agentic coding · ops · research").

- **Workspaces** — Graphify, Obsidian Vault, Interrogation Room.

- **Process Flows** — an expandable product → user-journey tree.

- **Command Palette** (`⌘K` / `Ctrl-K`) — jump to any section or tab by typing.

- **Theme** — light/dark toggle, persisted to `localStorage`.

- **⚙ Settings** — choose the data folder; see which credentials are detected.

The Agent workspace itself is a four-tab surface: **Chat** · **Ultracode** · **Skills** · **Notebook**.

## 4. Features

### 4.1 Chat — the operator console

A conversational surface that streams directly from a live `claude` CLI child process via `/api/claude/chat`.

- Real streaming of Anthropic stream-json events, a **live total-token counter** while generating, and a per-turn **tokens + \$ cost** line once it finishes (from the run's authoritative usage).

- **Attachments** — paste an image from the clipboard, drag-and-drop, or use ＋ to attach files. They're saved locally and the agent reads them with its Read tool (images + PDFs). Any visual the agent outputs (image, diagram, HTML) is click-to-enlarge.

- History grouped by day and hydrated from the vault (up to 30 days); auto-sticks to the latest message.

- An **LLM Usage** panel: live tokens, session totals, per-turn cost, a daily budget, and rate-limit hits.

**Complements Claude Code by:** giving the CLI a durable, searchable conversation that survives restarts, auto-summarises itself into the vault on idle, and extracts reusable skills (and follow-ups) from the exchange.

### 4.2 Ultracode — watch the swarm work

A visualiser for Claude's dynamic multi-agent workflows, run at **xhigh** effort with real tokens.

- Mission launcher with presets (security audit, find dead code, build a showcase, stress-test a plan) plus free-text missions.

- A constellation map: the orchestrator (CLAUDE) at the centre, sub-agents on concentric rings, colour-coded by role and sized by token spend.

- A verdict trail and final answer in a resizable split pane; reply box continues the same session (`--resume`) with full context.

- Replayable run history with cost, duration and sub-agent count; a Stop button (kills the run, even an orphaned child); export to PDF.

**Complements Claude Code by:** turning opaque, headless sub-agent orchestration into an observable, auditable, resumable artifact.

### 4.3 Skills — procedural memory (three sources)

A browser for SKILL.md procedural memory, split into **three tabs**, each with a usage counter:

- **Vinnstack Skills** — the generic, ship-with-the-product pipeline (read-only): `interrogate-business`, `interrogate-technical`, `epic-to-prd`, `prd-to-story`, `query-code-graph`, `grounded-bug-report`, `capture-skill`, `write-acceptance-tests` (+ planned build-loop stubs). A new team gets the full pipeline out of the box.

- **Your Skills** — agent-learned, auto-extracted from chats, editable and deletable.

- **Agent Kernel** — company-wide, read-only, mirrored from Bitbucket (with an AGENTS.md global-guidance entry).

Each skill card shows a **↻ usage counter** — how many runs actually read that skill. **Single source of truth:** each capability is defined once in its SKILL.md; the app composes the procedure + a code-owned output contract, so there's no drifting duplicate.

**Complements Claude Code by:** giving the CLI a growing, shareable library of how-we-do-things-here — reusable, measurable institutional knowledge.

### 4.4 Notebook — goals & journal

A two-column productivity surface, fully vault-synced.

- **Goals** — add / check / delete with a progress counter.

- **Journal** — a brain-dump textarea with an *AI rewrite* button that restructures messy notes into clean Markdown, optionally asking clarifying questions first.

**Complements Claude Code by:** capturing operator intent and daily context where the agent can read it.

### 4.5 Follow-up — people to talk to

A tab that surfaces, from your chats, who you need to talk to and who you're waiting on — person, their function, the topic, and direction (you reach out / they reach out). Auto-extracted on idle and editable by hand.

### 4.6 Graphify — code knowledge graphs

Force-directed code knowledge graphs for 18 axonivy-prod repositories.

- Per-repo build pipeline (queued → downloading → staging → scanning → aggregating) + a **Refresh all** button. Builds are **token-free** — pure local tooling, no LLM.

- **No source code at rest:** a refresh downloads a gitless Bitbucket tarball, scans it, then **deletes the source** — only the graph is kept. (Full security model: [Graphify Security Audit](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49513365644/Graphify+Security+Audit+Caveats+Installation).)

- When the agent queries the graph and **can't confirm** something, it falls back to checking the real source rather than reporting it unverified.

### 4.7 Obsidian Vault — the memory graph

A knowledge graph of the entire Obsidian vault (all notes + wikilinks), with an inline note editor — the agent's long-term memory backend, navigable and editable.

### 4.8 Interrogation Room — Epic → PRD → Story

The flagship governed workflow (rounds R1/R2/R3), with an interactive SDLC overview. See the [framework hub](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49459101783/AI-First+Framework+Mission+Team+Receive) for the full pipeline.

1.  **Business Interrogation (R1)** — surfaces only genuine business-judgement questions (self-answering anything derivable), each with options, a recommendation, and dependency gating.

2.  **Technical Interrogation (R2)** — grounded in Graphify, surfaces real engineering decisions with Mermaid sequence/impact diagrams.

3.  **PRD generation (R3)** — synthesises a visual PRD (architecture flowchart, sequence diagram, decision tables, traceability) that doubles as an implementation + test plan.

4.  **Approve & create Stories** — on approval it posts the PRD as an Epic comment and creates the Stories in Jira (idempotent, every key verified, create-only).

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

**Story creation uses a vertical-slice methodology:** each story is a thin, end-to-end "tracer bullet" through every layer, independently demoable, classified **AFK** (autonomously buildable) or **HITL** (needs a design call) by a concrete checklist, ordered by dependency, and filed via the team's `write-story` conventions.

</div>

</div>

### 4.9 Process Flows — user-journey catalogue

A navigable catalogue of ~38 user journeys across 5 product groups (eLetter, eArchive, LUZ-Store, Letterbox, Jobs), grounded in the Confluence catalogue and code research.

## 5. Integrations

<div>

|  |  |  |
|----|----|----|
| System | Direction | How |
| Jira / Confluence | Read + write | Atlassian MCP + REST — Epic reads, comments, story creation, PRD publishing. |
| Bitbucket | Read (write planned) | Read for the Agent Kernel sync, Graphify tarballs, and on-demand source checks; branch push + PR creation arrive with the build loop. |
| Obsidian vault | Read + write | Local filesystem — chats, goals, journal, skills, interrogations, long-term memory. |
| Graphify | Local tool | Profile-A sandbox (code-only, no egress); scans then deletes source — only graphs persist. |
| Claude CLI | Child process | The execution engine for every real action. Foreground work uses Opus; background transforms use a cheaper model (override via `ANTHROPIC_BACKGROUND_MODEL`). |

</div>

## 6. Architecture & configuration

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

**Runtime & setup**

- Next.js 14 app on port **3001**; real work = spawned `claude` CLI children.

- Model: `claude-opus-4-8` (foreground); cheaper model for background transforms.

- **Onboarding** picks one **data folder**; Vinnstack creates a `Vinnstack/` subtree there.

- Vinnstack **stores no secrets** — credentials are read from the OS environment.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

**Authentication**

- **Jira + Confluence** — `ATLASSIAN_EMAIL` + `ATLASSIAN_API_TOKEN`.

- **Bitbucket** — `ATLASSIAN_BITBUCKET_USERNAME` + `ATLASSIAN_BITBUCKET_APP_PASSWORD`.

- **Anthropic** & **GCP** — your Claude Code login & `gcloud` login.

Full detail: [Integrations & Authentication](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49516314638/Vinnstack+Integrations+amp+Authentication).

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div hasbody="true" macro-id="b2a0d721-5628-460f-b83c-370b17049c5f" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**How prompts are governed** (graph-first, not a `CLAUDE.md`), and the full enforcement stack, are covered on [Prompt Governance & Code Grounding](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49515102402/Vinnstack+Prompt+Governance+amp+Code+Grounding). Runtime internals are on [Architecture & Runtime](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49516544006/Vinnstack+Architecture+amp+Runtime).

</div>

</div>

## 7. Roadmap — closing the loop

Today Vinnstack's pipeline stops at Story creation. The next phase carries an **AFK** story to a reviewed PR — **test-first** (acceptance tests RED → green), in an isolated worktree, DEV as the human gate. In the wider framework this autonomous build is realized by the [multi-agent build framework](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259708519/Multi-Agentic+Architecture+Apply+in+AI+Driven+Testing); wiring the two halves into one loop (bridged via Polaris) is the open work — see the [framework hub](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49459101783/AI-First+Framework+Mission+Team+Receive) and the [Audit & Hardening](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49536106498/Audit+amp+Hardening+AI-First+Framework) page.

<div hasbody="true" macro-id="4da332fa-d102-4c9f-a771-5189c8271a9b" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Vinnstack runs entirely locally; all agent work executes through the operator's own Claude Code installation, and no credentials are stored by the app.

</div>

</div>

</div>

</div>

</div>

</div>
