---
ai_hash: 7850da3a82b90fc6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 2.31
entities: []
relevance: 0.746
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47337638444/ETL+testing+investigation
space: TS
status: reference
tags:
- confluence
- testing
- space/ts
title: ETL testing investigation
topic: testing
type: source
updated: 2023-04-20
---

# ETL testing investigation

> [!info] Imported from Confluence
> Space **TS** · updated 2023-04-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47337638444/ETL+testing+investigation)
> Relevance 0.746 · topic `testing`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47337638444_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-96205" macro-id="cfa7b306-a8a6-4a80-8683-f7f95ac5cfb9" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-96205" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-96205</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="774e8dae-d7d2-4e78-b096-56fc77f092c4" macro-name="toc">

</div>

# I. Overview

AI team want to test AI Data Feed automatically instead of testing manually.

# II. Testing methodologies

## 1. Unit vs Integration vs System vs E2E Testing

<a href="https://microsoft.github.io/code-with-engineering-playbook/automated-testing/e2e-testing/testing-comparison/" class="external-link" data-card-appearance="inline" rel="nofollow">https://microsoft.github.io/code-with-engineering-playbook/automated-testing/e2e-testing/testing-comparison/</a>

<div>

|  |  |  |  |  |
|----|----|----|----|----|
|  | **Unit Test** | **Integration Test** | **System Testing** | **E2E Test** |
| **Scope** | Modules, APIs | Modules, interfaces | Application, system | All sub-systems, network dependencies, services and databases |
| **Size** | Tiny | Small to medium | Large | X-Large |
| **Environment** | Development | Integration test | QA test | Production like |
| **Data** | Mock data | Test data | Test data | Copy of real production data |
| **System Under Test** | Isolated unit test | Interfaces and flow data between the modules | Particular system as a whole | Application flow from start to end |
| **Scenarios** | Developer perspectives | Developers and IT Pro tester perspectives | Developer and QA tester perspectives | End-user perspectives |
| **When** | After each build | After Unit testing | Before E2E testing and after Unit and Integration testing | After System testing |
| **Automated or Manual** | Automated | Manual or automated | Manual or automated | Manual |

</div>

## 2. CDC testing

<a href="https://microsoft.github.io/code-with-engineering-playbook/automated-testing/cdc-testing/" class="external-link" data-card-appearance="inline" rel="nofollow">https://microsoft.github.io/code-with-engineering-playbook/automated-testing/cdc-testing/</a>

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">


![[47337638444-image-20230328-084808.png]]



</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">


![[47337638444-image-20230328-075424.png]]



</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

**Pros:**

- E2E tests give developers the highest confidence to release as they are testing the "real" system

**Cons:**

- E2E tests are slow

- E2E tests break easily

- E2E tests are expensive and hard to maintain

- E2E tests of larger systems may be hard or impossible to run outside a dedicated testing environment

CDC solve E2E issues by testing interactions between components in isolation using mocks that conform to a shared understanding documented in a "contract". This effectively partitions a larger system into smaller pieces that can be tested individually in isolation of each other, leading to simpler, fast and stable tests that also give confidence to release.

Some E2E tests are still required to verify the system as a whole when deployed in the real environment, but most functional interactions between components can be covered with CDC tests.


![[47337638444-image-20230328-075700.png]]



------------------------------------------------------------------------

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Tool/library,…</strong></p></th>
<th></th>
<th><p><strong>Pros</strong></p></th>
<th><p><strong>Cons</strong></p></th>
</tr>
&#10;<tr>
<td><p>Unit Test</p></td>
<td><ul>
<li><p>Mokito</p></li>
<li><p>JUnit</p></li>
</ul></td>
<td><ul>
<li></li>
</ul></td>
<td><ul>
<li><p>Quick (write &amp; run)</p></li>
<li><p>Cheap ($)</p></li>
</ul></td>
<td><ul>
<li><p>Cannot test/cover real cases</p></li>
<li><p>Cannot cover full flow</p></li>
</ul></td>
</tr>
<tr>
<td><p>Integration Test</p></td>
<td><ul>
<li><p>Arquillian</p></li>
<li><p><a href="https://www.testcontainers.org/" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.testcontainers.org/</a></p></li>
</ul></td>
<td><ul>
<li><p><a href="https://bitbucket.org/axonivy-prod/klara_dev_day_2022/src/master/testcontainers-jakarta-demo/" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/klara_dev_day_2022/src/master/testcontainers-jakarta-demo/</a></p></li>
</ul></td>
<td><ul>
<li><p>Can cover full/partially BE flow</p></li>
</ul></td>
<td><ul>
<li><p>Slow (write &amp; run)</p></li>
<li><p>More expensive than UT</p></li>
<li><p>Cannot cover FE flow or real cases</p></li>
</ul></td>
</tr>
<tr>
<td><p>Consumer-driven Contract Testing (CDC)</p></td>
<td><ul>
<li><p><a href="https://docs.pact.io/" class="external-link" rel="nofollow">Pact</a></p></li>
<li><p><del>Arquillian Algernon</del></p></li>
</ul></td>
<td><ul>
<li></li>
</ul></td>
<td><ul>
<li></li>
</ul></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td><p>Browser Automation</p></td>
<td><ul>
<li><p>Selenium</p></li>
</ul></td>
<td><ul>
<li></li>
</ul></td>
<td><ul>
<li><p>Can cover full flow</p></li>
<li><p>Cover/test (some/almost) real cases</p></li>
</ul></td>
<td><ul>
<li><p>UI changes every sprint =&gt; Update tests</p></li>
<li><p>Slow (write &amp; run)</p></li>
<li><p>Expensive</p></li>
</ul></td>
</tr>
<tr>
<td><p>API Testing</p></td>
<td><ul>
<li><p>Postman</p></li>
</ul></td>
<td><p><a href="https://microsoft.github.io/code-with-engineering-playbook/automated-testing/e2e-testing/recipes/postman-testing/" class="external-link" data-card-appearance="inline" rel="nofollow">https://microsoft.github.io/code-with-engineering-playbook/automated-testing/e2e-testing/recipes/postman-testing/</a></p></td>
<td><ul>
<li><p>Can cover full/partially BE flow</p></li>
</ul></td>
<td><ul>
<li><p>Slow (write &amp; run)</p></li>
<li><p>More expensive than UT</p></li>
<li><p>Cannot cover FE flow</p></li>
</ul></td>
</tr>
<tr>
<td><p>Others</p></td>
<td><ul>
<li><p>Python/Java</p></li>
</ul></td>
<td><ul>
<li><p>Currently, AI team write some Python scripts to test data.</p></li>
</ul></td>
<td><ul>
<li><p>Verify data (number of records,…)</p></li>
</ul></td>
<td><ul>
<li><p>Need check details manually</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Rethink and decide for testing strategy]]
- [[Testing]]
- [[Intergration test for MicroProfile OpenAPI]]
- [[00. Test and code review report template]]
- [[Test and code review report template.2.93]]

%% ai-graph-end %%