---
ai_hash: 5c96a200002fe449
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.64
entities: []
relevance: 0.76
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/49723768860/mcp.klara.ch+current+evaluation
space: IO
status: reference
tags:
- confluence
- ai-ml
- space/io
title: mcp.klara.ch current evaluation
topic: ai_ml
type: source
updated: 2026-09-03
---

# mcp.klara.ch current evaluation

> [!info] Imported from Confluence
> Space **IO** · updated 2026-09-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/49723768860/mcp.klara.ch+current+evaluation)
> Relevance 0.76 · topic `ai_ml`

Owner: Gabe, Team Invisible

# Situation

The Marketing team built an application and published it on production under the klara.ch name. It passed none of the Inbetriebnahme requirements. The facts below are from public observation of the host on 2026-09-03.

- Reachable at https://mcp.klara.ch. Its TLS certificate was issued on 2026-08-28, so it has been serving since at least that date.

- Served by Vercel, edge region fra1 in Frankfurt. Vercel Inc. is a US company, so the provider is subject to US jurisdiction regardless of the serving region.

- TLS certificate issued by Let's Encrypt for CN mcp.klara.ch, valid 2026-08-28 to 2026-11-26. It is not in our certificate inventory and not in our renewal process.

- It runs an OAuth 2.0 authorization server under its own issuer https://mcp.klara.ch/, with dynamic client registration open at /register, PKCE required, a single scope named klara, and an MCP endpoint at /mcp.

- It asks users for an API key.

- No entry in the Asset-Inventar, no Confluence page, no Jira issue. Vercel has no supplier assessment in the ISMS.

- No data processing agreement covers what it processes, and what it processes is not recorded anywhere.

<div hasbody="true" macro-id="b7314a09-1aa8-407d-9794-fa8e26f7cd95" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

Incomplete analysis, only with current information.

</div>

</div>

# Rules this violates

<div>

|  |  |  |
|----|----|----|
| Rule | What it requires | Observation |
| 1.3 Dokumentation und Lifecycle | The Asset-Inventar is checked for currency and deviations are corrected without delay | A production asset under our own domain is absent from the inventory and was not detected by any process. |
| 1.7 Zugriffskontrolle und Rechteverwaltung | Access is granted through the access management process, need to know, approved by the Data Owner, logged and reviewed | Whatever the API key grants was never taken through the process. No approval, no owner, no logging we can read. |
| 1.8 Datenlokalisierung | Customer data is held in Switzerland, in the EU only for permitted data types under DSG and DSGVO | Data is processed in Frankfurt with no determination of which data types are involved and no DSG or DSGVO assessment. |
| 1.8 Anbieter ausserhalb CH und EU | Permitted only after Geschäftsleitung approval and a risk analysis covering official access | Vercel is a US provider. No risk analysis, no Geschäftsleitung approval. |
| 1.8 Lawful-Access-Check | Local law and protective measures are assessed before a new cloud service is introduced | Not performed. US jurisdiction over the provider was never assessed. |
| 1.8 ADV-Vertrag | A supplier with access to personal data of ePost customers has an ADV contract with binding TOMs | No ADV with Vercel. Encryption, access control, logging and data backup are contractually unbound. |
| 1.8 Benachrichtigung über Sicherheitsverletzungen | The supplier notifies ePost without delay of security breaches, with agreed escalation levels | Without a contract there is no notification duty. A breach at the provider would not reach us. |
| 1.8 Nachweis der Konformität | Evidence such as certificates or audit reports is requested according to the risk assessment | No evidence requested, because no risk assessment exists. |
| 1.8 Cloud-Sicherheitsmassnahmen | Encryption in transit and at rest, least privilege access, encrypted backups with defined RTO and RPO | None of these are established or verifiable for this host. |
| 1.9 Datenklassifikation und Schutz | Data is classified, C2 applies by default, and C2 and C3 data move only over approved channels and storage | No classification. C2 applies by default, and an unassessed third party host is not an approved storage location. |
| 1.11 Business Continuity | Systems in scope are covered by the risk assessment and the Business Impact Analysis | Not in the BIA, no RTO or RPO, not in the Piketdienst scope. |
| 1.13 SoA und Asset-Management | Every asset carries a risk rating and mapped controls, and the SoA is reconciled with the inventory | The asset does not exist in the inventory, so it has no rating, no controls and no SoA entry. |
| 4.1 Schlüsselmanagement | Keys are protected, not held in unprotected configuration, and managed in a central KMS under Team Invisible | The TLS key and any API keys the application holds are outside our key management entirely. |
| 4.1 Passwörter und Authentifizierung | Multi-factor authentication for all security critical applications | An API key entered into a form is a single factor bearer credential. |
| 4.2 Sicherheitsplanung und Risikobewertung | Security risks are identified and scored per development phase and fed into the risk management process | No risk assessment, no CVSS scoring, nothing in the risk register. |
| 4.2 Entwicklungsumgebungen | DEV, DEV-STAGING, TEST and PROD are separated, and production changes are rolled out only after full review and approval | Published straight to production with no test stage. |
| 4.2 Entwicklung und Tests | Code versioned in Bitbucket, SonarQube analysis, container CVE scans, CI/CD security checks with documented results | No catalogued repository, no pipeline, none of these checks ran. |
| 4.2 Bereitstellung und Betrieb | Deployment only after a successful security check and explicit PO approval, with continuous monitoring of the workload | No security check, no PO approval, no monitoring. |
| 4.2 Nachvollziehbarkeit | Changes, Inbetriebnahmen and configuration changes are logged, with four eyes where needed | No record of the Inbetriebnahme exists and no second pair of eyes was involved. |
| 4.2 Geheimhaltung und Speicherung von Zugangsdaten | Credentials are held in dedicated protected areas, managed by Team Invisible under least privilege | Credentials are held by a third party platform we do not administer. |
| 4.2 Sicherheitsüberprüfung von Drittanbieter-Diensten und APIs | Third party services and APIs are reviewed risk based, and HTTP traffic is monitored for attacks | The application proxies to third party services and holds their keys. No review, and no WAF in front of it. |
| 4.3 Schwachstellenmanagement | Regular internal scans, SonarQube in the development process, and external penetration tests for critical applications | The host is in the scope of none of our scanners and has never been penetration tested. |
| 4.4 Backup und Wiederherstellung | Automated encrypted backups, separate storage locations, tested restores, defined RTO and RPO | No backup coverage and no restore path. |
| 4.4 Incident Response und Forensik | Detection from WAF, endpoint and GCP logs, evidence preservation and a chain of custody document | We have no logs and no access to the platform, so we can neither detect nor preserve evidence. The DSG deadline of 72 hours and the ISG deadline of 24 hours cannot be met for this asset. |
| DSG | Personal data is disclosed to a processor only under a contract that binds it to equivalent protection, and a breach is reported to the FDPIC | Personal data reaching Vercel does so without a processor contract. |
| ISO 27001 certification scope | The certified scope covers the whole of ePost Service AG, and controls apply to every asset in it | An unregistered production asset inside the certified scope is a nonconformity and will surface at the next internal audit or Post audit. |

</div>

# Credential exposure

The application carries our domain name, runs on infrastructure we do not control, and asks users for an API key. That construct is indistinguishable from credential phishing. Operated by anyone outside the company it would be treated as brand abuse and credential interception, handled as an incident and pursued legally rather than accepted as a project. Every key entered there has been handed to a supplier that owes us no confidentiality, no TOMs and no breach notification, so every key must be treated as disclosed and rotated.

# Open points

- Who authorised the mcp.klara.ch DNS record, and who administers the Vercel account.

- What the API key grants, which system issues it, and how many have been entered.

- Which personal data the application receives, and where it is stored and for how long.

- Whether any KLARA or ePost production system is reachable with credentials this application holds.

%% ai-graph-start %%

**Related notes:**
- [[SSL certificate overview]]
- [[KLARA Documents Concept - Solution Design]]
- [[Security]]
- [[KLARA Integration (request access token & call API)]]
- [[Onboarding API - Investigation Identity Provider SSO]]

%% ai-graph-end %%