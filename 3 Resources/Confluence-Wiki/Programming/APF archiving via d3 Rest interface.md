---
title: "APF archiving via d3 Rest interface"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/47045346268/APF+archiving+via+d3+Rest+interface
space: "X4"
topic: programming
relevance: 0.804
depth: 2.84
updated: 2022-07-27
attachments: 7
tags:
  - confluence
  - programming
  - space/x4
---

# APF archiving via d3 Rest interface

> [!info] Imported from Confluence
> Space **X4** · updated 2022-07-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/47045346268/APF+archiving+via+d3+Rest+interface)
> Relevance 0.804 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="958d05b280eae3e09b5187e9c78ee8a6" macro-name="toc">

</div>

# General

D.velop cancels support of the web service as the interface to upload documents from third party tool to d3 server at 2023. Therefore APF team *singleton* has already implemented the interface over the d3 REST API like described in user stories <a href="https://axonivy.atlassian.net/wiki/pages/resumedraft.action?draftId=47045346268" rel="nofollow">AD-3729</a>, <a href="https://axonivy.atlassian.net/browse/AD-4349" class="external-link" rel="nofollow">AD-4349</a>, <a href="https://axonivy.atlassian.net/browse/AD-4269" class="external-link" rel="nofollow">AD-4269</a> and <a href="https://axonivy.atlassian.net/browse/AD-4352" class="external-link" rel="nofollow">AD-4352</a>. This implementation increases the cloud computing for an APF 4.x installation on a new level. It’s now possible to transfer data secure over the internet and the mapping of the d3 properties to archive or update documents is much more comfortable and clearer with the help of d3 sources. Furthermore this feature will have fully equiped two modes for the mapping and the mapping via webservice is still supported.

# Installation Guide of APF version v4.2.3 or higher

For saving a workflow configuration inclusive a d3 source data for d3 mapping validation it’s necessary to modify data base table *AdditionalProperty.* Please check value size of column *value* before installing APF artifacts (eapf_web, eapf_service, eapf_archived_d3, eapf_customer or eapf_customer_d3_xline) which supports archiving via d3 REST. It must be **varchar(MAX)**.

If not do following steps:

1.  You have to adapt the data base table *AdditionalProperty* by executing stored procedure below if you upgrade to this version.

2.  Stop ivy engine and wildfly

3.  **Please make a backup of the XAPF data base** before you adapt the data base table if something will go wrong otherwise you will loose parts of the work flow configuration.

4.  With following skript you can create a store procedure. Please study script before executing. Feel free to adapt it for your data base server.

5.  Please check data type of column *processStepId*. (See following SQL scripts)

6.  Restart wildfly and ivy egine

***Remark:***

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2942bf03-2bed-426b-b65c-d92f64c84301" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
ALTER TABLE [YOUR_XAPF_DATA_BASE].dbo.AdditionalProperty ALTER COLUMN value varchar(MAX) NULL;
```

</div>

</div>

  
If everything is fine now you can deploy the APF artifacts.

# Work flow configuration for archiving with REST

## Get access to d3 repository

Before you configure the access you need to have a d3 API KEY and a valid base URL of the d3 Server.

<div hasbody="true" macro-id="1c0cfecc-167e-4b33-a8d4-66466a3d6244" macro-name="tip">

<span class="aui-icon aui-icon-small aui-iconfont-approve confluence-information-macro-icon"> </span>

<div>

Example of a valid base URL: <a href="https://d3.soad.ch/" class="external-link" rel="nofollow">https://d3.soad.ch/</a>

</div>

</div>

If you have no API KEY you have to generate it for the user you want to give access. This can be done with the d3.one web service.

Please open:

<a href="https://d3.soad.ch/identityprovider/config/apikey/create" class="external-link" rel="nofollow">https://[host:port]/identityprovider/config/apikey/create</a>

for example:

<a href="https://d3.soad.ch/identityprovider/config/apikey/create" class="external-link" rel="nofollow">https://d3.soad.ch/identityprovider/config/apikey/create</a>


![[47045346268-grafik-20220204-150705.png]]



This API Key is the better described a **BEARER token**. This token must be pasted into workflow configuration to get access to the d3 repository.

If base url and a valid token is entered you have to save workflow to get registered repositories of d3 service.

Following picture illustrates a successful d3 configuration to d3 server on host <a href="http://d3.soad.ch" class="external-link" rel="nofollow"><em>d3.soad.ch</em></a> *(IP:52.59.86.148)*


![[47045346268-grafik-20220204-151426.png]]



## Secure Transmission

If you want have a secure transmission you can enter file path to the certificate. This file must end with \*.pem file with for example a content like following: (It’s the public key!)

<div hasbody="true" macro-id="c4e2a8de-402f-4ca4-a120-54e3216dc438" macro-name="tip">

<span class="aui-icon aui-icon-small aui-iconfont-approve confluence-information-macro-icon"> </span>

<div>

-----BEGIN CERTIFICATE-----  
MIIEVzCCAj+gAwIBAgIQAMVArSZZqbEyyBMQ6VyVczANBgkqhkiG9w0BAQsFADBJ  
MScwJQYDVQQFEx44Njc2ZWY0MzliNTRiZjk0YjM3OTBkZjMwOGUwYzcxHjAcBgNV  
BAMMFWQudmVsb3AgY3VzdG9tZXIgcm9vdDAeFw0yMTExMjMwMDAwMDBaFw0zMTEx  
MjMwMDAwMDBaMBYxFDASBgNVBAMMC2RlbW8tc2VydmVyMIIBIjANBgkqhkiG9w0B  
AQEFAAOCAQ8AMIIBCgKCAQEAwa7hChNKOZ62vKtpN3RF+LsOMUo1RtukdpNM1+4E  
1rKs/tFJ6sXGF8tCtoO97Bg3GJky9fnFcDpmu0ZnNt12VvKfSUFCcd99Lfl2SZEo  
QME3I8qaWPX8DRjnv7ho9csw17pnoxLLwQgiiJU0HZ3LYhiexPux81QZNhCnqGjZ  
zXv8n7ShaPPUaUTOwLgTtwRenbAt1KgeIr7XYVIRZLgGvUMR+Din4zt+37u/dRns  
OVIFY3o+rzQooPCBw5coVHT0PffFnp7clVZiP0luy0a5DDf8WPCXXfYl5qC7p+CY  
VoMislnIF81NcCZ63gI5KExZGFXBeRzKuNj7TpHAiLJnpQIDAQABo24wbDAWBgNV  
HREEDzANggtkZW1vLXNlcnZlcjAMBgNVHRMBAf8EAjAAMB8GA1UdIwQYMBaAFARl  
AlAYFMqKv3X1OkKqtluiRUZaMA4GA1UdDwEB/wQEAwIFoDATBgNVHSUEDDAKBggr  
BgEFBQcDATANBgkqhkiG9w0BAQsFAAOCAgEAcQEFXlIAZciRJ4f0RWesJHQ4duzl  
uMqaTTYjtiRWLn+3f4iGA9BQNFH2tdZR+FJP/ejxE71g4Vnp00jXvnvqHsU/LifF  
65+n3nzW5xHGSGMO2FdeAfAsuz//JDq0ePJroXWDT8P1/YVj5bTHehkvfijPZwEE  
PspENNl6tFEWoni/Z0kEN4UhEfpLhfoThSt+iLv9tcA4CExonPVnzsuh5rE351Vj  
KAVjCwcVvecIV6Vx9MZBxM0U4h+x/xG/Cdcw/L5DGwsR0c1VFJ/ru+aApayE2zZ9  
nIz1eMXB5wH5TTvGrbwuN9T12t6XirF7tjUg/C+6wrKQjmR54aLWwcrOIXaM85W1  
Z7WDy7w0B3aMgm9/f9pciJeyr+nRAwsRIuG6GBCAjYNNMr7SJCrJ3VpOnjJnEFpY  
izEtE1F0FVDFFpMp+NxS1xROa18zDtEo7f6WFGrNdsdwdRcBmVpy1FsDpZ+Yqcfw  
Wl3pkxopXpi/z0tebYhVeQYpot/xbi4dwh//zbUJIQDU5BHe9g8aistkZDUbKufr  
KcX27XwKMOd0D2yfC1KGAB87wCPTW0fmWShvUp8VjOxuYie8zM1IyS2vuMvv6yME  
tEk2M9Rr+MPMo13kLzoPGkNPJkmktFMm7i9Y8M3I8OKkUZ3qsGPoe99QzGtAjl57  
8SsUZvtHu5A3lxU=  
-----END CERTIFICATE-----

</div>

</div>

# D3 Mappings

## Mode 1: Upload and download documents direct from APF to d3 server

### General

The support of mode 1 is implemented by AD-3729. The mapping is done on host where APF is running.

### Pre conditions for the mapping item head fields with configuration files and d3 source:

1.  Succesfully configured link to a d3 repository

2.  Basic d3 knowlegde for managing document meta data.

3.  Two mapping files: You still need two files for mapping item head parameters as property of a archived document. Please copy files *dokDatFilter.json* and *dokDataFieldMapping.json* you have for archiving via web service if you update an existing installation.

4.  Default d3 source: Any installation of the d3 server must have a default d3 source configured. This default source can be download the by following console command.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="14262e2d-18b0-49d7-bb90-c168168f5db5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl -H 'Accept: application/hal+json' -H 'Authorization:  Bearer [API_KEY]'  -k -X GET  https://[host:port(optional)]/dms/r/[repoId]/source
```

</div>

</div>

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="980d3f62-71b7-4cc3-ba88-5183113cb1a4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl -H 'Accept: application/hal+json' -H 'Authorization:  Bearer QsiHLA+XXcjNGRxfFRcNayqtIQf8GaJK0K1Nxh0vFN+7Zb70xfa/gX7DMve2LCHRZ9eFfqJM2fMPTRS2h+OL1RkNh2N7VyFaG4MxRucX9ZrWWT0hRU9iW+9fCzqzQpKF&_z_A0V5ayCSubffFjDthSuB5EsNH3s935wxf-uzHxO8D3uRWOsiwnZQ52-7LkCVps3RTN44w6_SLMfgi8J3XfUuroxjlyHva'  -k -X GET  https://d3.soad.ch/dms/r/ea11f740-da49-5fe3-b93e-a30f9580d417/
```

</div>

</div>

If you get error error back from d3 please ask d3 admistrator to fix default mapping on d3 otherwise archiving via d3 is not possible.

The same JSON can also be found with pasting the Link (according to `https://[host:port(optional)]/dms/r/[repoId]/source`) in your Browser.

### Select a d3 source in the workflow configuration

The d3 source selection is needed to validate settings in the prepared mapping files. Please select the *source id* to have *property* and *category* **keys** for the validation by uploading or refreshing the properties of a document on d3. The default *source id* should be automatically listed in the workflow configuration.


![[47045346268-grafik-20220204-152852.png]]



Optional: For advanced cloud computing you have the possibility to configure more d3 sources. This offers you the possibility of different mappings for a current linked d3 repository. You also need this if your are using mode 2.

Example of valid source mapping file:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c57dfdd5-ebaf-449c-8712-3a4f427e6743" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "sources": [{
            "id": "/dms/r/ea11f740-da49-5fe3-b93e-a30f9580d417/source",
            "displayName": "Demoumgebung",
            "properties": [{
                    "key": "1",
                    "type": "String",
                    "displayName": "Barcode"
                }, {
                    "key": "3",
                    "type": "Date",
                    "displayName": "Belegdatum"
                }, {
                    "key": "5",
                    "type": "String",
                    "displayName": "Belegtyp"
                }, {
                    "key": "27",
                    "type": "String",
                    "displayName": "Dokument Nr"
                }, {
                    "key": "13",
                    "type": "String",
                    "displayName": "Kreditor Name"
                }, {
                    "key": "14",
                    "type": "String",
                    "displayName": "Kreditor Nr."
                }, {
                    "key": "30",
                    "type": "String",
                    "displayName": "Debitor Name"
                }, {
                    "key": "31",
                    "type": "String",
                    "displayName": "Debitor Nr."
                }, {
                    "key": "4",
                    "type": "String",
                    "displayName": "Belegnummer"
                }, {
                    "key": "12",
                    "type": "String",
                    "displayName": "Kreditor Auswahl"
                }, {
                    "key": "15",
                    "type": "String",
                    "displayName": "Kreditor Ort"
                }, {
                    "key": "16",
                    "type": "String",
                    "displayName": "Kreditor PLZ"
                }, {
                    "key": "8",
                    "type": "String",
                    "displayName": "ITEMKEY"
                }, {
                    "key": "26",
                    "type": "String",
                    "displayName": "Dokument ID"
                }, {
                    "key": "24",
                    "type": "String",
                    "displayName": "Währung"
                }, {
                    "key": "23",
                    "type": "Money",
                    "displayName": "Totalbetrag"
                }, {
                    "key": "32",
                    "type": "String",
                    "displayName": "Buchungstext"
                }, {
                    "key": "33",
                    "type": "String",
                    "displayName": "Kostenstelle"
                }, {
                    "key": "34",
                    "type": "String",
                    "displayName": "Xpenses Typ"
                }, {
                    "key": "36",
                    "type": "String",
                    "displayName": "Mitarbeiter Name"
                }, {
                    "key": "35",
                    "type": "String",
                    "displayName": "Mitarbeiter Nr."
                }, {
                    "key": "21",
                    "type": "String",
                    "displayName": "Rechnungsart"
                }, {
                    "key": "2",
                    "type": "String",
                    "displayName": "Barcode der Rechnung"
                }, {
                    "key": "9",
                    "type": "String",
                    "displayName": "Konto"
                }, {
                    "key": "11",
                    "type": "String",
                    "displayName": "Kostenstelle"
                }, {
                    "key": "25",
                    "type": "Date",
                    "displayName": "Zahlungsdatum"
                }, {
                    "key": "10",
                    "type": "String",
                    "displayName": "Kontonummer"
                }, {
                    "key": "7",
                    "type": "String",
                    "displayName": "Buchungstext"
                }, {
                    "key": "22",
                    "type": "String",
                    "displayName": "Scan/Index Benutzer"
                }, {
                    "key": "17",
                    "type": "String",
                    "displayName": "Kreditor-Kommentar"
                }, {
                    "key": "18",
                    "type": "String",
                    "displayName": "Kreditor-Prozess-Status"
                }, {
                    "key": "19",
                    "type": "String",
                    "displayName": "Kreditor-WFProtokoll-Kommentar"
                }, {
                    "key": "20",
                    "type": "String",
                    "displayName": "Kreditor-WFProtokoll-PDF"
                }, {
                    "key": "6",
                    "type": "Date",
                    "displayName": "Buchungsdatum"
                }, {
                    "key": "37",
                    "type": "String",
                    "displayName": "Belegstatus"
                }, {
                    "key": "28",
                    "type": "String",
                    "displayName": "Mandant"
                }, {
                    "key": "property_last_modified_date",
                    "type": "DateTime",
                    "displayName": "Last modified"
                }, {
                    "key": "property_last_alteration_date",
                    "type": "DateTime",
                    "displayName": "File changed on"
                }, {
                    "key": "property_editor",
                    "type": "String",
                    "displayName": "Editor"
                }, {
                    "key": "property_remark1",
                    "type": "String",
                    "displayName": "Comment 1"
                }, {
                    "key": "property_remark2",
                    "type": "String",
                    "displayName": "Comment 2"
                }, {
                    "key": "property_remark3",
                    "type": "String",
                    "displayName": "Comment 3"
                }, {
                    "key": "property_remark4",
                    "type": "String",
                    "displayName": "Comment 4"
                }, {
                    "key": "property_owner",
                    "type": "String",
                    "displayName": "Owner"
                }
            ],
            "categories": [{
                    "key": "DKREP",
                    "displayName": "Kreditoren Rechnungsprotokolle"
                }, {
                    "key": "AKUND",
                    "displayName": "Kundenakte"
                }, {
                    "key": "DLIPR",
                    "displayName": "XAPF Protokolle"
                }, {
                    "key": "ALIEF",
                    "displayName": "Lieferantenakte"
                }, {
                    "key": "DDREC",
                    "displayName": "Debitoren Rechnungen"
                }, {
                    "key": "DBEST",
                    "displayName": "Bestellungen"
                }, {
                    "key": "DKORR",
                    "displayName": "Korrespondenz"
                }, {
                    "key": "XPE",
                    "displayName": "Xpenses Quittungen"
                }, {
                    "key": "DKREC",
                    "displayName": "Kreditoren Rechnungen"
                }, {
                    "key": "DKRED",
                    "displayName": "KreditorenRgsPrivera"
                }, {
                    "key": "DLIRE",
                    "displayName": "Kreditoren Rechnungen 2"
                }, {
                    "key": "DMOBI",
                    "displayName": "d3MobileUploads"
                }
            ]
        }
    ]
}
```

</div>

</div>

### Mapping item head fields as document property for a certain d3 archive category.

The d3 source supports category and property keys with data type of string. Therefore it has be been necessary to enhance mapping file with a new attribute *propertyKey*. The property list must contain key of the selected source id in the work flow configuration to upload document successfully.

Example:


![[47045346268-grafik-20220205-092919.png]]

![[47045346268-grafik-20220205-093848.png]]



The *dokDatFilterId* is still used to map items to the d3 category you want to archive your document. The Before the upload of the document we check also the category key if its supported in the configured source id.

Example:


![[47045346268-grafik-20220205-095556.png]]

![[47045346268-grafik-20220205-095845.png]]



conclusion: **It’s possible to configure mappings for different d3 sources because of the new implemented validation of property and category keys.**

## Mode 2: Upload and download documents via “xapf connector”

### General

The support of mode 2 is implemented by AD-4269. This mode will have the advantage configure the mapping direct in d3. Unfortunattely a havy bug in d3 Http Gateway server was detected. Therefore we postbone the implemation till bug resolved. A detailed instructions will posted here if story is implemented.

### What is already implemented for this mode?

The link to request d3 sources is already implemented in the ivy *eapf_archive_d3* project.

# Good to know

## Setup system global Variables for using different type of protocols

Following global variables are implemented with story AD-4349. We have to build in these for a stable releas of ticket AD-3729.

***APF_ARCHIVE_D3_ENABLE_RADIO_BUTTONS_SWITCH_PROTOCOL_WF_CONFIG*** is used to enable radio buttons in the workflow configuration. The default value is *false*;

***APF_ARCHIVE_D3_PROTOCOL_TYPE*** sets the protocol type REST or SOAP. The default value is is REST

## Setup which is *NOT recommended* for customer procuction environments

APF_ARCHIVE_D3_ENABLE_RADIO_BUTTONS_SWITCH_PROTOCOL_WF_CONFIG = true

APF_ARCHIVE_D3_PROTOCOL_TYPE = \[SOAP, REST or empty\]

Here we can not garatuee the clean archive procedure espessally after restarting the ivy engine. We’re invest here much time for clean implementation. See ticket <a href="https://axonivy.atlassian.net/browse/AD-4352" class="external-link" rel="nofollow">AD-4352</a>

## Setup system global variables for mapping

*New global variable:* **APF_WEB_IS_USING_DEFAULT_SOURCE** which lists default source d3 in the select one menu automatically otherwise you have to store json file your disk and configure the file path in work flow configuration.  
*true*: default use will be requested while the workflow configuration  
*false*: use custom specific sources

**APF_ARCHIVE_D3_REST_DEFAULT_FILE_PATH_TO_SOURCES**

path to sources settings.
