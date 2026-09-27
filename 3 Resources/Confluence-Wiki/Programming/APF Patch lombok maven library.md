---
title: "APF Patch lombok maven library"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/48763305988/APF+Patch+lombok+maven+library
space: "X4"
topic: programming
relevance: 0.852
depth: 3
updated: 2025-10-24
attachments: 1
tags:
  - confluence
  - programming
  - space/x4
---

# APF Patch lombok maven library

> [!info] Imported from Confluence
> Space **X4** · updated 2025-10-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/48763305988/APF+Patch+lombok+maven+library)
> Relevance 0.852 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="a6702f92-e592-4d61-ad21-d46b372abee1" macro-name="toc">

</div>

# Sources

## Libraries

## Download by nexus Repository

<a href="http://3.71.136.55/nexus/content/repositories/releases/org/projectlombok/" class="external-link" rel="nofollow">http://3.71.136.55/nexus/content/repositories/releases/org/projectlombok/</a>

Or the Zip

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="01738e53-28d1-4331-9c33-c01c727c5c76" macro-name="view-file"><a href="../_attachments/48763305988-lombok-maven-plugin-1.18.30.0.7z" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/48763305988/lombok-maven-plugin-1.18.30.0.7z?version=1&amp;modificationDate=1761285444814&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[48763305988-lombok-maven-plugin-1.18.30.0.7z]]

</a></span>

# Patch installation

If maven does not resolve download URL correctly for the maven plugin then it’s possible to install the library on local maven repository with following maven commands.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="32a297ce-36f9-4e3e-ab2b-dd84b0ef8bcb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mvn install:install-file -DgroupId=org.projectlombok -DartifactId=lombok-maven -Dversion=1.18.30.0 -Dpackaging=jar -Dfile=[path/to/lib]/lombok-maven-1.18.30.0.jar
mvn install:install-file -DgroupId=org.projectlombok -DartifactId=lombok-maven-plugin -Dversion=1.18.30.0 -Dpackaging=jar -Dfile=[path/to/lib]/lombok-maven-plugin-1.18.30.0.jar
```

</div>

</div>
