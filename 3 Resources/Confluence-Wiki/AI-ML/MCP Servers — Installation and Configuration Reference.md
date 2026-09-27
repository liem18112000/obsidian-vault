---
title: "MCP Servers — Installation and Configuration Reference"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/PIX/pages/49218027578/MCP+Servers+Installation+and+Configuration+Reference
space: "PIX"
topic: ai_ml
relevance: 0.773
depth: 2.75
updated: 2026-03-10
attachments: 0
tags:
  - confluence
  - ai-ml
  - space/pix
---

# MCP Servers — Installation and Configuration Reference

> [!info] Imported from Confluence
> Space **PIX** · updated 2026-03-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/PIX/pages/49218027578/MCP+Servers+Installation+and+Configuration+Reference)
> Relevance 0.773 · topic `ai_ml`

# MCP Servers — Installation and Configuration Reference

This page lists all MCP (Model Context Protocol) servers used with GitHub Copilot in VS Code for the KLARA project. MCP servers extend Copilot's capabilities by connecting it to external services.

All MCP server configurations are stored in: `%APPDATA%/Code/User/mcp.json`

------------------------------------------------------------------------

## Overview

<div>

|  |  |  |  |
|----|----|----|----|
| \# | MCP Server | Type | Purpose |
| 1 | Atlassian MCP | HTTP | Jira and Confluence operations |
| 2 | Bitbucket MCP | stdio | Git repository, pull requests, pipelines |
| 3 | PostgreSQL MCP | VS Code Extension | Database queries and schema inspection |
| 4 | Awesome Copilot MCP | stdio (Docker) | Access curated Copilot instructions and patterns |
| 5 | Chrome DevTools MCP | stdio | Browser debugging and performance profiling |
| 6 | MarkItDown MCP | stdio | Convert files (PDF, DOCX, XLSX, images, etc.) to Markdown |

</div>

------------------------------------------------------------------------

## 1. Atlassian MCP Server

**ID:** `com.atlassian/atlassian-mcp-server`  
**Type:** HTTP (cloud-hosted by Atlassian)  
**Purpose:** All Jira and Confluence operations — read tickets, search issues, create/update Confluence pages, manage comments.

### Installation

1.  Add to `%APPDATA%/Code/User/mcp.json`:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c8e59772-6cde-4723-9902-e7bf2f029562" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "com.atlassian/atlassian-mcp-server": {
    "type": "http",
    "url": "https://mcp.atlassian.com/v1/mcp",
    "gallery": "https://api.mcp.github.com",
    "version": "1.1.1"
  }
}
```

</div>

</div>

2.  Restart VS Code or reload the window

3.  When you first use an Atlassian MCP tool in Copilot Chat, it will prompt you to authenticate via your browser (Atlassian OAuth2 flow). Follow the browser prompt to grant access to your Atlassian cloud site.

4.  **Verify:** In Copilot Chat, use any `mcp_com_atlassian_*` tool (e.g., search for a Jira issue) — it should work after authentication

**Note:** No API key or token is needed in `mcp.json`. Authentication is handled automatically via the OAuth2 flow on first use.

------------------------------------------------------------------------

## 2. Bitbucket MCP Server

**ID:** `bitbucket`  
**Type:** stdio (npx)  
**Purpose:** Repository operations, pull requests, pipelines, code review, branch management.

### Installation

1.  Make sure you have Node.js installed

2.  Create a Bitbucket API Key:

    - Go to <a href="https://id.atlassian.com/manage-profile/security/api-tokens" class="external-link" rel="nofollow">https://id.atlassian.com/manage-profile/security/api-tokens</a>

    - Click **Create API Token with scopes**

    - Give it a label (e.g., "MCP Server")

    - Select the required scopes

    - Click **Create**

    - **Copy the generated API key** — you will not be able to see it again

3.  Add to `%APPDATA%/Code/User/mcp.json`, replacing the password with your generated API key:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a4eea77c-523c-4bb8-9409-86f90c9d5a31" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "bitbucket": {
    "type": "stdio",
    "command": "npx",
    "args": [
      "bitbucket-mcp"
    ],
    "env": {
      "BITBUCKET_URL": "https://api.bitbucket.org/2.0",
      "BITBUCKET_USERNAME": "<your-email@axonactive.com>",
      "BITBUCKET_PASSWORD": "<your-api-key>",
      "BITBUCKET_WORKSPACE": "axonivy-prod"
    }
  }
}
```

</div>

</div>

4.  Restart VS Code or reload the window

5.  **Verify:** In Copilot Chat, use any `mcp_bitbucket_*` tool (e.g., list repositories)

------------------------------------------------------------------------

## 3. PostgreSQL MCP Server

**ID:** `PostgreSQL MCP` (pgsql\_\*)  
**Type:** VS Code Extension  
**Purpose:** Database queries, schema inspection, data exploration, and CSV import.

### Installation

1.  Install the VS Code PostgreSQL extension:

    - Open Extensions panel (Ctrl+Shift+X)

    - Search for `ms-ossdata.vscode-pgsql`

    - Click **Install**

2.  The MCP tools are provided by the extension automatically. No `mcp.json` configuration needed.

3.  Configure your database connection:

    - Open Command Palette (Ctrl+Shift+P)

    - Run "PostgreSQL: Add Connection"

    - Enter your host, port, database, username, and password

4.  **Verify:** In Copilot Chat (PostgreSQL DBA agent mode), check that `pgsql_*` tools are available

------------------------------------------------------------------------

## 4. Awesome Copilot MCP Server

**ID:** `awesome-copilot`  
**Type:** stdio (Docker)  
**Purpose:** Access curated GitHub Copilot instructions, patterns, and best practices from the awesome-copilot community repository.

### Prerequisites

**Docker Desktop must be installed and running.** The server runs as a Docker container.

- Download Docker Desktop: <a href="https://www.docker.com/products/docker-desktop/" class="external-link" rel="nofollow">https://www.docker.com/products/docker-desktop/</a>

- After installation, make sure the Docker daemon is running (Docker Desktop icon in system tray should show "Docker Desktop is running")

### Installation

1.  Install Docker Desktop and ensure the daemon is running

2.  Add to `%APPDATA%/Code/User/mcp.json`:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ce6bf4ae-0cf0-4e93-829d-4300103bec06" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "awesome-copilot": {
    "type": "stdio",
    "command": "docker",
    "args": [
      "run",
      "-i",
      "--rm",
      "ghcr.io/microsoft/mcp-dotnet-samples/awesome-copilot:latest"
    ]
  }
}
```

</div>

</div>

3.  Restart VS Code or reload the window

4.  On first use, Docker will pull the image `ghcr.io/microsoft/mcp-dotnet-samples/awesome-copilot:latest` (this may take a moment)

5.  **Verify:** In Copilot Chat, check that awesome-copilot tools are available

**Important:** If Docker Desktop is not running, this MCP server will fail to start. Always ensure the Docker daemon is active before using this tool.

------------------------------------------------------------------------

## 5. Chrome DevTools MCP Server

**ID:** `io.github.ChromeDevTools/chrome-devtools-mcp`  
**Type:** stdio (npx)  
**Purpose:** Browser debugging, DOM inspection, console interaction, network monitoring, and performance profiling directly from Copilot Chat.

### Installation

1.  Make sure you have Node.js installed

2.  Add to `%APPDATA%/Code/User/mcp.json`:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6b3c5d0c-2565-46a5-bc46-7d2d57b031ea" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "io.github.ChromeDevTools/chrome-devtools-mcp": {
    "type": "stdio",
    "command": "npx",
    "args": [
      "--registry",
      "https://registry.npmjs.org",
      "chrome-devtools-mcp@0.19.0"
    ],
    "gallery": "https://api.mcp.github.com",
    "version": "0.19.0"
  }
}
```

</div>

</div>

3.  Launch Chrome with remote debugging enabled:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1a77cbfb-0179-4085-b770-7009c6527e08" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222
```

</div>

</div>

4.  Restart VS Code or reload the window

5.  **Verify:** In Copilot Chat, check that Chrome DevTools tools are available

**Note:** Chrome must be launched with the `--remote-debugging-port` flag for the MCP server to connect to it.

------------------------------------------------------------------------

## 6. MarkItDown MCP Server

**ID:** `microsoft/markitdown`  
**Type:** stdio (uvx)  
**Purpose:** Convert various file formats (PDF, DOCX, XLSX, PPTX, images, HTML, audio, etc.) to Markdown text, making their content accessible to Copilot.

### Prerequisites

- Python 3.10+ must be installed

- `uv` package manager must be installed: `pip install uv` or see <a href="https://docs.astral.sh/uv/getting-started/installation/" class="external-link" rel="nofollow">https://docs.astral.sh/uv/getting-started/installation/</a>

### Installation

1.  Install `uv` if not already installed:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4ea75a9b-f9af-4e5b-add7-48964e9b89d8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
pip install uv
```

</div>

</div>

2.  Add to `%APPDATA%/Code/User/mcp.json`:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6e6abf8a-3adf-4a78-a932-d3668f9a5566" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "microsoft/markitdown": {
    "type": "stdio",
    "command": "uvx",
    "args": [
      "markitdown-mcp@0.0.1a4"
    ],
    "gallery": "https://api.mcp.github.com",
    "version": "1.0.0"
  }
}
```

</div>

</div>

3.  Restart VS Code or reload the window

4.  On first use, `uvx` will automatically download and install the `markitdown-mcp` package

5.  **Verify:** In Copilot Chat, check that markitdown tools are available

### Supported Formats

PDF, DOCX, XLSX, PPTX, HTML, CSV, JSON, XML, images (JPG, PNG), audio (MP3, WAV), ZIP archives
