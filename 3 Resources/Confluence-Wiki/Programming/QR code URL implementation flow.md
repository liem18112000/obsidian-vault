---
ai_hash: ef7c58b8bfd9f30e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 11
depth: 3
entities: []
relevance: 0.852
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46999142753/QR+code+URL+implementation+flow
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: QR code/URL implementation flow
topic: programming
type: source
updated: 2022-03-22
---

# QR code/URL implementation flow

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-03-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46999142753/QR+code+URL+implementation+flow)
> Relevance 0.852 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="94e43e3d-59e7-4aac-aec7-c301772c7013" macro-name="toc">

</div>

# QR code/URL creation

To generate a URL/QR code, the sender needs to make a request to our Public API


![[46999142753-api.png]]



**To use this API, the sender must have a running subscription of the Branded Folder widget.**

Based on the **Accept** header in the sender’s request, we will return either the **URL with “text/html” media type**,  
or **the QR code with “image/png” media type**.

The sender has the option to encrypt the token (with the query param **encrypt-token**) in the returned URL/QR code if it contains sensitive user credentials which is used for matching recipient and adding credential use cases.

The below table describes the acceptable input request body for the API and the expected output:  
Green = implemented, Red = not implemented yet

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Acceptable request body content</strong></p></th>
<th><p><strong>Output &amp; Use-case</strong></p></th>
<th><p><strong>Output token payload</strong></p></th>
</tr>
&#10;<tr>
<td><p>Empty</p></td>
<td><p>Use-case: Download/Redirect the App (by default)</p>
<p>When the URL/QR code is clicked/scanned, the user is redirected to the App store (if app not yet installed) or the app itself.</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="20174adf-edca-4ef8-a4cf-9cd2ddc6149f" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;iss&quot;: &quot;https://api.klara.ch&quot;,
    &quot;exp&quot;: 4102358400,
    &quot;iat&quot;: 1631173730,
    &quot;jti&quot;: &quot;4b4101de-d9e4-4fb7-924d-5f4c4771a1a2&quot;
}</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5d847e65-d2af-4692-88ac-17e351232c5d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;pinBrandedFolder&quot;: true
}</code></pre>
</div>
</div></td>
<td><p>Use-case: Download/Redirect the App (by default)<br />
<span class="inline-comment-marker" data-ref="54d439dd-24b3-449f-943b-a620e4989ab5">Use-case: Pin the branded folder</span></p>
<p><span class="inline-comment-marker" data-ref="92a71ea9-f9a9-41da-9d70-06d3881370e8">If </span><span class="inline-comment-marker" data-ref="92a71ea9-f9a9-41da-9d70-06d3881370e8"><code>pinBrandedFolder</code></span><span class="inline-comment-marker" data-ref="92a71ea9-f9a9-41da-9d70-06d3881370e8"> </span>is true then when scanned, the branded folder of the sender will be pinned in the recipient app. Ohterwise, nothing will be executed.</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="edf71112-591f-452e-9d32-530fdc87862d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;pinBrandedFolder&quot;: {
        &quot;senderTenantId&quot;: &quot;4b4101de-d9e4-4fb7-924d-5f4c4771a1a2&quot;
    },
    &quot;iss&quot;: &quot;https://api.klara.ch&quot;,
    &quot;exp&quot;: 4102358400,
    &quot;iat&quot;: 1631173730,
    &quot;jti&quot;: &quot;4b4101de-d9e4-4fb7-924d-5f4c4771a1a2&quot;
}</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="7ac58f99-11df-49c5-8a5e-9e71147c34d3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;addingCredential&quot;: {
    &quot;address&quot;: {
      &quot;additionalAddress&quot;: &quot;additional address&quot;,
      &quot;city&quot;: &quot;3000&quot;,
      &quot;firstName&quot;: &quot;Peter&quot;,
      &quot;lastName&quot;: &quot;Muster&quot;,
      &quot;postOfficeBoxNumber&quot;: &quot;12&quot;,
      &quot;postOfficeBoxTownName&quot;: &quot;Vervey 1&quot;,
      &quot;postOfficeBoxZip&quot;: &quot;1212&quot;,
      &quot;street&quot;: &quot;Bahnhofstrasse&quot;,
      &quot;streetNumber&quot;: &quot;20&quot;,
      &quot;type&quot;: &quot;PO_BOX&quot;,
      &quot;zipCode&quot;: &quot;3000&quot;
    },
    &quot;credentials&quot;: [
      {
        &quot;name&quot;: &quot;EMAIL&quot;,
        &quot;value&quot;: &quot;peter.muster@gmail.com&quot;
      }
    ]
  }
}</code></pre>
</div>
</div></td>
<td><p>Use-case: Download/Redirect the App (by default)<br />
Use-case: Add credentials to user profile</p>
<p>The output token will contain the content of the request body.</p>
<p>Use-case: A sender wants the user to add additional public or sender-specific credentials like a customer number or staff number to increase the accuracy of the matching process</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="920a51a3-a85d-4fd9-8424-d18202b2bb99" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;credentialData&quot;: {
        &quot;address&quot;: {
            &quot;additionalAddress&quot;: &quot;Cu Chi 456&quot;,
            &quot;city&quot;: &quot;Zurich&quot;,
            &quot;firstName&quot;: &quot;Peter&quot;,
            &quot;lastName&quot;: &quot;Muster&quot;,
            &quot;street&quot;: &quot;Bahnhofstrasse&quot;,
            &quot;streetNumber&quot;: &quot;24/3&quot;,
            &quot;type&quot;: &quot;POSTAL&quot;,
            &quot;zipCode&quot;: &quot;3000&quot;
        },
        &quot;credentials&quot;: [
            {
                &quot;name&quot;: &quot;EMAIL&quot;,
                &quot;value&quot;: &quot;peter.muster@gmail.com&quot;
            },
            {
                &quot;name&quot;: &quot;MOBILE&quot;,
                &quot;value&quot;: &quot;+84909112233&quot;
            }
        ]
    }
}</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

# QR code/URL usage

## QR code usage


![[46999142753-qr-code-redirect.png]]



**(1)** The user scan the QR code with their device (can be scanned with any app capable of scanning QR codes)

**(2)** The QR code is translated to an URL


![[46999142753-qr-code-redirect-2.png]]



**(3)** The URL is then opened in the web browser on the user’s device  
Then the whole flow of URL usage in the section below is executed.

## URL usage

**Context: The myLife/ePost app is not yet installed on user’s device**


![[46999142753-redirect-not-install (1)-1.png]]



**(1)** <span class="inline-comment-marker" ref="64d92562-1675-40e7-bbb1-1deee8725ddd">The URL generated by the Public API is entered into the web browser of the device</span>

**(2)** The browser send a request to the module behind the URL, luz-public-api-adapter

**(3)** luz-public-api-adapter perform some checks:

1.  Look for the user agent in the request to see if it is a iOS or Android device  
    Implementation hint: <a href="https://stackoverflow.com/a/21742107" class="external-link" data-card-appearance="inline" rel="nofollow">https://stackoverflow.com/a/21742107</a>

2.  Try to redirect to the ePost/myLife app in the device (will fail because app not installed)  
    Implementation hint: <a href="https://stackoverflow.com/a/2958870" class="external-link" data-card-appearance="inline" rel="nofollow">https://stackoverflow.com/a/2958870</a>  
    <a href="https://stackoverflow.com/a/13196998" class="external-link" data-card-appearance="inline" rel="nofollow">https://stackoverflow.com/a/13196998</a>

3.  If failed, redirect to a App store URL for the app

**(4)** <span class="inline-comment-marker" ref="fca986f0-7cd3-4668-acc9-fbfb790e99f7">luz-public-api-adapter returns the App store URL for the app</span>


![[46999142753-redirect-not-install-2.png]]



**(5)** The App store URL for the app is automatically entered in the web browser as a result of the redirection


![[46999142753-redirect-not-install-3.png]]



**(6)** The browser detects the App store URL and redirect to the App store so the user can download the app.

**Context: The myLife/ePost app is installed on user’s device**


![[46999142753-redirect-app-installed-2.png]]



**(1)** The URL generated by the Public API is entered into the web browser of the device

**(2)** The browser send a request to the module behind the URL, luz-public-api-adapter

**(3)** luz-public-api-adapter perform some checks:

1.  Look for the user agent in the request to see if it is a iOS or Android device  
    Implementation hint: <a href="https://stackoverflow.com/a/21742107" class="external-link" data-card-appearance="inline" rel="nofollow">https://stackoverflow.com/a/21742107</a>

2.  Try to redirect to the ePost/myLife app in the device (will succeed because app installed)  
    Implementation hint: <a href="https://stackoverflow.com/a/2958870" class="external-link" data-card-appearance="inline" rel="nofollow">https://stackoverflow.com/a/2958870</a>  
    <a href="https://stackoverflow.com/a/13196998" class="external-link" data-card-appearance="inline" rel="nofollow">https://stackoverflow.com/a/13196998</a>

3.  If failed, redirect to a App store URL for the app

**(4)** luz-public-api-adapter returns the app unique uri to the browser


![[46999142753-redirect-app-installed-3.png]]



**(5)** The browser redirects to the app

# E<span class="inline-comment-marker" ref="10624ca5-21e6-4fa8-bde9-a9f8a9ef34c7">xecute QR code/URL action</span>


![[46999142753-image_2021_11_18T04_45_31_007Z.png]]



**(1)** After the ePost/myLife is opened as a result of the redirection from the URL, it can still possibly keep the URL and its query param (token) (base on: <a href="https://stackoverflow.com/a/2958870" class="external-link" data-card-appearance="inline" rel="nofollow">https://stackoverflow.com/a/2958870</a> , need clarificaton from Optimus team)

**(2)** The app then make a POST request to the mobile adapter module carrying the token receieved at the app.

**(3)** <span class="inline-comment-marker" ref="86f9f90e-e9c1-45ad-94c5-869184d38169">Because we are now in a authenticated context (from App with a logged-in user, valid refresh token)</span>, the mobile app adapter then makes a POST request to an internal module for a specific tenant carrying the same token from the app and also a tenant specific access token.

**(4)** The internal module receives the token and decrypt the token if needed.  
Then verify the token to see if it is issued by KLARA or not

**(5)** If the token is valid, then we extract the token content

**(6)** And based on the token content, we verify it and execute the corressponding action (pin a branded folder, or add a credential,…)

High level user flow:

<a href="https://www.figma.com/file/MWqHGxm6skwTbRJ6scZWkY/Pin-Branded-Folder?node-id=0%3A1" class="external-link" data-card-appearance="embed" data-width="100.00" rel="nofollow">https://www.figma.com/file/MWqHGxm6skwTbRJ6scZWkY/Pin-Branded-Folder?node-id=0%3A1</a>

%% ai-graph-start %%

**Related notes:**
- [[Copy 2. QR-Code forwarding to correct app store]]
- [[2. QR-Code forwarding to correct app store]]
- [[Understanding Keycloak Authorization Code flow]]
- [[LUZ-110826 Public API - Widget subscription by activation code]]
- [[KLARA Integration (request access token & call API)]]

%% ai-graph-end %%