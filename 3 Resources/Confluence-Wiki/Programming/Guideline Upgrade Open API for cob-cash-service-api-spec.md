---
title: "Guideline Upgrade Open API for cob-cash-service-api-spec"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/47803367724/Guideline+Upgrade+Open+API+for+cob-cash-service-api-spec
space: "Arrow"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2024-05-07
attachments: 7
tags:
  - confluence
  - programming
  - space/arrow
---

# Guideline Upgrade Open API for cob-cash-service-api-spec

> [!info] Imported from Confluence
> Space **Arrow** · updated 2024-05-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/47803367724/Guideline+Upgrade+Open+API+for+cob-cash-service-api-spec)
> Relevance 0.738 · topic `programming`

## Github repo:

<a href="https://scm.axonfintech.io/cob/cob-cash-service-api-spec.git" class="external-link" rel="nofollow">https://scm.axonfintech.io/cob/cob-cash-service-api-spec.git</a>

**Clone source from git by:**

`git clone https://scm.axonfintech.io/cob/cob-cash-service-api-spec.git`

Repo struct:


![[47803367724-image-20240507-022228.png]]



** **

**Upgrade at the local machine:**

- **Prerequisites**:

  - Need devtools (to run cb command),

  - Java 11 is set default in “cb” (cb --install java 11 –default)

- **Steps**:

  - Change these dependencies “javax -\> jarkarta”.  

    

![[47803367724-image-20240507-070922.png]]



  - Run command  “cb” to build the project and upgrade Open API automatically.

    

![[47803367724-image-20240507-032314.png]]



- **Check result of upgrading:**

  - Version of Open API

    

![[47803367724-image-20240507-032425.png]]



  - Version of Gradle  

    

![[47803367724-image-20240507-032447.png]]
