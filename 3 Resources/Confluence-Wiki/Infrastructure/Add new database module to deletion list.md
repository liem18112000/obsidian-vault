---
title: "Add new database/module to deletion list"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47455142031/Add+new+database+module+to+deletion+list
space: "TS"
topic: infra
relevance: 0.734
depth: 2.85
updated: 2023-08-10
attachments: 2
tags:
  - confluence
  - infra
  - space/ts
---

# Add new database/module to deletion list

> [!info] Imported from Confluence
> Space **TS** · updated 2023-08-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47455142031/Add+new+database+module+to+deletion+list)
> Relevance 0.734 · topic `infra`

- Add API for deleting the schema: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525649426/Include+totally+NEW+module+into+development+process+-+GCP#Add-a-new-endpoint-to-delete-tenant-specific-schema" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525649426/Include+totally+NEW+module+into+development+process+-+GCP#Add-a-new-endpoint-to-delete-tenant-specific-schema</a>

- luz_versioning_release:

  - Add new database/module and schema deletion API in <a href="https://bitbucket.org/axonivy-prod/luz_versioning_release/src/master/databases_to_delete.txt" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_versioning_release/src/master/databases_to_delete.txt</a>

    - Format: `databaseName`=`baseUrl`. Eg: luzbooking=<a href="http://luz-booking:8080/luz_booking/api" class="external-link" rel="nofollow">http://luz-booking:8080/luz_booking/api</a>

    - <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-progress conf-macro output-inline" hasbody="false" macro-id="40a4de5d-c3fc-4b0e-8c41-a09ea62de6bf" macro-name="status">NOTE</span> Get baseUrl in k8s.yml. Eg: <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes/luz-booking/k8s.yaml" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes/luz-booking/k8s.yaml</a>

      - <a href="http://luz-booking:8080/luz_booking/api" class="external-link" rel="nofollow">http://luz-booking:8080/luz_booking/api</a>  

        

![[47455142031-image-20230810-084140.png]]



- luz_tenant_deletion:

  - Add new database/module and schema deletion API in <a href="https://bitbucket.org/axonivy-prod/luz_tenant_deletion/src/master/src/main/resources/deletion/company/databases_to_delete.txt" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_tenant_deletion/src/master/src/main/resources/deletion/company/databases_to_delete.txt</a>

    - Copy from <a href="https://bitbucket.org/axonivy-prod/luz_versioning_release/src/master/databases_to_delete.txt" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_versioning_release/src/master/databases_to_delete.txt</a> to <a href="https://bitbucket.org/axonivy-prod/luz_tenant_deletion/src/master/src/main/resources/deletion/company/databases_to_delete.txt" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_tenant_deletion/src/master/src/main/resources/deletion/company/databases_to_delete.txt</a>

  - Add new enum for deletion step: <a href="https://bitbucket.org/axonivy-prod/luz_tenant_deletion/src/master/src/main/java/ch/klara/luz/tenantdeletion/entity/step/DeletionStep.java" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_tenant_deletion/src/master/src/main/java/ch/klara/luz/tenantdeletion/entity/step/DeletionStep.java</a>

    - Format: `enumName`("`databaseName`"). Eg: `DELETE_TENANT_SCHEMA_LUZ_BOOKING("luzbooking")`
