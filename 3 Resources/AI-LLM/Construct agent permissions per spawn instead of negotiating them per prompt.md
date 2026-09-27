---
title: "Construct agent permissions per spawn instead of negotiating them per prompt"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Vinnstack vs Claude Code native (TK)"
tags: [agent-safety, permissions, autonomy, security, claude-code, confluence-distilled]
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
