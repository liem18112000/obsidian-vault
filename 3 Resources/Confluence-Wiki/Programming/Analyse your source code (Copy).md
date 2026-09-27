---
title: "Analyse your source code (Copy)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/30642747633/Analyse+your+source+code+Copy
space: "TK"
topic: programming
relevance: 0.898
depth: 3
updated: 2019-07-15
attachments: 0
tags:
  - confluence
  - programming
  - space/tk
---

# Analyse your source code (Copy)

> [!info] Imported from Confluence
> Space **TK** · updated 2019-07-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/30642747633/Analyse+your+source+code+Copy)
> Relevance 0.898 · topic `programming`

- Download *sonar-project.property* file: [sonar-project.properties  
    
  ](https://axonivy.atlassian.net/wiki/download/attachments/30642747601/sonar-project.properties?version=1&modificationDate=1563198346000&cacheVersion=1&api=v2)

- Copy this file to your project root dir. E.g: C:\ws\desk_individual\\  
    

- This file has been configured for IVY project and compatible with our SonarQube.   
  But there are some parameter need to be configured by yourself

  - **sonar.projectKey**=change_it_with_your_pom\_\<groupId\>:\<artifactId\> (e.g: ch.axonivy.desk:desk_individual)

  - **sonar.projectName**=change_it_with_your_pom\_\<artifactId\> (e.g: desk_individual)

  - **sonar.projectVersion**=change_it_with_your_pom\_\<version\>

  - **sonar.links.scm**=link_to_project_trunk (e.g: <a href="mailto:git@bitbucket.org" class="external-link" rel="nofollow">git@bitbucket.org</a>:axonivy-prod/desk_individual.git)

  - **sonar.scm.provider**=git  
      

- Save and close  
    

- run **mvn install **for all projects  
    

- Open a CMD at *your project root dir  
    *

- type **sonar-scanner.bat  
    **

- waiting for your code to be analyzed until it done  
    

- access localhost:9000/sonar to see the report
