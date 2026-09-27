---
title: "SSL renewal issue"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48507322370/SSL+renewal+issue
space: "LUZ"
topic: security
relevance: 0.777
depth: 3
updated: 2025-05-21
attachments: 0
tags:
  - confluence
  - security
  - space/luz
---

# SSL renewal issue

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-05-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48507322370/SSL+renewal+issue)
> Relevance 0.777 · topic `security`

<div class="toc-macro client-side-toc-macro non-printable conf-macro output-block" hasbody="false" headerelements="H1,H2,H3" macro-id="dc45e975-8b46-48d7-bfd5-53623929beea" macro-name="toc" numberedoutline="false" structure="list">

</div>

# Problem

Some SSL can be validated while some others cannot

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48507322370_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-97321" macro-id="4210e25e-c75c-476d-8860-0e064ba6260b" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-97321" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-97321</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

A own domain ingress will look like

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="91e65b0d-f009-4428-b384-0635e5cd572d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: webclient-nginx-ingress
    nginx.org/server-snippets: |-
      location / {proxy_pass https://binh-14-08.online-dev.klara.tech/;}

      location ~ /.well-known/pki-validation/2DB4742365CB7337A9F11F10BD603BD1.txt { return 200 "A6DF91145B1B048DC93D45B4473D8ABD1BF11420E226821892CD38C0861C3161 comodoca.com"; }

      if ($host = www.team-hacka.biz) {
        return 301 $scheme://team-hacka.biz$request_uri;
      }
  creationTimestamp: "2023-02-07T05:43:38Z"
  generation: 15
  labels:
    klara.ch/module: od-team-hacka-biz-ingress
  managedFields:
  - apiVersion: networking.k8s.io/v1
    fieldsType: FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .: {}
          f:kubernetes.io/ingress.class: {}
        f:labels:
          .: {}
          f:klara.ch/module: {}
      f:spec:
        f:rules: {}
    manager: Kubernetes Java Client
    operation: Update
    time: "2023-02-07T05:43:38Z"
  - apiVersion: networking.k8s.io/v1
    fieldsType: FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          f:kubernetes.io/ingress.allow-http: {}
          f:nginx.org/server-snippets: {}
      f:spec:
        f:tls: {}
    manager: GoogleCloudConsole
    operation: Update
    time: "2023-05-23T03:00:12Z"
  name: od-team-hacka-biz-ingress
  namespace: dev
  resourceVersion: "369165691"
  uid: b685bdd1-40e9-4750-8f7c-124d9c23d9b4
spec:
  rules:
  - host: team-hacka.biz
    http:
      paths:
      - backend:
          service:
            name: luz-online-web
            port:
              number: 8080
        path: /dummy-path
        pathType: ImplementationSpecific
  - host: www.team-hacka.biz
    http:
      paths:
      - backend:
          service:
            name: luz-online-web
            port:
              number: 8080
        path: /dummy-path
        pathType: ImplementationSpecific
  tls:
  - hosts:
    - team-hacka.biz
    - www.team-hacka.biz
    secretName: od-test-tls
status:
  loadBalancer: {}
```

</div>

</div>

# Root cause

### 1-From Hexonet support email

The problem is that the validation file selected that the file must be accessible without a redirect to HTTPS. Currently, your web server returns a "301 Moved Permanently" error when attempting to access the file over HTTP.

### 2-Looking to our config:

After installing SSL at the first time, we add TLS part to Own Domain Ingress. This results to all request from HTTP will be redirected to HTTPS. Therefore, validation link (http) will be redirected to https.

###### Question:

If the redirect to https is the root cause, then all renewal SSL progress should be failure. However, there are some of them still work.

# Workaround solution

Change validation method from URL http to URL https manually via <a href="https://secure.trust-provider.com/products/ORDERSTATUSCHECKER" class="external-link" data-card-appearance="inline" rel="nofollow">https://secure.trust-provider.com/products/ORDERSTATUSCHECKER</a>

# Idea

Base on the root cause, we have some ideas

### 1-Do not redirect URL validation link

(<a href="http://team-hacka.biz/.well-known/pki-validation/2DB4742365CB7337A9F11F10BD603BD1.txt" class="external-link" rel="nofollow">http://DOMAIN_NAME/.well-known/pki-validation/VALIDATION_FILENAME.txt</a>)

This idea is that we will config to not redirect to https if the request is validation link.

###### Technical issue:

Need to try it out but I don’t know if it is possible.

If it is possible, I think it does not take much time because we just add more configs without change any logic.

### 2-Remove TLS part when the renewal SSL process start

This idea is that we remove TLS part, then it will cannot redirect to https.

###### Technical issue:

It looks possible. We just add more logic at renewal SSL step. However, we have business logic that the renewal process have started before 4 hours when the current SSL expired. So if we remove TLS part at that time we will waste 4 hours of current SSL.

### 3-Remove TLS part when the current SSL expired

It is similar to Idea 2, just different from the time.

###### Technical issue:

It looks possible. We need to schedule to remove TLS part. This can be done by cron job, ivy task…

And depending on solution (cron job, ivy task, …), it maybe need a migration.

#### Note:

Suggestion from Future team, if we go with Idea 2 or 3, we may need block users access to their own domain (show maintenance page,… something like that) until SSL was installed for security.

# Try out result

### 1-Do not redirect URL validation link

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b2900707-c484-4ca2-a39e-548a88967279" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
By default the controller redirects (308) to HTTPS if TLS is enabled for that ingress. 
```

</div>

</div>

(For more information about annotations <a href="https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/" class="external-link" data-card-appearance="inline" rel="nofollow">https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/</a>)

The redirect block was added at the beginning of configuration. This results to all request will be redirected to https and we cannot disable it for specific url. So, I try config as below step:

1-Disable adding redirect block by GKE: line 2

2-Manually adding redirect block: line 9-11

3-Config to not redirect validation link: line 5-7

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4b462a67-3be1-44e1-81ae-7c8bd563f1bc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
annotations:
    ingress.kubernetes.io/ssl-redirect: "false"
    kubernetes.io/ingress.class: webclient-nginx-ingress
    nginx.org/server-snippets: |-
      if ($request_uri = /.well-known/pki-validation/2DB4742365CB7337A9F11F10BD603BD1.txt) {
        return 200 "A6DF91145B1B048DC93D45B4473D8ABD1BF11420E226821892CD38C0861C3161 comodoca.com";
      }

      if ($scheme = http) {
        return 301 https://$host:443$request_uri;
      }
      
      //other configs
```

</div>

</div>

##### Because Idea 1 works, so I do not try out Idea 2,3

# In conclusion

This idea 1 is possible and it take less time to implement compared to idea 2, 3 (which may need to change the renewal logic flow).

However, it still take time for some reasons:

1-Not only adding more config (Disable adding redirect block by GKE and Manually adding redirect block) but also modifying current config (Using “If statement” instead of “Location block”)

2-Need to migrate all current own domains

3-Because we do not still implement this story <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48507322370_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-93455" macro-id="5e744d69-0fd3-448e-8049-608bc920dbd0" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-93455" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-93455</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> , the migration may overwrite this config for the domain which supported by Arrow team. So we may need ask someone to config it again

------------------------------------------------------------------------

Source: Copy from [SSL renewal issue](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47378203323/SSL+renewal+issue)
