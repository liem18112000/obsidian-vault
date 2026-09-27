---
ai_hash: 5407329fe5afbdf4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 8
depth: 2.57
entities: []
relevance: 0.746
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47144928733/Problem+of+class+cast+exception
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Problem of class cast exception
topic: programming
type: source
updated: 2022-07-14
---

# Problem of class cast exception

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-07-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47144928733/Problem+of+class+cast+exception)
> Relevance 0.746 · topic `programming`

# Old structure

## Libraries

### luz_components


![[47144928733-image-20220713-042712.png]]




![[47144928733-image-20220713-042840.png]]



### luz_web


![[47144928733-image-20220713-043526.png]]



# New structure

### luz_components


![[47144928733-image-20220713-055013.png]]




![[47144928733-image-20220713-055137.png]]




![[47144928733-image-20220713-055259.png]]



### luz_web


![[47144928733-image-20220713-055440.png]]




![[47144928733-image-20220713-055551.png]]



# Solution

Mark the dependencies in pom file as provided. Then, only dependencies in .classpath file loaded

%% ai-graph-start %%

**Related notes:**
- [[luz_finance and luz_components move in lockstep SNAPSHOTs; a 'method not applicable' compile error usually means a skew]]
- [[Maven exclusion cannot fix a transitive javax bytecode dependency]]
- [[Avoid warning logs related to Java Problem on Ivy Designer]]
- [[API models libraries for reducing duplicated code and increasing the maintainability of our JEE]]
- [[Verify wildcard-to-explicit import cleanup by compiling]]

%% ai-graph-end %%