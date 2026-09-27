---
title: "Bitbucket MCP Server Integration Guide"
created: 2025-11-27
updated: 2025-11-27
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48914104341/Bitbucket+MCP+Server+Integration+Guide
confluence_id: "48914104341"
confluence_path: "Team Kepler > Developer note > AI Research > MCP"
tags: [confluence, mcp, search]
---

# Bitbucket MCP Server Integration Guide

*Confluence source · Team Kepler › Developer note › AI Research › MCP · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48914104341/Bitbucket+MCP+Server+Integration+Guide) · updated 2025-11-27*

This guide helps a newcomer stand up the Bitbucket Model Context Protocol (MCP) server in this repository and connect it to GitHub Copilot so Copilot can query live Bitbucket context (repositories, pull requests, branches, code, etc.).

------------------------------------------------------------------------

### 1. What Is MCP and Why Use It with Copilot?

Model Context Protocol (MCP) is an open standard that lets AI tools (like GitHub Copilot) securely fetch external context (APIs, files, systems) at runtime. Instead of pasting data manually or granting overly broad permissions, you expose a focused set of "tools" and resources. Copilot then requests only what it needs, when it needs it.

This repo uses the published container image `liem18112000/ts-mcp-bitbucket` to provide Bitbucket integration via MCP.

------------------------------------------------------------------------

### 2. High-Level Architecture

![[image-20251127-012124.png]]

Key points:

- Copilot acts as an MCP client.

- The Bitbucket MCP server runs locally on port 9001 (HTTP transport).

- You approve tool usage interactively, safeguarding credentials and scope.

------------------------------------------------------------------------

### 3. Prerequisites

|  |  |
|----|----|
| Item | Notes |
| Docker Desktop (or compatible) | For running the container via `docker-compose.yml` |
| GitHub Copilot (IDE Extension) | VS Code / JetBrains with MCP support |
| Bitbucket credentials | Scoped API Token (recommended) or App Password (legacy) |
| Network access | IDE must reach `http://localhost:9001` |
| Optional: Default workspace | Avoid specifying workspace in every query |

#### Bitbucket Environment Variables

Copy `.env.example` to `.env` in the `mcp-server-atlassian-bitbucket` folder and fill values:

**Option 1: Scoped API Token (Recommended - Future-Proof)**

```
# Enable debug logging (optional)
DEBUG=false

# Atlassian Configuration - Scoped API Token (recommended)
ATLASSIAN_USER_EMAIL=your-email@example.com
ATLASSIAN_API_TOKEN=ATATT3xFfGF0...your-scoped-api-token

# Optional: Default workspace for commands
BITBUCKET_DEFAULT_WORKSPACE=your-main-workspace-slug
```

**Option 2: App Password (Legacy - Will be deprecated June 2026)**

```
# Enable debug logging (optional)
DEBUG=false

# Bitbucket-specific authentication (legacy)
ATLASSIAN_BITBUCKET_USERNAME=your-bitbucket-username
ATLASSIAN_BITBUCKET_APP_PASSWORD=your-app-password

# Optional: Default workspace for commands
BITBUCKET_DEFAULT_WORKSPACE=your-main-workspace-slug
```

#### Getting Your Credentials

##### Scoped API Token (Recommended)

1.  Go to [Atlassian API Tokens](https://id.atlassian.com/manage-profile/security/api-tokens)

2.  Click **"Create API token with scopes"**

3.  Select **"Bitbucket"** as the product

4.  Choose the appropriate scopes:

    - **For read-only access**: `repository`, `workspace`

    - **For full functionality**: `repository`, `workspace`, `pullrequest`

5.  Copy the generated token (starts with `ATATT`)

6.  Use with your Atlassian email as the username

##### App Password (Legacy)

1.  Go to [Bitbucket App Passwords](https://bitbucket.org/account/settings/app-passwords/)

2.  Click "Create app password"

3.  Give it a name like "AI Assistant"

4.  Select permissions:

    - **Workspaces**: Read

    - **Repositories**: Read (and Write if you want AI to create PRs/comments)

    - **Pull Requests**: Read (and Write for PR management)

Security tip: Never commit `.env` containing real secrets.

------------------------------------------------------------------------

### 4. Deployment

#### Docker Compose Service Explanation

Below is the `docker-compose.yml` for the Bitbucket MCP server:

```
services:
  mcp-bitbucket-http:
    image: liem18112000/ts-mcp-bitbucket
    container_name: ts-mcp-bitbucket
    restart: unless-stopped
    ports:
      - "9001:9001"
    env_file:
      - .env
    environment:
      - TRANSPORT_MODE=http
      - PORT=9001
    healthcheck:
      test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://localhost:3000/"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 5s
```

Field breakdown:

- `image`: Pulls the published Bitbucket MCP image containing repository/PR/branch tool implementations.

- `container_name`: Friendly name used in logs/commands (`docker logs ts-mcp-bitbucket`).

- `env_file`: Loads credentials & configuration from `.env` (ATLASSIAN_USER_EMAIL, ATLASSIAN_API_TOKEN, etc.).

- `ports`: Maps host port 9001 to container port 9001 so Copilot can access `http://localhost:9001/mcp`.

- `environment`:

  - `TRANSPORT_MODE=http`: Uses HTTP transport suitable for web-based MCP clients.

  - `PORT=9001`: Binds the server inside the container to port 9001 (must match `ports` mapping).

- `restart: unless-stopped`: Automatically restarts the container on failure or daemon restart unless you explicitly stop it.

- `healthcheck`: Periodically checks server health for reliability.

Customizing the port (example: change to 9101):

1.  In `docker-compose.yml` modify:

```
    ports:
      - "9101:9101"
    environment:
      - TRANSPORT_MODE=http
      - PORT=9101
```

2.  Recreate:

```
docker compose up -d mcp-bitbucket-http
```

3.  Register in Copilot with URL: `http://localhost:9101/mcp`.

Environment variable lifecycle:

- Update `.env` when rotating API tokens.

- Run `docker compose restart ts-mcp-bitbucket` to apply changes.

Health & Logs:

```
docker ps --filter "name=ts-mcp-bitbucket"
docker logs ts-mcp-bitbucket --tail 50
curl http://localhost:9001/mcp
```

#### Run with Docker Compose

1.  Ensure `.env` is populated with Bitbucket credentials.

2.  From the `mcp-server-atlassian-bitbucket` directory:

```
docker compose up -d mcp-bitbucket-http
```

3.  Verify container:

```
docker ps --filter "name=ts-mcp-bitbucket"
```

4.  Test endpoint (basic connectivity):

```
curl http://localhost:9001/mcp
```

#### Updating Image

Pull latest and recreate:

```
docker pull liem18112000/ts-mcp-bitbucket:latest
docker compose up -d mcp-bitbucket-http
```

------------------------------------------------------------------------

### 5. Register Bitbucket MCP in GitHub Copilot

#### VS Code

1.  Ctrl+Shift+P → "Copilot: Add MCP Server".

2.  Transport: HTTP.

3.  Name: `mcp-bitbucket`

4.  URL: `http://localhost:9001/mcp`

5.  Accept permissions.

#### JetBrains

1.  Settings → Tools → GitHub Copilot → MCP / External Context.

2.  Add Server: Name `mcp-bitbucket`, URL `http://localhost:9001/mcp`.

3.  Apply & reopen Copilot Chat.

#### JSON Config (If Supported)

```
{
  "servers": [
    { "name": "mcp-bitbucket", "transport": "http", "url": "http://localhost:9001/mcp" }
  ]
}
```

Reload the IDE (format/path may vary with Copilot release).

------------------------------------------------------------------------

### 6. Typical Bitbucket MCP Capabilities

The Bitbucket MCP server exposes tools for interacting with your Bitbucket repositories. Common categories:

#### Workspaces

- List workspaces

- Get workspace details

#### Repositories

- List repositories in a workspace

- Get repository details

- Get commit history

- Get file content from a repository

- List branches

- Create new branches

- Clone repositories

#### Pull Requests

- List pull requests (filter by state: OPEN, MERGED, DECLINED, SUPERSEDED)

- Get pull request details (with full diff and comments)

- Create pull requests

- Update pull requests

- Approve/Reject pull requests

- Add comments to pull requests (general or inline code comments)

#### Search & Diff

- Search code, repositories, pull requests, or content

- Compare branches (diff)

- Compare commits (diff)

Ask Copilot: "List my Bitbucket workspaces" or "Show open PRs in repo backend-api" and it will surface available tools.

#### Sample Prompts

- "List all repositories in my workspace."

- "Show details for pull request \#42 in the backend-api repo."

- "What are the open pull requests that need review?"

- "Compare my feature branch with the main branch."

- "Search for files containing 'authentication' in my codebase."

- "Create a pull request from feature-login to main."

------------------------------------------------------------------------

### 7. Sequence of a Pull Request Query

![[image-20251127-012338.png]]

------------------------------------------------------------------------

### 8. Transport & Configuration

|  |  |
|----|----|
| Aspect | Value |
| Transport | HTTP |
| Port | 9001 |
| Endpoint | `http://localhost:9001/mcp` |
| Auth | Atlassian Scoped API Token or Bitbucket App Password via environment |
| Default Workspace | `BITBUCKET_DEFAULT_WORKSPACE` (optional) |

Change port by editing `docker-compose.yml` and recreating the container.

------------------------------------------------------------------------

### 9. Security & Least Privilege

- Use Scoped API Tokens (recommended) instead of App Passwords (being deprecated June 2026).

- Use distinct API tokens for MCP (not your personal high-privilege account if possible).

- Apply minimum required scopes (`repository`, `workspace`, `pullrequest` as needed).

- Set `BITBUCKET_DEFAULT_WORKSPACE` to limit default scope.

- Rotate tokens periodically; restart container after update.

- Do NOT share your `.env` file or commit it to version control.

------------------------------------------------------------------------

### 10. Troubleshooting

|  |  |  |
|----|----|----|
| Symptom | Cause | Resolution |
| 401 Unauthorized | Bad / expired token | Regenerate token, update `.env`, restart container |
| 403 Forbidden | Insufficient permissions | Add required scopes to your API token |
| Workspace not found | Wrong workspace slug | Run `bb_ls_workspaces` to see correct slugs |
| Repository not found | Wrong repo slug | Check URL format: `bitbucket.org/{workspace}/{repo}` |
| Copilot not listing tools | Registration failed | Re-add server; verify URL `http://localhost:9001/mcp` |
| Container not starting | Port conflict | Change port in docker-compose.yml |
| Authentication failed | Using wrong auth method | Check if using email+token or username+app-password correctly |

Logs:

```
docker logs ts-mcp-bitbucket --tail 100
```

Connectivity test:

```
curl http://localhost:9001/mcp
```

------------------------------------------------------------------------

### 11. Verification Checklist

1.  `.env` created & Bitbucket credentials inserted ✅

2.  `docker compose up -d mcp-bitbucket-http` succeeds ✅

3.  `curl http://localhost:9001/mcp` returns server response ✅

4.  Copilot registration shows `mcp-bitbucket` ✅

5.  Prompt: "List my Bitbucket workspaces" returns data ✅

------------------------------------------------------------------------

### 12. Maintenance

- Update image periodically (see section 4).

- Rotate credentials when required (especially before App Password deprecation in June 2026).

- Audit tool usage (inspect container logs).

- Migrate from App Passwords to Scoped API Tokens before deprecation.

------------------------------------------------------------------------

### 13. Extending / Customizing

If you need custom tooling beyond the image capabilities you can:

1.  Build a wrapper MCP server that calls Bitbucket REST plus internal systems.

2.  Add new tools (functions) with clear, narrow purpose.

3.  Run alongside other MCP servers (e.g., `mcp-atlassian` on port 9000) and register both.

------------------------------------------------------------------------

### 14. Glossary

- **Bitbucket**: Git-based source code repository hosting service by Atlassian.

- **Workspace**: Bitbucket organizational unit containing repositories.

- **Repository**: A Git repository hosted on Bitbucket.

- **Pull Request (PR)**: A request to merge code changes from one branch to another.

- **Branch**: A parallel version of a repository.

- **MCP Tool**: Exposed operation callable by AI client.

- **Scoped API Token**: New Atlassian authentication method with granular permissions.

- **App Password**: Legacy Bitbucket authentication method (being deprecated).

------------------------------------------------------------------------

### 15. Final Architecture Overview

```
MCP HTTP
```

`REST`

`Copilot Chat`

`ts-mcp-bitbucket :9001`

`Bitbucket Cloud`

------------------------------------------------------------------------

### 16. Quick Teardown

```
docker compose down mcp-bitbucket-http
```

Remove server from Copilot MCP configuration. Delete `.env` if decommissioning.

------------------------------------------------------------------------

### 17. Sample Advanced Prompts

- "List all branches in the user-service repository."

- "Show the commit history for the feature-auth branch."

- "Get the content of src/config.js from the main branch."

- "Compare the differences between my feature branch and main."

- "Create a new branch called hotfix-login from main."

- "Add a comment to PR \#15 saying the tests passed."

- "Search for JavaScript files that contain 'authentication'."

------------------------------------------------------------------------

### 18. Next Steps

- Add Jira/Confluence integration via `mcp-atlassian` server (port 9000).

- Migrate from App Passwords to Scoped API Tokens before June 2026.

- Implement caching for common queries.

- Set up multiple workspace configurations if needed.

Happy building! Update this guide as your Bitbucket usage evolves.
