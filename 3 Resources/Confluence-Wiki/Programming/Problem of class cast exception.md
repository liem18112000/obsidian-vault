---
title: "Problem of class cast exception"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47144928733/Problem+of+class+cast+exception
space: "LUZ"
topic: programming
relevance: 0.746
depth: 2.57
updated: 2022-07-14
attachments: 8
tags:
  - confluence
  - programming
  - space/luz
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
