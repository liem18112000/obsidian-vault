---
title: "Hexonet API Document"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508019826/Hexonet+API+Document
space: "LUZ"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2020-10-22
attachments: 5
tags:
  - confluence
  - programming
  - space/luz
---

# Hexonet API Document

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-10-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508019826/Hexonet+API+Document)
> Relevance 0.731 · topic `programming`

## <span class="legacy-color-text-blue3">Environment:</span>

- <span class="legacy-color-text-blue3">The base URL is <a href="https://coreapi.1api.net/api/call.cgi" class="external-link" rel="nofollow">https://coreapi.1api.net/api/call.cgi</a></span>
- <span class="legacy-color-text-blue3">The base URL for OT&E (Testbed System) is: <a href="https://api.ispapi.net/api/call.cgi?s_entity=1234" class="external-link" rel="nofollow">https://api.ispapi.net/api/call.cgi?s_entity=1234</a></span>

## <span class="legacy-color-text-blue3">Hexonet API Documentation</span>

- <span class="legacy-color-text-blue3"><a href="https://www.hexonet.net/sites/default/files/downloads/CORE_API_Reference.pdf" class="external-link" rel="nofollow">https://www.hexonet.net/sites/default/files/downloads/CORE_API_Reference.pdf</a></span>
- <span class="legacy-color-text-blue3"><a href="https://www.hexonet.net/sites/default/files/downloads/DOMAIN_API_Reference.pdf" class="external-link" rel="nofollow">https://www.hexonet.net/sites/default/files/downloads/DOMAIN_API_Reference.pdf</a></span>
- <span class="legacy-color-text-blue3"><a href="https://wiki.hexonet.net/wiki/Main_Page" class="external-link" rel="nofollow">https://wiki.hexonet.net/wiki/Main_Page</a></span>

## <span class="legacy-color-text-blue3">API Calls</span>

### <span class="legacy-color-text-blue3">The user wants to register a new domain</span>

**Submit the request using the following syntax:**

BASE-URL?s_login=<a href="http://reseller.com" class="external-link" rel="nofollow">reseller.com</a>&s_pw=secret&command=command& parameter1=value1&parameter2=value2&…

**Example URL:**

BASE-URL&s_login=\<user_name\>&s_pw=\<password\> &command=AddDomain&domain=<a href="http://testforreseller2.com" class="external-link" rel="nofollow">testforreseller2.com</a>&ownercontact0=P-ABC123&admincontact0=P-ABC123&techcontact0=P-ABC123&billingcontact0=P-ABC123&nameserver0=<a href="http://ns1.hexonet.net" class="external-link" rel="nofollow">ns1.hexonet.net</a>&nameserver1=<a href="http://ns2.hexonet.net" class="external-link" rel="nofollow">ns2.hexonet.net</a>

  

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th>Parameter</th>
<th>Obligation</th>
<th>Definition</th>
<th>Type</th>
</tr>
&#10;<tr>
<td><span>s_login</span></td>
<td>required</td>
<td>Login ID of the user account</td>
<td>TEXT</td>
</tr>
<tr>
<td><span>s_pw</span></td>
<td>required</td>
<td>Account password</td>
<td>TEXT</td>
</tr>
<tr>
<td><span>command</span></td>
<td>required</td>
<td>Name of command to be executed</td>
<td>TEXT</td>
</tr>
<tr>
<td><span>domain</span></td>
<td>required</td>
<td>Name of the domain</td>
<td>DOMAIN</td>
</tr>
<tr>
<td><span>ownercontactN</span></td>
<td>required</td>
<td>Owner/Registrant-Contact(s)</td>
<td>CONTACT [1..1]</td>
</tr>
<tr>
<td><span>admincontactN</span></td>
<td>required</td>
<td>Admin-Contact(s)</td>
<td>CONTACT [1..3]</td>
</tr>
<tr>
<td><span>techcontactN</span></td>
<td>required</td>
<td>Tech-Contact(s)</td>
<td>CONTACT [1..3]</td>
</tr>
<tr>
<td><span>billingcontactN</span></td>
<td>required</td>
<td>Billing-Contact(s)</td>
<td>CONTACT [1..3]</td>
</tr>
<tr>
<td><span>nameserverN</span></td>
<td>optional</td>
<td>Nameserver hostnames</td>
<td>NAMESERVER<br />
[0..12]</td>
</tr>
</tbody>
</table>

</div>

**Response When Domin is already Registered :**

\[RESPONSE\]  
DESCRIPTION=Attribute value is not unique  
CODE=540

QUEUETIME=0  
RUNTIME=0.004

EOF

**Response When Username or Password do not match :**

\[RESPONSE\]  
CODE=530  
DESCRIPTION=Authentication failed  
QUEUETIME=0  
RUNTIME=0.001

EOF

**Response When Call is a success :**

\[RESPONSE\]  
PROPERTY\[FINALIZATIONDATE\]\[0\]=2022-05-08 06:51:10  
PROPERTY\[PAIDUNTILDATE\]\[0\]=2022-05-07 06:51:10  
PROPERTY\[FAILUREDATE\]\[0\]=2022-06-20 06:51:10  
PROPERTY\[STATUS\]\[0\]=ACTIVE  
PROPERTY\[ACCOUNTINGDATE\]\[0\]=2022-05-02 06:51:10  
PROPERTY\[REGISTRATIONEXPIRATIONDATE\]\[0\]=2022-05-07 06:51:10  
DESCRIPTION=Command completed successfully  
CODE=200  
QUEUETIME=0  
RUNTIME=0.242

EOF

### <span class="legacy-color-text-blue3">The user wants to transfer a domain to hexonet</span>

**<span class="legacy-color-text-blue3">Submit the request using the following syntax:</span>**

BASE-URL?s_login=<a href="http://reseller.com" class="external-link" rel="nofollow">reseller.com</a>&s_pw=secret&command=command& parameter1=value1&parameter2=value2&parameter3=value3...

**Example URL:**

BASE-URL& s_login=\<user_name\>&s_pw=\<password\>&command=TransferDomain&domain=<a href="http://testforreseller.ch" class="external-link" rel="nofollow">testforreseller.ch</a>&action=usertransfer&auth=GZnjeQFzA9

**Note:**

- In above URL we have used *action=usertransfer* as we were transferring this to another hexonet test account for external registrar use *action=REQUEST*.
- Check that the Domain you are requesting for transfer is unlocked.

  

<div>

|           |            |                                |        |
|-----------|------------|--------------------------------|--------|
| Parameter | Obligation | Definition                     | Type   |
| s_login   | required   | Login ID of the user account   | TEXT   |
| s_pw      | required   | Account password               | TEXT   |
| command   | required   | Name of command to be executed | TEXT   |
| domain    | required   | Name of the domain             | DOMAIN |
| auth      | required   | Auth-Info code of the domain   | TEXT   |

</div>

**Response when the domain is locked:**

\[RESPONSE\]  
DESCRIPTION=Command failed; transfer not allowed  
CODE=549

QUEUETIME=0  
RUNTIME=0.378

EOF

**Response When Username or Password do not match :**

\[RESPONSE\]  
CODE=530  
DESCRIPTION=Authentication failed  
QUEUETIME=0  
RUNTIME=0.001

EOF

**Response When Call is a success :**

\[RESPONSE\]  
PENDING=1  
DESCRIPTION=Command completed successfully  
CODE=200  
QUEUETIME=0  
RUNTIME=0.242

EOF

### <span class="legacy-color-text-blue3">Check for domain availability</span>

**<span class="legacy-color-text-blue3">Submit the request using the following syntax:</span>**

BASE-URL?s_login=<a href="http://reseller.com" class="external-link" rel="nofollow">reseller.com</a>&s_pw=secret&command=command& parameter1=value1&parameter2=value2&parameter3=value3...

**Example URL:**

BASE-URL&s_login=\<user_name\>&s_pw=\<password\>&domain=reseller.com&command=CheckDomain

<div>

|           |            |                                |        |
|-----------|------------|--------------------------------|--------|
| Parameter | Obligation | Definition                     | Type   |
| s_login   | required   | Login ID of the user account   | TEXT   |
| s_pw      | required   | Account password               | TEXT   |
| command   | required   | Name of command to be executed | TEXT   |
| domain    | required   | Name of the domain             | DOMAIN |

</div>

**Response when the domain is already taken / not available :**

\[RESPONSE\]  
PROPERTY\[REASON\]\[0\]=Domain exists  
PROPERTY\[CLASS\]\[0\]=  
DESCRIPTION=Not Available  
QUEUETIME=0  
CODE=211  
RUNTIME=0.101

QUEUETIME=0  
RUNTIME=0.119

EOF

**Response When Username or Password do not match :**

\[RESPONSE\]  
CODE=530  
DESCRIPTION=Authentication failed  
QUEUETIME=0  
RUNTIME=0.001

EOF

**Response when the domain is not taken /available :**

\[RESPONSE\]  
PROPERTY\[REASON\]\[0\]=  
PROPERTY\[CLASS\]\[0\]=  
DESCRIPTION=Available  
QUEUETIME=0  
CODE=210  
RUNTIME=0.1

QUEUETIME=0  
RUNTIME=0.118

EOF

  

**Returned Properties and Values**

<div>

|      |                                   |
|------|-----------------------------------|
| Code | Description                       |
| 200  | Command completed successfully    |
| 541  | The command failed                |
| 540  | The attribute value is not unique |
| 530  | Authentication failed             |
| 210  | Available                         |
| 211  | Not Available                     |

</div>

## Flow Chart :

<span class="legacy-color-text-blue3">The user wants to register a new domain</span>

  

<span class="legacy-color-text-blue3">

![[20508019826-Hexonet Domain Registration Chart.png]]

</span>

  

<span class="legacy-color-text-blue3">The user wants to transfer a domain to hexonet</span>

<span class="legacy-color-text-blue3">

![[20508019826-Hexonet Domain Transfer Chart.png]]

</span>
