---
title: "Rerun Own domain migration api for all tenant"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48507093289/Rerun+Own+domain+migration+api+for+all+tenant
space: "LUZ"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2025-05-21
attachments: 0
tags:
  - confluence
  - programming
  - space/luz
---

# Rerun Own domain migration api for all tenant

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-05-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48507093289/Rerun+Own+domain+migration+api+for+all+tenant)
> Relevance 0.731 · topic `programming`

Somehow it fails to migrate existing own domain on Production, which is a blocker.  
Only some of the own domain was migrated correctly.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bc0ae246-f6ba-448f-919d-31c65a2f6124" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl -X PUT -H "Authorization: Bearer $ACCESS_TOKEN" "http://localhost:8090/luz_compensation/api/admin/domains/migration/ingresses"
```

</div>

</div>

- `ACCESS_TOKEN`: Use token of Cron User.

  
This is my example on dev:  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e815aecb-699c-4c93-a878-18d5b177d30d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl -X PUT -H "Authorization: Bearer eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjAsImNvbXBhbnlJZCI6MCwiY29tcGFueU5hbWUiOm51bGwsInN0YXR1cyI6bnVsbCwic3RhdHVzRXhwaXJ5RGF0ZSI6bnVsbCwiaWQiOjAsInJvbGVzIjpbImNvbXBhbnlfYWRtaW5pc3RyYXRvciIsImVtcGxveWVlIiwiZXh0ZXJuYWxfdXNlciJdLCJ0ZW5hbnRJZCI6IjdkMWQ0NzUzLWRmYTYtNDk5Yy1iZWM3LWY1ZTk2MWUwNTUzOCIsInVzZXJuYW1lIjoiYWRtaW4iLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiN2QxZDQ3NTMtZGZhNi00OTljLWJlYzctZjVlOTYxZTA1NTM4IiwidXNlcl9yb2xlcyI6WyJTeXN0ZW1BZG1pbmlzdHJhdG9yIiwiQ1JPTl9KT0IiLCJFdmVyeWJvZHkiLCJhbGxfdGVuYW50c19hY2Nlc3MiLCJBZG1pbmlzdHJhdG9yIiwiTmV3c19BZG1pbiIsImtsYXJhX2FkbWluaXN0cmF0aW9uX2FkbWluIiwia2xhcmFfc2VuZGVyX2FkbWluIiwia2xhcmFfZWxldHRlcl9qb3VybmFsX2FkbWluIl0sInBlcnNvbi10ZW5hbnQiOnsiaWQiOjAsInJvbGVzIjpbXSwidGVuYW50SWQiOiJhMWY3ZTdiYS0wNmVjLTQ3NGQtYjJmZC04OWFhZjIzNWNhMzciLCJ1c2VybmFtZSI6ImFkbWluIiwibmFtZSI6bnVsbCwidHlwZSI6IlBFUlNPTiIsImNyZWF0ZURhdGUiOiJXZWQgTm92IDMwIDExOjA3OjU5IENFVCAyMDE2IiwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImV4cCI6MTY5NDAxOTc0MSwiaWF0IjoxNjkzOTc2NTQxfQ.f-aaWvMi_UOLl0qF8GjAFQweXrryxI4ZqxYctL-KTYTa73dsKEuL2iEu9m-Menvdi3l92sqX37herzIQfYC7-YIwVzhJ_m7VkQTs5bzJzPU-4bOnmR8iVwY3GwV1CYfFWjWCf64xZNvdLq6Isli3c76sE2OrpVZPT7Ks-3iixhWQ7sHpDknTKt1HlK1sy32H-bW_RGrx7d4W9LuIS_TKpsa8c4f3BEcAI317CMHTGElzPzQAxvmeGnwUkm-11hT3T4ImWNIVmFuG0Tc5p_YVnsu9fc6emtSZiECe6BZsfMgAyM7lesuG183x-z8K2_xRZo1_C0Z6T1Bt0tmFOQoOxg" "http://localhost:8090/luz_compensation/api/admin/domains/migration/ingresses"
```

</div>

</div>

------------------------------------------------------------------------

Source: Copy from [Rerun Own domain migration api for all tenant](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47484534991/Rerun+Own+domain+migration+api+for+all+tenant)
