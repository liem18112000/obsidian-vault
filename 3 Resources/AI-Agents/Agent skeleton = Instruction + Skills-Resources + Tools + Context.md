---
ai_hash: 47605338f413fde2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities:
- Agent skeleton
- Instruction
- Skills-Resources
- Tools
- Context
- AI agent
- Stateless core
- System prompt
- Skills
- Resources
- Playbooks
- Procedures
- Scripts
- Data
- Assets
- External capabilities
- MCP
- Atlassian
- Gcloud CLI
- Memory
- Session state
- Agent core
- Outputs
- Report
- Q/A interrogation
- Underlying data/systems
- Luz
- View-controller
- Luz-docs
- Luz-vault
- JSON Store
- MongoDB
- Multi-level altitude reporting from a single-source agent answer
- Expanded anatomy (hub-and-spoke)
- Runtime
- Identity
- CodeBase
- Memory / History
- Confluence / JIRA
- Logs
- Instruction.md
- Skills.md
- Polaris
- Claude-Code skills
- Skill folder layout
- Skill
- skill.md
- script.py
- .psh
source: session 2026-08-17 agent-framework-skeleton diagram
status: seedling
tags:
- agents
- architecture
- mcp
- skills
title: Agent skeleton = Instruction + Skills-Resources + Tools + Context
type: model
---

# Agent skeleton = Instruction + Skills-Resources + Tools + Context

A minimal mental model for an AI agent: a **stateless** core plus four supplied building blocks. The agent itself holds no state between runs; everything it needs is injected each invocation.

- **Instruction (System prompt)** — WHO the agent is; kept stateless.
- **Skills** (`skills.md`) — HOW it works; playbooks / procedures. Skills reference →
- **Resources** — scripts, data, and assets the skills call on.
- **Tools** — external capabilities, typically via MCP (e.g. Atlassian, Gcloud CLI).
- **Context** — memory / session state passed in for this run.

So: **Agent = Instruction + Skills(→Resources) + Tools + Context.**

The four blocks **converge** into the agent core, which then produces outputs (e.g. a report or a Q/A interrogation). Underlying data/systems (in the Luz case: view-controller → Luz-docs ↔ Luz-vault → JSON Store → MongoDB) are surfaced to the agent through Tools + Context — they 'ground' the answer.

Example downstream use: [[Multi-level altitude reporting from a single-source agent answer]].

## Expanded anatomy (hub-and-spoke)

A fuller whiteboard version puts **Agent/s** at the hub with more spokes than the minimal four:

- **Runtime** — the execution environment the agent runs in.
- **Identity** — who/what the agent authenticates and acts as.
- **Context** decomposes into concrete sources: **CodeBase**, **Memory / History**, **Confluence / JIRA**, **Logs**.
- **Instruction.md** → explicitly **Stateless**.
- **Skills.md** → **Resources (scripts…)**.
- **Tools** → **MCP** → *Polaris / Atlassian*, plus **gcloud CLI**.

**Skill folder layout:** a skill is a folder containing `skill.md` (the HOW) + `script.py`/`.psh` (the runnable resource). This matches how Polaris/Claude-Code skills are packaged.

## Related

- [[Multi-level altitude reporting from a single-source agent answer]]

- [[Multi-level altitude reporting from a single-source agent answer]]

%% ai-graph-start %%

**Related notes:**
- [[vinnstack SKILL.md convention]]
- [[Testing Agent workflow step to AI Skill mapping]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[Multi-level altitude reporting from a single-source agent answer]]
- [[Claude Code Skill anatomy]]

**Relations:**
- Agent skeleton — *IS_A* — AI agent
- Agent skeleton — *HAS_COMPONENT* — Instruction
- Agent skeleton — *HAS_COMPONENT* — Skills-Resources
- Agent skeleton — *HAS_COMPONENT* — Tools
- Agent skeleton — *HAS_COMPONENT* — Context
- AI agent — *HAS_PROPERTY* — stateless core
- AI agent — *INJECTS* — Instruction
- AI agent — *INJECTS* — Skills
- AI agent — *INJECTS* — Resources
- AI agent — *INJECTS* — Tools
- AI agent — *INJECTS* — Context
- Instruction — *IS_A* — System prompt
- Instruction — *DEFINES* — WHO the agent is
- Instruction — *IS* — stateless
- Skills — *REPRESENTED_BY* — skills.md
- Skills — *DEFINES* — HOW it works
- Skills — *INCLUDES* — Playbooks
- Skills — *INCLUDES* — Procedures
- Skills — *REFERENCES* — Resources
- Resources — *INCLUDE* — Scripts
- Resources — *INCLUDE* — Data
- Resources — *INCLUDE* — Assets
- Skills — *CALLS_ON* — Resources
- Tools — *PROVIDE* — External capabilities
- Tools — *UTILIZES* — MCP
- MCP — *EXAMPLE* — Atlassian
- MCP — *EXAMPLE* — Gcloud CLI
- Context — *IS_A* — Memory
- Context — *IS_A* — Session state
- Agent — *EQUALS* — Instruction + Skills(→Resources) + Tools + Context
- Instruction — *CONVERGES_INTO* — Agent core
- Skills — *CONVERGES_INTO* — Agent core
- Resources — *CONVERGES_INTO* — Agent core
- Tools — *CONVERGES_INTO* — Agent core
- Context — *CONVERGES_INTO* — Agent core
- Agent core — *PRODUCES* — Outputs
- Outputs — *EXAMPLE* — Report
- Outputs — *EXAMPLE* — Q/A interrogation
- Underlying data/systems — *SURFACED_VIA* — Tools
- Underlying data/systems — *SURFACED_VIA* — Context
- Underlying data/systems — *GROUNDS* — answer
- Luz — *HAS_COMPONENT* — View-controller
- View-controller — *CONNECTS_TO* — Luz-docs
- Luz-docs — *CONNECTS_TO* — Luz-vault
- Luz-vault — *CONNECTS_TO* — JSON Store
- JSON Store — *CONNECTS_TO* — MongoDB
- Multi-level altitude reporting from a single-source agent answer — *IS_EXAMPLE_OF* — downstream use
- Expanded anatomy (hub-and-spoke) — *HAS_HUB* — Agent/s
- Expanded anatomy (hub-and-spoke) — *HAS_SPOKE* — Runtime
- Expanded anatomy (hub-and-spoke) — *HAS_SPOKE* — Identity
- Expanded anatomy (hub-and-spoke) — *HAS_SPOKE* — Context
- Expanded anatomy (hub-and-spoke) — *HAS_SPOKE* — Instruction.md
- Expanded anatomy (hub-and-spoke) — *HAS_SPOKE* — Skills.md
- Expanded anatomy (hub-and-spoke) — *HAS_SPOKE* — Tools
- Context — *DECOMPOSES_INTO* — CodeBase
- Context — *DECOMPOSES_INTO* — Memory / History
- Context — *DECOMPOSES_INTO* — Confluence / JIRA
- Context — *DECOMPOSES_INTO* — Logs
- Instruction.md — *IS* — Stateless
- Skills.md — *REFERENCES* — Resources
- Tools — *UTILIZES* — MCP
- MCP — *INCLUDES* — Polaris
- MCP — *INCLUDES* — Atlassian
- Tools — *INCLUDES* — gcloud CLI
- Skill folder layout — *DESCRIBES* — Skill packaging
- Skill — *IS_A* — folder
- Skill — *CONTAINS* — skill.md
- Skill — *CONTAINS* — script.py
- Skill — *CONTAINS* — .psh
- skill.md — *DEFINES* — the HOW
- script.py — *IS_A* — runnable resource
- .psh — *IS_A* — runnable resource
- Skill folder layout — *MATCHES* — Polaris
- Skill folder layout — *MATCHES* — Claude-Code skills
- Multi-level altitude reporting from a single-source agent answer — *IS_RELATED_TO* — Agent

%% ai-graph-end %%