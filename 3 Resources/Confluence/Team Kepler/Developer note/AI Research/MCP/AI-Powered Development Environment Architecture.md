---
ai_hash: 56ecf7f20b9bacdb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48918528054'
confluence_path: Team Kepler > Developer note > AI Research > MCP
created: 2025-11-28
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- mcp
- search
title: AI-Powered Development Environment Architecture
type: source
updated: 2025-11-28
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48918528054/AI-Powered+Development+Environment+Architecture
---

# AI-Powered Development Environment Architecture

*Confluence source · Team Kepler › Developer note › AI Research › MCP · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48918528054/AI-Powered+Development+Environment+Architecture) · updated 2025-11-28*

### Overall Architecture Diagram

![[MCP-architecture.png]]

*Figure: Complete architecture showing Local Environment, Agentic Services with MCP, and Agent Persistence Layers*

------------------------------------------------------------------------

### Overview

This document describes an AI-assisted development ecosystem that integrates LLM models with various tools, persistence layers, and external services via MCP (Model Context Protocol).

------------------------------------------------------------------------

### High-Level Architecture

![[image-20251128-041614.png]]

------------------------------------------------------------------------

### 1. User Interaction Layer

#### User Roles and Responsibilities

![[image-20251128-041847.png]]

|  |  |
|----|----|
| Role | Primary Use Cases |
| **Executives / POs** | Strategic queries, project status, high-level reports |
| **BAs / Testers** | Requirements validation, test case generation, documentation |
| **Developer / DevOps** | Code generation, debugging, infrastructure automation |

------------------------------------------------------------------------

### 2. Local Environment

#### Interface Options

![[image-20251128-041955.png]]

#### Interface Comparison

|  |  |  |
|----|----|----|
| Interface | Best For | Features |
| **dev IDE** | Developers | Code completion, inline suggestions, refactoring |
| **UI-Based App** | Non-technical users | Visual interface, guided workflows, dashboards |
| **Personal CLI** | Power users | Scripting, automation, batch processing |

------------------------------------------------------------------------

### 3. Agentic Service with MCP

#### MCP Integration Architecture

![[image-20251128-042124.png]]

#### MCP Services Detail

![[image-20251128-042301.png]]

#### MCP Service Capabilities

|  |  |  |
|----|----|----|
| MCP Server | Service | Capabilities |
| **Atlassian MCP** | Jira | Create issues, update status, query backlogs, generate reports |
| **Atlassian MCP** | Confluence | Search docs, create pages, update content, extract knowledge |
| **Custom MCP** | Bitbucket Repo | Clone repos, create PRs, code review, branch management |
| **Custom MCP** | Bitbucket Pipeline | Trigger builds, monitor status, deploy artifacts |
| **Custom MCP** | Google Cloud | Provision resources, manage services, monitor infrastructure |

------------------------------------------------------------------------

### 4. Agent Persistence Layers

#### Memory Architecture

![[image-20251128-042517.png]]

#### RAG Pipeline Detail

![[image-20251128-042840.png]]

#### Storage Systems Comparison

|  |  |  |  |
|----|----|----|----|
| Storage Type | Purpose | Search Method | Best For |
| **GH Copilot Memory Bank** | Code context | Pattern matching | Code-related queries |
| **Markdown-based Repository** | Structured docs | File-based search | Documentation, runbooks |
| **Claude Skills** | Custom tools | Direct invocation | Specialized tasks |
| **Memory Database** | Session history | Multiple techniques | Conversation continuity |
| **Vector Database** | Semantic search | Embedding similarity | Knowledge retrieval |

------------------------------------------------------------------------

### 5. Data Flow Architecture

#### Complete Request Flow

![[image-20251128-043028.png]]

#### Context Enrichment Flow

![[image-20251128-043212.png]]

------------------------------------------------------------------------

### 6. Component Integration Matrix

![[image-20251128-043336.png]]

------------------------------------------------------------------------

### 7. Security Considerations

![[image-20251128-043500.png]]

#### Security Checklist

|                     |                                            |
|---------------------|--------------------------------------------|
| Layer               | Consideration                              |
| **Authentication**  | SSO integration, API key management        |
| **Authorization**   | Role-based access, MCP permission scopes   |
| **Data Protection** | Encryption at rest and in transit          |
| **Audit**           | Logging all LLM interactions and API calls |
| **Privacy**         | PII handling in memory systems             |

------------------------------------------------------------------------

### 8. Deployment Architecture

![[image-20251128-043635.png]]

------------------------------------------------------------------------

### Summary

This architecture provides:

1.  **Flexible User Access** - Multiple interfaces for different user types

2.  **Powerful AI Core** - LLM models as the central intelligence

3.  **Rich Integrations** - MCP connections to enterprise tools

4.  **Persistent Memory** - Multiple storage options for context continuity

5.  **Scalable Design** - Modular components that can be extended

The system enables AI-assisted development workflows while maintaining enterprise-grade security and integration capabilities.

%% ai-graph-start %%

**Related notes:**
- [[Setup VS Code - Github Copilot - MCP Server]]
- [[Knowledge Base Solutions Comparison Guide]]
- [[MCP Servers — Installation and Configuration Reference]]
- [[Multi-Agentic Architecture - Apply in AI Driven Testing]]
- [[Atlassian MCP Server Integration Guide]]

%% ai-graph-end %%