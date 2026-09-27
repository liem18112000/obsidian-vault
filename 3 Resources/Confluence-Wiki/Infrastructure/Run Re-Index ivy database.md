---
title: "Run Re-Index ivy database"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47416345154/Run+Re-Index+ivy+database
space: "LUZ"
topic: infra
relevance: 0.724
depth: 2.48
updated: 2023-06-27
attachments: 0
tags:
  - confluence
  - infra
  - space/luz
---

# Run Re-Index ivy database

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-06-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47416345154/Run+Re-Index+ivy+database)
> Relevance 0.724 · topic `infra`

There was slow deployment due to cleanup some temporary projects which are created for deployment verification → deletion got slow.

Run re-index the whole ivy-system database would help to boost up this process.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f6d99d43-28c2-451d-b713-bbab91c51815" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
CREATE INDEX IF NOT EXISTS IWA_AsyncProcessCaseData_CaseIdIndex ON IWA_AsyncProcessCaseData (CaseId);
--CREATE INDEX IF NOT EXISTS IWA_Application_SecuritySystemIdIndex ON IWA_Application (SecuritySystemId);
CREATE INDEX IF NOT EXISTS IWA_ProcessModel_ApplicationIdIndex ON IWA_ProcessModel (ApplicationId);
CREATE INDEX IF NOT EXISTS IWA_ProcessModelVersion_ProcessModelIdIndex ON IWA_ProcessModelVersion (ProcessModelId);
CREATE INDEX IF NOT EXISTS IWA_Library_ApplicationIdIndex ON IWA_Library (ApplicationId);
CREATE INDEX IF NOT EXISTS IWA_LibrarySpecification_LibraryIdIndex ON IWA_LibrarySpecification (LibraryId);
CREATE INDEX IF NOT EXISTS IWA_LibraryVersionSpec_LibrarySpecificationIdIndex ON IWA_LibraryVersionSpec (LibrarySpecificationId);
CREATE INDEX IF NOT EXISTS IWA_CaseMap_ProcessModelVersionIdIndex ON IWA_CaseMap (ProcessModelVersionId);
CREATE INDEX IF NOT EXISTS IWA_CaseMap_ApplicationIdIndex ON IWA_CaseMap (ApplicationId);
--CREATE INDEX IF NOT EXISTS IWA_SecurityMember_SecuritySystemIdIndex ON IWA_SecurityMember (SecuritySystemId);
--CREATE INDEX IF NOT EXISTS IWA_Role_SecuritySystemIdIndex ON IWA_Role (SecuritySystemId);
--CREATE INDEX IF NOT EXISTS IWA_RoleRoleMember_RoleSecurityMemberIdIndex ON IWA_RoleRoleMember (RoleSecurityMemberId varchar_pattern_ops);
--CREATE INDEX IF NOT EXISTS IWA_RoleRoleMember_RoleMemberSecurityMemberIdIndex ON IWA_RoleRoleMember (RoleMemberSecurityMemberId varchar_pattern_ops);
--CREATE INDEX IF NOT EXISTS IWA_User_SecuritySystemIdIndex ON IWA_User (SecuritySystemId);
--CREATE INDEX IF NOT EXISTS IWA_UserRole_RoleSecurityMemberIdIndex ON IWA_UserRole (RoleSecurityMemberId varchar_pattern_ops);
--CREATE INDEX IF NOT EXISTS IWA_UserRole_UserSecurityMemberIdIndex ON IWA_UserRole (UserSecurityMemberId varchar_pattern_ops);
CREATE INDEX IF NOT EXISTS IWA_TaskElement_ProcessModelVersionIdIndex ON IWA_TaskElement (ProcessModelVersionId);
CREATE INDEX IF NOT EXISTS IWA_TaskStart_TaskElementIdIndex ON IWA_TaskStart (TaskElementId);
CREATE INDEX IF NOT EXISTS IWA_TaskEnd_TaskElementIdIndex ON IWA_TaskEnd (TaskElementId);
CREATE INDEX IF NOT EXISTS IWA_TaskSwitchEvent_TaskElementIdIndex ON IWA_TaskSwitchEvent (TaskElementId);
CREATE INDEX IF NOT EXISTS IWA_TaskSwitchEvent_CaseIdIndex ON IWA_TaskSwitchEvent (CaseId);
CREATE INDEX IF NOT EXISTS IWA_Case_ActivatorIdIndex ON IWA_Case (ActivatorId varchar_pattern_ops);
--CREATE INDEX IF NOT EXISTS IWA_CaseLocalized_CaseIdIndex ON IWA_CaseLocalized (CaseId);
--CREATE INDEX IF NOT EXISTS IWA_CaseLocalized_LanguageIdIndex ON IWA_CaseLocalized (LanguageId);
CREATE INDEX IF NOT EXISTS IWA_Task_StartTaskSwitchEventIdIndex ON IWA_Task (StartTaskSwitchEventId);
CREATE INDEX IF NOT EXISTS IWA_Task_EndTaskSwitchEventIdIndex ON IWA_Task (EndTaskSwitchEventId);
CREATE INDEX IF NOT EXISTS IWA_Task_TaskStartIdIndex ON IWA_Task (TaskStartId);
CREATE INDEX IF NOT EXISTS IWA_Task_TaskEndIdIndex ON IWA_Task (TaskEndId);
CREATE INDEX IF NOT EXISTS IWA_Task_ExpiredCreatorTaskIdIndex ON IWA_Task (ExpiredCreatorTaskId);
CREATE INDEX IF NOT EXISTS IWA_Task_TimeoutedCreatorIntrmdtEventIdIndex ON IWA_Task (TimeoutedCreatorIntrmdtEventId);
--CREATE INDEX IF NOT EXISTS IWA_TaskLocalized_TaskIdIndex ON IWA_TaskLocalized (TaskId);
--CREATE INDEX IF NOT EXISTS IWA_TaskLocalized_LanguageIdIndex ON IWA_TaskLocalized (LanguageId);
CREATE INDEX IF NOT EXISTS IWA_CaseMapEvent_ProcessCaseIdIndex ON IWA_CaseMapEvent (ProcessCaseId);
CREATE INDEX IF NOT EXISTS IWA_TaskNote_TaskIdIndex ON IWA_TaskNote (TaskId);
CREATE INDEX IF NOT EXISTS IWA_TaskNote_NoteIdIndex ON IWA_TaskNote (NoteId);
CREATE INDEX IF NOT EXISTS IWA_CaseNote_CaseIdIndex ON IWA_CaseNote (CaseId);
CREATE INDEX IF NOT EXISTS IWA_CaseNote_NoteIdIndex ON IWA_CaseNote (NoteId);
CREATE INDEX IF NOT EXISTS IWA_TaskCustomStringField_TaskIdIndex ON IWA_TaskCustomStringField (TaskId);
CREATE INDEX IF NOT EXISTS IWA_TaskCustomTextField_TaskIdIndex ON IWA_TaskCustomTextField (TaskId);
CREATE INDEX IF NOT EXISTS IWA_TaskCustomNumberField_TaskIdIndex ON IWA_TaskCustomNumberField (TaskId);
CREATE INDEX IF NOT EXISTS IWA_TaskCustomTimestampField_TaskIdIndex ON IWA_TaskCustomTimestampField (TaskId);
CREATE INDEX IF NOT EXISTS IWA_CaseCstmStringFld_CaseIdIndex ON IWA_CaseCustomStringField (CaseId);
CREATE INDEX IF NOT EXISTS IWA_CaseCstmTextFld_CaseIdIndex ON IWA_CaseCustomTextField (CaseId);
CREATE INDEX IF NOT EXISTS IWA_CaseCstmNumberFld_CaseIdIndex ON IWA_CaseCustomNumberField (CaseId);
CREATE INDEX IF NOT EXISTS IWA_CaseCstmTmstmpFld_CaseIdIndex ON IWA_CaseCustomTimestampField (CaseId);
CREATE INDEX IF NOT EXISTS IWA_TskSigEvntRcvr_SgTskTskId ON IWA_TaskSignalEventReceiver (SignaledTaskTaskStartId);
--CREATE INDEX IF NOT EXISTS IWA_SignalEvent_SecuritySystemId ON IWA_SignalEvent (SecuritySystemId);
CREATE INDEX IF NOT EXISTS IWA_SignalEvent_ApplicationId ON IWA_SignalEvent (ApplicationId);
CREATE INDEX IF NOT EXISTS IWA_SecurityDescriptor_SecurityDescriptorTypeId ON IWA_SecurityDescriptor (SecurityDescriptorTypeId);
--CREATE INDEX IF NOT EXISTS IWA_SecuritySystem_SecurityDescriptorId ON IWA_SecuritySystem (SecurityDescriptorId);
CREATE INDEX IF NOT EXISTS IWA_PermissionGroup_ParentPermissionGroupId ON IWA_PermissionGroup (ParentPermissionGroupId);
CREATE INDEX IF NOT EXISTS IWA_SecurityDescriptorType_RootPermissionGroupId ON IWA_SecurityDescriptorType (RootPermissionGroupId);
CREATE INDEX IF NOT EXISTS IWA_AccessControl_PermissionIdIdx ON IWA_AccessControl (PermissionId);
CREATE INDEX IF NOT EXISTS IWA_PermissionGroupPermission_PermissionGroupId ON IWA_PermissionGroupPermission (PermissionGroupId);
CREATE INDEX IF NOT EXISTS IWA_PermissionGroupPermission_PermissionId ON IWA_PermissionGroupPermission (PermissionId);
CREATE INDEX IF NOT EXISTS IWA_BusinessCaseData_BusinessCaseId ON IWA_BusinessCaseData (BusinessCaseId);
```

</div>

</div>
