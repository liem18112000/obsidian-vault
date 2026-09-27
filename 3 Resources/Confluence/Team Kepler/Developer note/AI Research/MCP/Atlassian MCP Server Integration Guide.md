---
ai_hash: 50aed7fe3273b0d1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48911777796'
confluence_path: Team Kepler > Developer note > AI Research > MCP
created: 2025-11-26
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- mcp
- search
title: Atlassian MCP Server Integration Guide
type: source
updated: 2025-11-26
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48911777796/Atlassian+MCP+Server+Integration+Guide
---

# Atlassian MCP Server Integration Guide

*Confluence source · Team Kepler › Developer note › AI Research › MCP · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48911777796/Atlassian+MCP+Server+Integration+Guide) · updated 2025-11-26*

## Atlassian MCP Server Integration Guide (for GitHub Copilot)

This guide helps a newcomer stand up the Atlassian Model Context Protocol (MCP) server in this repository and connect it to GitHub Copilot so Copilot can query live Jira and Confluence context (projects, issues, pages, etc.).

------------------------------------------------------------------------

### 1. What Is MCP and Why Use It with Copilot?

Model Context Protocol (MCP) is an open standard that lets AI tools (like GitHub Copilot) securely fetch external context (APIs, files, systems) at runtime. Instead of pasting data manually or granting overly broad permissions, you expose a focused set of “tools” and resources. Copilot then requests only what it needs, when it needs it.

This repo uses the published container image `ghcr.io/sooperset/mcp-atlassian:latest` to provide Jira / Confluence integration via MCP.

------------------------------------------------------------------------

### 2. High-Level Architecture

![[image-20251126-070601.png]]

Key points:

- Copilot acts as an MCP client.

- The Atlassian MCP server runs locally on port 9000 (HTTP transport: `streamable-http`).

- You approve tool usage interactively, safeguarding credentials and scope.

------------------------------------------------------------------------

### 3. Prerequisites

|  |  |
|----|----|
| Item | Notes |
| Docker Desktop (or compatible) | For running the container via `docker-compose.yml` |
| GitHub Copilot (IDE Extension) | VS Code / JetBrains with MCP support |
| Atlassian (Jira & Confluence) credentials | API tokens (Cloud) or Personal Access Tokens (Server/DC) |
| Network access | IDE must reach `<http://localhost:9000`\> |
| Optional filters | Limit projects/spaces for performance and privacy |

#### Atlassian Environment Variables

Copy `.env.example` to `.env` at repo root and fill values (remove example emails/tokens):

```
JIRA_URL=https://your-domain.atlassian.net
JIRA_USERNAME=your-email@domain.com
JIRA_API_TOKEN=your-jira-api-token

CONFLUENCE_URL=https://your-domain.atlassian.net/wiki
CONFLUENCE_USERNAME=your-email@domain.com
CONFLUENCE_API_TOKEN=your-confluence-api-token
```

For Server/Data Center (instead):

```
# JIRA_PERSONAL_TOKEN=your-personal-access-token
# CONFLUENCE_PERSONAL_TOKEN=your-personal-access-token
# JIRA_SSL_VERIFY=false             # only if you must bypass SSL
# CONFLUENCE_SSL_VERIFY=false       # only if you must bypass SSL
```

Optional scoped filters & flags:

```
# JIRA_PROJECTS_FILTER=PROJ1,PROJ2
# CONFLUENCE_SPACES_FILTER=SPACE1,SPACE2
# READ_ONLY_MODE=true
# MCP_VERBOSE=true
```

Security tip: Never commit `.env` containing real secrets.

------------------------------------------------------------------------

### 4. Deployment

#### Docker Compose Service Explanation

Below is the relevant portion of `docker-compose.yml` for the Atlassian MCP server (Bitbucket service omitted):

```
version: '3.8'
services:
  mcp-atlassian:
    image: ghcr.io/sooperset/mcp-atlassian:latest
    container_name: mcp-atlassian
    env_file:
      - .env
    ports:
      - "9000:9000"
    command: ["--transport", "streamable-http", "--port", "9000", "-vv"]
    restart: unless-stopped
```

Field breakdown:

- `image`: Pulls the published Atlassian MCP image containing Jira/Confluence tool implementations.

- `container_name`: Friendly name used in logs/commands (`docker logs mcp-atlassian`).

- `env_file`: Loads credentials & configuration from the root `.env` (JIRA_URL, JIRA_USERNAME, tokens, filters, etc.).

- `ports`: Maps host port 9000 to container port 9000 so Copilot can access `<http://localhost:9000/mcp`.\>

- `command`:

  - `--transport streamable-http`: Uses a streaming HTTP transport suitable for incremental responses.

  - `--port 9000`: Binds the server inside the container to port 9000 (must match `ports` mapping).

  - `-vv`: Enables verbose logging (two v flags). Reduce to `-v` or remove for quieter output.

- `restart: unless-stopped`: Automatically restarts the container on failure or daemon restart unless you explicitly stop it.

Customizing the port (example: change to 9100):

1.  In `docker-compose.yml` modify:

```
    ports:
      - "9100:9100"
    command: ["--transport", "streamable-http", "--port", "9100", "-vv"]
```

2.  Recreate:

```
docker compose up -d mcp-atlassian
```

3.  Register in Copilot with URL: `<http://localhost:9100/mcp`.\>

Environment variable lifecycle:

- Update `.env` when rotating API tokens.

- Run `docker compose restart mcp-atlassian` to apply changes.

- Use `READ_ONLY_MODE=true` for safer evaluation sessions (prevents write operations like create issue/page).

Health & Logs:

```
docker ps --filter "name=mcp-atlassian"
docker logs mcp-atlassian --tail 50
curl http://localhost:9000/mcp | Select-String "mcp-atlassian"
```

#### Run with Docker Compose

1.  Ensure `.env` is populated with Jira/Confluence credentials.

2.  From repository root:

```
docker compose up -d mcp-atlassian
```

3.  Verify container:

```
docker ps --filter "name=mcp-atlassian"
```

4.  Test endpoint (basic connectivity):

```
curl http://localhost:9000/mcp | Select-String "mcp-atlassian"
```

#### Updating Image

Pull latest and recreate:

```
docker pull ghcr.io/sooperset/mcp-atlassian:latest
docker compose up -d mcp-atlassian
```

------------------------------------------------------------------------

### 5. Register Atlassian MCP in GitHub Copilot

#### VS Code

1.  Ctrl+Shift+P → "Copilot: Add MCP Server".

2.  Transport: HTTP.

3.  Name: `mcp-atlassian`

4.  URL: `<http://localhost:9000/mcp`\>

5.  Accept permissions.

#### JetBrains

1.  Settings → Tools → GitHub Copilot → MCP / External Context.

2.  Add Server: Name `mcp-atlassian`, URL `<http://localhost:9000/mcp`.\>

3.  Apply & reopen Copilot Chat.

#### JSON Config (If Supported)

```
{
  "servers": [
    { "name": "mcp-atlassian", "transport": "http", "url": "http://localhost:9000/mcp" }
  ]
}
```

Reload the IDE (format/path may vary with Copilot release).

------------------------------------------------------------------------

### 6. Typical Atlassian MCP Capabilities

Because the image is external, exact tool names may evolve. Common categories you can expect:

- Jira: list projects, get project details, search issues (JQL), get issue, create issue, transition issue, add comment.

- Confluence: list spaces, get space, search pages, fetch page content, create/update page (if not in READ_ONLY_MODE).

- Metadata/Filters: restrict operations to configured project or space filters.

Ask Copilot: "Show Jira projects" or "Fetch Confluence pages in space DOCS" and it will surface available tools.

#### Sample Prompts

- "List open Jira issues in project ENG tagged performance."

- "Show details for Jira issue ENG-123 and summarize acceptance criteria."

- "Find Confluence pages mentioning 'architecture decision' in space ENG".

------------------------------------------------------------------------

### 7. Sequence of a Jira Issue Query

![[image-20251126-070644.png]]

------------------------------------------------------------------------

### 8. Transport & Configuration

|                    |                                                 |
|--------------------|-------------------------------------------------|
| Aspect             | Value                                           |
| Transport          | streamable-http (HTTP)                          |
| Port               | 9000                                            |
| Endpoint           | `<http://localhost:9000/mcp`\>                  |
| Auth               | Atlassian API/Personal tokens via environment   |
| Optional Read-Only | `READ_ONLY_MODE=true` prevents write operations |

Change port by editing `docker-compose.yml` and recreating the container.

------------------------------------------------------------------------

### 9. Security & Least Privilege

- Use distinct API tokens for MCP (not your personal high-privilege account if possible).

- Apply project/space filters to narrow scope.

- Enable `READ_ONLY_MODE=true` unless you explicitly need create/update.

- Rotate tokens periodically; restart container after update.

- Do NOT disable SSL verification unless in tightly controlled dev environment.

------------------------------------------------------------------------

### 10. Troubleshooting

|  |  |  |
|----|----|----|
| Symptom | Cause | Resolution |
| 401 Unauthorized | Bad / expired token | Regenerate token, update `.env`, restart container |
| Empty project or space list | Filters too restrictive | Remove or adjust `JIRA_PROJECTS_FILTER` / `CONFLUENCE_SPACES_FILTER` |
| Copilot not listing tools | Registration failed | Re-add server; verify URL `<http://localhost:9000/mcp`\> |
| Slow responses | Large queries (broad JQL) | Narrow JQL; apply project filters |
| SSL errors (Server/DC) | Self-signed cert | Use proper certs or set `JIRA_SSL_VERIFY=false` (last resort) |

Logs:

```
docker logs mcp-atlassian --tail 100
```

Connectivity test:

```
curl http://localhost:9000/mcp | Select-String "mcp-atlassian"
```

------------------------------------------------------------------------

### 11. Verification Checklist

1.  `.env` created & Jira/Confluence tokens inserted ✅

2.  `docker compose up -d mcp-atlassian` succeeds ✅

3.  `curl <http://localhost:9000/mcp`\> returns server banner ✅

4.  Copilot registration shows `mcp-atlassian` ✅

5.  Prompt: "List Jira projects" returns data ✅

6.  (Optional) `READ_ONLY_MODE=true` confirmed for safe use ✅

------------------------------------------------------------------------

### 12. Maintenance

- Update image periodically (see section 4).

- Review filters as new projects/spaces evolve.

- Audit tool usage (Copilot may show recent tool calls; otherwise inspect logs).

- Rotate credentials quarterly.

------------------------------------------------------------------------

### 13. Extending / Customizing

If you need custom tooling beyond the image capabilities you can:

1.  Build a wrapper MCP server that calls Jira/Confluence REST plus internal systems.

2.  Add new tools (functions) with clear, narrow purpose (e.g., dependency graph retrieval).

3.  Run alongside `mcp-atlassian` on a different port (e.g., 9002) and register both.

------------------------------------------------------------------------

### 14. Glossary

- Jira: Issue & project tracking platform.

- Confluence: Knowledge base / documentation wiki.

- JQL: Jira Query Language for flexible issue search.

- Space: Confluence content grouping.

- MCP Tool: Exposed operation callable by AI client.

------------------------------------------------------------------------

### 15. Final Architecture Overview

![[image-20251126-070727.png]]

------------------------------------------------------------------------

### 16. Quick Teardown

```
docker compose down mcp-atlassian
```

Remove server from Copilot MCP configuration. Delete `.env` if decommissioning.

------------------------------------------------------------------------

### 17. Sample Advanced Prompts

- "Summarize Jira issues updated in the last 2 days for project OPS."

- "List Confluence pages in space ENG containing the phrase 'performance benchmark'."

- "Generate a report of Jira issues blocked and suggest next actions."

------------------------------------------------------------------------

### 18. Next Steps

- Add internal knowledge base integration via a custom MCP server.

- Implement caching for common queries.

- Enable read-only mode for production safety.

Happy building! Update this guide as your Atlassian usage evolves.

%% ai-graph-start %%

**Related notes:**
- [[Bitbucket MCP Server Integration Guide]]
- [[MCP Servers — Installation and Configuration Reference]]
- [[Setup VS Code - Github Copilot - MCP Server]]
- [[AI-Powered Development Environment Architecture]]
- [[Expose an app as an MCP server by wrapping the same services container the webCLI use]]

%% ai-graph-end %%