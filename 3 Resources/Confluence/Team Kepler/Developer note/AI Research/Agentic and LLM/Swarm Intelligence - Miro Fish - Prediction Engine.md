---
ai_hash: 07d8578ba375abf9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49259839512'
confluence_path: Team Kepler > Developer note > AI Research > Agentic and LLM
created: 2026-03-23
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
- search
title: 'Swarm Intelligence: Miro Fish - Prediction Engine'
type: source
updated: 2026-03-23
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259839512/Swarm+Intelligence+Miro+Fish+-+Prediction+Engine
---

# Swarm Intelligence: Miro Fish - Prediction Engine

*Confluence source · Team Kepler › Developer note › AI Research › Agentic and LLM · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259839512/Swarm+Intelligence+Miro+Fish+-+Prediction+Engine) · updated 2026-03-23*

------------------------------------------------------------------------

Before you can jump into this page, please have a foundation about:

- Swarm Intelligence: [https://axonivy.atlassian.net/wiki/x/BwDXdws](https://axonivy.atlassian.net/wiki/x/BwDXdws)

- Muti-agentic Architecture: (Coming soon)

- Python 3.10.x

## The Practical: MiroFish

### What It Is

![[3 Resources/Confluence/Team Kepler/Developer note/AI Research/Agentic and LLM/attachments/swarm-intelligence-miro-fish-prediction-engine/04_mirofish_architecture.png]]

- Reference: [https://github.com/666ghj/MiroFish/blob/main/README-EN.md](https://github.com/666ghj/MiroFish/blob/main/README-EN.md)

- MiroFish is a next-generation AI prediction engine powered by multi-agent technology.

- By extracting seed information from the real world (such as breaking news, policy drafts, or financial signals), it automatically constructs a high-fidelity parallel digital world.

- Within this space, thousands of intelligent agents with independent personalities, long-term memory, and behavioral logic freely interact and undergo social evolution.

### How it work

![[3 Resources/Confluence/Team Kepler/Developer note/AI Research/Agentic and LLM/attachments/swarm-intelligence-miro-fish-prediction-engine/03_classical_si_to_mirofish_mapping.png]]

![[3 Resources/Confluence/Team Kepler/Developer note/AI Research/Agentic and LLM/attachments/swarm-intelligence-miro-fish-prediction-engine/01_mirofish_pipeline-20260320-020333.png]]

### Real-World Demonstrations

The project has showcased several demos: predicting public opinion dynamics around trending events (the Wuhan University incident), and notably, feeding the first 80 chapters of the classic Chinese novel *Dream of the Red Chamber* into MiroFish to predict its lost ending — a creative application that shows the engine isn't limited to news/financial scenarios.

Link demo: [mirofish-live-demo](https://666ghj.github.io/mirofish-demo/)

### Key Limitations

The research literature on agent-based social simulation also shows that these models are highly sensitive to initial conditions and behavioral assumptions. For decision-makers, the most honest framing is that it can surface scenarios and dynamics that might otherwise be missed, rather than deliver precise probability estimates.

The system handles 50-200 agents comfortably. Larger simulations (500+) are possible but require more compute and take longer to process.

![[3 Resources/Confluence/Team Kepler/Developer note/AI Research/Agentic and LLM/attachments/swarm-intelligence-miro-fish-prediction-engine/image-20260320-020903.png]]

![[3 Resources/Confluence/Team Kepler/Developer note/AI Research/Agentic and LLM/attachments/swarm-intelligence-miro-fish-prediction-engine/image-20260320-020934.png]]

%% ai-graph-start %%

**Related notes:**
- [[Swarm Intelligence - Theories]]

%% ai-graph-end %%