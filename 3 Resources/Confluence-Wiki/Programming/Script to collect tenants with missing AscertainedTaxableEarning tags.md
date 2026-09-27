---
title: "Script to collect tenants with missing AscertainedTaxableEarning tags"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49005232276/Script+to+collect+tenants+with+missing+AscertainedTaxableEarning+tags
space: "LUZ"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2025-12-26
attachments: 4
tags:
  - confluence
  - programming
  - space/luz
---

# Script to collect tenants with missing AscertainedTaxableEarning tags

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-12-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49005232276/Script+to+collect+tenants+with+missing+AscertainedTaxableEarning+tags)
> Relevance 0.731 · topic `programming`

Step 1: Open the new SQL script in the luzelm5 database  


![[49005232276-image-20251226-113301.png]]



Step 2: Copy the file’s content and then paste into the new script tab  
please select all the script (Ctrl + A) and the click the run icon  
  
<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="2d593df8-6ada-4254-b8ad-19de422fe373" macro-name="view-file"><a href="../_attachments/49005232276-collect_tenants_with_missing_tags.sql" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/49005232276/collect_tenants_with_missing_tags.sql?version=1&amp;modificationDate=1766749731130&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[49005232276-collect_tenants_with_missing_tags.sql]]

</a></span>  
  


![[49005232276-image-20251226-113539.png]]




![[49005232276-image-20251226-113936.png]]



Step 3: After the script finishes, the result would be available in the “result tab“, please export the result by clicking the “export data“ button and give it back to us as a csv file.
