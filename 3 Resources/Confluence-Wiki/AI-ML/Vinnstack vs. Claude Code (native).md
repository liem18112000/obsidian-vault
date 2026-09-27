---
title: "Vinnstack vs. Claude Code (native)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49497735170/Vinnstack+vs.+Claude+Code+native
space: "TK"
topic: ai_ml
relevance: 0.731
depth: 2.27
updated: 2026-06-11
attachments: 0
tags:
  - confluence
  - ai-ml
  - space/tk
---

# Vinnstack vs. Claude Code (native)

> [!info] Imported from Confluence
> Space **TK** · updated 2026-06-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49497735170/Vinnstack+vs.+Claude+Code+native)
> Relevance 0.731 · topic `ai_ml`

<div hasbody="true" macro-id="2df4bbe0-3f95-47c7-9ed5-dc828cd8d5b9" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Vinnstack does not replace Claude Code — it **runs** Claude Code. Every agent turn is a spawn of the official `claude` CLI. The comparison below is therefore "Claude Code wrapped by Vinnstack" vs. "Claude Code used natively in a terminal", and the question it answers is: *what does the wrapper add, and when is the bare terminal still the better tool?*

</div>

</div>

# Advantages over native Claude Code

## 1. Memory that survives — and is inspectable

Native Claude Code remembers within a session and via per-project files the user maintains by hand. Vinnstack makes memory a system property: a rolling resumed session, automatic priming of new sessions from vault history, and a curated `Memory.md` folded into the system prompt on *every* request. Durable facts are never lost to a closed terminal, and because all of it is plain Markdown in the Obsidian vault, you can read, correct, and graph exactly what the agent knows.

## 2. Knowledge capture is automatic, not a discipline

In a terminal, session insights die with the scrollback unless someone writes them down. Vinnstack writes them down structurally: 1:1 transcripts per day, idle-triggered synthesized notes (decisions, action items, open questions) with backlinks into the existing vault, and stub notes for new topics. A working session leaves behind an organized, linked knowledge base as a side effect.

## 3. Skills accumulate by themselves

Native skills/commands must be authored deliberately. Vinnstack adds a server-driven extraction pass that judges each settled exchange for a genuinely reusable procedure and writes a versioned `SKILL.md` when it finds one. The agent is also instructed to consult the skill library before tasks — so the system gets measurably better at recurring work without anyone maintaining it.

## 4. Org-wide guidance built in (Agent Kernel)

Company-wide skills and engineering guidance from the `axonivy-prod/agent-kernel` repo are synced read-only into every session's context. Natively, each developer would have to clone, update, and wire this in themselves; in Vinnstack it is one Sync button and authoritative by default.

## 5. Guardrails are constructed, not negotiated per prompt

Native Claude Code's safety model is interactive: the user approves tool calls, or broadens permissions ad hoc. Vinnstack spawns every agent with a centrally maintained allow/deny list (`dontAsk` mode, deny wins). The flagship example: **Bitbucket is read-only by construction** — MCP write verbs denied, `git push` denied, raw REST writes denied. The agent can run unattended without anyone watching permission prompts, and the policy cannot drift per session.

## 6. Grounded code intelligence across 18 repositories

Natively, the agent answers cross-repo structure questions by grepping whatever is checked out. Vinnstack maintains locally built Graphify knowledge graphs for all 18 axonivy-prod repositories (under the IT-Security-approved zero-egress wrapper — see parent page) and the chat agent queries them directly. Structure questions get answered from a graph, not from guesswork.

## 7. Product surfaces non-terminal users can use

Kepler sprint board with live Jira refresh, the Process Flow explorer (38 user journeys, interactive built flows with business and technical views), the repo graph canvas, the skills browser — these are shareable screens, not terminal output. Vinnstack turns agent tooling into something a PO or stakeholder can stand in front of.

## 8. Observability and replay

Every Ultracode workflow run is persisted and can be replayed, inspected, or stopped from the UI; chat generation survives navigating between tabs. Natively, a closed terminal is a lost run.

## 9. One credential model

Everything runs through CLI OAuth under the business license — no `ANTHROPIC_API_KEY` in anyone's environment, which is also what makes the IT-Security caveat "no cloud keys in runtime environments" hold across the whole platform.

# Comparison at a glance

<div>

|  |  |  |
|----|----|----|
| Capability | Claude Code (native) | Vinnstack |
| Cross-session memory | Per-project files, manually maintained | Three-layer automatic (resume + priming + curated Memory.md), inspectable in Obsidian |
| Session knowledge capture | Manual notes | Auto-summaries with backlinks, topic stubs, raw transcripts |
| Skill creation | Hand-authored | Auto-extracted from real sessions + hand-authored |
| Org-wide guidance | Per-developer setup | Agent Kernel synced read-only, on by default |
| Permission policy | Interactive, per session/user | Central allow/deny per spawn; Bitbucket read-only enforced |
| Code intelligence | grep/read of checked-out code | Queryable graphs of 18 repos (zero-egress build) |
| UI | Terminal / IDE | Web command deck: chat, swarm view, sprint board, process flows, graphs |
| Run persistence | Session transcript on disk, terminal-bound UX | Persisted, replayable runs; navigation-safe streaming |
| Credentials | OAuth or API key per setup | OAuth only, business license |

</div>

# When native Claude Code is still the better tool

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

An honest boundary keeps the product credible:

</div>

</div>

- **Hands-on coding in a repository.** Editing code with interactive permission prompts, plan mode, IDE integration, and git write access is exactly what the native CLI is built for. Vinnstack's agent deliberately cannot push.

- **Native slash-command/skill ergonomics.** Vault skills are read-based by design (so they are browsable and graphable); native `/command` discovery only exists in the terminal.

- **Anything multi-user or remote.** Vinnstack is currently a single-user, local-workstation product; there is no auth layer and no deployment story yet.

- **Version drift.** Vinnstack inherits whatever the installed CLI does; new CLI features arrive in the terminal first and in Vinnstack's permission/flag wiring second.

# The one-sentence pitch

Claude Code is the engine; Vinnstack is the vehicle: persistent memory, automatic knowledge and skill capture, enforced guardrails, org-wide context, and a command deck — so agent work compounds instead of evaporating when the terminal closes.
