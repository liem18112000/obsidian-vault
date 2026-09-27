---
title: "Places Hubspot features have been applying to"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513313973/Places+Hubspot+features+have+been+applying+to
space: "LUZ"
topic: programming
relevance: 0.779
depth: 3
updated: 2025-07-02
attachments: 5
tags:
  - confluence
  - programming
  - space/luz
---

# Places Hubspot features have been applying to

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-07-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513313973/Places+Hubspot+features+have+been+applying+to)
> Relevance 0.779 · topic `programming`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

  

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Companysynchronization" style="text-align: center;"><strong><span class="legacy-color-text-red2">Company synchronization</span></strong></h2></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>2</td>
<td><p>When user create a company</p></td>
<td><p>CompanyService.java (method: add)</p></td>
<td><p>/luz_hubspot/api/company/create</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>When user update address of an company</p></td>
<td><p>luz_compensation</p></td>
<td><p>CompanyService.java (method: updateAddresses)</p></td>
<td><p>/luz_hubspot/api/company/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>4</td>
<td><p>When user update an marketing company information</p></td>
<td><p>luz_compensation</p></td>
<td><p>CompanyService.java (method: updateMarketingInformation)</p></td>
<td><p>/luz_hubspot/api/company/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>5</td>
<td><p>When user update a company</p></td>
<td><p>luz_compensation</p></td>
<td><p>CompanyService.java (method: update)</p></td>
<td><p>/luz_hubspot/api/company/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>6</td>
<td><p>When user partial update a company</p></td>
<td><p>luz_compensation</p></td>
<td><p>CompanyService.java (method: partialUpdate)</p></td>
<td><p>/luz_hubspot/api/company/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>7</td>
<td><p>When user change the address of company (sync address verification status)</p></td>
<td><p>luz_web</p></td>
<td><p>AuthenticateAddressUtils (method: triggerNewAuthenticationCode)</p></td>
<td><p>/luz_hubspot/api/company/update-partial</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>8</td>
<td><p>When user change the address of house hold (sync address verification status)</p></td>
<td><p>luz_web</p></td>
<td><p>AuthenticateAddressUtils (method: triggerNewAuthenticationCodeForHouseHold)</p></td>
<td><p>/luz_hubspot/api/company/update-partial</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>9</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Usersynchronization" style="text-align: center;"><strong><span class="legacy-color-text-red2">User synchronization</span></strong></h2></td>
</tr>
<tr>
<td>10</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>11</td>
<td><p>When user changes their preferred language</p></td>
<td><p>luz_components</p></td>
<td><ul>
<li><p>HeaderComponentProcess.mod (method: applyChangeLanguage)</p></li>
<li><p>MyLifeHeaderComponentProcess.mod (method: applyChangeLanguage)</p></li>
<li><p>CompanyRegistrationProcess.mod (method: updateLanguage)</p></li>
<li><p>CompanyHouseHoldRegistrationProcess.mod (method: updateLanguage)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/user/create</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>12</td>
<td><p>When user select home or business</p></td>
<td><p>luz_components</p></td>
<td><p>CompanyRegistrationRoutingPageProcess.mod (method: redirectToKlaraBusiness, redirectToKlaraHome)</p></td>
<td><p>/luz_hubspot/api/user/create</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>13</td>
<td><p>When user create a company</p></td>
<td><p>luz_hubspot</p></td>
<td><p>HSSyncCompanyService.java (method: syncCreateCompany)</p></td>
<td><p>Integrated with Company flow above</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>14</td>
<td><p>When user update a company</p></td>
<td><p>luz_hubspot</p></td>
<td><p>HSSyncCompanyService.java (method: syncUpdateCompany)</p></td>
<td><p>Integrated with Company flow above</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>15</td>
<td><p>When user is assigned, reassigned, removed from/to a company</p></td>
<td><p>luztenant_service</p></td>
<td><p>HubspotUserRoleSyncEventTracker.java</p></td>
<td><p>/luz_hubspot/api/association</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>16</td>
<td><p>When user login</p></td>
<td><p>keycloak_klara_theme</p></td>
<td><p>user-registration.js (setHubspotIdentify)</p></td>
<td><p>Hubspot CRM API</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>17</td>
<td><p>When customer input their information in marketing form</p></td>
<td><p>Klara home page</p></td>
<td><p>No information. it is not implemented by Hubspot team</p></td>
<td><p>Hubspot Form API</p></td>
<td><p>No information</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>18</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Associationsynchronization" style="text-align: center;"><strong><span class="legacy-color-text-red2">Association synchronization </span></strong></h2></td>
</tr>
<tr>
<td>19</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>20</td>
<td><p>When user is assigned to a company</p></td>
<td><p>luztenant_service</p></td>
<td><p>TenantManagerService.java (method: mapUserToCompany)</p></td>
<td><p>/luz_hubspot/api/association/add</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>21</td>
<td><p>When the role of user is updated</p></td>
<td><p>luztenant_service</p></td>
<td><p>TenantManagerService.java (method: update)</p></td>
<td><p>/luz_hubspot/api/association/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>22</td>
<td><p>When user is removed from a company</p></td>
<td><p>luztenant_service</p></td>
<td><p>TenantManagerService.java (method: remove)</p></td>
<td><p>/luz_hubspot/api/association/remove</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>23</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Subscriptionsynchronization" style="text-align: center;"><strong><span class="legacy-color-text-red2">Subscription synchronization</span></strong></h2></td>
</tr>
<tr>
<td>24</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>25</td>
<td><p>When user subscribe a widget</p></td>
<td><p>luz_store</p></td>
<td><p>SpecificTenantSubscriptionService.java (method: create)</p></td>
<td><p>/luz_hubspot/api/subscription/create</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>26</td>
<td><p>When user unsubscribe a widget</p></td>
<td><p>luz_store_web</p></td>
<td><p>WidgetStoreDetailProcess.mod (send signal Script)</p></td>
<td><p>/luz_hubspot/api/subscription/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>27</td>
<td><p>When user top up a widget</p></td>
<td><p>luz_store</p></td>
<td><p>SpecificTenantSubscriptionService.java (method: create)</p></td>
<td><p>/luz_hubspot/api/subscription/create</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>28</td>
<td><p>When admin create a subscription</p></td>
<td><p>luz_store</p></td>
<td><p>SpecificTenantSubscriptionService.java (method createManually)</p></td>
<td><p>/luz_hubspot/api/subscription/create</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>29</td>
<td><p>When admin update a subscription</p></td>
<td><p>luz_store_web</p></td>
<td><p>SubscriptionUpdateProcess.mod</p></td>
<td><p>/luz_hubspot/api/subscription/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>30</td>
<td><p>When subscription is deleted</p></td>
<td><p>luz_store</p></td>
<td><p>SubscriptionService.java (method: delete)</p></td>
<td><p>/luz_hubspot/api/subscription/delete</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>31</td>
<td><p>When user enter marketing code while creating company</p></td>
<td><p>luz_store</p></td>
<td><p>SpecificTenantSubscriptionService.java (method: createByMarketingCodes)</p></td>
<td><p>/luz_hubspot/api/subscription/create</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>32</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-ePost/Mylifesynchronization" style="text-align: center;"><strong><span class="legacy-color-text-red2">ePost/Mylife synchronization</span></strong></h2></td>
</tr>
<tr>
<td>33</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>34</td>
<td><p>When the users register a new account</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><p>UserRegisterService (method: userRegistration)</p></td>
<td><p>/luz_hubspot/api/user/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>35</td>
<td><p>When the users log in for the first time</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><p>TokenService (method: getRefreshTokenAndUserInfo)</p></td>
<td><p>/luz_hubspot/api/user/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>36</td>
<td><p>When KLP users log in for the first time</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><p>TokenService (method: createKlpUserProfile, createKlpUserProfileV2)</p></td>
<td><p>/luz_hubspot/api/user/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>37</td>
<td><p>When the users enter OTP to verify the address</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><ul>
<li><p>ProfileService (method: verifyAddressOpt)</p></li>
<li><p>ProfileServiceV2 (method: verifyAddressOpt)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/user/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>38</td>
<td><p>When the users update the profile information</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><ul>
<li><p>ProfileService (method: updateUserProfile)</p></li>
<li><p>ProfileServiceV2 (method: updateUserProfile)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/user/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>39</td>
<td><p>When the users add an address</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><p>ProfileServiceV2 (method: addAddress)</p></td>
<td><p>/luz_hubspot/api/user/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>40</td>
<td><p>When the users update an address</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><p>ProfileServiceV2 (method: updateAddress)</p></td>
<td><p>/luz_hubspot/api/user/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>41</td>
<td><p>When the users delete an address</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><p>ProfileServiceV2 (method: deleteAddress)</p></td>
<td><p>/luz_hubspot/api/user/update</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>42</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-POSindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">POS indicator</span></strong></h2></td>
</tr>
<tr>
<td>43</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>44</td>
<td><p>POS_2: At least 1 article is flagged for POS</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_article_web</p></li>
</ul></td>
<td><ul>
<li><p>POSForArticle.java</p></li>
<li><p>ArticleOverviewProcess.mod</p></li>
<li><p>ArticlePageProcess.mod</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=pos_2;online_shop_2</p></td>
<td><p><span class="legacy-color-text-blue3">18/Aug/20</span></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>45</td>
<td><p>POS_3: At least 1 seller/agent given</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_pos_web</p></li>
</ul></td>
<td><ul>
<li><p>POSForNumberOfCashRegister.java</p></li>
<li><p>PosActivationProcess.mod</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=pos_3;pos_10</p></td>
<td><p><span class="legacy-color-text-blue3">04/Aug/20</span></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>46</td>
<td><p>POS_4: Tablet and server are synchronised</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_pos</p></li>
</ul></td>
<td><ul>
<li><p>POSForActivation.java</p></li>
<li><p>PosActivationService.java</p></li>
<li><p>PosActivationSyncEventObserver.java</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=pos_4</p></td>
<td><p><span class="legacy-color-text-blue3">18/Aug/20</span></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>47</td>
<td><p><span class="legacy-color-text-blue3">POS_5: Number of accounting tools connected to the POS</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_pos_web</p></li>
</ul></td>
<td><ul>
<li><p>POSForConnectedAccounting.java</p></li>
<li><p>CashRegistersProcess.mod</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=pos_5</p></td>
<td><p><span class="legacy-color-text-blue3">01/Sep/20</span></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>48</td>
<td><p>POS_10: Number of used POS</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_pos_web</p></li>
</ul></td>
<td><ul>
<li><p>POSForNumberOfCashRegister.java</p></li>
<li><p>PosActivationProcess.mod</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=pos_3;pos_10</p></td>
<td><p><span class="legacy-color-text-blue3">28/Aug/20</span></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>49</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Onlineshopindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Online shop indicator</span></strong></h2></td>
</tr>
<tr>
<td>50</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>51</td>
<td><p>ONLINE_SHOP_3: At least 1 article is flagged to be used in the Online Shop</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_article_web</p></li>
</ul></td>
<td><ul>
<li><p>POSForArticle.java</p></li>
<li><p>ArticleOverviewProcess.mod (method: onDeleteArticle)</p></li>
<li><p>ArticlePageProcess.mod (method: saveBasicInfomation())</p></li>
<li><p>ArticleImportProcess.mod (method: finishImportArticles)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_shop_2</p></td>
<td><p><span class="legacy-color-text-blue3">16/Sep/20</span></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>52</td>
<td><p>ONLINE_SHOP_5: Online shop is active</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_components</p></li>
</ul></td>
<td><ul>
<li><p>OnlineShopForActivation.java</p></li>
<li><p>OnlineShopMenuHandler.java</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_shop_5</p></td>
<td><p><span class="legacy-color-text-blue3">28/Sep/20</span></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>53</td>
<td><p>ONLINE_SHOP_6: A customer order was made</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luzfin_finance</p></li>
</ul></td>
<td><ul>
<li><p>ItemCreationIndicatorRule.java</p></li>
<li><p>ConfirmationService.java</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_shop_6</p></td>
<td><p>13/Oct/20</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>54</td>
<td><p>ONLINE_SHOP_7: A bill was created from an order via Online Shop</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>ItemCreationIndicatorRule.java</p></li>
<li><p>InvoicePageProcess.mod</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_shop_7</p></td>
<td><p>13/Oct/20</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>55</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Klaraprojectindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Klara project indicator</span></strong></h2></td>
</tr>
<tr>
<td>56</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>57</td>
<td><p>PROJECT_2: At least 1 service type is saved</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_components</p></li>
</ul></td>
<td><ul>
<li><p>KlaraProjectForServiceType.java</p></li>
<li><p>ServiceTypeProcess.mod</p></li>
<li><p>ServiceTypeCreationDialogProcess.mod</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=project_2</p></td>
<td><p><span class="legacy-color-text-blue3">12/Oct/20 </span></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>58</td>
<td><p>PROJECT_3: At least 1 reporter is given</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>KlaraProjectForReporter.java</p></li>
<li><p>ReporterOverviewProcess.mod</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=project_3</p></td>
<td><p>13/Oct/20</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>59</td>
<td><p>PROJECT_4: <span class="legacy-color-text-blue3">Date of last task created</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>ItemCreationIndicatorRule.java</p></li>
<li><p>AddTaskProcess.mod </p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=project_4</p></td>
<td><p>22/Oct/20</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>60</td>
<td><p>PROJECT_5: Date of last create report</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luzfin_finance</p></li>
</ul></td>
<td><ul>
<li><p>ItemCreationIndicatorRule.java</p></li>
<li><p>TaskActivityService.class (method: createOrderTaskActivity)</p></li>
<li><p>MaterialService.class (method: createReportMaterial)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=project_5</p></td>
<td><p>22/Oct/20</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>61</td>
<td><p>PROJECT_6: Date of last bill created from report</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>ItemCreationIndicatorRule.java</p></li>
<li><p>InvoicePageProcess.mod</p></li>
<li><p>InvoicingPageProcess.mod</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=project_6</p></td>
<td><p>22/Oct/20</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>62</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Websitebasicindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Website basic indicator</span></strong></h2></td>
</tr>
<tr>
<td>63</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>64</td>
<td><p>WEBSITE_BASIC_1: <span class="legacy-color-text-blue3">The Webpage is online</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_components</p></li>
</ul></td>
<td><ul>
<li><p>WebsiteBasicForWebPageOnline.java</p></li>
<li><p>GeneralSettingsHandler.java (method: toggleActivation)</p></li>
<li><p>WebpageActivationHandler.java (method: toggleActivation)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=website_basic_1</p></td>
<td><p>20/Oct/20</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>65</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Onlinenewsindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Online news indicator</span></strong></h2></td>
</tr>
<tr>
<td>66</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>67</td>
<td><p>online_news_2: The Online News is active</p></td>
<td><ul>
<li><p>luz_components</p></li>
</ul></td>
<td><p><span class="legacy-color-text-blue3">OnlineShopMenuHandler.java (</span>method: syncHubspotIndicator)</p></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_news_basic_2;online_news_plus_2</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513315733/0.01.81.00+13.10.2020+-+26.10.2020">0.01.81.00 (13.10.2020 - 26.10.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>68</td>
<td><p>online_news_3: Date of last published Post</p>
<p><br />
</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_news</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">OnlineNewsForPostCreation.java</span></p></li>
<li><p><span class="legacy-color-text-blue3">PostController.java (</span>method: createPost)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_news_basic_3;online_news_plus_3</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513322727/0.01.82.00+27.10.2020+-+09.11.2020">0.01.82.00 (27.10.2020 - 09.11.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>69</td>
<td><p>online_news_4: <span class="legacy-color-text-blue3">Marketing information and Post are synchronized to Google</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_marketing</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">OnlineNewsForMarketing.java</span></p></li>
<li><p><sup>LinkedChannelService.java (method:</sup></p>
<p><sup>createLinkedChannel,</sup></p>
<p><sup>updateLinkedChannel,</sup></p>
<p><sup>deleteLinkedChannel</sup></p>
<p><sup>)</sup></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_news_plus_4;online_presence_plus_4</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513322727/0.01.82.00+27.10.2020+-+09.11.2020">0.01.82.00 (27.10.2020 - 09.11.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>70</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Onlinepresenceindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Online presence indicator</span></strong></h2></td>
</tr>
<tr>
<td>71</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>72</td>
<td><p><span class="legacy-color-text-blue3">online_presence_2: A Cover Image and Company Description are saved</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_components</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">OnlinePresenceForConfiguration.java</span></p></li>
<li><p>WebpageIntroductionConfigurationHandler<span class="legacy-color-text-blue3">.java (</span>method: save)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_presence_2</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513322727/0.01.82.00+27.10.2020+-+09.11.2020">0.01.82.00 (27.10.2020 - 09.11.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>73</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Bookingindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Booking indicator</span></strong></h2></td>
</tr>
<tr>
<td>74</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>75</td>
<td><p><span class="legacy-color-text-blue3">online_booking_2: At least a Resource Group is saved</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_booking_web</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">OnlineBookingForResourcePool.java</span></p></li>
<li><p>ResourcePoolFormDialogProcess.mod</p></li>
<li><p><span class="legacy-color-text-blue3">ResourcePoolListPanelProcess.mod</span></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_booking_2</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519060048/0.01.83.00+10.11.2020+-+23.11.2020">0.01.83.00 (10.11.2020 - 23.11.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>76</td>
<td><p><span class="legacy-color-text-blue3">online_booking_3: At least a Service for Booking is saved</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_booking_web</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">OnlineBookingForService.java</span></p></li>
<li><p><span class="legacy-color-text-blue3">ServiceFormDialogProcess</span>.mod</p></li>
<li><p><span class="legacy-color-text-blue3">ServiceListPanelProcess.mod</span></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_booking_3</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519060048/0.01.83.00+10.11.2020+-+23.11.2020">0.01.83.00 (10.11.2020 - 23.11.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>77</td>
<td><p><span class="legacy-color-text-blue3">online_booking_4: The Online Booking is active</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_components</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">WebPageMenuActivationIndicatorRule.java</span></p></li>
<li><p>OnlineShopMenuHandler.java (method: save(String))</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_booking_4</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519060048/0.01.83.00+10.11.2020+-+23.11.2020">0.01.83.00 (10.11.2020 - 23.11.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>78</td>
<td><p><span class="legacy-color-text-blue3">online_booking_5: Date of last booking</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_booking</p></li>
</ul></td>
<td><ul>
<li><p><sup>ItemCreationIndicatorRule.java</sup></p></li>
<li><p><span class="legacy-color-text-blue3">BookingService.java (method: </span>add(BookingDto))</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=online_booking_5</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519060048/0.01.83.00+10.11.2020+-+23.11.2020">0.01.83.00 (10.11.2020 - 23.11.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>79</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Accountingindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Accounting indicator</span></strong></h2></td>
</tr>
<tr>
<td>80</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>81</td>
<td><p><span class="legacy-color-text-blue3">accounting_1: Accounting set-up is complete</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">AccountingForOnboarding</span>.java</p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/sale/bank/AccountingOverviewProcess.mod (method: finish)</span></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_1</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519718771/0.01.84.00+24.11.2020+-+07.12.2020">0.01.84.00 (24.11.2020 - 07.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>82</td>
<td><p><span class="legacy-color-text-blue3">accounting_2: Date of last accounting booking</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_accounting</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">AccountingForBooking.java</span></p></li>
<li><p><sup>BookingService.java (method:saveBookingHeader)</sup></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_2</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724594/0.01.85.00+08.12.2020+-+21.12.2020">0.01.85.00 (08.12.2020 - 21.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>83</td>
<td><p><span class="legacy-color-text-blue3">accounting_3: Date of last conciliation with the bank</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>DateIndicatorRule.java</p></li>
<li><p>BookingTemplatesBean.java (method: bookAndReconcile(boolean))</p></li>
<li><p><span class="legacy-color-text-blue3">BankReconciliationController.java (method:</span></p></li>
</ul>
<p>confirmReconciliation,<br />
</p>
<p>submitAllAutoReconciliation)</p>
<p><br />
</p></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_3</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519718771/0.01.84.00+24.11.2020+-+07.12.2020">0.01.84.00 (24.11.2020 - 07.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>84</td>
<td><p>accounting_4: Newest business year: from date</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>DateIndicatorRule.java</p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/sale/bank/AccountingOverviewProcess.mod (method: finish)</span></p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/accounting/business/year/AccountingOverviewProcess.mod (method: finish)</span></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_4</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724594/0.01.85.00+08.12.2020+-+21.12.2020">0.01.85.00 (08.12.2020 - 21.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>85</td>
<td><p>accounting_5: Newest business year: to date</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>DateIndicatorRule.java</p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/sale/bank/AccountingOverviewProcess.mod (method: finish)</span></p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/accounting/business/year/AccountingOverviewProcess.mod (method: finish)</span></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_5</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724594/0.01.85.00+08.12.2020+-+21.12.2020">0.01.85.00 (08.12.2020 - 21.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>86</td>
<td><p>accounting_6: Closed business year: from date</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>DateIndicatorRule.java</p></li>
<li><p>BusinessYearBean.java (method: sealBusinessYear)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_6</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724594/0.01.85.00+08.12.2020+-+21.12.2020">0.01.85.00 (08.12.2020 - 21.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>87</td>
<td><p>accounting_7: Closed business year: to date</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>DateIndicatorRule.java</p></li>
<li><p>BusinessYearBean.java (method: sealBusinessYear)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_7</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724594/0.01.85.00+08.12.2020+-+21.12.2020">0.01.85.00 (08.12.2020 - 21.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>88</td>
<td><p>accounting_8: Number of booking headers</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_accounting</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">AccountingForBooking.java</span></p></li>
<li><p><sup>BookingService.java (method:saveBookingHeader)</sup></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_8</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724594/0.01.85.00+08.12.2020+-+21.12.2020">0.01.85.00 (08.12.2020 - 21.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>89</td>
<td><p><span class="legacy-color-text-blue3">accounting_9: Legal form for accounting tool</span></p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">AccountingForLegalForm.java</span></p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/sale/bank/AccountingOverviewProcess.mod (method: finish)</span></p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/accounting/business/year/AccountingOverviewProcess.mod (method: finish)</span></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=accounting_9</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724594/0.01.85.00+08.12.2020+-+21.12.2020">0.01.85.00 (08.12.2020 - 21.12.2020)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>90</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Salaryindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Salary indicator</span></strong></h2></td>
</tr>
<tr>
<td>91</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>92</td>
<td><p>salary_1: Salary set-up is complete</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_web</p></li>
</ul></td>
<td><ul>
<li><p>SalaryForOnboarding.java</p></li>
<li><p>InsuranceContractsProcess.mod (method: saveFES())</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=salary_1</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519733997/0.01.87.00+05.01.2021+-+18.01.2021">0.01.87.00 (05.01.2021 - 18.01.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>93</td>
<td><p>salary_3: At least 1 employee is saved</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_compensation</p></li>
</ul></td>
<td><ul>
<li><p>SalaryForEmployee.java</p></li>
<li><p>EmployeeService.java (method: delete, add)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=salary_3</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519733997/0.01.87.00+05.01.2021+-+18.01.2021">0.01.87.00 (05.01.2021 - 18.01.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>94</td>
<td><p>salary_4: Date of last salary processing</p></td>
<td><ul>
<li><p>luz_xhrm_processes</p></li>
<li><p>luz_store</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">SalaryPaymentV2Bean.java (method:createTheBillForSalaryPayment)</span></p></li>
<li><p><span class="legacy-color-text-blue3">TenantBillingResource.class (method: </span>createBillingManually)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=salary_4</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519733997/0.01.87.00+05.01.2021+-+18.01.2021">0.01.87.00 (05.01.2021 - 18.01.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>95</td>
<td><p>salary_5: Date of last reporting of Wage (EWR)</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_components</p></li>
</ul></td>
<td><ul>
<li><p>DateIndicatorRule.java</p></li>
<li><p>TransmitSalaryProcess.mod (method: transmit())</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=salary_5</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519735793/0.01.88.00+19.01.2021+-+01.02.2021">0.01.88.00 (19.01.2021 - 01.02.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>96</td>
<td><p>salary_6: <span class="legacy-color-text-blue3">number of pay slips in last salary processing</span></p></td>
<td><ul>
<li><p>luz_xhrm_processes</p></li>
<li><p>luz_store</p></li>
</ul></td>
<td><ul>
<li><p><span class="legacy-color-text-blue3">SalaryPaymentV2Bean.java (method:createTheBillForSalaryPayment)</span></p></li>
<li><p><span class="legacy-color-text-blue3">TenantBillingResource.class (method: </span>createBillingManually)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=salary_6</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47787868305/0.02.69.00+23.04.2024+-+06.05.2024">0.02.69.00 (23.04.2024 - 06.05.2024)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>97</td>
<td><p>salary_7: <span class="legacy-color-text-blue3">sum of all pay slips in all salary processings</span></p></td>
<td><p>go with salary_6</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47787868305/0.02.69.00+23.04.2024+-+06.05.2024">0.02.69.00 (23.04.2024 - 06.05.2024)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>98</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-OrderManagementindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">Order Management indicator</span></strong></h2></td>
</tr>
<tr>
<td>99</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>100</td>
<td><p>order_management_1: At least 1 article is saved</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_article_web</p></li>
</ul></td>
<td><ul>
<li><p>OrderManagementForArticle.java</p></li>
<li><p>ArticleOverviewProcess.mod (method: onDeleteArticle)</p></li>
<li><p>ArticlePageProcess.mod (method: saveBasicInfomation())</p></li>
<li><p>ArticleImportProcess.mod (method: finishImportArticles)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=order_management_1</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519735793/0.01.88.00+19.01.2021+-+01.02.2021">0.01.88.00 (19.01.2021 - 01.02.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>101</td>
<td><p>order_management_2: Date of last document created</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luzfin_finance</p></li>
</ul></td>
<td><ul>
<li><p>DateIndicatorRule.java</p></li>
<li><p>ConfirmationService.java (method: saveConfirmation)</p></li>
<li><p>DeliveryNoteService.java (method: saveDeliveryNote)</p></li>
<li><p>InvoiceService.java (method: saveInvoice)</p></li>
<li><p>OfferService.java (method: saveOffer)</p></li>
<li><p>OrderService.java (method: saveOrder)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=order_management_2</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519735793/0.01.88.00+19.01.2021+-+01.02.2021">0.01.88.00 (19.01.2021 - 01.02.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>102</td>
<td><p>order_management_3: At least 1 customized template is active</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_components</p></li>
</ul></td>
<td><ul>
<li><p>OrderManagementForTemplateStatus.java</p></li>
<li><p>PrintingTemplateProcess.mod (method: saveTemplate())</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=order_management_3</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519735793/0.01.88.00+19.01.2021+-+01.02.2021">0.01.88.00 (19.01.2021 - 01.02.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>103</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-CRMindicator" style="text-align: center;"><strong><span class="legacy-color-text-red2">CRM indicator</span></strong></h2></td>
</tr>
<tr>
<td>104</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>105</td>
<td><p>crm_1: At least 1 customer / partner is saved</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_booking_web</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>CrmForCustomer.java</p></li>
<li><p>CustomerService.java (method: create)</p></li>
<li><p>PartnerImportController.java (method: savingPartnerImport)</p></li>
<li><p>CustomerController.java (method: createCustomer)</p></li>
<li><p>InvoiceController.java (method: createInvoice)</p></li>
<li><p>RecurringInvoiceTemplateController.java (method: createRecurringInvoiceTemplate)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=crm_1</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525650311/0.01.89.00+02.02.2021+-+02.03.2021">0.01.89.00 (02.02.2021 - 02.03.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>106</td>
<td><p>crm_2: Date of last note created on a customer</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>DateIndicatorRule.java</p></li>
<li><p>CustomerController.java (method: createCustomer)</p></li>
<li><p>InvoiceController.java (method: createInvoice)</p></li>
<li><p>RecurringInvoiceTemplateController.java (method: createRecurringInvoiceTemplate)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=crm_2</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525650311/0.01.89.00+02.02.2021+-+02.03.2021">0.01.89.00 (02.02.2021 - 02.03.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>107</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-CRMindicator.1" style="text-align: center;"><strong><span class="legacy-color-text-red2">CRM indicator</span></strong></h2></td>
</tr>
<tr>
<td>108</td>
<td><p><strong>Feature</strong></p></td>
<td><p><strong>Modified Module</strong></p></td>
<td><p><strong>Class</strong></p></td>
<td><p><strong>Hubspot API</strong></p></td>
<td><p><strong>Applied From</strong></p></td>
<td><p><strong>Applied To</strong></p></td>
</tr>
<tr>
<td>109</td>
<td><p>inventory_2: A storage is created</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_article</p></li>
</ul></td>
<td><ul>
<li><p>InventoryForStorage.java</p></li>
<li><p>StorageService.java (method: createStorage, deleteStorageById)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=inventory_2</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525676520/0.01.93.00+13.04.2021+-+26.04.2021">0.01.93.00 (13.04.2021 - 26.04.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>110</td>
<td><p>inventory_3: At least 1 article is flagged for inventory</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_article_web</p></li>
</ul></td>
<td><ul>
<li><p>InventoryForArticle.java</p></li>
<li><p>ArticleOverviewProcess.mod (method: onDeleteArticle)</p></li>
<li><p>ArticlePageProcess.mod (method: saveBasicInfomation())</p></li>
<li><p>ArticleImportProcess.mod (method: finishImportArticles)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=inventory_3</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530435467/0.01.95.00+11.05.2021+-+25.05.2021">0.01.95.00 (11.05.2021 - 25.05.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>111</td>
<td><p>inventory_4: An account is created for inventory</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_finance</p></li>
</ul></td>
<td><ul>
<li><p>IndicatorHasAtLeastOneItem.java</p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/sale/bank/AccountingOverviewProcess.mod (method: finish)</span></p></li>
<li><p><span class="legacy-color-text-blue3">klara/luz/fin/accounting/business/year/AccountingOverviewProcess.mod (method: finish)</span></p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=inventory_4</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530435467/0.01.95.00+11.05.2021+-+25.05.2021">0.01.95.00 (11.05.2021 - 25.05.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>112</td>
<td><p>inventory_5: At least 1 article is put in stock</p></td>
<td><ul>
<li><p>luz_hubspot</p></li>
<li><p>luz_article</p></li>
</ul></td>
<td><ul>
<li><p>InventoryForPutInStock.java</p></li>
<li><p>TransactionService.java (method: createPutInTransactions)</p></li>
</ul></td>
<td><p>/luz_hubspot/api/indicator?indicator-ids=inventory_5</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530441135/0.01.96.00+26.05.2021+-+07.06.2021">0.01.96.00 (26.05.2021 - 07.06.2021)</a></p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>113</td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Custombehaviorevent" style="text-align: center;"><strong><span class="legacy-color-text-red2">Custom behavior event</span></strong></h2></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>114</td>
<td><p>Inactive user for offboarding flow</p></td>
<td><p>luz_retention</p></td>
<td><p>CronJobService.fetchInactiveUsersAndSendRetention()</p></td>
<td><p>/luz_hubspot/api/custom-behavioral-events</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>115</td>
<td><p>Epost share inbox</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><p>SendHubspotMailAndPnService.sendMylifeNotification()</p></td>
<td><p>/luz_hubspot/api/custom-behavioral-events</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>116</td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Custombehaviorevent.1" style="text-align: center;"><strong><span class="legacy-color-text-red2">Custom behavior event</span></strong></h2></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>117</td>
<td><p>Epost share inbox</p></td>
<td><p>luz_mylife_epost_adapter</p></td>
<td><p>SendHubspotMailAndPnService.sendInvitationMailToTrustedUser()</p></td>
<td><p>/luz_hubspot/api/email</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>118</td>
<td><p>Inactive user for offboarding flow</p></td>
<td><p>luz_retention</p></td>
<td><p>CronJobService.fetchInactiveUsersAndSendRetention()</p></td>
<td><p>/luz_hubspot/api/email</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td>119</td>
<td colspan="6"><h2 id="PlacesHubspotfeatureshavebeenapplyingto-Adyenonboarding" style="text-align: center;"><strong><span>Adyen onboarding</span></strong></h2></td>
</tr>
<tr>
<td>120</td>
<td><p>Adyen onboarding status<br />
(onboarding_payments_verifizierung)<br />
</p></td>
<td><p>luz_adyen<br />
</p></td>
<td><p><code>OnboardingService.getOnboardingState()</code><br />
</p></td>
<td><p><code>/luz_hubspot/api/subscription/update-partial/{id}</code><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

  

**Sync user language**


![[20513313973-image2023-2-1_18-14-8.png]]



List indicators sync by jobs:  
accounting_2  
online_shop_6  
project_5  
accounting_3  
salary_4  
salary_5  
order_management_2  
crm_2

online_shop_7  
project_4  
project_6  
accounting_4  
accounting_5  
accounting_6  
accounting_7  
crm_3  
salary_6  
online_booking_5  
hos_online_booking_5  
hom_online_booking_5  
hol_online_booking_5  
online_booking_11

online_news_basic_3  
online_news_plus_3  
hol_online_news_3

pos_4  
hom_pos_4  
hol_pos_4

pos_5  
hom_pos_5  
hol_pos_5  

</div>

</div>

</div>

</div>
