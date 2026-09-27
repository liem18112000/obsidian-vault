---
title: "SSL certificate API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519060767/SSL+certificate+API
space: "LUZ"
topic: security
relevance: 0.794
depth: 2.9
updated: 2020-11-23
attachments: 12
tags:
  - confluence
  - security
  - space/luz
---

# SSL certificate API

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-11-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519060767/SSL+certificate+API)
> Relevance 0.794 · topic `security`

1. SSL Certificate Validation Info: <a href="https://wiki.hexonet.net/wiki/SSL_Certificate_Validation_Info" class="external-link" rel="nofollow">https://wiki.hexonet.net/wiki/SSL_Certificate_Validation_Info</a>  
  
    - We will use the "Domain validated SSL certificates (DV)" with "Validation method URL".

<span class="mw-headline">2. Order and get SSL certificate status API: <a href="https://wiki.hexonet.net/wiki/SSL#tab=Other_commands__28API_29" class="external-link" rel="nofollow">https://wiki.hexonet.net/wiki/SSL#tab=Other_commands__28API_29</a></span>

### <span class="mw-headline">**Order an SSL certificate**</span>

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td style="text-align: left;">Request</td>
<td style="text-align: left;"><p><span><span><span class="nolink"><a href="https://api.ispapi.net/api/call.cgi?s_entity=1234&amp;" class="external-link" rel="nofollow" style="text-decoration: none;">https://api.ispapi.net/api/call.cgi?s_entity=1234&amp;</a> </span>s_login=</span><span> </span></span><span class="resolvedVariable" style="text-decoration: none;"><span>hcmc-hacka</span><span> </span></span><span><span>&amp;s_pw=</span><span> </span></span><span class="resolvedVariable" style="text-decoration: none;"><span>Hacka2019</span></span><span><span>&amp;command=CreateSSLCert&amp;sslcertclass=COMODO_ESSENTIALSSL&amp;csr0=-----BEGIN NEW CERTIFICATE REQUEST-----&amp;csr1=MIIC7TCCAdUCAQAweDELMAkGA1UEBhMCQ0gxEDAOBgNVBAgTB0x1Y2VybmUxGDAW&amp;csr2=BgNVBAcTD1dpbGhlbG1zaG9laGUgMTESMBAGA1UEChMJVGlnZXIgRnV0MRIwEAYD&amp;csr3=VQQLEwlUaWdlciBGdXQxFTATBgNVBAMTDHRpZ2VyZnV0LmNvbTCCASIwDQYJKoZI&amp;csr4=hvcNAQEBBQADggEPADCCAQoCggEBAK5zrFWeUOaiU4gX7MAS0h%2BvDy0tgknAzQkE&amp;csr5=cRouNssMcTnggSNl4DIU0gYF4RvXU2BAxnSVaUjnh1K09sqxNPZ35Z9hURbeLYYY&amp;csr6=%2BgGvShpK630540395pmWRW6eVdvoRB9gfwW1dBWC3tBLMm%2FZe0xP4TBA1iKXnFuy&amp;csr7=WBAiWGdHzO8QfBY9%2FhFCXcrvP2SiK3aHJX9%2BpkIBmePWlz3/K8QJf3l1BM6HwNUH&amp;csr8=qwmQPZvsFXNOR2NlF72WGmssmy0O7dujtl8ZNQUpQnmNfq3xGTInX3LaWwD7K8a4&amp;csr9=CJ9GvUfI3Lmusz726PAr6g2WgmkVQUtaXRN8aUCH5gh6RgRKGrMCAwEAAaAwMC4G&amp;csr10=CSqGSIb3DQEJDjEhMB8wHQYDVR0OBBYEFI6lTXes7ftbJU25NASfImKL2ft%2FMA0G&amp;csr11=CSqGSIb3DQEBCwUAA4IBAQBdd4GZ%2BZqwREUdaWNutMoWwwZmY5ouJqkgO51KdNkk&amp;csr12=Mxu5TuZuGhdQ1bNTvvp2C6x6PmsOMShUGsTysvCE6exGdvx2STZLkZXSzEx18p2Y&amp;csr13=3XH6oDFB73QgaQOnDpORR9Tgr3DE26xVZ%2BaYV3yh9qa2HA8dUENHhbrqGruwRKFi&amp;csr14=CGA9B27WNT4pDCyuxV6pJ%2BDZ3uKPzSjq3dBhB2RiPdN9XrvNnCTO2IUmwndleMIw&amp;csr15=u4xbWnByH4zxLSpTdLz2gHHUnVhnv5UgTfU7b%2Fj7XeCUe2KKdHzrVVD%2F42nqsI8W&amp;csr16=yh323cPjkWtQfTrv4rHfchD8YYMcDdHEnZvzWTIQQyiY&amp;csr17=-----END NEW CERTIFICATE REQUEST-----&amp;period=1&amp;validation0=URL&amp;domain0=<a href="http://tigerfut.com" class="external-link" rel="nofollow">tigerfut.com</a>&amp;internaldns=1&amp;ownercontact0=P-KCK2921210&amp;admincontact0=P-KCK2921210&amp;techcontact0=P-KCK2921210&amp;billingcontact0=P-KCK2921210</span></span></p>
<p><strong><span>Command: </span></strong><span>CreateSSLCert</span></p>
<p><strong><span>Required:</span><span> </span></strong><span> s_login, s_pw, command, domain, validation0, billingcontact0, techcontact0, admincontact0, ownercontact0 </span></p>
<p><span>                  (Example: ownercontact0  = P-KCK2921210 =&gt;P-KCK2921210 is the contact ID that we created here: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519060751/Contact+Domain+and+DNS+API">Contact ,Domain and DNS API</a>)</span></p>
<p><span><strong>Should Define: </strong> period = 1 =&gt;It means 1 year period SSL certificate.</span></p>
<p><span>                           internaldns = 1  =&gt;for the case that the user buys the domain from Hexonet. It will automatically add a DNS record to DNS Zone for us.</span></p></td>
</tr>
<tr>
<td style="text-align: left;">Response</td>
<td style="text-align: left;"><div class="content-wrapper">
<p><br />
</p>

![[20519060767-image2020-11-23_17-26-45.png]]

<br />
&#10;

![[20519060767-image2020-11-23_17-28-5.png]]



![[20519060767-image2020-11-23_17-28-39.png]]


</div></td>
</tr>
</tbody>
</table>

</div>

            PROPERTY\[SSLCERTID\]\[0\]=161854   

                    =\> It's used for getting the SSL certificate status later.

            PROPERTY\[VALIDATIONURL\]\[0\]=<a href="http://tigerfut.com/.well-known/pki-validation/1ED06A9B245ACF0A8D38D526F8096FC4.txt" class="external-link" rel="nofollow">http://tigerfut.com/.well-known/pki-validation/1ED06A9B245ACF0A8D38D526F8096FC4.txt</a>

            PROPERTY\[VALIDATIONURLCONTENT\]\[0\]=A83B7B839E7144FCD9E7DB0AF52015F80136CFEEC4E7BDC1350817949C65C3C6 <a href="http://comodoca.com" class="external-link" rel="nofollow">comodoca.com</a>

                   =\> These 2 properties are used for validating the domain name.

### **Get SSL certificate status**

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td style="text-align: left;">Request</td>
<td style="text-align: left;"><p><span><span><span class="nolink"><a href="https://api.ispapi.net/api/call.cgi?s_entity=1234&amp;" class="external-link" rel="nofollow" style="text-decoration: none;">https://api.ispapi.net/api/call.cgi?s_entity=1234&amp;</a> </span>s_login=</span><span> </span></span><span class="resolvedVariable" style="text-decoration: none;"><span>hcmc-hacka</span><span> </span></span><span><span>&amp;s_pw=</span><span> </span></span><span class="resolvedVariable" style="text-decoration: none;"><span>Hacka2019</span></span><span><span>&amp;command=StatusSSLCert&amp;sslcertid=161829</span></span></p>
<p><strong><span>Command:</span> <span> <span>StatusSSLCert</span></span></strong></p>
<p><strong><span>Required:</span><span> </span></strong><span> s_login, s_pw, command, <span>sslcertid</span></span></p></td>
</tr>
<tr>
<td style="text-align: left;">Response</td>
<td style="text-align: left;"><div class="content-wrapper">

![[20519060767-image2020-11-23_17-35-33.png]]



![[20519060767-image2020-11-23_17-36-0.png]]



![[20519060767-image2020-11-23_17-36-28.png]]



![[20519060767-image2020-11-23_17-36-56.png]]


</div></td>
</tr>
</tbody>
</table>

</div>

If the status is ACTIVE then we can get the SSL certificate from this API.
