---
ai_hash: ddb7c3756c77fbcb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.29
entities: []
relevance: 0.701
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2507907755/Global+model+evaluation
space: AI
status: reference
tags:
- confluence
- ai-ml
- space/ai
title: Global model evaluation
topic: ai_ml
type: source
updated: 2020-02-12
---

# Global model evaluation

> [!info] Imported from Confluence
> Space **AI** · updated 2020-02-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2507907755/Global+model+evaluation)
> Relevance 0.701 · topic `ai_ml`

Model: Neural Network

  

## Version 1

vocabulary size = 500 (high quality base)

<div>

|                |               |            |         |        |        |        |
|----------------|---------------|------------|---------|--------|--------|--------|
| **BCT**        | **Precision** | **Recall** | **F05** | **TP** | **FP** | **FN** |
| **BCTG_OTHER** | 71%           | 70%        | 0.71    | 3454   | 1380   | 1483   |
| **BCTG_60**    | 60%           | 62%        | 0.61    | 2256   | 1483   | 1380   |

</div>

## Version 2

vocabulary size = 2000 (high quality base)

<div>

|                |               |            |         |        |        |        |
|----------------|---------------|------------|---------|--------|--------|--------|
| **BCT**        | **Precision** | **Recall** | **F05** | **TP** | **FP** | **FN** |
| **BCTG_OTHER** | 80%           | 77%        | 0.79    | 3824   | 984    | 1113   |
| **BCTG_60**    | 70%           | 73%        | 0.71    | 2652   | 1113   | 984    |

</div>

## Version 3

vocabulary size = 2000 (all quality base)

<div>

|                |               |            |         |        |        |        |
|----------------|---------------|------------|---------|--------|--------|--------|
| **BCT**        | **Precision** | **Recall** | **F05** | **TP** | **FP** | **FN** |
| **BCTG_OTHER** | 80%           | 77%        | 0.79    | 3826   | 965    | 1111   |
| **BCTG_60**    | 71%           | 73%        | 0.71    | 2671   | 1111   | 965    |

</div>

## Version 4

vocabulary size = 2000 (all quality base) - epoch optimize

<div>

|                |               |            |         |        |        |        |
|----------------|---------------|------------|---------|--------|--------|--------|
| **BCT**        | **Precision** | **Recall** | **F05** | **TP** | **FP** | **FN** |
| **BCTG_OTHER** | 81%           | 78%        | 0.80    | 3835   | 899    | 1102   |
| **BCTG_60**    | 71%           | 75%        | 0.72    | 2737   | 1102   | 899    |

</div>

%% ai-graph-start %%

**Related notes:**
- [[RAE Parser Quality Report 2024-05-03]]
- [[RAE Parser Quality Report 2024-06-06]]
- [[RAE Parser Quality Report 2024-07-08]]

%% ai-graph-end %%