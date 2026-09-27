---
ai_hash: 6bdf590759fce31c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23
status: seedling
tags:
- claude-code
- docker
- npm
- gotcha
- install
title: claude-code npm install breaks in slim Docker — use the official install.sh
type: lesson
---

# claude-code npm install breaks in slim Docker — use the official install.sh

GOTCHA: `npm install -g @anthropic-ai/claude-code` in a slim Linux container (node:20-slim) produces a BROKEN CLI: the `claude` bin is only a ~500-byte error stub ("claude native binary not installed... postinstall did not run or the platform-native optional dependency was not installed"), npm leaves a temp symlink `.claude-XXXX -> bin/claude.exe`, and the platform binary (`@anthropic-ai/claude-code-linux-x64/claude`) **Bus-errors** (corrupt/truncated, esp. over a flaky network). So `docker exec ... claude` fails with "executable file not found in $PATH".

FIX: use the OFFICIAL installer — `curl -fsSL https://claude.ai/install.sh | bash` — which drops a working binary at `~/.local/bin/claude` (v2.1.280 here) and even cleans up the broken npm copy. In a Dockerfile: apt-get ca-certificates + curl, run install.sh, then `ln -sf /root/.local/bin/claude /usr/local/bin/claude` so it is on PATH for `docker exec` AND the server process. Used for the test-agent-v2 claude-proxy sidecar. SIDE GOTCHA: Git Bash MANGLES absolute container paths passed to `docker exec` (`/usr/local/... -> C:/Program Files/Git/usr/local/...`); prefix with `MSYS_NO_PATHCONV=1` — but that ALSO breaks `~` expansion, so do not leave it exported for later commands. See [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]].

## Related

- [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]]

%% ai-graph-start %%

**Related notes:**
- [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]]
- [[claude-proxy login expires mid-session - implement silently degrades to heuristic stubs (score 0.00)]]
- [[Node spawn shellfalse on Windows won't run .cmd.ps1 wrappers (ENOENT)]]
- [[Why local test-agent is slow claude -p ships a 17.5K agent prompt on Opus, x many serial calls]]
- [[Claude Code headless auth setup-token prints a 1-year token, inject via CLAUDE_CODE_OAUTH_TOKEN]]

%% ai-graph-end %%