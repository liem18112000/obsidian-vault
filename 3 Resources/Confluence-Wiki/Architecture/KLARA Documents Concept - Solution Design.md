---
ai_hash: b611e9594c74e689
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 27
depth: 2.93
entities: []
relevance: 0.87
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525663582/KLARA+Documents+Concept+-+Solution+Design
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: KLARA Documents Concept - Solution Design
topic: architecture
type: source
updated: 2021-03-21
---

# KLARA Documents Concept - Solution Design

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-03-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525663582/KLARA+Documents+Concept+-+Solution+Design)
> Relevance 0.87 · topic `architecture`

<div class="plugin-tabmeta-details conf-macro output-block" hasbody="true" macro-id="41a977e4-2157-431f-acbf-3c5ef754f433" macro-name="details">

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td>Author / Lead</td>
<td><div class="content-wrapper">
<p><a href="https://axonivy.atlassian.net/wiki/people/60095218dfb0c70069351dd9?ref=confluence" class="confluence-userlink user-mention" data-account-id="60095218dfb0c70069351dd9" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Laurent Salamin</a><br />
<a href="https://axonivy.atlassian.net/wiki/people/557058:077937e9-545a-4d59-abd2-435d99193914?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:077937e9-545a-4d59-abd2-435d99193914" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Daniel Gauch</a></p>
</div></td>
</tr>
<tr>
<td>Creation Date</td>
<td><div class="content-wrapper">
<p>05 Nov 2020 </p>
</div></td>
</tr>
<tr>
<td>Status</td>
<td><div class="content-wrapper">
<p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="05dfd723-2232-4053-90ef-30a68be3c558" data-macro-name="status">DRAFT</span></p>
</div></td>
</tr>
<tr>
<td>Reviewers</td>
<td><div class="content-wrapper">
<p><a href="https://axonivy.atlassian.net/wiki/people/557058:3e761865-5eb3-4f59-a6df-32c4d45efd08?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:3e761865-5eb3-4f59-a6df-32c4d45efd08" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Renato Stalder</a><br />
<a href="https://axonivy.atlassian.net/wiki/people/557058:45079719-85c5-4946-a074-88c2de9ba725?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:45079719-85c5-4946-a074-88c2de9ba725" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Tobias Hofer</a><br />
<a href="https://axonivy.atlassian.net/wiki/people/61090524ae72b2006f51a790?ref=confluence" class="confluence-userlink user-mention" data-account-id="61090524ae72b2006f51a790" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Mirco Calzolari</a><br />
<a href="https://axonivy.atlassian.net/wiki/people/557058:ffde665f-1319-40c8-bacb-b489d7ca5883?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:ffde665f-1319-40c8-bacb-b489d7ca5883" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Jürg Schmocker (Unlicensed)</a></p>
</div></td>
</tr>
<tr>
<td>Related Epic(s)</td>
<td><ul>
<li>Epic 1 : Document storage</li>
<li>Epic 2 : Document indexing</li>
<li>Epic 3 : Base parameters and filing structure</li>
</ul></td>
</tr>
<tr>
<td>Related Documentation</td>
<td><ul>
<li><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519720611/KLARA+tenants">KLARA tenants</a></li>
<li><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519720815/KLARA+digital+letterbox">KLARA digital letterbox</a></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

# Solution design

### COMMON

**<span class="legacy-color-text-blue3">Cloud native is the preferred approach. Only if there are very good reasons for not doing so can we choose a different approach.</span>**

- From a Minio perspective, as long as the client application will be keeping track of each user and automating all tasks, there is no issue to run MinIO containers on K8s that are on GCP. 

### <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">DATA OWNER, PRIVACY AND SHARING</span>

**<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">In case of data loss or hacking, the whole </span><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Klara</span><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc"> business will be dramatically negatively </span><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">impacted. To avoid that :</span>**

- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">“Privacy by design” is the right answer</span>
- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc"><span class="inline-comment-marker" ref="8fa45438-1175-4743-9009-37ff76c71939">Physical separation or logical separation if provided by the infrastructure are the best guarantees to achieve this requirement</span></span>
- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Encryption can be a complementary feature and will address the hacking risk</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">For document indexes, encryption will require physical separation of data or encryption/decryption of sensible data based on symetric cryptography</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Encryption implies high computing processing and requires adequate infrastructure resources</span>

**<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Data owner</span>**

- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">The following entities that can own data and documents :</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Businesses, associations, public organization, etc.</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Household (e.g. Family)</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Individuals</span>
- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc"><span class="inline-comment-marker" ref="c5e39418-31ea-4caf-9786-355ce74d00b7">The the legal proprietary of data and documents must be considered as the entity that own the related data.</span></span>
- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">A business is the legal proprietary of the data and documents sent to it. Each business will therefore be implemented as a separated tenant.</span>
- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">For households, in general, the family is the legal proprietary of data and documents addressed to more than one individual of the family; that’s the case for taxes, Serafe, etc.</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Documents addressed to one single household member, can be legally owned by :</span>
    - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">The individual addressee alone</span>
    - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Some of the household members</span>
    - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">All household members</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">In practice, it’s the decision of the addressee how the family letter box is managed. The box owner</span>
    - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Decides who will get a key to open the box</span>
    - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Defines rules to determine who is allowed to open and access what</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">In the case of a divorce, when a child reach the legal majority or when a household's member leaves, proprietary of data and documents (archived documents or new incoming mail) can be assigned exclusively to the concerned household's member</span>

**<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Sharing documents</span>**

<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">When sharing documents </span><u><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">inside</span></u><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc"> a household or a business, the following cases can be encountered :</span>

  

- **<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Logical </span><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">and temporary sharing of a </span><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">document :</span>**<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc"> <span class="inline-comment-marker" ref="6492ae28-88b2-4561-affd-bcc38099c14d">for instance when an individual gives a document or a file to one of the household's member and ask him to put it back in its original place after use</span></span>
- **<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Duplication of a </span><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">document :</span>**<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc"> for instance if it is needed to annotate it</span>

<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">When sharing documents </span><u><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">outside</span></u><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc"> a household or a business :</span>

- **<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Documents </span><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">are duplicated in most of the </span><span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">cases</span>**

<span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">When a DMS is implemented :</span>

- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">Documents can be shared through a link integrating the security allowing accessing them</span>
- <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">In such cases, very often, documents are then duplicated by the addressee in order</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">To be filed in the right place according to the addressee’s organization</span>
  - <span class="inline-comment-marker" ref="00ebc165-a215-40eb-9d25-95b33f1001dc">To be sure that the documents will still be accessible if the link is deleted or the authorization revoked</span>

### **Proposed design :**

- **One "Tenant" is created for each legal entity (Company or Individual) which is the owner of :**
  - **Data and documents**
  - **Households**
- **A tenant can create as many additional "Letter box" as he needs**
- **By default, one "Letter box" per household which is identified by**
  1.  **A housing address and a legal entity**
  2.  **A housing address and multiple legal entity e.g multiple persons, multiple companies (consortium)**
- **This means that at the same housing address, one or multiple "Letter boxes" can be set up**
- **"Letter box" administrator can then :**
  - **Setup additional "Letter boxes"**
  - **Create additional users**
  - **Define subsequent "Letter boxes" administrators**
  - **Duplicate or move documents from the letter box he owns to another he has created**
  - **Define logical access rules to setup which user can perform the following actions on the mail :**
    - Access documents
    - Dispatch documents
    - Process documents
    - Duplicate documents
- **One physically separated repository (incoming mail and document's archive) per "Tenant"**
- **Sharing between users of a "Tenat" is implemented through links and access rights**
- **Sharing between users of different "Tenants" is implemented in one of the following ways :**
  - **Sending a link integrating the security allowing accessing to the shared documents**
  - **Duplicating the shared documents and send them to one or multiple adressees of other "Letter boxes" ==\> Simplier, thus it is the preferred solution**

  


![[20525663582-Tenants and Users structure.png]]



  

## **<u>Software validation test definition’s content :</u>**

Each software validation test must be defined in details and must contain the following chapters :

- Basic conditions of the test and environment
- Test’s variable parameters
- Workloads
- Measures
- Subsequent validations

%% ai-graph-start %%

**Related notes:**
- [[KLARA Documents solution implementation]]
- [[Architecture]]
- [[Business concept for frontend]]
- [[Proof of concept Export and download storage]]
- [[Postgres Architecture Blueprint V2023]]

%% ai-graph-end %%