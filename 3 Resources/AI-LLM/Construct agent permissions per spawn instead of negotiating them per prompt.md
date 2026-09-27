---
ai_hash: 6bb7fd42cb8b027f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Agent Permissions
- Spawn Time
- Prompt
- Interactive Permission Model
- Agent
- Human
- Tool Call
- Autonomous Work
- Approval
- Session
- Policy
- Operator
- Allow/Deny List
- dontAsk Mode
- Deny Wins
- Bitbucket
- Read-Only
- MCP Write Verbs
- Git Push
- Raw REST Writes
- Unattended Operation
- Precedence
- Allow Rule
- Deny Rule
- Permissions
- Route
- Capability
- MCP Tool
- Shell Command
- HTTP API
- Language SDK
- Source Repository
- Constructed Policy
- Policy Review
- Code
- Versioning
- Diff Changes
- Testing
- Denied Operation
- Typo
- Credential Model
- CLI OAuth
- Business Licence
- API Key
- Environment
- Cloud Keys
- Runtime Environments Rule
- Platform
- Env Var
- Script
- Wrap the agent CLI rather than reimplementing the agent loop
- Vinnstack vs. Claude Code (native)
- Confluence
- Process
- Dangerous Operations
- Babysitting
- Blanket-Approving
- Maintained Source
source: 'Confluence: Vinnstack vs Claude Code native (TK)'
status: seedling
tags:
- agent-safety
- permissions
- autonomy
- security
- claude-code
- confluence-distilled
title: Construct agent permissions per spawn instead of negotiating them per prompt
type: lesson
---

# Construct agent permissions per spawn instead of negotiating them per prompt

An interactive permission model — the agent asks, a human approves each tool call — does not survive contact with autonomous work. Nobody watches prompts for an hour, so approvals get broadened "just for this session", and the effective policy becomes whatever the most impatient operator clicked.

The alternative is to **construct the policy at spawn time**:

> Every agent is spawned with a centrally maintained allow/deny list (`dontAsk` mode, **deny wins**). The flagship example: **Bitbucket is read-only by construction** — MCP write verbs denied, `git push` denied, raw REST writes denied. The agent can run unattended, and the policy **cannot drift per session**.

**Three properties that make this work, and each matters on its own:**

1. **Deny wins.** Precedence is decided once, structurally. An allow rule can never accidentally re-open something a deny rule closed, so adding permissions is safe.
2. **Every route to the capability is closed, not just the obvious one.** Read-only Bitbucket means denying the MCP write verbs **and** `git push` **and** raw REST calls. Closing one door and leaving two open is the usual failure — the agent is resourceful and will find the others.
3. **The policy is central and per-spawn.** It is applied when the process starts, from one maintained source, so it is identical for every operator and cannot be renegotiated mid-run.

**The payoff is that unattended operation becomes safe rather than brave.** With interactive approval, "run this overnight" means either babysitting or blanket-approving. With constructed permissions, the dangerous operations are *impossible* for that process, so nobody has to be watching.

> [!tip] Enumerate capabilities, then enumerate routes
> Write the policy in terms of *what must not happen* ("no writes to the source repository"), then list every mechanism that could achieve it — the MCP tool, the shell command, the HTTP API, a language SDK. The gap between the capability and the route list is where these policies leak.

> [!warning] Constructed policy is only as good as its review
> Because it is central and invisible at runtime, nobody is prompted to notice it is wrong. It needs the same review as code: version it, diff changes, and test that a denied operation actually fails — an allow-list with a typo denies nothing and no prompt will tell you.

> [!note] One credential model is part of the same picture
> Running everything through CLI OAuth under a business licence means **no API key in anyone's environment** — which is what lets a "no cloud keys in runtime environments" rule hold platform-wide. A policy that blocks tool calls is undermined by a key sitting in an env var that any script can use.

Related: [[Wrap the agent CLI rather than reimplementing the agent loop]].

Source: [[Vinnstack vs. Claude Code (native)]] (TK, Confluence).

## Related

- [[Wrap the agent CLI rather than reimplementing the agent loop]]

%% ai-graph-start %%

**Related notes:**
- [[Vinnstack · Prompt Governance &amp; Code Grounding]]
- [[Wrap the agent CLI rather than reimplementing the agent loop]]
- [[Vinnstack vs. Claude Code (native)]]
- [[Hard-exclude an AI agent from a resource by shrinking its file grant, not by prompting]]
- [[Vinnstack withholds gitgh from the model in BDD step implementation]]

**Relations:**
- Constructed Policy — *is alternative to* — Interactive Permission Model
- Interactive Permission Model — *involves* — Agent asks Human
- Human — *approves* — Tool Call
- Interactive Permission Model — *does not survive* — Autonomous Work
- Approval — *broadened per* — Session
- Broadened Approval — *leads to* — Effective Policy
- Effective Policy — *determined by* — Impatient Operator
- Policy — *constructed at* — Spawn Time
- Agent — *spawned with* — Allow/Deny List
- Allow/Deny List — *is* — centrally maintained
- Allow/Deny List — *is also called* — dontAsk Mode
- dontAsk Mode — *has property* — Deny Wins
- Bitbucket — *is* — Read-Only by construction
- Read-Only Bitbucket — *denies* — MCP Write Verbs
- Read-Only Bitbucket — *denies* — Git Push
- Read-Only Bitbucket — *denies* — Raw REST Writes
- Agent — *can run* — Unattended Operation
- Policy — *cannot drift per* — Session
- Deny Wins — *decides* — Precedence
- Allow Rule — *cannot re-open* — Deny Rule
- Adding Permissions — *is* — safe
- Every Route — *to* — Capability is closed
- Agent — *is* — resourceful
- Policy — *is* — central
- Policy — *is* — per-spawn
- Policy — *applied when* — Process starts
- Policy — *comes from* — Maintained Source
- Policy — *is identical for every* — Operator
- Policy — *cannot be renegotiated* — mid-run
- Constructed Policy — *makes* — Unattended Operation safe
- Interactive Permission Model — *requires* — Babysitting
- Interactive Permission Model — *requires* — Blanket-Approving
- Constructed Policy — *makes* — Dangerous Operations impossible for Process
- Policy — *written in terms of* — Capabilities
- Capabilities — *achieved by* — Routes
- Routes — *include* — MCP Tool
- Routes — *include* — Shell Command
- Routes — *include* — HTTP API
- Routes — *include* — Language SDK
- Policy — *prevents* — writes to Source Repository
- Gap between Capability and Route List — *causes* — Policy leaks
- Constructed Policy — *needs* — Policy Review
- Constructed Policy — *is* — invisible at runtime
- Policy Review — *is like* — Code Review
- Policy Review — *involves* — Versioning
- Policy Review — *involves* — Diff Changes
- Policy Review — *involves* — Testing Denied Operation fails
- Typo in Allow-List — *causes* — denies nothing
- Credential Model — *is part of* — Policy
- CLI OAuth — *under Business Licence means* — no API Key in Environment
- No API Key in Environment — *enables* — Runtime Environments Rule
- Runtime Environments Rule — *holds* — Platform-wide
- API Key in Env Var — *undermines* — Policy that blocks Tool Call
- Script — *can use* — API Key in Env Var
- Constructed Policy — *is related to* — Wrap the agent CLI rather than reimplementing the agent loop
- Vinnstack vs. Claude Code (native) — *is a* — Source
- Vinnstack vs. Claude Code (native) — *is on* — Confluence

%% ai-graph-end %%