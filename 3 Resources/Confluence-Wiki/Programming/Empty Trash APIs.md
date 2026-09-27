---
ai_hash: bf3de99fd5abf0eb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.89
entities: []
relevance: 0.716
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47144894684/Empty+Trash+APIs
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Empty Trash APIs
topic: programming
type: source
updated: 2022-07-12
---

# Empty Trash APIs

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-07-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47144894684/Empty+Trash+APIs)
> Relevance 0.716 · topic `programming`

# 1. API


![[47144894684-image-20220712-094240.png]]



- Endpoint: /api/{tenant-id}/trash

- Method: DELETE

- Success Response Status 204

Request sample:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d9c6d2f6-9cde-4035-8cf6-47a874d23b0d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request DELETE 'http://localhost:8080/luz_docs/api/114f1fba-1520-420c-a495-7acea20d1dde/trash' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhdS5uZ3V5ZW5waHVvY0BheG9uYWN0aXZlLmNvbSIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjc4MzAsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJBVU5HIEx0ZCIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImlkIjoxMjM4OTEsInJvbGVzIjpbImNvbXBhbnlfYWRtaW5pc3RyYXRvciJdLCJ0ZW5hbnRJZCI6IjExNGYxZmJhLTE1MjAtNDIwYy1hNDk1LTdhY2VhMjBkMWRkZSIsInVzZXJuYW1lIjoiYXUubmd1eWVucGh1b2NAYXhvbmFjdGl2ZS5jb20iLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiMTE0ZjFmYmEtMTUyMC00MjBjLWE0OTUtN2FjZWEyMGQxZGRlIiwidXNlcl9yb2xlcyI6WyJFdmVyeWJvZHkiLCJBZG1pbmlzdHJhdG9yIl0sInBlcnNvbi10ZW5hbnQiOnsiaWQiOjAsInJvbGVzIjpbImNvbXBhbnlfYWRtaW5pc3RyYXRvciJdLCJ0ZW5hbnRJZCI6IjI3NWY2ODQ1LWM4YWQtNDQwNC1iMDk3LTM0NzhmYmYwNjMwZiIsInVzZXJuYW1lIjoiYXUubmd1eWVucGh1b2NAYXhvbmFjdGl2ZS5jb20iLCJuYW1lIjpudWxsLCJ0eXBlIjoiUEVSU09OIiwiY3JlYXRlRGF0ZSI6Ik1vbiBBcHIgMDQgMDg6NTM6NDcgQ0VTVCAyMDIyIiwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImV4cCI6MTY1NzY1NDQzOCwiaWF0IjoxNjU3NjExMjM4LCJzZWN1cml0eV9jbGFzc2VzIjpbXX0.Kizt8ZxOzV9SNjRWDPZ7NRGL3MpGx5S-uINrosHCiTXqvkYVm_nYnTV-U60VimqN8tZooq3CJHI5wQkSz_0cVsRl4ptUsCXRnL2nyRxQhMlCv9YPFywnt6E89BQ4UrrYHhHJfgvzcJzZYGniuVipihJhE2iKszIx0UJYfuXAUNEjSzX0IxXs89MEBdaV4PHvHN2DWvK7cDcxWPBNYeT6tinfhoKZIfVNoStNZdm5kKIOvuj3GkJ3K2VRbi30-h0TWMWSSgWyhV5FkiUTiSoeooFIAcwjcDL2PYMT9vySgiw5EUb6Yb4urAnvzkTPmyGipYkBQCqViVo-lF3MqhpwKQ'
```

</div>

</div>

  
Response sample:


![[47144894684-image-20220712-093908.png]]

%% ai-graph-start %%

**Related notes:**
- [[How to run export API for specific tenant and date - Manual export]]
- [[Delete company - Old way]]
- [[Rerun Own domain migration api for all tenant]]
- [[Download user audit logs export files]]
- [[Enhancements for API Delete and Restore]]

%% ai-graph-end %%