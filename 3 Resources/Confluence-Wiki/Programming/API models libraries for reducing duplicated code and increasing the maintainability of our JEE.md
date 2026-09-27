---
title: "API models libraries for reducing duplicated code and increasing the maintainability of our JEE modules"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20456915762/API+models+libraries+for+reducing+duplicated+code+and+increasing+the+maintainability+of+our+JEE+modules
space: "LUZ"
topic: programming
relevance: 0.798
depth: 2.92
updated: 2017-11-29
attachments: 10
tags:
  - confluence
  - programming
  - space/luz
---

# API models libraries for reducing duplicated code and increasing the maintainability of our JEE modules

> [!info] Imported from Confluence
> Space **LUZ** · updated 2017-11-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20456915762/API+models+libraries+for+reducing+duplicated+code+and+increasing+the+maintainability+of+our+JEE+modules)
> Relevance 0.798 · topic `programming`

Related to  <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20456915762_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="AIPROD-125" macro-id="c4eacf87-040d-4a8b-b79a-4489e3899d13" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/AIPROD-125" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>AIPROD-125</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## Problem statement

Recently we had to introduce a new field in the Person table (external origin reference for the CONOS reference object id). This new field information is coming from the GUI and can come from 2 places:

- A new Employee created through the following chain xent-rest → luz_compensation → luz_person → database
- A new Customer created through the following chain: luzfin_finance → luz_person → database

At first one would have expected to have a very simple change : a new field in the luz_person JPA Entity, a new field in the Person DTO Model and a migration script for the database.

It turned to be a mess because:

- each module implied in the process use its own model classes
- each module have more or less complex solutions for merging DTO model Classes into their Entity JPA  classes (from utility methods to Annotation)

## Solution design

Mainly 3 solutions were discussed:

- used of a common model for all the modules. This solution has been rejected.
- used of JSON parsing only. This solution has been rejected because it would imply too many changes as we use the DTO pattern.
- Migrating to latest Wildfly for profiting from a newest Jackson library version?
- each modules uses its own model api library which can be used by other modules: This is the solution that has been proposed.  
  

![[20456915762-modules.png]]



## luz_person_api as PoC

A luz_person_api module (type jar) has been created. It holds the luz_person model classes and some necessary validation and helper classes: <a href="https://bitbucket.org/axonivy-prod/luz_person_api" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_person_api</a>


![[20456915762-image2017-11-1_15-24-37.png]]



A luz_person branch has been created: <a href="https://bitbucket.org/axonivy-prod/luz_person/branch/AIPROD-125-api-modules-poc" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_person/branch/AIPROD-125-api-modules-poc</a> It is dependent on the luz_person_api and has no model classes anymore. It gets these classes from the luz_person_api module:


![[20456915762-image2017-11-1_11-30-12.png]]



A luzfin_finance branch has been created: <a href="https://bitbucket.org/axonivy-prod/luzfin_finance/branch/AIPROD-125-api-modules-poc" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luzfin_finance/branch/AIPROD-125-api-modules-poc</a> It is dependent on the luz_person_api and has no model classes related to person in the ordermanagement anymore. It gets these classes from the luz_person_api module:


![[20456915762-image2017-11-1_15-27-49.png]]



In luz_finace, there were only some refactoring needed (in src and test classes) + adding the com.axonivy.person:luz_person_api dependency to the ITs Deployment (Tests source: com.axonivy.finance.rest.Deployment)

## Impediments and problems found in luz_person

In luz_person the way the DTO is Mapped to the Entity counterpart is made with the help of some Annotations and Translators which make some DTO classes dependent of the Entity Model. Like this:

  

![[20456915762-image2017-11-1_15-58-51.png]]

  
  
So it is very difficult to extract the model to an external module.   
For solving this problem, the mapping origin has been transfered to the Entity Side.   
  

![[20456915762-image2017-11-1_16-11-38.png]]

  
  
This had also several other consequences which were driven by some ITs failures :  

- In the CompanyService and PersonService, the way the BeanCopier is called has been also changed. For example in the CompanyService add method:  
  

![[20456915762-image2017-11-10_15-8-23.png]]

  
  The BeanCopier.out method gets the declared Fields from the source (here the DTO) and the BeanCopier.in method gets these fields fro the destination (Entity class). So we need to take the fields from the Entity in that case. 
- In the luz_person_api there was the need of creating a new LocalDateTimeToDateTranslator beside the existing DateToLocalDateTimeTranslator, for the reason that we also reverse the way some DTOs and Entites are mapped to each others.
- The Email, Phone and Address Translators in luz_person have also be chaneg in the direction of the translation:  
  

![[20456915762-image2017-11-10_15-21-55.png]]
