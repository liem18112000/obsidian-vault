---
title: "Knowledge Base Solutions Comparison Guide"
created: 2025-11-27
updated: 2025-11-28
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48915513456/Knowledge+Base+Solutions+Comparison+Guide
confluence_id: "48915513456"
confluence_path: "Team Kepler > Developer note > AI Research > MCP"
tags: [confluence, mcp, search]
---

# Knowledge Base Solutions Comparison Guide

*Confluence source · Team Kepler › Developer note › AI Research › MCP · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48915513456/Knowledge+Base+Solutions+Comparison+Guide) · updated 2025-11-28*

A comprehensive evaluation for AI-driven software development teams seeking to leverage persistent context and best practices.

[[knowledge-base.pdf|knowledge-base.pdf]]

------------------------------------------------------------------------

### Executive Summary

#### Tool-Based Solutions (Developer-Focused)

|  |  |  |  |  |
|----|----|----|----|----|
| Solution | Best For | Learning Curve | Team Sharing | Cost |
| **GitHub Copilot Memory Bank** | Teams heavily using Copilot + Git workflows | Low | Excellent (Git-native) | Free |
| **Claude Code Skills** | Teams using Claude Code for complex workflows | Medium | Good (Git-based) | Free |
| **Memori (GibsonAI)** | Multi-agent systems, custom AI apps | High | Moderate (DB-based) | Free (self-hosted) |
| **Markdown Repository** | Any team, tool-agnostic approach | Very Low | Excellent (Git-native) | Free |

#### Architecture Patterns (System-Level)

|  |  |  |  |  |
|----|----|----|----|----|
| Pattern | Best For | Learning Curve | Infrastructure | Cost |
| **RAG (Retrieval-Augmented Generation)** | Large/dynamic knowledge bases, real-time data | High | Vector DB + Embeddings | Medium-High |
| **CAG (Cache-Augmented Generation)** | Static/stable knowledge, low-latency needs | Medium | LLM with large context window | Low-Medium |

#### Recommendation for Your Team (5 SWEs + 1 AI Engineer)

- **Immediate**: Hybrid approach using **GitHub Copilot Memory Bank** + **Markdown Repository** for immediate value

- **Short-term**: Add **Claude Code Skills** for advanced automation

- **Long-term**: Evaluate **CAG** for stable domain knowledge, **RAG** for dynamic/large-scale knowledge retrieval

------------------------------------------------------------------------

### 1. Detailed Feature Comparison

#### Core Capabilities (Tool-Based Solutions)

|  |  |  |  |  |
|----|----|----|----|----|
| Feature | Copilot Memory Bank | Claude Code Skills | Memori | Markdown Repo |
| **Persistent Memory** | File-based (Git) | File-based (Git) | SQL Database | File-based (Git) |
| **Auto-context Injection** | Yes (via instructions) | Yes (auto-discovery) | Yes (API intercept) | Manual reference |
| **Cross-session Persistence** | Yes | Yes | Yes | Yes |
| **Semantic Search** | No | No | Yes (full-text SQL) | No (manual) |
| **Entity Extraction** | No | No | Yes (automatic) | No |
| **Multi-user Isolation** | Per-repo | Per-repo | Yes (user-scoped) | Per-repo |
| **Real-time Sync** | Git-based | Git-based | Database-based | Git-based |

#### Core Capabilities (Architecture Patterns)

|  |  |  |
|----|----|----|
| Feature | RAG | CAG |
| **Knowledge Storage** | Vector Database + Document Store | LLM KV Cache (precomputed) |
| **Retrieval Method** | Real-time semantic search | Pre-loaded context window |
| **Auto-context Injection** | Yes (per-query retrieval) | Yes (cached at startup) |
| **Semantic Search** | Yes (embeddings-based) | No (full context loaded) |
| **Dynamic Updates** | Yes (real-time) | No (requires re-caching) |
| **Multi-hop Reasoning** | Limited (chunk-based) | Excellent (unified context) |
| **Latency** | Higher (retrieval overhead) | Lower (no retrieval step) |
| **Knowledge Size Limit** | Virtually unlimited | Context window (~128K-2M tokens) |

#### Integration & Compatibility

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| Aspect | Copilot Memory Bank | Claude Code Skills | Memori | Markdown Repo | RAG | CAG |
| **Primary Tool** | GitHub Copilot | Claude Code | Any LLM | Any AI tool | Any LLM | LLMs with large context |
| **IDE Support** | VS Code, JetBrains, Xcode | Terminal/VS Code | Python apps | Universal | Custom apps | Custom apps |
| **Framework Support** | GitHub ecosystem | Anthropic ecosystem | OpenAI, Anthropic, LangChain | N/A | LangChain, LlamaIndex, Haystack | Native LLM APIs |

#### Setup & Maintenance

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| Aspect | Copilot Memory Bank | Claude Code Skills | Memori | Markdown Repo | RAG | CAG |
| **Setup Time** | 15-30 minutes | 30-60 minutes | 1-2 hours | 15 minutes | 2-8 hours | 1-2 hours |
| **Infrastructure** | None (files only) | None (files only) | SQL Database | None (files only) | Vector DB + Embeddings API | Large context LLM |
| **Maintenance Effort** | Low | Low | Medium | Very Low | High | Low-Medium |
| **Learning Curve** | Low | Medium | High | Very Low | High | Medium |

------------------------------------------------------------------------

### 2. Solution Overviews

#### GitHub Copilot Memory Bank

**What It Is:** A file-based system where Copilot reads `.github/copilot-instructions.md` and `memory-bank/` folder contents before every suggestion.

**Key Mechanisms:**

- Copilot reads instructions before every suggestion

- Memory bank files provide reloadable context per command

- Instructions can be scoped to specific file patterns

- Priority: Personal \> Repository \> Organization instructions

**Strengths:** Zero infrastructure, Git-native versioning, team gets context on `git pull`

**Limitations:** No semantic search, manual maintenance, limited to ~64K-128K token context

------------------------------------------------------------------------

#### Claude Code Skills

**What It Is:** Modular capabilities in `.claude/skills/` folders that Claude auto-discovers and applies based on context.

**Key Mechanisms:**

- **Auto-discovery**: Claude reads descriptions and applies relevant skills automatically

- **Model-invoked**: No explicit user trigger needed

- **Tool restrictions**: Can limit which tools a skill can use

- Three storage locations: Personal, Project, Plugin

**Strengths:** Autonomous application, includes scripts/templates, tool access control

**Limitations:** Only works with Claude Code, requires well-crafted descriptions

------------------------------------------------------------------------

#### Memori (GibsonAI)

**What It Is:** A memory engine that stores LLM conversations in SQL databases with automatic entity extraction.

**Key Mechanisms:**

- **Pre-call injection**: Retrieves relevant memories before LLM API call

- **Post-call recording**: Memory Agent extracts new information

- **Conscious Agent**: Every 6 hours, analyzes patterns and promotes memories

**Strengths:** Works with any LLM, 80-90% cost savings vs vector DBs, semantic search

**Limitations:** Requires database infrastructure, Python-only, higher setup complexity

------------------------------------------------------------------------

#### Markdown Repository

**What It Is:** A simple Git repository containing best practice prompts, templates, and guidelines.

**Key Mechanisms:**

- Manual reference by team members

- Copy-paste prompts into any AI tool

- Version controlled via Git

**Strengths:** Tool-agnostic, lowest setup effort, excellent for onboarding

**Limitations:** No automatic context injection, manual maintenance required

------------------------------------------------------------------------

#### RAG (Retrieval-Augmented Generation)

**What It Is:** A pattern that fetches relevant documents from a vector database before generating responses.

**RAG Architecture Types (2025):**

|  |  |  |
|----|----|----|
| Type | Description | Use Case |
| **Simple RAG** | Basic retrieve + generate | Small, static knowledge bases |
| **RAG with Memory** | Retains conversation history | Multi-turn conversations |
| **Self-RAG** | Self-reflective retrieval decisions | High-accuracy requirements |
| **Corrective RAG** | Validates and corrects retrieval | Critical applications |
| **GraphRAG** | Knowledge graph + vector retrieval | Complex entity relationships |
| **Adaptive RAG** | Dynamically adjusts retrieval strategy | Variable query complexity |

**Strengths:** Unlimited knowledge bases, real-time updates, works with any LLM, mature ecosystem

**Limitations:** Retrieval latency (50-500ms), chunk boundaries break context, infrastructure complexity

**Cost Breakdown:**

|                       |                         |
|-----------------------|-------------------------|
| Component             | Typical Cost            |
| Embeddings (OpenAI)   | ~\$0.0001 per 1K tokens |
| Vector DB (Pinecone)  | \$70-700/month          |
| LLM Inference         | Standard API pricing    |
| **Total for 1M docs** | ~\$100-500/month        |

------------------------------------------------------------------------

#### CAG (Cache-Augmented Generation)

**What It Is:** A pattern that pre-loads all knowledge into the LLM's context window and caches it for reuse.

**CAG vs RAG Performance (Benchmark Results):**

|                         |            |                  |               |
|-------------------------|------------|------------------|---------------|
| Metric                  | RAG (BM25) | RAG (Embeddings) | CAG           |
| **SQuAD Accuracy**      | 78.2%      | 82.5%            | **87.3%**     |
| **HotPotQA Accuracy**   | 71.4%      | 76.8%            | **84.1%**     |
| **Latency (avg)**       | 450ms      | 380ms            | **120ms**     |
| **Multi-hop Reasoning** | Poor       | Moderate         | **Excellent** |

*Source: "Don't Do RAG" (Chan et al., 2024) - ACM Web Conference 2025*

**Strengths:** 3-4x faster inference, superior multi-hop reasoning, simpler infrastructure

**Limitations:** Context window limit (~128K-2M tokens), re-caching for updates, higher initial cost

**Context Window Evolution:**

|                      |                |                    |
|----------------------|----------------|--------------------|
| Model                | Context Window | Knowledge Capacity |
| GPT-4 Turbo          | 128K tokens    | ~100K words        |
| Claude 3.5           | 200K tokens    | ~150K words        |
| Gemini 2.5 Pro       | 1M tokens      | ~750K words        |
| Gemini 2.5 (planned) | 2M tokens      | ~1.5M words        |

**When to Choose CAG over RAG:**

|                                     |                             |
|-------------------------------------|-----------------------------|
| Choose CAG When...                  | Choose RAG When...          |
| Knowledge base \< 1M tokens         | Knowledge base \> 1M tokens |
| Data changes infrequently           | Data changes frequently     |
| Low latency is critical             | Real-time updates needed    |
| Multi-hop reasoning required        | Simple fact retrieval       |
| Infrastructure simplicity preferred | Already have vector DB      |

------------------------------------------------------------------------

### 3. Team-Specific Evaluation

#### For Your Team Composition (5 SWEs + 1 AI Engineer)

##### Tool-Based Solutions

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| Criterion | Weight | Copilot Memory Bank | Claude Code Skills | Memori | Markdown Repo |
| **Ease of Adoption** | 25% | 9/10 | 7/10 | 5/10 | 10/10 |
| **Team Collaboration** | 20% | 9/10 | 8/10 | 6/10 | 9/10 |
| **Automation Potential** | 20% | 7/10 | 9/10 | 10/10 | 4/10 |
| **Maintenance Overhead** | 15% | 7/10 | 7/10 | 5/10 | 8/10 |
| **Flexibility** | 10% | 6/10 | 7/10 | 9/10 | 10/10 |
| **Advanced AI Features** | 10% | 6/10 | 8/10 | 10/10 | 3/10 |
| **Weighted Score** | 100% | **7.65** | **7.60** | **6.90** | **7.65** |

##### Architecture Patterns

|                          |        |          |          |
|--------------------------|--------|----------|----------|
| Criterion                | Weight | RAG      | CAG      |
| **Ease of Adoption**     | 25%    | 4/10     | 6/10     |
| **Team Collaboration**   | 20%    | 5/10     | 5/10     |
| **Automation Potential** | 20%    | 10/10    | 9/10     |
| **Maintenance Overhead** | 15%    | 4/10     | 7/10     |
| **Flexibility**          | 10%    | 9/10     | 6/10     |
| **Advanced AI Features** | 10%    | 10/10    | 8/10     |
| **Weighted Score**       | 100%   | **6.35** | **6.70** |

**Note**: RAG and CAG are infrastructure-level patterns typically implemented by the AI Engineer for custom applications, not daily tools for SWEs.

#### Role-Based Recommendations

|  |  |  |  |  |
|----|----|----|----|----|
| Role | Primary Solution | Secondary Solution | Advanced (Custom Apps) | Rationale |
| **Software Engineers (5)** | Copilot Memory Bank | Markdown Repo | N/A | Low friction, immediate productivity gains |
| **AI-Driven Engineer (1)** | Claude Code Skills | Memori | CAG → RAG | Advanced automation, build custom knowledge systems |
| **Team Lead** | Markdown Repo | Copilot Memory Bank | N/A | Standards documentation, easy auditing |

------------------------------------------------------------------------

### 4. Implementation Scenarios

#### Scenario A: Code Review Automation

|                         |                                       |
|-------------------------|---------------------------------------|
| Solution                | Automation Level                      |
| **Copilot Memory Bank** | Semi-auto (context-aware suggestions) |
| **Claude Code Skills**  | Auto (triggered on review requests)   |
| **Memori**              | Full-auto (learns from history)       |
| **Markdown Repo**       | Manual (copy-paste)                   |
| **RAG**                 | Full-auto (semantic search)           |
| **CAG**                 | Full-auto (unified context)           |

#### Scenario B: Onboarding New Team Members

|                         |                      |
|-------------------------|----------------------|
| Solution                | Effectiveness        |
| **Copilot Memory Bank** | High (immediate)     |
| **Claude Code Skills**  | High (gradual)       |
| **Memori**              | Low (needs ramp-up)  |
| **Markdown Repo**       | Medium (self-study)  |
| **RAG**                 | High (on-demand)     |
| **CAG**                 | High (comprehensive) |

#### Scenario C: Cross-Project Knowledge Sharing

|                         |                           |
|-------------------------|---------------------------|
| Solution                | Scalability               |
| **Copilot Memory Bank** | Good                      |
| **Claude Code Skills**  | Good                      |
| **Memori**              | Excellent                 |
| **Markdown Repo**       | Excellent                 |
| **RAG**                 | Excellent                 |
| **CAG**                 | Moderate (context limits) |

#### Scenario D: Internal Documentation Q&A Bot

|                         |                       |
|-------------------------|-----------------------|
| Solution                | Suitability           |
| **Copilot Memory Bank** | N/A                   |
| **Claude Code Skills**  | Good (manual)         |
| **Memori**              | Good (conversational) |
| **Markdown Repo**       | Poor                  |
| **RAG**                 | **Excellent**         |
| **CAG**                 | **Excellent**         |

#### Scenario E: Real-Time Data Integration (e.g., Jira, Confluence)

|                         |               |
|-------------------------|---------------|
| Solution                | Suitability   |
| **Copilot Memory Bank** | Poor          |
| **Claude Code Skills**  | Good          |
| **Memori**              | Moderate      |
| **Markdown Repo**       | Poor          |
| **RAG**                 | **Excellent** |
| **CAG**                 | Poor          |

------------------------------------------------------------------------

### 5. Phased Rollout Strategy

#### Phase 1: Foundation (Week 1-2)

**Implement: Markdown Repository + Copilot Memory Bank**

- AI Engineer creates base knowledge structure

- Team contributes domain-specific prompts

- Enable Copilot custom instructions across repos

#### Phase 2: Automation (Week 3-4)

**Add: Claude Code Skills**

- AI Engineer creates skills for repetitive workflows

- Test with pilot SWE before team rollout

- Iterate based on feedback

#### Phase 3: Advanced (Month 2+)

**Evaluate: Memori for Multi-Agent Systems**

Criteria for adoption:

- Building custom AI tools/agents

- Need for semantic memory search

- Multi-user AI assistant requirements

#### Phase 4: Infrastructure (Month 3+)

**Implement: CAG or RAG for Custom Knowledge Systems**

**Criteria for CAG:**

- Team documentation \< 500K tokens

- Coding standards and guidelines

- Architecture decision records

- Static best practices

**Criteria for RAG:**

- Documentation \> 2M tokens

- Confluence/Wiki integration

- Customer support knowledge

- Frequently updated content

------------------------------------------------------------------------

### 6. Maintenance & Governance

#### Update Cadence

|  |  |  |
|----|----|----|
| Solution | Recommended Frequency | Responsible Role |
| Copilot Memory Bank | Weekly (activeContext), Monthly (patterns) | Rotating SWE |
| Claude Code Skills | As needed (new workflows) | AI Engineer |
| Memori | Automatic (self-learning) | AI Engineer (monitoring) |
| Markdown Repo | Sprint-based reviews | Team Lead |
| RAG | Continuous (auto-sync) or Daily (batch) | AI Engineer |
| CAG | On knowledge change (re-cache) | AI Engineer |

#### Quality Metrics

|  |  |  |
|----|----|----|
| Metric | How to Measure | Target |
| **Adoption Rate** | Team members using KB per sprint | 80% |
| **Context Accuracy** | Relevant suggestions / Total suggestions | 70% |
| **Time Saved** | Self-reported hours saved per week | 2 hrs/person |
| **Knowledge Currency** | Last updated \< N days | \<30 days |

#### RAG/CAG Specific Metrics

|  |  |  |  |
|----|----|----|----|
| Metric | RAG Target | CAG Target | How to Measure |
| **Retrieval Precision** | 85% | N/A | Relevant chunks / Retrieved chunks |
| **Response Latency** | \<500ms | \<200ms | P95 latency monitoring |
| **Answer Accuracy** | 80% | 85% | Human evaluation sampling |
| **Cache Hit Rate** | N/A | 90% | Cache analytics |
| **Index Freshness** | \<1 hour | \<24 hours | Last update timestamp |

------------------------------------------------------------------------

### 7. Security Considerations

#### Tool-Based Solutions

|  |  |  |  |  |
|----|----|----|----|----|
| Aspect | Copilot Memory Bank | Claude Code Skills | Memori | Markdown Repo |
| **Data Location** | Git repo (your control) | Git repo (your control) | Self-hosted DB | Git repo (your control) |
| **Secrets Handling** | Never commit secrets | Never commit secrets | Encrypt sensitive fields | Never commit secrets |
| **Access Control** | Git permissions | Git permissions | DB-level ACL | Git permissions |
| **Audit Trail** | Git history | Git history | DB logs | Git history |
| **Compliance** | SOC2/GDPR (if Git compliant) | SOC2/GDPR (if Git compliant) | Self-managed | SOC2/GDPR (if Git compliant) |

#### Architecture Patterns (RAG/CAG)

|  |  |  |
|----|----|----|
| Aspect | RAG | CAG |
| **Data Location** | Vector DB (self-hosted or cloud) | LLM context (API provider) |
| **Secrets Handling** | Exclude from indexing, use metadata filters | Exclude from context loading |
| **Access Control** | Namespace isolation, API keys, RBAC | API-level authentication |
| **Audit Trail** | Query logs, retrieval logs | API request logs |
| **Compliance** | Depends on vector DB provider | Depends on LLM provider |
| **Data Residency** | Configurable (self-hosted options) | Provider-dependent |
| **PII Handling** | Anonymize before indexing | Anonymize before loading |

------------------------------------------------------------------------

### 8. Quick Reference: When to Use What

#### Tool-Based Solutions

|  |  |
|----|----|
| Use Case | Recommended Solution |
| "I want Copilot to understand our coding standards" | **Copilot Memory Bank** |
| "I want Claude to auto-apply our review process" | **Claude Code Skills** |
| "I'm building a custom AI agent that remembers users" | **Memori** |
| "I need tool-agnostic best practice prompts" | **Markdown Repository** |
| "I want the lowest setup effort" | **Markdown Repository** |
| "I want maximum automation" | **Claude Code Skills + Memori** |
| "I want the best team collaboration" | **Copilot Memory Bank** |
| "I need semantic search across knowledge" | **Memori** |

#### Architecture Patterns (RAG vs CAG)

|  |  |
|----|----|
| Use Case | Recommended Pattern |
| "I need to query a massive documentation set (\>1M tokens)" | **RAG** |
| "I need real-time data from APIs (Jira, Confluence)" | **RAG** |
| "My knowledge base changes frequently" | **RAG** |
| "I need the lowest latency responses" | **CAG** |
| "I need complex multi-hop reasoning" | **CAG** |
| "My knowledge fits in context (\<500K tokens)" | **CAG** |
| "I want the simplest infrastructure" | **CAG** |
| "I need multimodal search (images, audio)" | **RAG** |
| "I'm building a Q&A bot for static docs" | **CAG** |
| "I need hybrid: some static + some dynamic data" | **RAG + CAG Hybrid** |

### 8. Decision Matrix Template

#### Tool-Based Solutions

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| Your Priority | Weight (1-10) | Copilot MB | Claude Skills | Memori | Markdown |
| Low setup effort | \_\_\_ | 9 | 7 | 5 | 10 |
| Team adoption | \_\_\_ | 9 | 7 | 5 | 9 |
| Automation level | \_\_\_ | 7 | 9 | 10 | 4 |
| Tool flexibility | \_\_\_ | 6 | 6 | 9 | 10 |
| Advanced AI features | \_\_\_ | 6 | 8 | 10 | 3 |
| **Your Weighted Score** |  |  |  |  |  |

#### Architecture Patterns (for Custom Applications)

|                           |               |     |     |
|---------------------------|---------------|-----|-----|
| Your Priority             | Weight (1-10) | RAG | CAG |
| Low setup effort          | \_\_\_        | 4   | 7   |
| Scalability (large KB)    | \_\_\_        | 10  | 4   |
| Response latency          | \_\_\_        | 5   | 9   |
| Real-time updates         | \_\_\_        | 10  | 3   |
| Multi-hop reasoning       | \_\_\_        | 5   | 10  |
| Infrastructure simplicity | \_\_\_        | 3   | 8   |
| Cost efficiency           | \_\_\_        | 6   | 8   |
| **Your Weighted Score**   |               |     |     |

------------------------------------------------------------------------

### 9. Resources & References

#### GitHub Copilot Memory Bank

- [GitHub Docs: Custom Instructions](https://docs.github.com/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot)

- [Awesome Copilot Repository](https://github.com/github/awesome-copilot)

- [MemoriPilot Extension](https://github.com/Deltaidiots/memoripilot)

#### Claude Code Skills

- [Official Skills Documentation](https://code.claude.com/docs/en/skills.md)

#### Memori

- [GitHub Repository](https://github.com/GibsonAI/Memori)

- [Official Documentation](https://memorilabs.ai/docs)

#### RAG (Retrieval-Augmented Generation)

- [The 2025 Guide to RAG (Eden AI)](https://www.edenai.co/post/the-2025-guide-to-retrieval-augmented-generation-rag)

- [8 RAG Architectures You Should Know (Humanloop)](https://humanloop.com/blog/rag-architectures)

- [LangChain Documentation](https://python.langchain.com/docs/tutorials/rag/)

#### CAG (Cache-Augmented Generation)

- [Don't Do RAG: When CAG is All You Need (arXiv)](https://arxiv.org/html/2412.15605v1)

- [CAG vs RAG Comparison (Analytics Vidhya)](https://www.analyticsvidhya.com/blog/2025/03/cache-augmented-generation-cag/)

- [Beyond RAG: How CAG Reduces Latency (VentureBeat)](https://venturebeat.com/ai/beyond-rag-how-cache-augmented-generation-reduces-latency-complexity-for-smaller-workloads)

## Attachments

*Attached to the Confluence page but not embedded in its body.*

- [[3 Resources/Confluence/Team Kepler/Developer note/AI Research/MCP/attachments/knowledge-base-solutions-comparison-guide/knowledge-base.md|knowledge-base.md]]
