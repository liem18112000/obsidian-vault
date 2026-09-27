---
ai_hash: 4f37cbf50ed7ee11
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47730360321/Public+API+-+letterbox+-+change+letter+status+to+read+unread
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Public API - letterbox - change letter status to read/unread
topic: programming
type: source
updated: 2024-03-21
---

# Public API - letterbox - change letter status to read/unread

> [!info] Imported from Confluence
> Space **TS** · updated 2024-03-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47730360321/Public+API+-+letterbox+-+change+letter+status+to+read+unread)
> Relevance 0.738 · topic `programming`

<div>

|  |  |  |  |  |
|----|----|----|----|----|
|  | **Description** | **Steps** | **Expectation** | **Status** |
| 1 | Verify that the API successfully marks a letter as **read** | Send a request to the API to change the status of a letter to **read** | API responds with a success status code (e.g., 200 OK) and the letter status is updated to **read** in the database | 

![[47730360321-check.png]]

 |
| 2 | Verify that the API successfully marks a letter as **unread** | Send a request to the API to change the status of a letter to **unread** | API responds with a success status code (e.g., 200 OK) and the letter status is updated to **unread** in the database | 

![[47730360321-check.png]]

 |
| 3 | Verify that the API handles invalid letter IDs gracefully | Send a request with an invalid letter ID (e.g., non-existent ID) | API responds with an appropriate error status code (e.g., 404 Not Found) and an error message indicating that the letter does not exist | 

![[47730360321-warning.png]]

 right now if we put the invalid letterId the API still return 200 OK |
| 4 | Verify that the API rejects unauthorized requests | Send a request without proper authentication or authorization | API responds with an error status code (e.g., 401 Unauthorized) and an error message indicating insufficient permissions | 

![[47730360321-check.png]]

 |
| 5 | Verify that the API handles server errors | Simulate a server error (e.g., database connection failure) | API responds with an appropriate error status code (e.g., 500 Internal Server Error) and an error message indicating the issue | 

![[47730360321-check.png]]

 |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Public API - letterbox - API get deleted letters from trash]]
- [[LUZ-115505 Public API - letterbox Part 3]]
- [[5. How to extend modify ONE API delivery API Research]]
- [[Copy 5. How to extend modify ONE API delivery API Research]]
- [[Public API Eletter (0.02.09.00)]]

%% ai-graph-end %%