---
ai_hash: 8edf5129233bdf15
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-14
entities:
- Vinnstack
- Polaris
- statusOnly provider card
- lib/account/authProviders.ts
- CLI
- ~/.polaris/state.json
- tunnel
- Google ADC
- login
- logout
- mcp__polaris
- CHAT_ALLOWED_TOOLS
- lib/ultracode/ultracodeRunner.ts
- Claude
- Polaris tools
- AGENTS.md pointer block
- static '@polaris' guidance
- Agent Kernel
- lib/ultracode/agentKernel.ts
- MCP
- company-wide skills system
- doc/polaris-mcp-integration-plan.md
- phased integration plan
- Phases 0-3
- Phase 4
- Polaris orchestration
- Polaris 0.2.0
- agents
- skills
- rules
- MCP tunnel
source: session 2026-07-14
status: seedling
tags:
- vinnstack
- polaris
- mcp
- integration
title: Vinnstack Polaris integration is three passive touchpoints
type: observation
---

# Vinnstack Polaris integration is three passive touchpoints

As of 2026-07-14, Vinnstack wires Polaris in only **three passive places** — it never surfaces, controls, or routes through Polaris:
1. **statusOnly provider card** (`lib/account/authProviders.ts`, `polarisProvider`) — detects the CLI, reads `~/.polaris/state.json`, TCP-probes the tunnel on :3003, checks Google ADC. `login`/`logout` are no-ops that point back to the CLI.
2. **`mcp__polaris` in `CHAT_ALLOWED_TOOLS`** (`lib/ultracode/ultracodeRunner.ts`) — a spawned `claude` *may* call Polaris tools, but only if the tunnel was brought up separately in a terminal; silent when down.
3. **`AGENTS.md` pointer block** — static '@polaris' guidance the runner doesn't act on.

**Overlap gotcha:** Vinnstack's Bitbucket **Agent Kernel** read-only mirror (`lib/ultracode/agentKernel.ts`) is the conceptual *predecessor* of what Polaris now serves live over MCP. That creates a **replace-vs-coexist decision** — two parallel 'company-wide skills' systems. Recommendation on file: coexist short-term (label clearly), supersede once Polaris coverage is confirmed.

Full phased integration plan: `doc/polaris-mcp-integration-plan.md` (Phases 0-3 = MVP visibility+control; Phase 4 = route runs through Polaris orchestration).

## Related

- [[Polaris 0.2.0 serves agentsskillsrules over an MCP tunnel]]

%% ai-graph-start %%

**Related notes:**
- [[Making Polaris MCP tools reachable by Vinnstack's spawned agent (discovery + allowlist)]]
- [[Vinnstack auth providers two patterns and the rule for adding one]]
- [[Wiring an external MCP-serving CLI into a Next.js app status-on-provider, actions-on-dedicated-route]]
- [[Polaris 0.2.0 serves agentsskillsrules over an MCP tunnel]]
- [[Polaris 3003 MCP server is persistent — TCP probe not equal to polaris tunnel state]]

**Relations:**
- Vinnstack — *has integration with* — Polaris
- Vinnstack — *wires* — Polaris
- Polaris — *is wired in* — statusOnly provider card
- Polaris — *is wired in* — mcp__polaris
- Polaris — *is wired in* — AGENTS.md pointer block
- statusOnly provider card — *is defined in* — lib/account/authProviders.ts
- statusOnly provider card — *detects* — CLI
- statusOnly provider card — *reads* — ~/.polaris/state.json
- statusOnly provider card — *TCP-probes* — tunnel
- statusOnly provider card — *checks* — Google ADC
- login — *is a* — no-op
- logout — *is a* — no-op
- no-op — *points to* — CLI
- mcp__polaris — *is in* — CHAT_ALLOWED_TOOLS
- CHAT_ALLOWED_TOOLS — *is defined in* — lib/ultracode/ultracodeRunner.ts
- Claude — *may call* — Polaris tools
- AGENTS.md pointer block — *provides* — static '@polaris' guidance
- Agent Kernel — *is part of* — Vinnstack
- Agent Kernel — *is a* — read-only mirror
- Agent Kernel — *is defined in* — lib/ultracode/agentKernel.ts
- Agent Kernel — *is predecessor of* — Polaris
- Polaris — *serves live over* — MCP
- Agent Kernel — *is a* — company-wide skills system
- Polaris — *is a* — company-wide skills system
- doc/polaris-mcp-integration-plan.md — *contains* — phased integration plan
- phased integration plan — *describes* — Phases 0-3
- phased integration plan — *describes* — Phase 4
- Phases 0-3 — *achieve* — MVP visibility+control
- Phase 4 — *involves* — Polaris orchestration
- Polaris 0.2.0 — *serves* — agents
- Polaris 0.2.0 — *serves* — skills
- Polaris 0.2.0 — *serves* — rules
- Polaris 0.2.0 — *serves over* — MCP tunnel

%% ai-graph-end %%