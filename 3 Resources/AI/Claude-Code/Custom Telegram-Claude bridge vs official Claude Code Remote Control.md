---
ai_hash: 8777fed35c7510d8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-18
entities:
- Custom Telegram-Claude bridge
- Official Claude Code Remote Control
- Telegram
- Claude
- notify hook
- long-poll daemon
- headless `claude --resume -p`
- output
- API-key authentication
- claude.ai OAuth
- Claude subscription tiers
- API keys
- Organizational policy block
- Team/Enterprise admin toggle
- claude process
- Terminal
- Sessions
- Network outage timeout
- Ultraplan
- claude.ai/code surface
- Projects
- Chat
- Server mode
- Claude Code v2.1.51+
- Permission approvals
- Inline chat buttons
- Claude mobile app
- Remote permission approval via a blocking PreToolUse hook
- Headless turns
- Live interactive terminal session
- Tmux/PTY-injection bots
- Injection
- Unix
- Windows
- Headless resume
- ccgram
- JessyTsui Claude-Code-Remote
- Official documentation
- code.claude.com/docs/en/remote-control
- Community bots
- github.com/jsayubi/ccgram
- github.com/JessyTsui/Claude-Code-Remote
- Windows claude subprocess is a process tree — taskkill T to reap it
source: session 2026-06-18
status: seedling
tags:
- claude-code
- telegram
- remote-control
- reference
title: Custom Telegram-Claude bridge vs official Claude Code Remote Control
type: concept
---

# Custom Telegram-Claude bridge vs official Claude Code Remote Control

A homegrown Telegram→Claude bridge (a notify hook for Stop/Notification + a long-poll daemon that runs headless `claude --resume -p` and streams output back) can do several things the **official `claude remote-control`** cannot:

- **API-key auth works.** Official RC requires claude.ai OAuth on Pro/Max/Team/Enterprise; API keys are rejected.
- **Survives an org policy block.** Official RC can be disabled by a Team/Enterprise admin toggle; the bridge is independent.
- **Survives closing the terminal.** Official RC dies when the `claude` process exits; the bridge resumes sessions on demand, so there is nothing to keep alive.
- **No ~10-minute network-outage timeout** (official RC times out).
- **Coexists with ultraplan** (official RC disconnects when ultraplan starts — both want the claude.ai/code surface).
- **Many sessions/projects from one chat** (official RC = one remote session per interactive process outside server mode).
- **No version floor** (official RC needs Claude Code v2.1.51+).
- **Permission approvals as inline chat buttons** without the Claude mobile app — see [[Remote permission approval via a blocking PreToolUse hook]].

**Tradeoff / what it gives up:** the bridge runs *separate headless turns* of a session (`claude --resume -p`). It does NOT steer the live interactive terminal session you are watching. Tmux/PTY-injection bots (ccgram, JessyTsui Claude-Code-Remote) and official RC *do* drive the live session — but injection is Unix-oriented and brittle on Windows, so headless-resume is the better fit there.

Reference points: official docs `code.claude.com/docs/en/remote-control`; community bots `github.com/jsayubi/ccgram`, `github.com/JessyTsui/Claude-Code-Remote`.

## Related

- [[Remote permission approval via a blocking PreToolUse hook]]
- [[Windows claude subprocess is a process tree — taskkill T to reap it]]

%% ai-graph-start %%

**Related notes:**
- [[Claude Code official remote-control surfaces (web, Remote Control, Dispatch)]]
- [[Remote permission approval via a blocking PreToolUse hook]]
- [[Claude Code hooks fire for any spawned claude process, not just interactive sessions]]
- [[Windows claude subprocess is a process tree — taskkill T to reap it]]
- [[vinnstack spawns the local claude CLI for subscription-authenticated automation]]

**Relations:**
- Custom Telegram-Claude bridge — *is a* — notify hook
- Custom Telegram-Claude bridge — *is a* — long-poll daemon
- Custom Telegram-Claude bridge — *runs* — headless `claude --resume -p`
- Custom Telegram-Claude bridge — *streams* — output
- Custom Telegram-Claude bridge — *supports* — API-key authentication
- Official Claude Code Remote Control — *requires* — claude.ai OAuth
- claude.ai OAuth — *is for* — Claude subscription tiers
- Official Claude Code Remote Control — *rejects* — API keys
- Custom Telegram-Claude bridge — *survives* — Organizational policy block
- Official Claude Code Remote Control — *can be disabled by* — Team/Enterprise admin toggle
- Custom Telegram-Claude bridge — *is independent of* — Team/Enterprise admin toggle
- Official Claude Code Remote Control — *dies when* — claude process exits
- Custom Telegram-Claude bridge — *resumes* — Sessions
- Official Claude Code Remote Control — *times out after* — Network outage timeout
- Custom Telegram-Claude bridge — *coexists with* — Ultraplan
- Official Claude Code Remote Control — *disconnects when* — Ultraplan starts
- Official Claude Code Remote Control — *wants* — claude.ai/code surface
- Ultraplan — *wants* — claude.ai/code surface
- Custom Telegram-Claude bridge — *supports many* — Sessions
- Custom Telegram-Claude bridge — *supports many* — Projects
- Custom Telegram-Claude bridge — *from one* — Chat
- Official Claude Code Remote Control — *supports one remote session per* — interactive process outside server mode
- Custom Telegram-Claude bridge — *has* — No version floor
- Official Claude Code Remote Control — *needs* — Claude Code v2.1.51+
- Custom Telegram-Claude bridge — *provides* — Permission approvals as Inline chat buttons
- Custom Telegram-Claude bridge — *provides* — Permission approvals without Claude mobile app
- Remote permission approval via a blocking PreToolUse hook — *is related to* — Permission approvals
- Custom Telegram-Claude bridge — *runs* — separate Headless turns
- Custom Telegram-Claude bridge — *does not steer* — Live interactive terminal session
- Tmux/PTY-injection bots — *drive* — Live interactive terminal session
- Official Claude Code Remote Control — *drives* — Live interactive terminal session
- Injection — *is* — Unix-oriented
- Injection — *is* — brittle on Windows
- Headless resume — *is a better fit for* — Windows
- ccgram — *is a type of* — Tmux/PTY-injection bots
- JessyTsui Claude-Code-Remote — *is a type of* — Tmux/PTY-injection bots
- Official documentation — *for* — Official Claude Code Remote Control
- Official documentation — *is located at* — code.claude.com/docs/en/remote-control
- Community bots — *include* — ccgram
- Community bots — *include* — JessyTsui Claude-Code-Remote
- github.com/jsayubi/ccgram — *is a reference for* — ccgram
- github.com/JessyTsui/Claude-Code-Remote — *is a reference for* — JessyTsui Claude-Code-Remote
- Custom Telegram-Claude bridge — *is compared to* — Official Claude Code Remote Control
- Custom Telegram-Claude bridge — *is related to* — Remote permission approval via a blocking PreToolUse hook
- Custom Telegram-Claude bridge — *is related to* — Windows claude subprocess is a process tree — taskkill T to reap it

%% ai-graph-end %%