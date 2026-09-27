---
ai_hash: d20f1f1eb32febc7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.33
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38139200448/Static+analysis+tool+for+Android
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Static analysis tool for Android
topic: programming
type: source
updated: 2016-01-12
---

# Static analysis tool for Android

> [!info] Imported from Confluence
> Space **Helios** · updated 2016-01-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38139200448/Static+analysis+tool+for+Android)
> Relevance 0.711 · topic `programming`

To improve quality and syntax of our Android code, we setup automatic tools such as Checkstyle, Findbugs, PMD and Android Lint in Gradle and integrate to CI build.

### Checkstyle

Checkstyle is a development tool to help programmers write Java code that adheres to a coding standard. It automates the process of checking Java code to spare humans of this boring (but important) task.

**Website:** <a href="http://checkstyle.sourceforge.net/" class="external-link" rel="nofollow">http://checkstyle.sourceforge.net/</a>

#### Install:

- - In plugins dialog of Android studio, search with keyword CheckStyle

<!-- -->

- - Choose QAPlug - Checkstyle to install

#### Configuration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6d2dc7f9-0239-45f4-aaf2-7c9324be16d2" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Gradle task:**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
task checkstyle(type: Checkstyle) {
    ignoreFailures = true
    configFile file("${project.rootDir}/config/quality/checkstyle/checkstyle.xml")
    configProperties.checkstyleSuppressionsPath = file("${project.rootDir}/config/quality/checkstyle/suppressions.xml").absolutePath
    source 'src'
    include '**/*.java'
    exclude '**/gen/**'
    classpath = files()
}
```

</div>

</div>

 

- - The task will analyse our code according to checkstyle.xml and suppressions.xml files in \[root_project\]/config/quality/checkstyle folder

<!-- -->

- - The report file will be generated in \[root_project\]/app/build/reports

### Findbugs

FindBugs uses static analysis to inspect Java bytecode for occurrences of bug patterns. Findbugs basically just need the bytecode of a program to do the analysis, so it is very easy to use. It will detect common error such as wrong boolean operator. Findbugs is also able to detect error due to misunderstood of language features.

**Website:** <a href="http://findbugs.sourceforge.net/" class="external-link" rel="nofollow">http://findbugs.sourceforge.net/</a>

#### Install:

- - In plugins dialog, search with keyword Findbugs
  - Choose QAPlug - Findbugs to install

#### Configuration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f0544c7f-7a34-4453-9cfd-09e9e6f5bbbb" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Gradle task**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
task findbugs(type: FindBugs, dependsOn: assembleDebug) {
    ignoreFailures = true
    effort = "max"
    reportLevel = "high"
    excludeFilter = new File("${project.rootDir}/config/quality/findbugs/findbugs-filter.xml")
    classes = files("${project.rootDir}/app/build/intermediates/classes")
    source 'src'
    include '**/*.java'
    exclude '**/gen/**'
    reports {
        xml.enabled = false
        html.enabled = true
        xml {
            destination "$project.buildDir/reports/findbugs/findbugs.xml"
        }
        html {
            destination "$project.buildDir/reports/findbugs/findbugs.html"
        }
    }
    classpath = files()
}
```

</div>

</div>

 

- - Config file is located in \[root_project\]/config/quality/findbugs

<!-- -->

- - Report file will be generated in \[root_project\]/config/quality/findbugs folder

### PMD:

PMD is a very powerful tool which works a little bit like Findbugs, but inspect directly the source code, and not the bytecode. The goal is globally the same, find patterns which can lead to bugs using static analysis. So PMD can sometimes find bugs which Findbugs wont, and vice versa.  
**Website:** <a href="https://pmd.github.io/" class="external-link" rel="nofollow">https://pmd.github.io/</a>

#### Install:

- - In plugins dialog, search with keyword PMD
  - Choose QAPlug - PMD to install

#### Configuration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cb725a1d-29ee-4eef-b897-547de1203dbc" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Gradle task**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
task pmd(type: Pmd) {
    ignoreFailures = true
    ruleSetFiles = files("${project.rootDir}/config/quality/pmd/pmd-ruleset.xml")
    ruleSets = []
    source 'src'
    include '**/*.java'
    exclude '**/gen/**'
    reports {
        xml.enabled = false
        html.enabled = true
        xml {
            destination "$project.buildDir/reports/pmd/pmd.xml"
        }
        html {
            destination "$project.buildDir/reports/pmd/pmd.html"
        }
    }
}
```

</div>

</div>

 

- - Config file is located in \[root_project\]/config/quality/pmd

<!-- -->

- - Report file will be generated in \[root_project\]/config/quality/pmd folder

### Android Lint:

The Android lint tool is a static code analysis tool that checks your Android project source files for potential bugs and optimization improvements for correctness, security, performance, usability, accessibility, and internationalization.

#### Configuration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6f69abc4-bc7c-4e55-a497-e4d5de885b50" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Gradle config:**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
android {
    lintOptions {
        abortOnError true
        xmlReport false
        htmlReport true
        lintConfig file("${project.rootDir}/config/quality/lint/lint.xml")
        htmlOutput file("$project.buildDir/reports/lint/lint-result.html")
        xmlOutput file("$project.buildDir/reports/lint/lint-result.xml")
    }
}
```

</div>

</div>

 

- - Config file is located in \[root_project\]/config/quality/lint

<!-- -->

- - Report file will be generated in \[root_project\]/config/quality/lint folder

### Run all check:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4d4ac988-2933-4336-8a66-ebf53b69957f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gradlew check
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[A linter with ignoreFailures is reporting, not gating; ratchet it instead]]
- [[Test and code review report template]]
- [[Source Analysis]]
- [[00. Test and code review report template]]
- [[Test and code review report template.2]]

%% ai-graph-end %%