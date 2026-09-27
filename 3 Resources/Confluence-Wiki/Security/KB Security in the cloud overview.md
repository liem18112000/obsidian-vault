---
title: "KB: Security in the cloud overview"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3575060462/KB+Security+in+the+cloud+overview
space: "AII"
topic: security
relevance: 0.701
depth: 2.4
updated: 2020-02-10
attachments: 0
tags:
  - confluence
  - security
  - space/aii
---

# KB: Security in the cloud overview

> [!info] Imported from Confluence
> Space **AII** · updated 2020-02-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3575060462/KB+Security+in+the+cloud+overview)
> Relevance 0.701 · topic `security`

- [Axonivy AWS Infrastructure](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3507531574/Axonivy+AWS+Infrastructure): All the instances created in private network. Only VPN servers and NAT server have a public ip address which can be access by outside with limited
- All instances being protected by security group [/wiki/spaces/AII/pages/3520340964](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3520340964)
- Axonivy resources could be reachable at Axonivy, AAVN office via VPN [List of site2site VPN connections](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3569425597/List+of+site2site+VPN+connections)
- Instances, databases being backup by AWS snapshot [Backup Solutions](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3507537343/Backup+Solutions), the backup schedule base on [00_Develop a backup strategy for every cloud environment](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3507532653/00_Develop+a+backup+strategy+for+every+cloud+environment)
- Patching: We will run security update in every server by Rundeck monthly.
- Dev servers could not access each others by default.
- We are using Zabbix for monitoring and send alert to ICT Telegram group. [Zabbix Monitoring](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3516694991/Zabbix+Monitoring)
- Protect bruceforce to Jenkins by fall2ban, it will block ip address if detect login incorrect credential many times
- SSH to server by LDAP account. SSH key managed by ICT and not provided to anyone.
- ...
