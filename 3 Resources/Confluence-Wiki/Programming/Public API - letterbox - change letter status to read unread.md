---
title: "Public API - letterbox - change letter status to read/unread"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47730360321/Public+API+-+letterbox+-+change+letter+status+to+read+unread
space: "TS"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2024-03-21
attachments: 2
tags:
  - confluence
  - programming
  - space/ts
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
