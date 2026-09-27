---
title: "Editor rule by Code Service (Visual Code in Browser)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48127312337/Editor+rule+by+Code+Service+Visual+Code+in+Browser
space: "LUZ"
topic: programming
relevance: 0.721
depth: 2.53
updated: 2024-11-12
attachments: 16
tags:
  - confluence
  - programming
  - space/luz
---

# Editor rule by Code Service (Visual Code in Browser)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-11-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48127312337/Editor+rule+by+Code+Service+Visual+Code+in+Browser)
> Relevance 0.721 · topic `programming`

# Introduction

I asked the Kogito community about the best tool for editing DRL rules, and they mentioned that VS Code is currently the most reliable choice. Therefore, I looked for ways to deploy VS Code in our cloud environment.

Link: <a href="https://github.com/apache/incubator-kie-tools/issues/2715" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/apache/incubator-kie-tools/issues/2715</a>


![[48127312337-image-20241031-092438.png]]



Document of Kogito also mention about this: <a href="https://docs.drools.org/8.36.0.Final/drools-docs/docs-website/drools/migration-guide/index.html#business-central_migration-guide" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.drools.org/8.36.0.Final/drools-docs/docs-website/drools/migration-guide/index.html#business-central_migration-guide</a>


![[48127312337-image-20241112-033202.png]]



# Main Interfaces

## Login (<a href="https://rules-dev.klara.tech/code-server" class="external-link" rel="nofollow">https://rules-dev.klara.tech/code-server</a>) (Temporary Link)


![[48127312337-image-20241031-101049.png]]



## **Sync source first**


![[48127312337-image-20241101-064207.png]]

![[48127312337-image-20241101-064249.png]]



## **Editing Interface**

Choice **src/main/resources/ch/klara/luz/store** for rule editing


![[48127312337-image-20241101-064407.png]]



## Push Change Interface (Rule Modification)

Choice file rule change


![[48127312337-image-20241101-064613.png]]



Type message and click commit.


![[48127312337-image-20241101-064709.png]]



Click Sync Change to push


![[48127312337-image-20241101-064747.png]]
