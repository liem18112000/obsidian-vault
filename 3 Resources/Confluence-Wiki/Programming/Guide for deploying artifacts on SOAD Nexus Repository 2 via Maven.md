---
ai_hash: 28e15022950a50ea
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.49
entities: []
relevance: 0.753
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/34412315274/Guide+for+deploying+artifacts+on+SOAD+Nexus+Repository+2+via+Maven
space: X4
status: reference
tags:
- confluence
- programming
- space/x4
title: Guide for deploying artifacts on SOAD Nexus Repository 2 via Maven
topic: programming
type: source
updated: 2020-03-04
---

# Guide for deploying artifacts on SOAD Nexus Repository 2 via Maven

> [!info] Imported from Confluence
> Space **X4** · updated 2020-03-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/34412315274/Guide+for+deploying+artifacts+on+SOAD+Nexus+Repository+2+via+Maven)
> Relevance 0.753 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="b7b0a82c-044f-4473-813f-f789b4733cb5" macro-name="toc">

</div>

## Preperation

Please add the IP Adress to the DNS configuration of your network adapter on your host

[/wiki/spaces/X4/pages/34384318830](https://axonivy.atlassian.net/wiki/spaces/X4/pages/34384318830)

## Maven plugins to upload the artifacts to remote repository

The access to a Nexus Repository will be possible if you add following code snippet into build configuration script in the pom.xml

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ddeb11df-b131-4ed6-9b27-32da43f4b884" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<project>
.
    <properties>
        <skip.maven.deploy.plugin>true</skip.maven.deploy.plugin>
    </properties>
.
.
   </build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-deploy-plugin</artifactId>
                <version>3.0.0-M1</version>
                <configuration>
                    <skip>${skip.maven.deploy.plugin}</skip>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.sonatype.plugins</groupId>
                <artifactId>nexus-staging-maven-plugin</artifactId>
                <version>1.5.1</version>
                <executions>
                    <execution>
                        <id>default-deploy</id>
                        <phase>deploy</phase>
                        <goals>
                            <goal>deploy</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <serverId>nexus</serverId>
                    <nexusUrl>http://www.soad.ch/nexus/</nexusUrl>
                    <skipStaging>true</skipStaging>
                </configuration>
            </plugin>
        </plugins>
   </build>
.
.
</project>
```

</div>

</div>

## Settings of the Repository URL

Following code describes the URL settings in the pom.xml file

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ecee0305-789d-4e1d-bd3b-d82fb2553141" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
        <profile>
            <id>ch_dev_soad</id>
            <repositories>
                <repository>
                    <id>mvnRepository</id>
                    <url>https://repo1.maven.org/maven2/</url>
                </repository>
                <repository>
                    <id>continuous</id>
                    <url>http://www.soad.ch/nexus/content/repositories/continuous</url>
                </repository>
                <repository>
                    <id>releases</id>
                    <url>http://www.soad.ch/nexus/content/repositories/releases</url>
                </repository>
                <repository>
                    <id>nexus-snapshots</id>
                    <url>http://www.soad.ch/nexus/content/repositories/snapshots</url>
                </repository>
            </repositories>
            <distributionManagement>
                <repository>
                    <id>nexus</id>
                    <url>http://www.soad.ch/nexus/content/repositories/releases</url>
                </repository>
                <snapshotRepository>
                    <id>nexus-snapshots</id>
                    <url>http://www.soad.ch/nexus/content/repositories/snapshots</url>
                </snapshotRepository>
            </distributionManagement>
        </profile>
```

</div>

</div>

## Settings for the Upload

Create a settings.xml file with following content and place it to the local repository

~/.m2/settings.xml

  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="200a3fe4-960a-4da5-891b-a1a8c3b43ee8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<settings>
    <servers>
        <server>
            <id>nexus</id>
            <username>${repo.login}</username>
            <password>${repo.pwd}</password>
        </server>
        <server>
            <id>nexus-snapshots</id>
            <username>${repo.login}</username>
            <password>${repo.pwd}</password>
        </server>
    </servers>
</settings>
```

</div>

</div>

  

## Deploying artifacts 

if the version of the artifact has the postfix SNAPSHOT the library will be upload automatically the nexus-snapshots repository

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="48230a7c-d162-45e0-a2b7-16eb7ad2fab9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mvn deploy -Pch_dev_soad -Drepo.login=admin -Drepo.pwd=admin123 -DskipTests=true
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[APF Patch lombok maven library]]
- [[Add Ivy jars Maven plugin]]
- [[Jenkins - deploy eportal to SE-Server by jenkins]]
- [[Migrate to Quarkus (WIP)]]
- [[Document flow setup build Jenkins job Maven]]

%% ai-graph-end %%