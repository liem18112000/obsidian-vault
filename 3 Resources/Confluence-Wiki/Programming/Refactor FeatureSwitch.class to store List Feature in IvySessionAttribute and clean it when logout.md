---
title: "Refactor FeatureSwitch.class to store List<Feature> in IvySessionAttribute and clean it when logout/switch profile"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47528968567/Refactor+FeatureSwitch.class+to+store+List+Feature+in+IvySessionAttribute+and+clean+it+when+logout+switch+profile
space: "LUZ"
topic: programming
relevance: 0.852
depth: 3
updated: 2023-10-23
attachments: 1
tags:
  - confluence
  - programming
  - space/luz
---

# Refactor FeatureSwitch.class to store List<Feature> in IvySessionAttribute and clean it when logout/switch profile

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-10-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47528968567/Refactor+FeatureSwitch.class+to+store+List+Feature+in+IvySessionAttribute+and+clean+it+when+logout+switch+profile)
> Relevance 0.852 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47528968567_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-108248" macro-id="0379a1d6-cedd-494b-8374-9b64320e57bf" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-108248" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-108248</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

**Change the way to retrieve features applied on the**`FeatureSwitch.class`.

**Current context:**

- The constructor of the `FeatureSwitch.class` calls an API to get information about feature switches.

- `FeatureSwitch.class` is currently `@ViewScoped` → *This class will be re-initialized by JSF when loading web pages* → *Calling the API multiple times is unnecessary.*


![[47528968567-error.png]]



**Objective:**

1.  When accessing the web page for the first time → Call the API → Get the result returned by the API and save it in `IvySessionAttribute` as `List<Feature>`.

2.  Navigation to other pages → Check if `IvySessionAttribute` already has the `List<Feature>` information → Don't call the API.

3.  Logout/Switch profile, delete the `List<Feature>` information from `IvySessionAttribute`.
