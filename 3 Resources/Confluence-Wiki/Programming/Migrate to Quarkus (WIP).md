---
ai_hash: e6a5368d1d6577eb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.837
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47603745266/Migrate+to+Quarkus+WIP
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Migrate to Quarkus (WIP)
topic: programming
type: source
updated: 2024-01-02
---

# Migrate to Quarkus (WIP)

> [!info] Imported from Confluence
> Space **TS** · updated 2024-01-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47603745266/Migrate+to+Quarkus+WIP)
> Relevance 0.837 · topic `programming`

**Microservice to tryout:** luz_url_shortener

<a href="https://bitbucket.org/axonivy-prod/luz_url_shortener/branch/miracle/LUZ-112502/migrate-to-quarkus" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_url_shortener/branch/miracle/LUZ-112502/migrate-to-quarkus</a>

**Try out:**

### Open Rewrite: <a href="https://docs.openrewrite.org/" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.openrewrite.org/</a>

Automate the project conversion for common patterns.

**Java EE → Jakarta EE (namespace and dependency change)**

<a href="https://docs.openrewrite.org/running-recipes/popular-recipe-guides/migrate-to-jakarta-ee-10" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.openrewrite.org/running-recipes/popular-recipe-guides/migrate-to-jakarta-ee-10</a>

<div id="expander-282550635" class="expand-container conf-macro output-block" hasbody="true" macro-id="bc2f6874-4c4e-4e83-b6e5-4623d98b0ec3" macro-name="expand">

<div id="expander-control-282550635" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Java EE → Jakarta EE (namespace and dependency change)</span>

</div>

<div id="expander-content-282550635" class="expand-content expand-hidden">

Add this section to pom.xml under build/plugins

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e849cc92-4cdf-4ab1-8c49-3bc091991549" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<plugin>
      <groupId>org.openrewrite.maven</groupId>
      <artifactId>rewrite-maven-plugin</artifactId>
      <version>5.17.1</version>
      <configuration>
        <activeRecipes>
          <recipe>org.openrewrite.java.migrate.jakarta.JakartaEE10</recipe>
        </activeRecipes>
      </configuration>
      <dependencies>
        <dependency>
          <groupId>org.openrewrite.recipe</groupId>
          <artifactId>rewrite-migrate-java</artifactId>
          <version>2.5.0</version>
        </dependency>
      </dependencies>
</plugin>
```

</div>

</div>

Run this command to trigger migration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b7523f41-02b1-4794-85f1-158f7af953b6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mvn rewrite:run
```

</div>

</div>

After running the migration you can inspect the results with `git diff` (or equivalent), manually fix anything that wasn't able to be migrated automatically, and commit the results.

Afterward, remove the OpenRewrite plugin from pom.xml

</div>

</div>

**JUnit 4 → JUnit 5**

<a href="https://docs.openrewrite.org/running-recipes/popular-recipe-guides/migrate-from-junit-4-to-junit-5" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.openrewrite.org/running-recipes/popular-recipe-guides/migrate-from-junit-4-to-junit-5</a>

<div id="expander-547625526" class="expand-container conf-macro output-block" hasbody="true" macro-id="fd6268da-e3e1-4549-8b3a-fe8ba694cb3f" macro-name="expand">

<div id="expander-control-547625526" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">JUnit 4 → JUnit 5</span>

</div>

<div id="expander-content-547625526" class="expand-content expand-hidden">

Add this section to pom.xml under build/plugins

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9f570be3-8b08-4e61-9ddc-28273fc07fb0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
    <plugin>
      <groupId>org.openrewrite.maven</groupId>
      <artifactId>rewrite-maven-plugin</artifactId>
      <version>5.17.1</version>
      <configuration>
        <activeRecipes>
          <recipe>org.openrewrite.java.testing.junit5.JUnit5BestPractices</recipe>
        </activeRecipes>
      </configuration>
      <dependencies>
        <dependency>
          <groupId>org.openrewrite.recipe</groupId>
          <artifactId>rewrite-testing-frameworks</artifactId>
          <version>2.1.5</version>
        </dependency>
      </dependencies>
    </plugin>
```

</div>

</div>

Run this command to trigger migration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0283f4a8-1041-4763-85d9-25cb96c04c72" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mvn rewrite:run
```

</div>

</div>

After running the migration you can inspect the results with `git diff` (or equivalent), manually fix anything that wasn't able to be migrated automatically, and commit the results.

Afterward, remove the OpenRewrite plugin from pom.xml  
**Known Limitations**

Not every JUnit 4 feature or library has a direct JUnit 5 equivalent. In these cases, manual changes will be required after the automation has run.


![[47603745266-image-20231228-062558.png]]



</div>

</div>

**PowerMock → Raw Mockito (Mockito 4.x+)**

<a href="https://docs.openrewrite.org/recipes/java/testing/mockito/replacepowermockito" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.openrewrite.org/recipes/java/testing/mockito/replacepowermockito</a>

<div id="expander-4120957" class="expand-container conf-macro output-block" hasbody="true" macro-id="6dabfada-83c6-4cbc-967a-597fcbd3402f" macro-name="expand">

<div id="expander-control-4120957" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">PowerMock → Raw Mockito</span>

</div>

<div id="expander-content-4120957" class="expand-content expand-hidden">

Add this section to pom.xml under build/plugins

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="35c2d5dc-d12d-4921-a754-f1a42fb4d525" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<plugin>
        <groupId>org.openrewrite.maven</groupId>
        <artifactId>rewrite-maven-plugin</artifactId>
        <version>5.17.1</version>
        <configuration>
          <activeRecipes>
            <recipe>org.openrewrite.java.testing.mockito.ReplacePowerMockito</recipe>
          </activeRecipes>
        </configuration>
        <dependencies>
          <dependency>
            <groupId>org.openrewrite.recipe</groupId>
            <artifactId>rewrite-testing-frameworks</artifactId>
            <version>2.1.5</version>
          </dependency>
        </dependencies>
</plugin>
```

</div>

</div>

Run this command to trigger migration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="edc8cfa0-cc71-4736-b4e3-6718b1188401" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mvn rewrite:run
```

</div>

</div>

After running the migration you can inspect the results with `git diff` (or equivalent), manually fix anything that wasn't able to be migrated automatically, and commit the results.

Afterward, remove the OpenRewrite plugin from pom.xml  
This recipe is a compound recipe which do these following things


![[47603745266-image-20240102-012512.png]]



</div>

</div>

**Testcontainers**: <a href="https://java.testcontainers.org/" class="external-link" data-card-appearance="inline" rel="nofollow">https://java.testcontainers.org/</a>

%% ai-graph-start %%

**Related notes:**
- [[WIP Recipe How to adapt Unit Test to be able to run with Junit 5]]
- [[Recipe Quarkus.io getting started]]
- [[Architecture Overview LUZ]]
- [[Groovy scripts refactor]]
- [[Adapt luz-jsonstore to allow luz-docs to persist data as MongoDB Date via API]]

%% ai-graph-end %%