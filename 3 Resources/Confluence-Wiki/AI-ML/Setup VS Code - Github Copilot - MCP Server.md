---
ai_hash: 0d3e276c242de006
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.861
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48837132304/Setup+VS+Code+-+Github+Copilot+-+MCP+Server
space: FUT
status: reference
tags:
- confluence
- ai-ml
- space/fut
title: Setup VS Code - Github Copilot - MCP Server
topic: ai_ml
type: source
updated: 2025-11-05
---

# Setup VS Code - Github Copilot - MCP Server

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-11-05 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48837132304/Setup+VS+Code+-+Github+Copilot+-+MCP+Server)
> Relevance 0.861 · topic `ai_ml`

## Install VS code

Follow this instruction to install VS Code for windows <a href="https://code.visualstudio.com/docs/setup/windows" class="external-link" rel="nofollow">https://code.visualstudio.com/docs/setup/windows</a>

## Setup Github Copilot

Follow this instruction to install Github Copilot in VS Code <a href="https://code.visualstudio.com/docs/copilot/setup" class="external-link" rel="nofollow">https://code.visualstudio.com/docs/copilot/setup</a>

## Setup MCP

<a href="https://cloud.google.com/ai/llms" class="external-link" rel="nofollow"><u>Large language models</u></a> (LLMs) are powerful, but they have two major limitations:

- knowledge is frozen at the time of their training,

- can't interact with the outside world.

MCP provides a secure and standardized "language" for LLMs to communicate with external data, applications, and services. It acts as a bridge, allowing AI to move beyond static knowledge and become a dynamic <a href="https://cloud.google.com/discover/what-are-ai-agents" class="external-link" rel="nofollow"><u>agent</u></a> that can retrieve current information and take action, making it more accurate, useful, and automated.

Read this documentation to have more knowledge about MCP, MCP server and how it works <a href="https://code.visualstudio.com/docs/copilot/customization/mcp-servers" class="external-link" rel="nofollow">https://code.visualstudio.com/docs/copilot/customization/mcp-servers</a>.

### Add MCP server

Now, go to <a href="https://github.com/mcp" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/mcp</a> and search the MCP you want to install. For example, I search for “Atlassian”

Then click on “Install“ button, select “\<\> Install in VS Code“.

Check your installed MCP server as this picture below


![[48837132304-image-20251105-034743.png]]

![[48837132304-image-20251105-035751.png]]



You can see that atlassian-mcp-server has provides many tools to search, get, create, update many things in Jira and confluence. They are the tools that allow LLM can perform actions as we want it to do. Such as search all documentations in confluence regarding the topic “Authorization“, and then make a short summarize so that I can understand from overview to the detail, depends on what I prompt.

Try it out!

%% ai-graph-start %%

**Related notes:**
- [[Atlassian MCP Server Integration Guide]]
- [[MCP Servers — Installation and Configuration Reference]]
- [[AI-Powered Development Environment Architecture]]
- [[Bitbucket MCP Server Integration Guide]]
- [[Recipe Github copilot]]

%% ai-graph-end %%