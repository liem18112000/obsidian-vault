---
title: "CICD for Kogito"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48127672366/CICD+for+Kogito
space: "LUZ"
topic: infra
relevance: 0.794
depth: 3
updated: 2024-11-12
attachments: 18
tags:
  - confluence
  - infra
  - space/luz
---

# CICD for Kogito

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-11-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48127672366/CICD+for+Kogito)
> Relevance 0.794 · topic `infra`

# How Kogito Replaces KIE Server

## KIE Server


![[48127672366-output_L6at2s.gif]]



## Kogito


![[48127672366-output_7cNvnc.gif]]



Further Details on CI/CD Process: [CICD for Kogito](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48127672366/CICD+for+Kogito)

# CICD with Cloud Build

Here is the cloudbuild.yaml file of this flow: <a href="https://bitbucket.org/axonivy-prod/luz_kogito_rule/src/dev/cloudbuild.yaml" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kogito_rule/src/dev/cloudbuild.yaml</a>

Here is the PR of gcloud to create Cloud Build resources: <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/6630/diff" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/6630/diff</a>


![[48127672366-CICD kogito.jpg]]



# Branch Management

Based on the flow described above, when a user edits a rule:

1.  The UI pushes the changes to the `luz-kogito-rule` repository. The branch used depends on the environment, as shown in the table below.

2.  After triggering Cloud Build, the process updates the new image hash in the `luz_kubernetes` repository. Both the branch and YAML file vary depending on the environment, as detailed in the table below.

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Environment</strong></p></th>
<th><p><strong>luz_kogito_rule</strong></p></th>
<th><p><strong>luz_kubernetes</strong></p></th>
</tr>
&#10;<tr>
<td><p>dev-vn</p></td>
<td><p>Branch: <strong>dev-vn</strong></p></td>
<td><p>Branch: <strong>master</strong></p>
<p>File: kubernetes-overlays/<strong>env-dev-vn</strong>/luz-kogito-rule/k8s-luz-kogito-rule.yaml</p></td>
</tr>
<tr>
<td><p>dev</p></td>
<td><p>Branch: <strong>dev</strong></p></td>
<td><p>Branch: <strong>master</strong></p>
<p>File: kubernetes-overlays/<strong>env-dev</strong>/luz-kogito-rule/k8s-luz-kogito-rule.yaml</p></td>
</tr>
<tr>
<td><p>staging</p></td>
<td><p>Branch: <strong>staging</strong></p></td>
<td><p>Branch: <strong>master</strong></p>
<p>File: kubernetes-overlays/<strong>env-staging</strong>/luz-kogito-rule/k8s-luz-kogito-rule.yaml</p></td>
</tr>
<tr>
<td><p>swissdec</p></td>
<td><p>Branch: <strong>swissdec</strong></p></td>
<td><p>Branch: <strong>master</strong></p>
<p>File: Kubernetes-overlays/<strong>env-swissdec</strong>/luz-kogito-rule/k8s-luz-kogito-rule.yaml</p></td>
</tr>
<tr>
<td><p>performance</p></td>
<td><p>Branch: <strong>performance</strong></p></td>
<td><p>Branch: <strong>master</strong></p>
<p>File: Kubernetes-overlays/<strong>env-performance</strong>/luz-kogito-rule/k8s-luz-kogito-rule.yaml</p></td>
</tr>
<tr>
<td><p>test</p></td>
<td><p>Branch: <strong>test</strong></p></td>
<td><p>Branch: <strong>test</strong></p>
<p>File: Kubernetes-overlays/<strong>env-test</strong>/luz-kogito-rule/k8s-luz-kogito-rule.yaml</p></td>
</tr>
<tr>
<td><p>prod</p></td>
<td><p>Branch: <strong>prod</strong></p></td>
<td><p>Branch: <strong>production</strong></p>
<p>File: Kubernetes-overlays/<strong>env-prod</strong>/luz-kogito-rule/k8s-luz-kogito-rule.yaml</p></td>
</tr>
</tbody>
</table>

</div>

<div id="expander-150030485" class="expand-container conf-macro output-block" hasbody="true" macro-id="f1e541d3-d5c3-45df-830b-a7ea86443e01" macro-name="expand">

<div id="expander-control-150030485" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">For more detail, please refer this diagram:</span>

</div>

<div id="expander-content-150030485" class="expand-content expand-hidden">


![[48127672366-CICD kogito - branch.jpg]]



</div>

</div>

# Credential Management

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Kinds of credential</strong></p></th>
<th><p><strong>Purpose</strong></p></th>
<th><p><strong>Status</strong></p></th>
<th><p><strong>Jiyan comment</strong></p></th>
</tr>
&#10;<tr>
<td><p>App passwords</p></td>
<td><p>The Kogito UI requires an account and password to pull or push code to Bitbucket.</p></td>
<td><p>We are using the developer's account: <a href="https://bitbucket.org/account/settings/app-passwords/" class="external-link" rel="nofollow">https://bitbucket.org/account/settings/app-passwords/</a></p>

![[48127672366-image-20241106-064025.png]]


<p>Concern:</p>
<ol>
<li><p>Is there a generic account available to support this?</p></li>
<li><p>Can this account be separated by environment?</p></li>
</ol></td>
<td></td>
</tr>
<tr>
<td><p>Bitbucket Access Token</p>
<p>(luz_kogito_rule)</p></td>
<td><p>Cloud Build require 2 access token to connect to Bitbucket.</p>

![[48127672366-image-20241031-100655.png]]

</td>
<td><p>Currently we create tokens here:</p>
<p><a href="https://bitbucket.org/axonivy-prod/luz_kogito_rule/admin/access-tokens" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kogito_rule/admin/access-tokens</a></p>

![[48127672366-image-20241106-063858.png]]


<p>When running the gcloud to create resources, we need to input this tokens. (maybe in command line)</p>
<p>Example:</p>

![[48127672366-image-20241031-101350.png]]


<p><strong>Concern</strong>: A single token is currently used across all environments, meaning the token for production is the same as the one used for development. Is it OK?</p></td>
<td></td>
</tr>
<tr>
<td><p>Bitbucket Bot Token</p>
<p>(luz_kubernetes)</p></td>
<td><p>Cloud Build need to pull and push new hash code of image to luz_kubernetes.</p>

![[48127672366-image-20241031-102715.png]]

</td>
<td><p>Currently, we are using the account: <code>rfcv8wuv5v8xwhkkn72ub4k2yfp9ic@bots.bitbucket.org</code></p>
<p>And the <a href="https://console.cloud.google.com/security/secret-manager/secret/cloudbuild_bitbucket_luz-kubernetes_repo_admin_token/versions?project=klara-nonprod" class="external-link" rel="nofollow">token here</a>: <code>projects/335505349498/secrets/cloudbuild_bitbucket_luz-kubernetes_repo_admin_token/versions/1</code></p>
<p><strong>Concern</strong>:</p>
<ol>
<li><p>We are reusing this token without knowing its origin.</p></li>
<li><p>This token only exists in <code>klara-nonprod</code>, and it’s unclear whether it also exists in the performance and production environments.</p></li>
<li><p>In cloudbuild.yaml, how can we make this configuration dynamic based on the environment?</p></li>
</ol></td>
<td></td>
</tr>
<tr>
<td><p>Service Account</p></td>
<td><p>There is a step to apply new changes to GKE, so we need to ensure the Cloud Build service account has the necessary permissions to perform this action.</p>

![[48127672366-image-20241031-103444.png]]

</td>
<td><p>We re-use the current service account on klara-nonprod for now.</p>
<p><strong>Concern</strong>:</p>
<ol>
<li><p>We’re uncertain about the origin of this service account. Do we need a gcloud script to create it in production?</p></li>
<li><p>If the service account already exists, does it have consistent permissions across all environments?</p></li>
</ol></td>
<td></td>
</tr>
</tbody>
</table>

</div>

# Troubleshooting

<div id="expander-1806512944" class="expand-container conf-macro output-block" hasbody="true" macro-id="5c0648d0-caee-4d43-a3e8-faf041171be1" macro-name="expand">

<div id="expander-control-1806512944" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Why we do not use native for luz_kogito_rule</span>

</div>

<div id="expander-content-1806512944" class="expand-content expand-hidden">

We previously tried using native, but had to revert:<a href="https://bitbucket.org/axonivy-prod/luz_kogito_rule/commits/cf6899ef49c4a6ec10ff5bc94aedcadfb3e7164f" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kogito_rule/commits/cf6899ef49c4a6ec10ff5bc94aedcadfb3e7164f</a>

- `gcr.io/klara-repo/luz-kogito-compile-and-package:1.0.2` This supports Docker for native builds (in case you’d like to try again).


![[48127672366-image-20241101-103251.png]]



Due to this error, which originates from a library within the module and cannot be fixed on our end, we have continued to use the JVM.


![[48127672366-image-20241101-103239.png]]



</div>

</div>

# Remaining Tasks

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Tasks</strong></p></th>
<th><p><strong>Status</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>Update the CI/CD pipeline to integrate credential management from Jiyan.</p>
<p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/edit-v2/48127672366#Credential-Management" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/edit-v2/48127672366#Credential-Management</a></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="cc6d033e-da75-4636-907d-657468c255b3" data-macro-name="status">NOT STARTED YET</span></p></td>
<td><p>Waiting for Jiyan</p></td>
</tr>
</tbody>
</table>

</div>
