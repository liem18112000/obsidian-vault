---
ai_hash: c5b07fe0d5495d5e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47604531278/Add+Ivy+jars+Maven+plugin
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Add Ivy jars Maven plugin
topic: programming
type: source
updated: 2024-01-05
---

# Add Ivy jars Maven plugin

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-01-05 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47604531278/Add+Ivy+jars+Maven+plugin)
> Relevance 0.731 · topic `programming`

# Context

As build Klara’s libraries (e.g. xent_proccess_manager, xent_task_manager, etc.) for luz_webclient these libs are depending on dependencies/libs from Ivy Designer. Because these dependencies are not public so we have to have a way to copy them into our libs. This maven plugin is to support this purpose.

# How-to-use

Add this plugin into build session in the pom.xml of the project

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dc9140bf-2aa4-4d3d-a78c-67b5fb4bd47a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
  <build>
    <plugins>
      <plugin>
        <groupId>ch.klara.luz</groupId>
        <artifactId>addivyjars_maven_plugin</artifactId>
        <version>1.0.02.0</version>
        <executions>
          <execution>
            <goals>
              <goal>add-ivy-jars</goal>
            </goals>
            <configuration>
              <excludeFiles>byte-buddy</excludeFiles>
            </configuration>
          </execution>
        </executions>
      </plugin>
      ...
```

</div>

</div>

**Note**:

Remember to update `ivyVersion `and `ivy-server-path` in the `.m2/setting` to corresponding version and path

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="45bdf1da-1697-414b-af01-77d90b2c9849" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
           <properties>
                <ivyVersion>10.0.15</ivyVersion>
                <project-build-plugin-version>${ivyVersion}</project-build-plugin-version>
                <ivy.engine.version>${ivyVersion}</ivy.engine.version>
                <ivy-server-path>C:\Users\dvdat\.m2\repository\.cache\ivy\10.0.15</ivy-server-path>
            </properties>
```

</div>

</div>

**Note:** Where are the dependencies/libs in Ivy engine: \<ivy_engine_root\>/system/plugins/

# Reference

<a href="https://bitbucket.org/axonivy-prod/addivyjars_maven_plugin/src/master/" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/addivyjars_maven_plugin/src/master/</a>

%% ai-graph-start %%

**Related notes:**
- [[KlaraLuz Axon Ivy projects on master still target Ivy 10.0.15, not 12]]
- [[Architecture Overview LUZ]]
- [[KlaraLuz Maven builds resolve dependencies from Google Artifact Registry and require gcloud auth]]
- [[APF Patch lombok maven library]]
- [[WIP Recipe How to adapt Unit Test to be able to run with Junit 5]]

%% ai-graph-end %%