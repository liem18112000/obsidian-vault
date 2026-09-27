---
title: "implement_plan on claude-proxy exceeds the 300s MCP idle timeout — raise per-server timeout"
created: 2026-09-23
type: lesson
status: seedling
source: "session 2026-09-23"
tags: [mcp, timeout, claude-proxy, implement, testing-agent, gotcha]
---

# implement_plan on claude-proxy exceeds the 300s MCP idle timeout — raise per-server timeout

GOTCHA (local testing-agent pipeline): implement_plan (the P4 assured scenario-generation loop) fails on the LOCAL stack with "MCP server sent no response or progress for 300s; aborting". Root cause: the LLM is the local claude-proxy (each `claude -p` = a Claude Code cold start, ~15-30s), and the assured loop makes many calls per chunk (generate N scenarios + judge + reflect + regenerate), so a single chunk runs past Claude Codes DEFAULT 300s MCP idle timeout. gather/refine/define_plan survive (fewer/faster calls); implement does not. FIX: give the MCP server a per-server timeout — in ~/.claude.json under `projects/<cwd>/mcpServers/testing-agent-local` add `"timeout": 1800000` (30min) — OR set env `CLAUDE_CODE_MCP_TOOL_IDLE_TIMEOUT` globally (0 disables). Config loads at init, so `/mcp` reconnect (or new session) is required to apply. Server-side generation is GCS/state-checkpointed, so a retry resumes from the last chunk. Deeper fix = a faster LLM for the generation-heavy step (Ollama/real API) since subscription cold-start latency dominates. See [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]] and [[gather(repo=) build hits MCP idle timeout]].

## Related

- [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]]
