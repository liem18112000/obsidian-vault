---
ai_hash: 908fc922805f8878
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48652288234'
confluence_path: Team Kepler > Risk & Issues > Issues
created: 2025-09-05
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Error Log: "Failed to store"'
type: source
updated: 2026-03-11
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48652288234/Error+Log+Failed+to+store
---

# Error Log: "Failed to store"

*Confluence source · Team Kepler › Risk & Issues › Issues · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48652288234/Error+Log+Failed+to+store) · updated 2026-03-11*

> [!info]
>
>
>
> Summary of reported “Failed to store” errors
>
>

### Action Items - Bug Fixes:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p>**Result of problem analysis**</p></th>
<th><p>**Bug fix**</p></th>
<th><p>**Team Responsible**</p></th>
</tr>
&#10;<tr>
<td><p>The root cause is that luz-docs is sometimes unable to reach **jsonstore** during the document creation process—either when adding a new document or when updating the `_isBeingCreated` field.</p>
<p>Currently, luz-docs performs **3 retry attempts**, with approximately **1 second delay** between each attempt. However, if jsonstore remains unavailable during this short window, all retries fail, resulting in a “Failed to store” error.</p>
<p>[https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954966029/Service+Error+Analysis+Report+FAILED_TO_STORE+on+Production?themeState=dark%3Adark%20light%3Alight%20spacing%3Aspacing%20typography%3Atypography%20colorMode%3Alight](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954966029/Service+Error+Analysis+Report+FAILED_TO_STORE+on+Production?themeState=dark%3Adark%20light%3Alight%20spacing%3Aspacing%20typography%3Atypography%20colorMode%3Alight)</p></td>
<td><p>[LUZ-143956](https://axonivy.atlassian.net/browse/LUZ-143956)</p>
<p>[LUZ-145168](https://axonivy.atlassian.net/browse/LUZ-145168)</p></td>
<td><p>Kepler</p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Error Log:

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
<td><p>**Delivery ID**</p></td>
<td><p>**Sender ID**</p></td>
<td><p>**Sender**</p></td>
<td><p>**Sender Tenant ID**</p></td>
<td><p>**Root Cause**</p></td>
<td><p>**Team Responsible**</p></td>
<td><p>**Status**</p></td>
</tr>
<tr>
<td><p>2025-12-05T09:16:42.072</p></td>
<td><p>8250</p></td>
<td><p>Gabriel Hanin, Coiffeurgeschäft</p></td>
<td><p>64ee6c43-5300-4d0e-99de-d9624ff191aa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-05T15:15:50.971</p></td>
<td><p>27748</p></td>
<td><p>HeyLight AG</p></td>
<td><p>261c752f-81ad-4a57-bc9d-9070af8feea4</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-10T08:59:19.416</p></td>
<td><p>8276</p></td>
<td><p>Gabriel Hanin, Coiffeurgeschäft</p></td>
<td><p>64ee6c43-5300-4d0e-99de-d9624ff191aa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-10T07:44:58.014</p></td>
<td><p>22075</p></td>
<td><p>Coiffure Kopfstand GmbH</p></td>
<td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T10:03:20.934</p></td>
<td><p>43802</p></td>
<td><p>Coiffure Fatos Haxhija</p></td>
<td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T09:34:48.061</p></td>
<td><p>171</p></td>
<td><p>Fiorista</p></td>
<td><p>bcb3ae22-fb1d-4d9c-921b-0ee91a6162ec</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T09:34:37.270</p></td>
<td><p>14844</p></td>
<td><p>Coiffeur Figaro Damen- und Herrensalon GmbH</p></td>
<td><p>ad5d7462-4972-4d63-9208-0108d9757d4a</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T09:33:22.634</p></td>
<td><p>19126</p></td>
<td><p>Coiffeur Schelbert</p></td>
<td><p>5c915c45-038f-4677-a57e-9585ddd6f05b</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T09:33:21.236</p></td>
<td><p>22313</p></td>
<td><p>Coiffure Kopfstand GmbH</p></td>
<td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T09:33:20.067</p></td>
<td><p>43797</p></td>
<td><p>Coiffure Fatos Haxhija</p></td>
<td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T09:33:18.460</p></td>
<td><p>22312</p></td>
<td><p>Coiffure Kopfstand GmbH</p></td>
<td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T08:59:07.422</p></td>
<td><p>15847</p></td>
<td><p>Nails & Beauty Marisa Maurer - Huggenberger</p></td>
<td><p>a54041b0-bc72-476b-ad36-3284fb38410c</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T08:58:39.354</p></td>
<td><p>228725</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T08:56:51.161</p></td>
<td><p>15843</p></td>
<td><p>Nails & Beauty Marisa Maurer - Huggenberger</p></td>
<td><p>a54041b0-bc72-476b-ad36-3284fb38410c</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T08:55:25.438</p></td>
<td><p>1008</p></td>
<td><p>Die Schweizerische Post (Digitale Belege)</p></td>
<td><p>7224acf5-0e6c-4e0b-9e70-0d7b9cdcb0e9</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T07:59:05.900</p></td>
<td><p>650</p></td>
<td><p>Beautywave</p></td>
<td><p>88615037-4ea5-4396-9456-e6424a81e996</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T07:41:29.714</p></td>
<td><p>228651</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T04:56:01.093</p></td>
<td><p>84</p></td>
<td><p>pitswiss.com</p></td>
<td><p>b1e8d065-fe2b-470a-9a49-6d2789ff54bc</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T03:00:23.933</p></td>
<td><p>228595</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T02:43:02.322</p></td>
<td><p>7683</p></td>
<td><p>CrossFit Basel GmbH</p></td>
<td><p>c3d68e60-93f3-4c7e-a71b-13f15d6b0499</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T02:42:37.059</p></td>
<td><p>7678</p></td>
<td><p>CrossFit Basel GmbH</p></td>
<td><p>c3d68e60-93f3-4c7e-a71b-13f15d6b0499</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T02:42:18.555</p></td>
<td><p>7676</p></td>
<td><p>CrossFit Basel GmbH</p></td>
<td><p>c3d68e60-93f3-4c7e-a71b-13f15d6b0499</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-11T20:13:39.511</p></td>
<td><p>11998</p></td>
<td><p>Velvet Skin by Berisha</p></td>
<td><p>c1d4928c-f93b-4771-9bcf-595f5ef11995</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-11T20:10:23.850</p></td>
<td><p>494</p></td>
<td><p>Overcut Hairdesign</p></td>
<td><p>1f6036d8-bb35-42e3-8994-ee3930cfd6ab</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-11T20:08:54.039</p></td>
<td><p>15830</p></td>
<td><p>Nails & Beauty Marisa Maurer - Huggenberger</p></td>
<td><p>a54041b0-bc72-476b-ad36-3284fb38410c</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-11T19:32:18.888</p></td>
<td><p>43776</p></td>
<td><p>Coiffure Fatos Haxhija</p></td>
<td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-11T10:19:40.407</p></td>
<td><p>228324</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p>
<p>matched participant ID: 6c1bb232-f348-4ad0-b956-f83a64ab3c6f</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-12T11:26:23.923</p></td>
<td><p>228881</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p>
<p>matched participant ID: 992eb54d-1f61-441c-b54a-9cf6c0b1cdd5</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td><p>Kepler</p></td>
<td><p>`TO BE ANALYSED`</p></td>
</tr>
<tr>
<td><p>2025-12-15T08:55:18.292</p></td>
<td><p>229291</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td rowspan="14"><p>Luz-eletter,<br />
Luz-docs-view-controller</p></td>
<td rowspan="14">![[image-20251217-101049.png]]
<p>**luz-eletter** calls **luz-docs-view-controller**, but the request times out during the read phase and is terminated at **luz-docs-view-controller** without reaching **luz-docs.**</p></td>
</tr>
<tr>
<td><p>2025-12-15T08:54:49.849</p></td>
<td><p>229290</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-15T08:54:26.358</p></td>
<td><p>229289</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-15T08:53:57.975</p></td>
<td><p>229288</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-15T08:53:30.240</p></td>
<td><p>229287</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-15T08:53:23.204</p></td>
<td><p>229286</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-12T16:10:28.482</p></td>
<td><p>228987</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-16T11:11:36.766</p></td>
<td><p>1110</p></td>
<td><p>Die Schweizerische Post (Digitale Belege)</p></td>
<td><p>7224acf5-0e6c-4e0b-9e70-0d7b9cdcb0e9</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-16T10:08:30.220</p></td>
<td><p>230102</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-16T10:08:44.016</p></td>
<td><p>1107</p></td>
<td><p>Die Schweizerische Post (Digitale Belege)</p></td>
<td><p>7224acf5-0e6c-4e0b-9e70-0d7b9cdcb0e9</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-16T09:51:25.944</p></td>
<td><p>230066</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-16T14:02:44.025</p></td>
<td><p>1116</p></td>
<td><p>Die Schweizerische Post (Digitale Belege)</p></td>
<td><p>7224acf5-0e6c-4e0b-9e70-0d7b9cdcb0e9</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-16T11:40:31.033</p></td>
<td><p>230172</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-16T11:11:36.766</p></td>
<td><p>1110</p></td>
<td><p>Die Schweizerische Post (Digitale Belege)</p></td>
<td><p>7224acf5-0e6c-4e0b-9e70-0d7b9cdcb0e9</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
</tr>
<tr>
<td><p>2025-12-17T13:23:23.310</p></td>
<td><p>231187</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-01-08T17:45:34.325</p></td>
<td><p>1515</p></td>
<td><p>Die Schweizerische Post (Digitale Belege)</p></td>
<td><p>7224acf5-0e6c-4e0b-9e70-0d7b9cdcb0e9</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-01-22T10:04:50.346</p></td>
<td><p>247370</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-01-20T17:54:40.342</p></td>
<td><p>12048</p></td>
<td><p>La Société Coopérative Migros Vaud</p></td>
<td><p>d1c6e7ae-bc2c-457c-a3c8-eee2336a4b5e</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-01-28T15:47:49.489</p></td>
<td><p>150</p></td>
<td><p>HR Campus AG</p></td>
<td><p>08857281-39d1-4c4b-b81e-b4b3630aa11b</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-01-28T15:46:06.829</p></td>
<td><p>1153</p></td>
<td><p>Caisse interprofessionnelle AVS de la Fédération des Entreprises Romandes Genève</p></td>
<td><p>db21e1d3-214a-46cb-92d0-d866cec1437c</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-01-28T15:46:06.814</p></td>
<td><p>1153</p></td>
<td><p>Caisse interprofessionnelle AVS de la Fédération des Entreprises Romandes Genève</p></td>
<td><p>db21e1d3-214a-46cb-92d0-d866cec1437c</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-01-28T15:47:16.957</p></td>
<td><p>11374</p></td>
<td><p>libs Industrielle Berufslehren Schweiz</p></td>
<td><p>515560f8-d179-4e0e-a28f-69ee55a32d65</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-02-02T14:24:39.678</p></td>
<td><p>2715</p></td>
<td><p>Império Assurances SA</p></td>
<td><p>2e0937f2-fe28-44c1-b4ac-80a00d4be6d3</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-02-04T11:53:03.280</p></td>
<td><p>260843</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-02-05T11:18:24.530</p></td>
<td><p>261646</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-02-24T12:00:26.151</p></td>
<td><p>272359</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-03T12:03:16.405</p></td>
<td><p>31252</p></td>
<td><p>HeyLight AG</p></td>
<td><p>261c752f-81ad-4a57-bc9d-9070af8feea4</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-03T12:02:58.234</p></td>
<td><p>280492</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-03T12:01:32.240</p></td>
<td><p>280491</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-03T11:59:51.657</p></td>
<td><p>280487</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-03T11:58:05.296</p></td>
<td><p>280484</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-03T11:54:38.176</p></td>
<td><p>280483</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-03T08:27:43.575</p></td>
<td><p>280310</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-03T08:25:55.347</p></td>
<td><p>280307</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-05T11:14:31.358</p></td>
<td><p>281684</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-05T11:13:15.850</p></td>
<td><p>281683</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-05T11:09:35.051</p></td>
<td><p>8001</p></td>
<td><p>Die Schweizerische Post (Digitale Belege)</p></td>
<td><p>7224acf5-0e6c-4e0b-9e70-0d7b9cdcb0e9</p></td>
<td><p>FAILED_TO_STORE</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>2026-03-05T11:14:31.358</p></td>
<td><p>281684</p></td>
<td><p>Post CH Kommunikation AG</p></td>
<td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
<td><p>DIGITAL_DELIVERY_FAILED</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

> [!note]- Archived as agreed with Hacka
>
>
> <table style="width:100%;">
> <colgroup>
> <col style="width: 16%" />
> <col style="width: 16%" />
> <col style="width: 16%" />
> <col style="width: 16%" />
> <col style="width: 16%" />
> <col style="width: 16%" />
> </colgroup>
> <tbody>
> <tr>
> <th><p>**Delivery ID**</p></th>
> <th><p>**Sender ID**</p></th>
> <th><p>**Date**</p></th>
> <th><p>**Root cause**</p></th>
> <th><p>**Team Responsible**</p></th>
> <th><p>**Status**</p></th>
> </tr>
> &#10;<tr>
> <td><p>[place](https://cloudlogging.app.goo.gl/14HKZ6dP56gw2AYQA)<br />
> [holder](https://cloudlogging.app.goo.gl/14HKZ6dP56gw2AYQA)</p></td>
> <td></td>
> <td><p>02.09.2025</p></td>
> <td>![[image-20250910-074602.png]]
> <p>When **luz-docs** attempted to call **luz-jsonstore** to update the `_isBeingCreated` field, the request returned a `503 Service Unavailable` response. As a result, the document creation process failed.</p></td>
> <td><ul>
> <li><p>**luz-jsonstore**: The service was unavailable.</p></li>
> <li><p>**luz-docs**: Needs to handle this scenario more gracefully.</p></li>
> </ul>
> <p>When attempting to update the `_isBeingCreated` field, if **luz-docs** receives a `503 Service Unavailable` from **luz-jsonstore**, it should also propagate a `503` response to the client, allowing the client to retry.</p>
> <p>Currently, **luz-docs** returns a `500 Internal Server Error` in this case, which does not accurately reflect the root cause.</p></td>
> <td><p>[LUZ-141031](https://axonivy.atlassian.net/browse/LUZ-141031)</p></td>
> </tr>
> <tr>
> <td><p>[place](https://cloudlogging.app.goo.gl/RrNtiQHYr98YmVWGA)<br />
> [holder](https://cloudlogging.app.goo.gl/RrNtiQHYr98YmVWGA)</p></td>
> <td></td>
> <td><p>03.09.2025</p></td>
> <td>![[image-20250910-075955.png]]
> <p>The call to **luz-jsonstore** failed with a *connection refused* error, resulting in a `503 Service Unavailable` response being returned to the client.</p></td>
> <td><p>**luz-jsonstore**: The service was unavailable.</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>[place](https://cloudlogging.app.goo.gl/8CdtYXykhTmq9pV27)<br />
> [holder](https://cloudlogging.app.goo.gl/8CdtYXykhTmq9pV27)</p></td>
> <td></td>
> <td><p>03.09.2025</p></td>
> <td>![[image-20250910-083655.png]]
> <p>No requests were found in either **luz-docs-view-controller-batch** or **luz-docs-batch** at this timestamp.</p>
> <p>Update info: There was an error reading timeout during the call to luz_jsonstore [https://cloudlogging.app.goo.gl/1fQJxnFhU4sscKZ69](https://cloudlogging.app.goo.gl/1fQJxnFhU4sscKZ69)</p>
> ![[image-20250924-031743.png]]![[image-20250924-040359.png]]
> <p>Besides, there was a 500 error code at the luz_antivirus.</p>
> ![[image-20250924-032243.png]]
> <p>The link of log: [https://cloudlogging.app.goo.gl/an8dkWqtxcDtLDe49](https://cloudlogging.app.goo.gl/an8dkWqtxcDtLDe49)</p></td>
> <td><p>**luz-jsonstore**: Read timeout.</p>
> <p>**luz_antivirus**: cannot scan file</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>[195217](https://cloudlogging.app.goo.gl/4QDGayNuxmmXDwnq8)</p></td>
> <td></td>
> <td><p>03.09.2025</p></td>
> <td>![[image-20250915-030603.png]]
> <p>The luz_docs got timeout issue (500) from luz_jsonstore</p>
> <p>[luz_docs](https://cloudlogging.app.goo.gl/zvS3QLkGDG4MW7k37)</p></td>
> <td><p>luz_jsonstore</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>373</p></td>
> <td><p>4d10f5ba-46c4-412e-8bbf-246eb01914ad</p></td>
> <td><p>05.09.2025</p></td>
> <td></td>
> <td></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204543</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td rowspan="11">![[image-20250910-072144.png]]
> <p>At the time of the failure, **luz-docs** attempted to call **luz-jsonstore** to persist metadata, but received a `503 Service Unavailable` response.</p>
> <p>We suspect that some **luz-jsonstore** pods were unavailable at that moment.</p>
> ![[image-20250910-072248.png]]</td>
> <td rowspan="11"><p>Luz-jsonstore</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204532</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204518</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204495</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204525</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204528</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204511</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204491</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204487</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204500</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>204448</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td><p>09.09.2025</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>199822</p></td>
> <td></td>
> <td><p>10.09.2025</p></td>
> <td>![[image-20250916-045835.png]]
> <p>~~No requests were found in either~~ **~~luz-docs-view-controller-batch~~** ~~or~~ **~~luz-docs-batch~~** ~~at this timestamp.~~</p>
> <p>**Update on 24.09**: after checking, the root cause is that luz-antivirus was not available.</p>
> ![[image-20250924-031239.png]]</td>
> <td><p>~~Luz-eletter should check~~</p>
> <p>luz-antivirus</p></td>
> <td><p>`OPEN`</p></td>
> </tr>
> <tr>
> <td><p>682</p></td>
> <td><p>5e75d0ae-6605-48c2-b87c-e3d3aad71647</p></td>
> <td><p>~~15.09.2025~~</p>
> <p>**12.09.2025**</p></td>
> <td>![[image-20250930-031740.png]]![[image-20250930-032034.png]]
> <p>The luz_docs returned a 503 code. The link is here [https://cloudlogging.app.goo.gl/R3wytgCKixMC4B6C8](https://cloudlogging.app.goo.gl/R3wytgCKixMC4B6C8)</p>
> ![[image-20250930-032305.png]]</td>
> <td><p>**luz-antivirus**: The service was unavailable</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>764</p></td>
> <td><p>c8af1851-8aab-474e-a99a-fa3cd4e3dd16</p></td>
> <td><p>~~24.09.2025~~</p>
> <p>**23.09.2025**</p></td>
> <td>![[image-20250930-031028.png]]![[image-20250930-031116.png]]
> <p>The luz_docs returned a 503 code. The link is here [https://cloudlogging.app.goo.gl/bYpS4p9mu3TFszxu7](https://cloudlogging.app.goo.gl/bYpS4p9mu3TFszxu7)</p>
> ![[image-20250930-031400.png]]</td>
> <td><p>**luz-antivirus**: The service was unavailable</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>202785</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>24.09.2025</p></td>
> <td>![[image-20250929-074843.png]]
> <p>luz-antivirus was not available.</p></td>
> <td><p>**luz-antivirus**: The service was unavailable.</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>11413</p></td>
> <td><p>c1d4928c-f93b-4771-9bcf-595f5ef11995</p></td>
> <td><p>25.09.2025</p></td>
> <td>![[image-20250929-074333.png]]
> <p>malware file detected</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>205963</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>25.09.2025</p></td>
> <td>![[image-20250929-073534.png]]
> <p>luz-antivirus was not available.</p></td>
> <td><p>**luz-antivirus**: The service was unavailable.</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>45255</p></td>
> <td><p>7c25ffa3-c40e-4781-b3e6-55af4e6b2b48</p></td>
> <td><p>26.09.2025</p></td>
> <td>![[image-20250929-072823.png]]
> <p>luz-antivirus was not available.</p></td>
> <td><p>**luz-antivirus**: The service was unavailable.</p></td>
> <td></td>
> </tr>
> </tbody>
> </table>
>
>
>
> <table style="width:100%;">
> <colgroup>
> <col style="width: 14%" />
> <col style="width: 14%" />
> <col style="width: 14%" />
> <col style="width: 14%" />
> <col style="width: 14%" />
> <col style="width: 14%" />
> <col style="width: 14%" />
> </colgroup>
> <tbody>
> <tr>
> <td><p>Created Time</p></td>
> <td><p>Delivery Id</p></td>
> <td><p>Sender</p></td>
> <td><p>Sender Tenant ID</p></td>
> <td><p>Root Cause</p></td>
> <td><p>Team Responsible</p></td>
> <td><p>Status</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T11:32:46.466</p></td>
> <td><p>433095</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td rowspan="4">![[image-20251017-095951.png]]
> <p>**luz-antivirus**: The service was unavailable.</p></td>
> <td></td>
> <td rowspan="34"><p>`CHECKED`</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T11:32:42.856</p></td>
> <td><p>433094</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T11:32:39.215</p></td>
> <td><p>433093</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-14T11:32:31.405</p></td>
> <td><p>433091</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:01:00.672</p></td>
> <td><p>41024</p></td>
> <td><p>Coiffure Fatos Haxhija</p></td>
> <td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
> <td rowspan="10">![[image-20251017-095759.png]]
> <p>**luz-antivirus**: The service was unavailable.</p></td>
> <td rowspan="10"><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:01:00.266</p></td>
> <td><p>3949</p></td>
> <td><p>Haar-Oase by Andji</p></td>
> <td><p>aa51d319-999f-42ae-92a5-a67b1a199f45</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:00:59.886</p></td>
> <td><p>8942</p></td>
> <td><p>Hoorwärk Ruswil</p></td>
> <td><p>ed22d94e-f346-49ec-8a87-4fbbf52cfba7</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:00:59.797</p></td>
> <td><p>1804</p></td>
> <td><p>GS Xtrem GmbH</p></td>
> <td><p>6ece35fa-5c68-4d1d-a32e-1dcaeda5a279</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:00:59.759</p></td>
> <td><p>11543</p></td>
> <td><p>Velvet Skin by Berisha</p></td>
> <td><p>c1d4928c-f93b-4771-9bcf-595f5ef11995</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:00:59.704</p></td>
> <td><p>41023</p></td>
> <td><p>Coiffure Fatos Haxhija</p></td>
> <td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:00:59.545</p></td>
> <td><p>4936</p></td>
> <td><p>NIcole Stadler</p></td>
> <td><p>7885ff69-21b1-44d9-94f4-7838a72f6cd6</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:00:58.624</p></td>
> <td><p>17591</p></td>
> <td><p>HOUSE OF CUT - Carecci</p></td>
> <td><p>3da908b1-735b-49fe-a599-1fc6ee0b2c36</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T17:00:56.317</p></td>
> <td><p>209246</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> </tr>
> <tr>
> <td><p>2025-10-13T16:00:59.860</p></td>
> <td><p>537</p></td>
> <td><p>neon</p></td>
> <td><p>c75ec61e-d6c2-4c59-81a1-ce964fbbc468</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T15:30:55.265</p></td>
> <td><p>41065</p></td>
> <td><p>Coiffure Fatos Haxhija</p></td>
> <td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
> <td rowspan="4">![[image-20251017-095603.png]]
> <p>**luz-antivirus**: The service was unavailable.</p></td>
> <td rowspan="4"><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T15:30:55.223</p></td>
> <td><p>45906</p></td>
> <td><p>Beauty Angel GmbH</p></td>
> <td><p>7c25ffa3-c40e-4781-b3e6-55af4e6b2b48</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T15:30:55.224</p></td>
> <td><p>17993</p></td>
> <td><p>Coiffure Kopfstand GmbH</p></td>
> <td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T15:30:55.071</p></td>
> <td><p>539</p></td>
> <td><p>neon</p></td>
> <td><p>c75ec61e-d6c2-4c59-81a1-ce964fbbc468</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:31:23.398</p></td>
> <td><p>209391</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td>![[image-20251017-095413.png]]
> <p>**luz-antivirus**: The service was unavailable.</p></td>
> <td><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:30:55.280</p></td>
> <td><p>298</p></td>
> <td><p>Coiffure Hair Shop GmbH (Berikon)</p></td>
> <td><p>05eb2860-d241-4c7a-b869-9966abe05c5a</p></td>
> <td>![[image-20251017-095254.png]]
> <p>**luz-antivirus**: The service was unavailable.</p></td>
> <td><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:01:05.959</p></td>
> <td><p>41062</p></td>
> <td><p>Coiffure Fatos Haxhija</p></td>
> <td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
> <td>![[image-20251017-095131.png]]
> <p>**luz-antivirus**: The service was unavailable.</p></td>
> <td><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:59.306</p></td>
> <td><p>17976</p></td>
> <td><p>Coiffure Kopfstand GmbH</p></td>
> <td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
> <td rowspan="9">![[image-20251017-094929.png]]
> <p>**luz-antivirus**: The service was unavailable.</p></td>
> <td rowspan="9"><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:59.205</p></td>
> <td><p>45902</p></td>
> <td><p>Beauty Angel GmbH</p></td>
> <td><p>7c25ffa3-c40e-4781-b3e6-55af4e6b2b48</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:58.734</p></td>
> <td><p>490</p></td>
> <td><p>Fasola Pneumatici</p></td>
> <td><p>ac388830-f711-43a7-a2f0-f3da09a5b22b</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:58.537</p></td>
> <td><p>162</p></td>
> <td><p>Chez Leslie</p></td>
> <td><p>34ab44a2-137a-490b-9573-f2502e0c7d16</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:57.908</p></td>
> <td><p>5567</p></td>
> <td><p>The Nails Factory</p></td>
> <td><p>7743d9dc-1437-4cfa-a38e-a7b52233462f</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:57.158</p></td>
> <td><p>45901</p></td>
> <td><p>Beauty Angel GmbH</p></td>
> <td><p>7c25ffa3-c40e-4781-b3e6-55af4e6b2b48</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:57.105</p></td>
> <td><p>41061</p></td>
> <td><p>Coiffure Fatos Haxhija</p></td>
> <td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:56.038</p></td>
> <td><p>828</p></td>
> <td><p>Le Coiffeur-Luzern</p></td>
> <td><p>539ea489-b836-4c27-bdd8-198eaa3719bc</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T14:00:55.444</p></td>
> <td><p>8951</p></td>
> <td><p>Brunnmatt Kosmetik GmbH</p></td>
> <td><p>1256c9c8-9eff-4a2f-a635-d7b7525ad2c3</p></td>
> </tr>
> <tr>
> <td><p>2025-10-14T17:30:57.093</p></td>
> <td><p>9274</p></td>
> <td><p>Kampaus GmbH</p></td>
> <td><p>ea1096a6-afaf-4fcf-85e5-a9d5a671c674</p></td>
> <td>![[image-20251017-094652.png]]
> <p>**luz-antivirus**: The service was unavailable.</p></td>
> <td><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-15T16:01:08.258</p></td>
> <td><p>5943</p></td>
> <td><p>Besa Bajrami, Prestige Hair & Makeup</p></td>
> <td><p>7c713bb3-2d1b-4743-92dd-3c68f587daf2</p></td>
> <td rowspan="3"><p>**luz-antivirus**: The service was unavailable.</p>
> ![[image-20251017-093829.png]]</td>
> <td rowspan="3"><p>luz-antivirus</p></td>
> </tr>
> <tr>
> <td><p>2025-10-15T16:01:07.565</p></td>
> <td><p>7258</p></td>
> <td><p>Golden Salon Leonora</p></td>
> <td><p>c81640ad-67fd-4798-b77d-e13cfe139037</p></td>
> </tr>
> <tr>
> <td><p>2025-10-15T16:01:04.821</p></td>
> <td><p>5851</p></td>
> <td><p>Bodyline GmbH</p></td>
> <td><p>6367d96f-ca5a-44ee-af9e-2260f80d987f</p></td>
> </tr>
> <tr>
> <td><p>2025-10-17T09:31:23.972</p></td>
> <td><p>18127</p></td>
> <td><p>Coiffeur Schelbert</p></td>
> <td><p>5c915c45-038f-4677-a57e-9585ddd6f05b</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-16T18:31:29.656</p></td>
> <td><p>41157</p></td>
> <td><p>Coiffure Fatos Haxhija</p></td>
> <td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-16T18:31:19.544</p></td>
> <td><p>338</p></td>
> <td><p>Setpember Vin & Vinyl GmbH</p></td>
> <td><p>a9400538-4ab8-480b-a9c1-ef0e13f48d39</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-17T17:04:12.578</p></td>
> <td><p>210025</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-17T17:03:56.464</p></td>
> <td><p>210024</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-17T15:00:57.208</p></td>
> <td><p>8022</p></td>
> <td><p>Gabriel Hanin, Coiffeurgeschäft</p></td>
> <td><p>64ee6c43-5300-4d0e-99de-d9624ff191aa</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-17T15:00:56.550</p></td>
> <td><p>46042</p></td>
> <td><p>Beauty Angel GmbH</p></td>
> <td><p>7c25ffa3-c40e-4781-b3e6-55af4e6b2b48</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-17T15:00:55.374</p></td>
> <td><p>41203</p></td>
> <td><p>Coiffure Fatos Haxhija</p></td>
> <td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-17T15:00:45.168</p></td>
> <td><p>5155</p></td>
> <td><p>BKW Energie AG</p></td>
> <td><p>b52e07e0-2574-4a9c-ab5b-6d469a3b4320</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T11:30:30.774</p></td>
> <td><p>436592</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T10:30:59.650</p></td>
> <td><p>435740</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T10:30:56.063</p></td>
> <td><p>435739</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T10:30:52.118</p></td>
> <td><p>435738</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T10:30:48.086</p></td>
> <td><p>435737</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T10:30:44.819</p></td>
> <td><p>316</p></td>
> <td><p>Hanami fiori&deco Sagl</p></td>
> <td><p>e3157bcb-a85a-4c31-bd36-fb0b4c47ba4e</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T10:30:44.686</p></td>
> <td><p>435736</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T10:16:40.944</p></td>
> <td><p>435547</p></td>
> <td><p>Publicare AG</p></td>
> <td><p>608a96f8-41fe-4253-9edb-3df8978d7365</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-20T23:00:59.781</p></td>
> <td><p>18393</p></td>
> <td><p>Coiffure Kopfstand GmbH</p></td>
> <td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T14:02:03.618</p></td>
> <td><p>14871</p></td>
> <td><p>Nails & Beauty Marisa Maurer - Huggenberger</p></td>
> <td><p>a54041b0-bc72-476b-ad36-3284fb38410c</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-21T14:01:37.792</p></td>
> <td><p>46180</p></td>
> <td><p>Beauty Angel GmbH</p></td>
> <td><p>7c25ffa3-c40e-4781-b3e6-55af4e6b2b48</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T09:07:41.450</p></td>
> <td><p>213505</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T09:07:12.062</p></td>
> <td><p>213500</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T09:06:49.580</p></td>
> <td><p>213498</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T09:04:19.043</p></td>
> <td><p>213493</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T08:09:45.476</p></td>
> <td><p>213007</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T08:03:18.083</p></td>
> <td><p>212954</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-23T15:31:58.502</p></td>
> <td><p>212076</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-22T15:32:31.117</p></td>
> <td><p>630</p></td>
> <td><p>Fasola Pneumatici</p></td>
> <td><p>ac388830-f711-43a7-a2f0-f3da09a5b22b</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T07:37:15.186</p></td>
> <td><p>212854</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>FAILED_TO_STORE</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-23T19:16:42.689</p></td>
> <td><p>982</p></td>
> <td><p>Märlizauber GmbH</p></td>
> <td><p>b0b20d41-cfe4-49d8-a018-861be74b3596</p></td>
> <td rowspan="7"><p>Antivirus is not available</p>
> ![[image-20251119-103349.png]]</td>
> <td rowspan="7"><p>luz-antivirus</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-23T19:01:04.657</p></td>
> <td><p>292</p></td>
> <td><p>Casaca Reinigung, Maria de Lurdes Ferraz dos Santos Carvalho Casaca</p></td>
> <td><p>02d08ca2-bbd9-433f-8f8a-16ccd51abf89</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-23T18:31:13.724</p></td>
> <td><p>890</p></td>
> <td><p>Le Coiffeur-Luzern</p></td>
> <td><p>539ea489-b836-4c27-bdd8-198eaa3719bc</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-23T17:03:38.487</p></td>
> <td><p>97</p></td>
> <td><p>Voltige Pegasus</p></td>
> <td><p>153d2416-afae-4311-a265-50454cb36a4d</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-23T07:01:22.623</p></td>
> <td><p>19117</p></td>
> <td><p>Nicole's Hair Shop GmbH</p></td>
> <td><p>816697d8-d93a-4763-82f7-8b3ba49ef271</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-23T07:01:22.427</p></td>
> <td><p>18604</p></td>
> <td><p>Coiffure Kopfstand GmbH</p></td>
> <td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-23T07:01:22.161</p></td>
> <td><p>18603</p></td>
> <td><p>Coiffure Kopfstand GmbH</p></td>
> <td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-22T17:31:39.538</p></td>
> <td><p>4990</p></td>
> <td><p>NIcole Stadler</p></td>
> <td><p>7885ff69-21b1-44d9-94f4-7838a72f6cd6</p></td>
> <td rowspan="3"><p>Antivirus is not available</p>
> ![[image-20251119-102734.png]]</td>
> <td rowspan="3"><p>luz-antivirus</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-22T15:32:30.654</p></td>
> <td><p>18581</p></td>
> <td><p>Coiffure Kopfstand GmbH</p></td>
> <td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-22T14:04:03.886</p></td>
> <td><p>211186</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T14:34:07.516</p></td>
> <td><p>5191</p></td>
> <td><p>BKW Energie AG</p></td>
> <td><p>b52e07e0-2574-4a9c-ab5b-6d469a3b4320</p></td>
> <td><p>Antivirus is not available</p>
> ![[image-20251119-102533.png]]</td>
> <td><p>luz-antivirus</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T18:04:38.041</p></td>
> <td><p>4050</p></td>
> <td><p>MAEVA Hair & Skin</p></td>
> <td><p>91e35401-b3b6-460a-8889-00ec87a51d03</p></td>
> <td><p>Antivirus is not available</p>
> ![[image-20251119-102245.png]]</td>
> <td><p>luz-antivirus</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-24T17:04:17.800</p></td>
> <td><p>7378</p></td>
> <td><p>Home of Hair and more GmbH</p></td>
> <td><p>6474bac6-127d-46b5-a1cb-b705b1f218e0</p></td>
> <td><p>Reference file is malware</p>
> ![[image-20251119-091245.png]]</td>
> <td><p>Sender?</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-29T15:17:20.572</p></td>
> <td><p>215433</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>Jsonstore is unavailable</p>
> ![[image-20251119-090614.png]]</td>
> <td><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-10-30T15:17:14.723</p></td>
> <td><p>215712</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td><p>DIGITAL_DELIVERY_FAILED</p></td>
> <td></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:55.468</p></td>
> <td><p>216368</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td rowspan="17"><p>Too many failed call to jsonstore make circuit breaker open</p>
> ![[image-20251119-090217.png]]</td>
> <td rowspan="17"><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:32.866</p></td>
> <td><p>3366</p></td>
> <td><p>Schneider Schriften AG</p></td>
> <td><p>7553fea8-0bde-48c1-a10d-73ee9c62af54</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:44.480</p></td>
> <td><p>216366</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:23.797</p></td>
> <td><p>3365</p></td>
> <td><p>Schneider Schriften AG</p></td>
> <td><p>7553fea8-0bde-48c1-a10d-73ee9c62af54</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:14.189</p></td>
> <td><p>41903</p></td>
> <td><p>Coiffure Fatos Haxhija</p></td>
> <td><p>08cd9b48-c525-4993-a884-f2908450ac6e</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:12.654</p></td>
> <td><p>14615</p></td>
> <td><p>Coiffure Elegance</p></td>
> <td><p>7d3279d7-7b31-43f4-a587-fa360a9b729c</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:12.514</p></td>
> <td><p>974</p></td>
> <td><p>Fasola Pneumatici</p></td>
> <td><p>ac388830-f711-43a7-a2f0-f3da09a5b22b</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:12.343</p></td>
> <td><p>8102</p></td>
> <td><p>Gabriel Hanin, Coiffeurgeschäft</p></td>
> <td><p>64ee6c43-5300-4d0e-99de-d9624ff191aa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:12.513</p></td>
> <td><p>19327</p></td>
> <td><p>Nicole's Hair Shop GmbH</p></td>
> <td><p>816697d8-d93a-4763-82f7-8b3ba49ef271</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:12.078</p></td>
> <td><p>6480</p></td>
> <td><p>Coiffure Hairzog S. Herzog</p></td>
> <td><p>b546618b-dfec-417a-9c32-5ee768739c19</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:08.535</p></td>
> <td><p>3364</p></td>
> <td><p>Schneider Schriften AG</p></td>
> <td><p>7553fea8-0bde-48c1-a10d-73ee9c62af54</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:08.295</p></td>
> <td><p>216367</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:32:07.081</p></td>
> <td><p>216365</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:21:10.251</p></td>
> <td><p>216363</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:19:07.054</p></td>
> <td><p>216362</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:19:04.816</p></td>
> <td><p>1589</p></td>
> <td><p>Coiffure Hair Shop GmbH</p></td>
> <td><p>f9ab51f4-ac9a-428f-955f-892e951214b7</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-04T09:19:10.436</p></td>
> <td><p>17953</p></td>
> <td><p>HOUSE OF CUT - Carecci</p></td>
> <td><p>3da908b1-735b-49fe-a599-1fc6ee0b2c36</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-05T08:09:34.309</p></td>
> <td><p>216616</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td rowspan="4"><p>Too many failed call to jsonstore make circuit breaker open</p>
> ![[image-20251119-085948.png]]</td>
> <td rowspan="4"><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-05T08:09:09.660</p></td>
> <td><p>216615</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-05T08:09:21.460</p></td>
> <td><p>216614</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-05T08:08:04.323</p></td>
> <td><p>216613</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-07T09:16:42.119</p></td>
> <td><p>577</p></td>
> <td><p>ECAB - Etablissement Cantonal des Bâtiments</p></td>
> <td><p>ed6e4832-f53d-46ea-8d84-1b83f7f33d89</p></td>
> <td><p>Jsonstore is unavailable while trying to update document to finish (_isBeingCreated)</p>
> ![[image-20251119-085642.png]]</td>
> <td><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-06T15:16:46.853</p></td>
> <td><p>526</p></td>
> <td><p>Die Schweizerische Post (Digitale Belege)</p></td>
> <td><p>7224acf5-0e6c-4e0b-9e70-0d7b9cdcb0e9</p></td>
> <td><p>Jsonstore is unavailable while trying to update document to finish (_isBeingCreated)</p>
> ![[image-20251119-085642.png]]
> ![[image-20251119-085229.png]]</td>
> <td><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-05T12:16:37.292</p></td>
> <td><p>26689</p></td>
> <td><p>HeyLight AG</p></td>
> <td><p>261c752f-81ad-4a57-bc9d-9070af8feea4</p></td>
> <td><p>Jsonstore is unavailable</p>
> ![[image-20251119-083640.png]]</td>
> <td><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-10T12:11:21.994</p></td>
> <td><p>590</p></td>
> <td><p>ECAB - Etablissement Cantonal des Bâtiments</p></td>
> <td><p>ed6e4832-f53d-46ea-8d84-1b83f7f33d89</p></td>
> <td rowspan="2"><p>Too many failed call to jsonstore make circuit breaker open</p>
> ![[image-20251119-082233.png]]</td>
> <td rowspan="2"><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-10T12:11:11.770</p></td>
> <td><p>589</p></td>
> <td><p>ECAB - Etablissement Cantonal des Bâtiments</p></td>
> <td><p>ed6e4832-f53d-46ea-8d84-1b83f7f33d89</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-10T12:10:56.658</p></td>
> <td><p>3507</p></td>
> <td><p>Golden Elite GmbH</p></td>
> <td><p>88c2d16b-3dfe-4d1a-8e91-738043dad42a</p></td>
> <td rowspan="2"><p>Too many failed call to jsonstore make circuit breaker open</p>
> ![[image-20251119-075905.png]]</td>
> <td rowspan="2"><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-10T12:10:53.314</p></td>
> <td><p>588</p></td>
> <td><p>ECAB - Etablissement Cantonal des Bâtiments</p></td>
> <td><p>ed6e4832-f53d-46ea-8d84-1b83f7f33d89</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-10T12:04:37.348</p></td>
> <td><p>280380</p></td>
> <td><p>ePost Service AG</p></td>
> <td><p>a41bca24-eb53-4828-a4b5-106bd4277427</p></td>
> <td rowspan="2"><p>Too many failed call to jsonstore make circuit breaker open</p>
> ![[image-20251119-073903.png]]</td>
> <td rowspan="2"><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-10T12:04:31.221</p></td>
> <td><p>217599</p></td>
> <td><p>Post CH Kommunikation AG</p></td>
> <td><p>c41c8f19-0044-43d6-ac47-c01c53f95eaa</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-11T18:16:41.742</p></td>
> <td><p>643</p></td>
> <td><p>neon</p></td>
> <td><p>c75ec61e-d6c2-4c59-81a1-ce964fbbc468</p></td>
> <td><p>Can not connect to jsonstore</p>
> ![[image-20251119-072118.png]]</td>
> <td><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> <tr>
> <td><p>2025-11-13T12:16:27.402</p></td>
> <td><p>20211</p></td>
> <td><p>Coiffure Kopfstand GmbH</p></td>
> <td><p>2a6adee3-4d60-4de4-b10c-da85e4977a9b</p></td>
> <td><p>Jsonstore is failed to response</p>
> ![[image-20251119-071837.png]]</td>
> <td><p>luz-jsonstore</p></td>
> <td></td>
> </tr>
> </tbody>
> </table>
>
>

%% ai-graph-start %%

**Related notes:**
- [[Part A - luz-jsonstore Analysis]]
- [[Case Report - DocumentId 218735]]
- [[Case Report - DocumentId 458224]]
- [[Case Report - DocumentId 27748]]
- [[Case Report - DocumentId 1741]]

%% ai-graph-end %%