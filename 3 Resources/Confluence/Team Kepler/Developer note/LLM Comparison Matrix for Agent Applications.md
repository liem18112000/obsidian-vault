---
ai_hash: b1a4516759ac7123
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48857448548'
confluence_path: Team Kepler > Developer note
created: 2025-11-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
title: LLM Comparison Matrix for Agent Applications
type: source
updated: 2025-11-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48857448548/LLM+Comparison+Matrix+for+Agent+Applications
---

# LLM Comparison Matrix for Agent Applications

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48857448548/LLM+Comparison+Matrix+for+Agent+Applications) · updated 2025-11-10*

I'll create a comprehensive comparison matrix for leading LLMs optimized for agent use-cases.

| Type | Key | Summary | Assignee | Priority | Status | Updated |
|----|----|----|----|----|----|----|
| ![[10321.png]] | [LUZ-143336](https://axonivy.atlassian.net/browse/LUZ-143336) | Evaluate and compare leading LLMs for agent use-cases | ![[48.png]] \[Kepler\] - Liem Doan | ![[minor.svg]] | Resolved | 17 Nov 2025, 02:43 |

|  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|
| Model | Context Window | Hallucination Rate | Cost (per 1M tokens) | Integration | Tool Use | Reasoning | Speed |
| **GPT-4 Turbo** | 128K | Low-Medium | \$10/\$30 | Excellent API, Wide SDK support | Native function calling | Strong | Medium |
| **GPT-4o** | 128K | Low | \$5/\$15 | Excellent API, Wide SDK support | Native function calling | Strong | Fast |
| **Claude 3.5 Sonnet** | 200K | Very Low | \$3/\$15 | Good API, Growing ecosystem | Native tool use | Very Strong | Fast |
| **Claude 3 Opus** | 200K | Very Low | \$15/\$75 | Good API, Growing ecosystem | Native tool use | Excellent | Medium |
| **Gemini 1.5 Pro** | 2M | Low-Medium | \$3.50/\$10.50 | Good API, Google integration | Native function calling | Strong | Fast |
| **Llama 3.1 405B** | 128K | Medium | Self-hosted/Variable | Open-source, Flexible | Via frameworks | Strong | Slow |
| **Llama 3.1 70B** | 128K | Medium | Self-hosted/Variable | Open-source, Flexible | Via frameworks | Good | Medium |
| **Mistral Large** | 128K | Medium | \$2/\$6 | API available | Function calling | Good | Fast |

### Key Considerations for Agent Use-Cases

- **Best for Complex Reasoning**: Claude 3 Opus, Claude 3.5 Sonnet

- **Best Cost-Performance**: Claude 3.5 Sonnet, Gemini 1.5 Pro

- **Best for Long Context**: Gemini 1.5 Pro (2M tokens)

- **Best for Privacy/Control**: Llama models (self-hosted)

- **Best Ecosystem**: GPT-4 variants

- **Hallucination Scale**: Very Low \< Low \< Low-Medium \< Medium \< High

- **Cost Format**: Input/Output pricing

This matrix reflects current market positioning as of early 2025, with emphasis on capabilities most critical for autonomous agent deployment.

%% ai-graph-start %%

**Related notes:**
- [[Local LLM choice for the test-agent workload (Ollama)]]
- [[A 2-4GB local model cannot match Sonnet 5 — plug the real API instead]]
- [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]]
- [[Knowledge Base Solutions Comparison Guide]]
- [[Skill-based Compression Techniques - Overview]]

%% ai-graph-end %%