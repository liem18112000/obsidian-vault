---
ai_hash: 36007c0f8e4cceb3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.68
entities: []
relevance: 0.769
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48611459314/Routing+API+REST+CALLS+Overview
space: Arrow
status: reference
tags:
- confluence
- programming
- space/arrow
title: Routing | API REST CALLS | Overview
topic: programming
type: source
updated: 2025-08-18
---

# Routing | API REST CALLS | Overview

> [!info] Imported from Confluence
> Space **Arrow** · updated 2025-08-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48611459314/Routing+API+REST+CALLS+Overview)
> Relevance 0.769 · topic `programming`

This page documented all the API calls of routing tool backend.

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="9bcba46b-781e-40d6-81bb-25d9a797ab20" macro-name="toc">

</div>

# APIs common for ping, authentication, export, health check

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>API path</strong></p></th>
<th><p><strong>Required authentication</strong></p></th>
<th><p><strong>Called by</strong></p></th>
<th><p><strong>Description</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p><code>/api/ping</code></p></td>
<td><p>N</p></td>
<td><p>Routing FE</p></td>
<td><p>Example: this is used for monitoring the service</p>
<p>Sample request: xxxx</p>
<p>Sample response: xxxx</p></td>
</tr>
<tr>
<td>2</td>
<td><p><code>/api/pingSession</code></p></td>
<td><p>N</p></td>
<td><p>Routing FE</p></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p><code>/api/authenticate</code></p></td>
<td><p>N</p></td>
<td><p>Routing FE</p></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p><code>/api/logout</code></p></td>
<td><p>Y</p></td>
<td><p>Routing FE</p></td>
<td></td>
</tr>
<tr>
<td>5</td>
<td><p><code>/api/logout/checkOut</code></p></td>
<td><p>Y</p></td>
<td><p>Routing FE</p></td>
<td></td>
</tr>
<tr>
<td>6</td>
<td><p><code>/api/logout/checkIn</code></p></td>
<td><p>Y</p></td>
<td><p>Routing FE</p></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td><p><code>/api/export/coComments</code></p></td>
<td><p>Y</p></td>
<td><p>Routing FE</p></td>
<td></td>
</tr>
<tr>
<td>8</td>
<td><p><code>/api/export/activeSessions</code></p></td>
<td><p>Y</p></td>
<td><p>Routing FE</p></td>
<td></td>
</tr>
<tr>
<td>9</td>
<td><p><code>/api/healthcheck/sobHealthCheckResult</code></p></td>
<td><p>N</p></td>
<td><p>Routing FE</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

# APIs called by cronjob

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/route/solveCallRecoms` | N | Cronjob |
| 2 | `/api/route/solveCobTasks` | N | Cronjob |
| 3 | `/api/route/deleteOldAlerts` | N | Cronjob |
| 4 | `/api/route/deleteOldChatAlerts` | N | Cronjob |
| 5 | `/api/route/deleteOldCallRecoms` | N | Cronjob |
| 6 | `/api/route/logOutAllUsers` | N | Cronjob |
| 7 | `/api/route/deleteOldSupervisorRequests` | N | Cronjob |
| 8 | `/api/route/deleteAllAlertNotifications` | N | Cronjob |
| 9 | `/api/route/deleteAllChatAlertNotifications` | N | Cronjob |
| 10 | `/api/route/deleteAllSupervisorNotifications` | N | Cronjob |
| 11 | `/api/route/deleteOldCoComments` | N | Cronjob |
| 12 | `/api/route/loadOperationalPlans` | N | Cronjob |
| 13 | `/api/route/deleteAllCobIdScanData` | N | Cronjob |

</div>

# APIs for users

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/users/getUserData` | Y | Routing FE |
| 2 | `/api/users/deleteUser` | Y | Routing FE |
| 3 | `/api/users/updateAgentGroup` | Y | Routing FE |
| 4 | `/api/users/updateIsSupervisorActiv` | Y | Routing FE |
| 5 | `/api/users/updateIsSupervisorAvailable` | Y | Routing FE |
| 6 | `/api/users/updateIsEnableChat` | Y | Routing FE |
| 7 | `/api/users/updateEscalations` | Y | Routing FE |
| 8 | `/api/users/statActiveAgentsPerAgentGroup` | Y | Routing FE |
| 9 | `/api/users/statActiveAgentsPerLang` | Y | Routing FE |
| 10 | `/api/users`(POST) | Y | Routing FE |
| 11 | `/api/users`(GET) | Y | Routing FE |
| 12 | `/api/users`(PUT) | Y | Routing FE |

</div>

# APIs for change password

<div>

|     |                       |                             |               |
|-----|-----------------------|-----------------------------|---------------|
|     | **API path**          | **Required authentication** | **Called by** |
| 1   | `/api/changePassword` | Y                           | Routing FE    |

</div>

# APIs for links

<div>

|     |                         |                             |               |
|-----|-------------------------|-----------------------------|---------------|
|     | **API path**            | **Required authentication** | **Called by** |
| 1   | `/api/links/deleteLink` | Y                           | Routing FE    |
| 2   | `/api/links`(POST)      | Y                           | Routing FE    |
| 3   | `/api/links`(PUT)       | Y                           | Routing FE    |
| 4   | `/api/links`(GET)       | Y                           | Routing FE    |

</div>

# APIs for supervisor

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/supervisor/deleteSupervisorRequest` | Y | Routing FE |
| 2 | `/api/supervisor/allSupervisorRequests` | Y | Routing FE |
| 3 | `/api/supervisor/openSupervisorRequests` | Y | Routing FE |
| 4 | `/api/supervisor/addSupervisorNotification` | Y | Routing FE |
| 5 | `/api/supervisor`(POST) | Y | Routing FE |
| 6 | `/api/supervisor`(PUT) | Y | Routing FE |
| 7 | `/api/supervisor`(GET) | Y | Routing FE |

</div>

# APIs for tasks

<div>

|     |                             |                             |               |
|-----|-----------------------------|-----------------------------|---------------|
|     | **API path**                | **Required authentication** | **Called by** |
| 1   | `/api/tasks`(POST)          | Y                           | Routing FE    |
| 2   | `/api/tasks`(PUT)           | Y                           | Routing FE    |
| 3   | `/api/tasks`(GET)           | Y                           | Routing FE    |
| 4   | `/api/tasks/deleteTaskInfo` | Y                           | Routing FE    |

</div>

# APIs for alerts

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/alerts/getMyAlerts` | Y | Routing FE |
| 2 | `/api/alerts/getMyActiveAlerts` | Y | Routing FE |
| 3 | `/api/alerts/getMyActiveChatAlerts` | Y | Routing FE |
| 4 | `/api/alerts/getStatAlertsByCobStatus` | Y | Routing FE |
| 5 | `/api/alerts/all` | Y | Routing FE |
| 6 | `/api/alerts/allActive` | Y | Routing FE |
| 7 | `/api/alerts/allActiveChats` | Y | Routing FE |
| 8 | `/api/alerts/getByCobId` | Y | Routing FE |
| 9 | `/api/alerts/settleChatAlert` | Y | Routing FE |
| 10 | `/api/alerts/closeChatAlert` | Y | Routing FE |
| 11 | `/api/alerts/closeTaskAlert` | Y | Routing FE |
| 12 | `/api/alerts/addAlertNotification` | Y | Routing FE |
| 13 | `/api/alerts/addChatNotification` | Y | Routing FE |
| 14 | `/api/alerts/forwardAlert` | Y | Routing FE |
| 15 | `/api/alerts/forwardChatAlert` | Y | Routing FE |
| 16 | `/api/alerts/search` | Y | Routing FE |
| 17 | `/api/alerts/updateCurrentAssignee` | Y | Routing FE |
| 18 | `/api/alerts/getOutOfServiceAlerts` | Y | Routing FE |
| 19 | `/api/alerts/getOutOfServiceChatAlerts` | Y | Routing FE |
| 20 | `/api/alerts/updateChatProcessingStatus` | Y | Routing FE |

</div>

# APIs for obh categories

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/obhcategories`(GET) | Y | Routing FE |
| 2 | `/api/obhcategories/{id}`(PUT) | Y | Routing FE |

</div>

# APIs for tenant config

<div>

|     |                               |                             |               |
|-----|-------------------------------|-----------------------------|---------------|
|     | **API path**                  | **Required authentication** | **Called by** |
| 1   | `/api/tenantconfig`(GET)      | Y                           | Routing FE    |
| 2   | `/api/tenantconfig/{id}`(PUT) | Y                           | Routing FE    |

</div>

# APIs for call recoms

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/callRecoms/getCallRecomsByCobId` | Y | Routing FE |

</div>

# APIs for CO comments

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/coComments/add` | Y | Routing FE |
| 2 | `/api/coComments/update` | Y | Routing FE |
| 3 | `/api/coComments/getByDossierId` | Y | Routing FE |
| 4 | `/api/coComments/list` | Y | Routing FE |
| 5 | `/api/coComments/delete` | Y | Routing FE |

</div>

# APIs for client log

<div>

|     |                  |                             |               |
|-----|------------------|-----------------------------|---------------|
|     | **API path**     | **Required authentication** | **Called by** |
| 1   | `/api/clientlog` | Y                           | Routing FE    |

</div>

# APIs for kyc check

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/kyccheck/person/search` | Y | Routing FE |
| 2 | `/api/kyccheck/person/personProfileDetail` | Y | Routing FE |
| 3 | `/api/kyccheck/person/indexStatistics` | Y | Routing FE |
| 4 | `/api/kyccheck/person/indexVersions` | Y | Routing FE |
| 5 | `/api/kyccheck/person/personsToCheck` | Y | Routing FE |

</div>

# APIs for operational plan

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/operationalplan`(POST) | Y | Routing FE |
| 2 | `/api/operationalplan`(GET) | Y | Routing FE |
| 3 | `/api/operationalplan/delete` | Y | Routing FE |
| 4 | `/api/operationalplan/updateGrp` | Y | Routing FE |
| 5 | `/api/operationalplan/updateSv` | Y | Routing FE |
| 6 | `/api/operationalplan/reset` | Y | Routing FE |

</div>

# APIs for config

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/config/serviceTime`(PUT) | Y | Routing FE |
| 2 | `/api/config/serviceTime`(GET) | Y | Routing FE |
| 3 | `/api/config/holiday`(POST) | Y | Routing FE |
| 4 | `/api/config/holiday`(PUT) | Y | Routing FE |
| 5 | `/api/config/holiday/list`(GET) | Y | Routing FE |
| 6 | `/api/config/holiday/delete`(DELETE) | Y | Routing FE |

</div>

# APIs for message

<div>

|     |                             |                             |               |
|-----|-----------------------------|-----------------------------|---------------|
|     | **API path**                | **Required authentication** | **Called by** |
| 1   | `/api/config/messages`(PUT) | Y                           | Routing FE    |
| 2   | `/api/config/messages`(GET) | Y                           | Routing FE    |

</div>

# APIs for live reporting

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/livereporting/getBasicReport` | Y | Routing FE |

</div>

# APIs for user actions

<div>

|     |                    |                             |               |
|-----|--------------------|-----------------------------|---------------|
|     | **API path**       | **Required authentication** | **Called by** |
| 1   | `/api/userActions` | Y                           | Routing FE    |

</div>

# APIs for cob id scan Api resolver

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/cobIdScanApiResolver/image/{id}`(GET) | Y | Routing FE |
| 2 | `/api/cobIdScanApiResolver/orig/{tenant}/{cobid}`(GET) | Y | Routing FE |
| 3 | `/api/cobIdScanApiResolver/{tenant}/{cobid}`(GET) | Y | Routing FE |

</div>

# APIs for cob dossier Api proxy

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/cobDossierApiProxy/{tenant}/{cobid}`(GET) | Y | Routing FE |

</div>

# APIs for dwh resolver

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/dwhResolver/search`(POST) | Y | Routing FE |

</div>

# APIs for id now Api proxy

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/idnowApiProxy/list`(GET) | Y | Routing FE |
| 2 | `/api/idnowApiProxy/zip/{transactionId}`(GET) | Y | Routing FE |

</div>

# APIs for tasks and chats

**Noted: these APIs located at module cob-routing-service**

<div>

|  |  |  |  |
|----|----|----|----|
|  | **API path** | **Required authentication** | **Called by** |
| 1 | `/api/routing-tasks`(POST) | Y | COB, agent-review |
| 2 | `/api/routing-tasks/{taskId}`(PATCH) | Y | COB, agent-review |
| 3 | `/api/routing-tasks/chats`(POST) | Y | COB |

</div>

# Postman collection for all the APIs above

This collection includes:

- API + description.

- API request example.

- API response example.

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="a5ea2001-9f3f-402b-8232-3e64a43249a5" macro-name="view-file"><a href="../_attachments/48611459314-Routing All Rest APIs.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/48611459314/Routing%20All%20Rest%20APIs.postman_collection.json?version=1&amp;modificationDate=1755494719244&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[48611459314-Routing All Rest APIs.postman_collection.json]]

</a></span>

%% ai-graph-start %%

**Related notes:**
- [[Login]]
- [[luz_cor_api]]
- [[Getting tenant list]]
- [[How to run export API for specific tenant and date - Manual export]]
- [[IVY API Calls Overview for luz Modules]]

%% ai-graph-end %%