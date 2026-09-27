---
ai_hash: 1f6bad84602f0ab9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 3
entities: []
relevance: 0.89
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519727808/Analyze+N+1+queries+for+REST+API+calculate+payslip+for+1+employee
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Analyze N+1 queries for REST API calculate payslip for 1 employee
topic: programming
type: source
updated: 2021-01-08
---

# Analyze N+1 queries for REST API calculate payslip for 1 employee

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-01-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519727808/Analyze+N+1+queries+for+REST+API+calculate+payslip+for+1+employee)
> Relevance 0.89 · topic `programming`

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Ticket</th>
<th><div class="content-wrapper">
<p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20519727808_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-34099" data-macro-id="b13ee084-017e-45ba-90e0-87105b119b68" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-34099" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-34099</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p>
</div></th>
</tr>
&#10;<tr>
<td>Team</td>
<td><strong>WOW</strong></td>
</tr>
</tbody>
</table>

</div>

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="f08de71d-da47-44d1-871a-ccb8603b5800" macro-name="toc">

</div>

# **Overview of issue**

During calculate payslip for single contract, we might have some N+1 query problems. This might effect to performance of this service.

# **Service call**

<span class="legacy-color-text-blue3">When a request to calculate hits the server, the following REST APIs are called:</span>

<span class="legacy-color-text-blue4">**/luz_compensation/api/{company-tenant-id}/companies/{company-id}/contracts/{contract-id}/payslips/current-payslip?recalculate=true**</span>

## **Access logs**

The whole access log for this service call following the below logs:

<div hasbody="true" macro-id="ec2df7a2-6883-4a71-8bd3-c5db3c541269" macro-name="info">

Access logs

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

\[30/Dec/2020:11:30:34 +0700\] "GET /luz_person/api/states?ids=4 HTTP/1.1" 200 11  
\[30/Dec/2020:11:30:34 +0700\] "GET /luz_person/api/777bfa0f-eef1-4641-8041-6853d026f57f/companies?module=luz_compensation&ids=1%2C+6%2C+1 HTTP/1.1" 200 49  
\[30/Dec/2020:11:30:34 +0700\] "GET /luz_person/api/cities?ids=1113%2C+3158%2C+3121%2C+2040 HTTP/1.1" 200 15  
\[30/Dec/2020:11:30:34 +0700\] "GET /luz_person/api/states?ids=27 HTTP/1.1" 200 9  
\[30/Dec/2020:11:30:34 +0700\] "POST /luz_person/api/777bfa0f-eef1-4641-8041-6853d026f57f/persons/fetch HTTP/1.1" 200 55  
\[30/Dec/2020:11:30:34 +0700\] "GET /luz_compensation/api/777bfa0f-eef1-4641-8041-6853d026f57f/companies/1/contracts/22/payslips/current-payslip?recalculate=true HTTP/1.1" 200 956

</div>

</div>

## **Query logs**

The whole postgresql log for this service call following the attachment file <a href="../_attachments/20519727808-postgresql_log.txt" data-nice-type="Text File">postgresql_log.txt</a>

## **Analyze details and suggestion solution**

### **1. N + 1**

- **Getting salary configuration(during execute groovy for Salary Items)**

Salary configuration is loaded with list of salary item type, each salary item type will trigger another query to load itself.

Detail logs: <a href="../_attachments/20519727808-Salary_configuration_Nplus1.txt" data-nice-type="Text File">Salary_configuration_Nplus1.txt</a>

<span class="legacy-color-text-red2">=\></span> <span class="legacy-color-text-red2">Using entity graph or customize query to get salary configuration with necessary information</span>

### **2. Performance**

- **Getting company info(ContractService.getFullCompanyInfoFromContract)**

When getting company including workplaces, insurance contracts. Somehow, the service get one by one.

Query logs: <a href="../_attachments/20519727808-GettingInsurance_and_workplaces.txt" data-nice-type="Text File">GettingInsurance_and_workplaces.txt</a>

<span class="legacy-color-text-red2">=\> Get list of insurances/ workplaces at once.</span>

### **3. Other issues**

- **Get person by uris(CompanyService.getEmployeeInfo =\> PersonService.findPersonsByURIs)**

When getting person info, we will call to luz_person to get full info of employees


![[20519727808-PersonEntityGraph.PNG]]



Query log:

<div hasbody="true" macro-id="74869cce-86dc-47f7-8047-a89f76538946" macro-name="info">

Get peson by ids

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

select distinct <a href="http://personenti0_.id" class="external-link" rel="nofollow">personenti0_.id</a> as id1_18_0\_, <a href="http://emailentit2_.id" class="external-link" rel="nofollow">emailentit2_.id</a> as id1_17_1\_, <a href="http://customfiel3_.id" class="external-link" rel="nofollow">customfiel3_.id</a> as id1_22_2\_, <a href="http://addressent5_.id" class="external-link" rel="nofollow">addressent5_.id</a> as id1_4_3\_, <a href="http://cityentity6_.id" class="external-link" rel="nofollow">cityentity6_.id</a> as id1_0_4\_, <a href="http://countryent7_.id" class="external-link" rel="nofollow">countryent7_.id</a> as id1_2_5\_, <a href="http://stateentit8_.id" class="external-link" rel="nofollow">stateentit8_.id</a> as id1_3_6\_, <a href="http://communitye9_.id" class="external-link" rel="nofollow">communitye9_.id</a> as id1_1_7\_, <a href="http://phoneentit11_.id" class="external-link" rel="nofollow">phoneentit11_.id</a> as id1_27_8\_,  
<a href="http://categories12_.id" class="external-link" rel="nofollow">categories12_.id</a> as id1_21_9\_, <a href="http://onlineplat13_.id" class="external-link" rel="nofollow">onlineplat13_.id</a> as id1_24_10\_, personenti0\_.create_by as create_b2_18_0\_, personenti0\_.create_date as create_d3_18_0\_, personenti0\_.update_by as update_b4_18_0\_, personenti0\_.update_date as update_d5_18_0\_, personenti0\_.additional_name as addition6_18_0\_,  
personenti0\_.birthday as birthday7_18_0\_, personenti0\_.correspondence as correspo8_18_0\_, personenti0\_.first_name as first_na9_18_0\_, personenti0\_.image_id as image_i10_18_0\_, personenti0\_.language as languag11_18_0\_, personenti0\_.last_name as last_na12_18_0\_,  
personenti0\_.nationality as nationa13_18_0\_, personenti0\_.person_number as person_14_18_0\_, personenti0\_.origin_reference as origin_15_18_0\_, personenti0\_.place_of_origin as place_o16_18_0\_, personenti0\_.responsible_counterpart as respons17_18_0\_, personenti0\_.salutation as salutat18_18_0\_,  
personenti0\_.sex as sex19_18_0\_, personenti0\_.social_security_number as social_20_18_0\_, personenti0\_.website as website21_18_0\_, emailentit2\_.create_by as create_b2_17_1\_, emailentit2\_.create_date as create_d3_17_1\_, emailentit2\_.update_by as update_b4_17_1\_,  
emailentit2\_.update_date as update_d5_17_1\_, emailentit2\_.email_address as email_ad6_17_1\_, emailentit2\_.type as type7_17_1\_, emailentit2_1\_.person_id as person_i1_23_1\_, emailentit2_2\_.company_id as company_1_11_1\_, emaillist1\_.person_id as person_i1_23_0\_\_,  
emaillist1\_.email_id as email_id2_23_0\_\_, customfiel3\_.create_by as create_b2_22_2\_, customfiel3\_.create_date as create_d3_22_2\_, customfiel3\_.update_by as update_b4_22_2\_, customfiel3\_.update_date as update_d5_22_2\_, customfiel3\_.custom_name as custom_n6_22_2\_,  
customfiel3\_.custom_value as custom_v7_22_2\_, customfiel3\_.person_id as person_i8_22_2\_, customfiel3\_.person_id as person_i8_22_1\_\_, <a href="http://customfiel3_.id" class="external-link" rel="nofollow">customfiel3_.id</a> as id1_22_1\_\_, addressent5\_.create_by as create_b2_4_3\_, addressent5\_.create_date as create_d3_4_3\_, addressent5\_.update_by as update_b4_4_3\_,  
addressent5\_.update_date as update_d5_4_3\_, addressent5\_.valid_from as valid_fr6_4_3\_, addressent5\_.valid_to as valid_to7_4_3\_, addressent5\_.additional_address as addition8_4_3\_, addressent5\_.address_lines as address_9_4_3\_, addressent5\_.address_type as address10_4_3\_,  
addressent5\_.city_id as city_id20_4_3\_, addressent5\_.company_name as company11_4_3\_, addressent5\_.definition_name as definit12_4_3\_, addressent5\_.first_name as first_n13_4_3\_, addressent5\_.geo_hash as geo_has14_4_3\_, addressent5\_.last_name as last_na15_4_3\_,  
addressent5\_.latitude as latitud16_4_3\_, addressent5\_.longitude as longitu17_4_3\_, addressent5\_.salutation as salutat18_4_3\_, addressent5\_.type as type19_4_3\_, addressent5_1\_.person_id as person_i1_20_3\_, addressent5_2\_.company_id as company_1_8_3\_,  
addresslis4\_.person_id as person_i1_20_2\_\_, addresslis4\_.address_id as address_2_20_2\_\_, cityentity6\_.create_by as create_b2_0_4\_, cityentity6\_.create_date as create_d3_0_4\_, cityentity6\_.update_by as update_b4_0_4\_, cityentity6\_.update_date as update_d5_0_4\_,  
cityentity6\_.additional_postcode as addition6_0_4\_, cityentity6\_.address_postcode as address_7_0_4\_, cityentity6\_.alternative_language as alternat8_0_4\_, cityentity6\_.basic_postcode as basic_po9_0_4\_, cityentity6\_.city_name_18 as city_na10_0_4\_, cityentity6\_.city_name_27 as city_na11_0_4\_,  
cityentity6\_.community_id as communi19_0_4\_, cityentity6\_.country_id as country20_0_4\_, cityentity6\_.delivery_office as deliver12_0_4\_, cityentity6\_.delivery_office_postcode as deliver13_0_4\_, cityentity6\_.language_code as languag14_0_4\_, cityentity6\_.onrp as onrp15_0_4\_,  
cityentity6\_.plz_coff as plz_cof16_0_4\_, cityentity6\_.postcode_type as postcod17_0_4\_, cityentity6\_.state_id as state_i21_0_4\_, cityentity6\_.validity as validit18_0_4\_, countryent7\_.create_by as create_b2_2_5\_, countryent7\_.create_date as create_d3_2_5\_,  
countryent7\_.update_by as update_b4_2_5\_, countryent7\_.update_date as update_d5_2_5\_, countryent7\_.country_name as country_6_2_5\_, countryent7\_.iso_2code as iso_7_2_5\_, countryent7\_.iso_3code as iso_8_2_5\_, countryent7\_.numeric_code as numeric_9_2_5\_,  
countryent7\_.phone_code as phone_c10_2_5\_, stateentit8\_.create_by as create_b2_3_6\_, stateentit8\_.create_date as create_d3_3_6\_, stateentit8\_.update_by as update_b4_3_6\_, stateentit8\_.update_date as update_d5_3_6\_, stateentit8\_.code as code6_3_6\_,  
stateentit8\_.description as descript7_3_6\_, communitye9\_.create_by as create_b2_1_7\_, communitye9\_.create_date as create_d3_1_7\_, communitye9\_.update_by as update_b4_1_7\_, communitye9\_.update_date as update_d5_1_7\_, communitye9\_.bfsnr as bfsnr6_1_7\_,  
communitye9\_.community_name as communit7_1_7\_, communitye9\_.conurbation_number as conurbat8_1_7\_, communitye9\_.state_id as state_id9_1_7\_, phoneentit11\_.create_by as create_b2_27_8\_, phoneentit11\_.create_date as create_d3_27_8\_, phoneentit11\_.update_by as update_b4_27_8\_,  
phoneentit11\_.update_date as update_d5_27_8\_, phoneentit11\_.phone_number as phone_nu6_27_8\_, phoneentit11\_.type as type7_27_8\_, phoneentit11_1\_.company_id as company_1_13_8\_, phoneentit11_2\_.person_id as person_i1_25_8\_, phonelist10\_.person_id as person_i1_25_3\_\_,  
phonelist10\_.phone_id as phone_id2_25_3\_\_, categories12\_.create_by as create_b2_21_9\_, categories12\_.create_date as create_d3_21_9\_, categories12\_.update_by as update_b4_21_9\_, categories12\_.update_date as update_d5_21_9\_, categories12\_.category as category6_21_9\_,  
categories12\_.person_id as person_i7_21_9\_, categories12\_.person_id as person_i7_21_4\_\_, <a href="http://categories12_.id" class="external-link" rel="nofollow">categories12_.id</a> as id1_21_4\_\_, onlineplat13\_.create_by as create_b2_24_10\_, onlineplat13\_.create_date as create_d3_24_10\_, onlineplat13\_.update_by as update_b4_24_10\_,  
onlineplat13\_.update_date as update_d5_24_10\_, onlineplat13\_.platform_name as platform6_24_10\_, onlineplat13\_.platform_value as platform7_24_10\_, onlineplat13\_.person_id as person_i8_24_10\_, onlineplat13\_.person_id as person_i8_24_5\_\_,  
<a href="http://onlineplat13_.id" class="external-link" rel="nofollow">onlineplat13_.id</a> as id1_24_5\_\_ from person personenti0\_ left outer join person_email emaillist1\_ on <a href="http://personenti0_.id" class="external-link" rel="nofollow">personenti0_.id</a>=emaillist1\_.person_id left outer join email emailentit2\_ on emaillist1\_.email_id=<a href="http://emailentit2_.id" class="external-link" rel="nofollow">emailentit2_.id</a> left outer join person_email emailentit2_1\_ on <a href="http://emailentit2_.id" class="external-link" rel="nofollow">emailentit2_.id</a>=emailentit2_1\_.email_id  
left outer join company_email emailentit2_2\_ on <a href="http://emailentit2_.id" class="external-link" rel="nofollow">emailentit2_.id</a>=emailentit2_2\_.email_id left outer join person_custom_field customfiel3\_ on <a href="http://personenti0_.id" class="external-link" rel="nofollow">personenti0_.id</a>=customfiel3\_.person_id left outer join person_address addresslis4\_ on <a href="http://personenti0_.id" class="external-link" rel="nofollow">personenti0_.id</a>=addresslis4\_.person_id  
left outer join address addressent5\_ on addresslis4\_.address_id=<a href="http://addressent5_.id" class="external-link" rel="nofollow">addressent5_.id</a> left outer join person_address addressent5_1\_ on <a href="http://addressent5_.id" class="external-link" rel="nofollow">addressent5_.id</a>=addressent5_1\_.address_id left outer join company_address addressent5_2\_ on <a href="http://addressent5_.id" class="external-link" rel="nofollow">addressent5_.id</a>=addressent5_2\_.address_id  
left outer join public.city cityentity6\_ on addressent5\_.city_id=<a href="http://cityentity6_.id" class="external-link" rel="nofollow">cityentity6_.id</a> left outer join public.country countryent7\_ on cityentity6\_.country_id=<a href="http://countryent7_.id" class="external-link" rel="nofollow">countryent7_.id</a> left outer join public.state stateentit8\_ on cityentity6\_.state_id=<a href="http://stateentit8_.id" class="external-link" rel="nofollow">stateentit8_.id</a>  
left outer join public.community communitye9\_ on cityentity6\_.community_id=<a href="http://communitye9_.id" class="external-link" rel="nofollow">communitye9_.id</a> left outer join person_phone phonelist10\_ on <a href="http://personenti0_.id" class="external-link" rel="nofollow">personenti0_.id</a>=phonelist10\_.person_id left outer join phone phoneentit11\_ on phonelist10\_.phone_id=<a href="http://phoneentit11_.id" class="external-link" rel="nofollow">phoneentit11_.id</a>  
left outer join company_phone phoneentit11_1\_ on <a href="http://phoneentit11_.id" class="external-link" rel="nofollow">phoneentit11_.id</a>=phoneentit11_1\_.phone_id left outer join person_phone phoneentit11_2\_ on <a href="http://phoneentit11_.id" class="external-link" rel="nofollow">phoneentit11_.id</a>=phoneentit11_2\_.phone_id left outer join person_category categories12\_ on <a href="http://personenti0_.id" class="external-link" rel="nofollow">personenti0_.id</a>=categories12\_.person_id  
left outer join person_online_platform onlineplat13\_ on <a href="http://personenti0_.id" class="external-link" rel="nofollow">personenti0_.id</a>=onlineplat13\_.person_id  
where <a href="http://personenti0_.id" class="external-link" rel="nofollow">personenti0_.id</a> in (27 , 37 , 29) order by <a href="http://emailentit2_.id" class="external-link" rel="nofollow">emailentit2_.id</a> asc, <a href="http://addressent5_.id" class="external-link" rel="nofollow">addressent5_.id</a> asc, <a href="http://phoneentit11_.id" class="external-link" rel="nofollow">phoneentit11_.id</a> asc, categories12\_.category asc, onlineplat13\_.platform_name asc

</div>

</div>

Performance get 1 URI: 

<div hasbody="true" macro-id="25478313-49e3-4076-be38-09f1040866a4" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

\[30/Dec/2020:16:12:21 +0700\] "POST /luz_person/api/777bfa0f-eef1-4641-8041-6853d026f57f/persons/fetch HTTP/1.1" 200 51

</div>

</div>

Performance get 10 URIs: 

<div hasbody="true" macro-id="840f7635-ef15-48d0-bd05-0eb69a04a9c5" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

\[30/Dec/2020:16:13:44 +0700\] "POST /luz_person/api/777bfa0f-eef1-4641-8041-6853d026f57f/persons/fetch HTTP/1.1" 200 84

</div>

</div>

Performance get 40 URIs: 

<div hasbody="true" macro-id="7125c4f3-0c15-4181-8274-6337b6837fbe" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

\[30/Dec/2020:16:18:14 +0700\] "POST /luz_person/api/777bfa0f-eef1-4641-8041-6853d026f57f/persons/fetch HTTP/1.1" 200 240

</div>

</div>

<span class="legacy-color-text-red2">=\> Split the long query into small one(getting phones, emails, address... separately then join with person)</span>

%% ai-graph-start %%

**Related notes:**
- [[Analyze N+1 queries for REST API calculate payslips for overview salary processing]]
- [[Analyze performance for REST API calculate payslips for overview salary processing]]
- [[REST API calculate 1 employee's pay-slip]]
- [[Helios myKLARA app(luz-mobile) - API Response Performance Analysis]]
- [[N+1 hides at the service-call layer too, not just in the ORM]]

%% ai-graph-end %%