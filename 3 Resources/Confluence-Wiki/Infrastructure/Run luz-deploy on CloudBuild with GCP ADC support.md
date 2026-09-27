---
ai_hash: 960ea8c9bbb5bef6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.48
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47343501531/Run+luz-deploy+on+CloudBuild+with+GCP+ADC+support
space: FUT
status: reference
tags:
- confluence
- infra
- space/fut
title: Run luz-deploy on CloudBuild with GCP ADC support
topic: infra
type: source
updated: 2023-08-04
---

# Run luz-deploy on CloudBuild with GCP ADC support

> [!info] Imported from Confluence
> Space **FUT** · updated 2023-08-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47343501531/Run+luz-deploy+on+CloudBuild+with+GCP+ADC+support)
> Relevance 0.724 · topic `infra`

## Situation

To deploy Klara’s modules, we are currently using a container of *luz-deploy* to run the *kustomize* plugin to generate the k8s.yaml file and then apply it to the target environment.

But, some modules which are using KMS to encrypt/decrypt secrets. It requires <a href="https://cloud.google.com/docs/authentication/production" class="external-link" rel="nofollow">Application Default Credentials</a> (ADC) provided to get the master key. Using the *luz-deploy* container in the same way as Jenkins will not work because SOPS can not get the master key from KMS to decrypt the secrets. The reason is that the running *luz-deploy* is not authenticated to ADC.

Ex:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="28e49f57-d0bc-4cb9-b099-3185b00456d4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Failed to get the data key required to decrypt the SOPS file.

Group 0: FAILED
  projects/klara-infra/locations/europe-west6/keyRings/env-infra/cryptoKeys/kubernetes-secrets: FAILED
    - | Error decrypting key: Post
      | https://cloudkms.googleapis.com/v1/projects/klara-infra/locations/europe-west6/keyRings/env-infra/cryptoKeys/kubernetes-secrets:decrypt?alt=json&prettyPrint=false:
      | Get
      | http://169.254.169.254/computeMetadata/v1/instance/service-accounts/default/token?scopes=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform:
      | dial tcp 169.254.169.254:80: i/o timeout

Recovery failed because no master key was able to decrypt the file. In
order for SOPS to recover the file, at least one key has to be successful,
but none were.
```

</div>

</div>

## Solution

To provide ADC to *luz-deploy* container, we have to use a non-interactive method so that we can use it without manual intervention.

Cloud Build uses a special service account to execute builds on our behalf <a href="https://cloud.google.com/build/docs/cloud-build-service-account" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/build/docs/cloud-build-service-account</a>. So, we need to grant Cloud KMS permission to that account <a href="https://cloud.google.com/build/docs/securing-builds/configure-access-for-cloud-build-service-account" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/build/docs/securing-builds/configure-access-for-cloud-build-service-account</a> :

When Cloud Build runs each build step, it attaches the step's container to a local Docker network named `cloudbuild`. The `cloudbuild` network hosts <a href="https://cloud.google.com/docs/authentication/production" class="external-link" rel="nofollow">Application Default Credentials</a> (ADC) that Google Cloud services can use to automatically find your credentials.

We are running nested Docker container *luz-deploy* and want to expose ADC to *luz-deploy* container, we can use the `--network` flag in our docker `run` step <a href="https://cloud.google.com/build/docs/build-config-file-schema#network" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/build/docs/build-config-file-schema#network</a>

### Builders

- `gcr.io/cloud-builders/gke-deploy`

### Configuration

- `entrypoint: 'bash'`

- <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="832b7695-8ca7-4e73-b087-be00ae1c1c60" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
   args:
      - '-c'
      - |
        docker run --network=cloudbuild \
          -v /workspace/local/path/to/deploy-script-folder/:/container/path/to/deploy-script-folder \
          $_KLARA_DOCKER_REPO/$_LUZ_DEPLOY_IMAGE_NAME:$_LUZ_DEPLOY_IMAGE_VERSION \
          /container/path/to/deploy-script-folder/deploy_to_stdout.sh $ENV > /workspace/local-file.yaml;
        gcloud container --project=$_GKE_PROJECT_ID clusters get-credentials $_GKE_CLUSTER --zone=$_GCP_LOCATION;
        kubectl apply -f /workspace/local-file.yaml;
  ```

  </div>

  </div>

%% ai-graph-start %%

**Related notes:**
- [[Deploy luz-epc-redis-service on GCP]]
- [[Recipe Deploy with Terraform]]
- [[Kubernetes knowledge]]
- [[Klara Cloud Build pushes images to klara-repo Artifact Registry with the SA on the trigger]]
- [[Apply changes on luz_kubernetes]]

%% ai-graph-end %%