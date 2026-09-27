---
ai_hash: feb0769ebf9df502
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 15
depth: 2.97
entities: []
relevance: 0.741
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48207527957/Create+a+work+space
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Create a work space
topic: programming
type: source
updated: 2025-02-12
---

# Create a work space

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-02-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48207527957/Create+a+work+space)
> Relevance 0.741 · topic `programming`

# Overview


![[48207527957-Kogito - Workspace in VS Code - separate account.drawio-20250212-064641.png]]



# Step 1

Access link <a href="https://rules-dev.klara.tech/luz-rule-frontend/?folder=/home/script" class="external-link" rel="nofollow">https://rules-dev.klara.tech/luz-rule-frontend/?folder=/home/script</a>

You will see an interface like this.


![[48207527957-image-20241212-094000.png]]



# Step 2

Click on the **menu icon** → select **Terminal** → choose **New Terminal**.


![[48207527957-image-20241212-094246.png]]



# Step 3

In the **terminal interface**, enter the following **command line**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9ce2a4a4-f0ce-4edc-8bcd-10cf2d426efa" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./create_ws.sh <git-username> <git-password> <git-user-email>
```

</div>

</div>

**If you don’t know to get \<git-username\> and \<git-password\>. Following this guideline:** [Setup personal app password and get account name](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48265789637/Setup+personal+app+password+and+get+account+name)


![[48207527957-Screenshot 2025-01-17 154420-20250117-084758.png]]



After you’re done, press **Enter** to complete.

And here is the **result**.


![[48207527957-Screenshot 2025-01-17 155008-20250117-085220.png]]



Pay attention to the **green section**. Remember to save the **link** to your workspace. You will use it next time to avoid recreating it.

# Step 4

Copy the **provided link** and access your **workspace**.


![[48207527957-Screenshot 2025-01-17 155558-20250117-085709.png]]



**<span style="background-color: rgb(254,222,200);"><u>COMMIT AND PUSH CODE</u></span>**

Click on “**Manage Unsafe Repositories**“


![[48207527957-Screenshot 2025-01-17 155910-20250117-085957.png]]



Choose your workspace / repository


![[48207527957-Screenshot 2025-01-17 160321-20250117-090407.png]]




![[48207527957-Screenshot 2025-01-17 160558-20250117-090739.png]]



Check commit


![[48207527957-Screenshot 2025-01-17 160833-20250117-090918.png]]

%% ai-graph-start %%

**Related notes:**
- [[Recipe Deploy with Terraform]]
- [[Editor rule by Code Service (Visual Code in Browser)]]
- [[Recipe Github copilot]]
- [[CICD for Kogito]]
- [[Kubernetes knowledge]]

%% ai-graph-end %%