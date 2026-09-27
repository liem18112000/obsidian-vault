---
title: "SQL script to get SOB (APP & WEB) dossier progress"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48222699826/SQL+script+to+get+SOB+APP+WEB+dossier+progress
space: "GRAVITY"
topic: programming
relevance: 0.786
depth: 3
updated: 2024-12-24
attachments: 1
tags:
  - confluence
  - programming
  - space/gravity
---

# SQL script to get SOB (APP & WEB) dossier progress

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2024-12-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48222699826/SQL+script+to+get+SOB+APP+WEB+dossier+progress)
> Relevance 0.786 · topic `programming`

- Connect to the database of Ivy Engine

- Run the SQL script below to find all SOB dossier progress

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="07f9e670-ac51-46b7-9a0c-9c345bfd730c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
select 
    json_extract_path_text(businessdata.objectvalue::json, 'cobId') as cob_id,
    businessdata.businessdataid as dossier_id,
    json_extract_path_text(businessdata.objectvalue::json, 'sobData', 'onboardingKey') as onboarding_key,
    json_extract_path_text(businessdata.objectvalue::json, 'sobData', 'dossierOrigin') as dossier_origin,
    dossier_progress.state,
    dossier_progress.status,
    json_extract_path_text(businessdata.objectvalue::json, 'sobData', 'isConsentAnalytics') as is_consent_analytics,
    businessdata.createdat as created_at,
    json_extract_path_text(businessdata.objectvalue::json, 'sobData', 'onboardingEvents', '1') as onboarding_events
from iwa_businessdata as businessdata
left join cob_unattended_dossier_progress dossier_progress
on businessdataid = dossier_progress.dossierid
where 
    objecttype = 'ch.axonivy.desk.individual.IndividualDossier'
    and createdat between '2024-12-11 00:00:00' and '2024-12-18 23:59:59'
    and json_extract_path_text(businessdata.objectvalue::json, 'sobData', 'dossierOrigin') in ('APP', 'WEB')
order by
    dossier_origin,
    case dossier_progress.state
        when 'INITIALIZATION' then 1
        when 'IDENTIFICATION' then 2
        when 'REGISTRATION' then 3
        when 'COLLECTION' then 4
        when 'SIGNING' then 5
        else 6
    end
;
```

</div>

</div>

- After running the script above, the result will be similar to the image below.


![[48222699826-image-20241223-085902.png]]
