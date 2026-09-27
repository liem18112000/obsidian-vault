---
ai_hash: 661c4dccba13cf20
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 3
entities: []
relevance: 0.827
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47102132712/Setup+Redis+and+DNS+on+TEST+and+PROD
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Setup Redis and DNS on TEST and PROD
topic: infra
type: source
updated: 2022-05-11
---

# Setup Redis and DNS on TEST and PROD

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-05-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47102132712/Setup+Redis+and+DNS+on+TEST+and+PROD)
> Relevance 0.827 · topic `infra`

luz_cache behind the scene uses Redis server to manipulate with cache data. This guideline will guide you how to setup Redis server instance on GCP and setup DNS to allow luz-redis (k8s service) to be able to find the Redis server instance.

Normally, the setup is done by three steps:

1.  Create Redis instance for Memstore in GCP

2.  Next, enable authentication mode for Redis. After the authentication mode is enabled you will receive an authentication code. The authentication code will be used to create k8s secret for using by rest client inside luz_cache to be able to authenticated to the Redis server.

3.  Finally, create DNS for internal communication between luz-redis (k8s service) and Redis server as a k8s external service.

*<u>Notes</u>:*

1\. While running script if you find this error, please ignore:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ea0c3f13-6369-45de-addd-b8909a8a0931" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
ERROR: (gcloud.dns.record-sets.transaction.abort) Transaction not found at [transaction.yaml]
```

</div>

</div>

2\. <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47102132712/Setup+Redis+and+DNS+on+TEST+and+PROD#Output-samples" rel="nofollow">Output samples</a> are available in the last section of this page.

## Setup on TEST

1.  Create Redis instance: skip because of using same Redis server on DEV.

2.  Create k8s secret: was mentioned here (luz-cache record): [Secrets on TEST and PROD - 0.02.18.00](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47096168768/Secrets+on+TEST+and+PROD+-+0.02.18.00)

3.  Create DNS by following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="05c7dfe2-2552-4c3d-b587-c0da75a6938f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
export LUZ_GCP_REGION=europe-west6
export LUZ_GCP_STORAGE_REGION=$(echo $LUZ_GCP_REGION | tr '[:lower:]' '[:upper:]')
export LUZ_GCP_ZONE=${LUZ_GCP_REGION}-a
export LUZ_GCP_PROJECT_SUFFIX=nonprod
export LUZ_GCP_PROJECT_ID=klara-nonprod
export LUZ_GCP_CLUSTER_NAME=$LUZ_GCP_PROJECT_ID

export REDIS_INSTANCE_NAME=luz-redis-dev
export REDIS_INSTANCE_TIER=standard
export REDIS_INSTANCE_SIZE=1
export REDIS_INSTANCE_VERSION=redis_5_0
export PRIVATE_ZONE_NAME=test-internal
export PRIVATE_DNS_NAME=test.internal.

# create memory store for redis on GCP
REDIS_INSTANCE_IP=127.0.0.1
CURRENT_MAPPING_IP=""
REDIS_AUTHEN_CODE=""
if [ "x" != "x$REDIS_INSTANCE_NAME" ]; then
  # create redis instance if it's not exists
  RESULT=$(($(gcloud --quiet --project "$LUZ_GCP_PROJECT_ID" redis instances list --region "$LUZ_GCP_REGION" | grep "$REDIS_INSTANCE_NAME" -c)))
    if [ 0 -eq "$RESULT" ]; then
      # create redis instance
      echo "Creating redis server..."
      gcloud redis instances create "$REDIS_INSTANCE_NAME" --tier="$REDIS_INSTANCE_TIER" --size="$REDIS_INSTANCE_SIZE" \
      --region="$LUZ_GCP_REGION" --zone="$LUZ_GCP_ZONE" --redis-version="$REDIS_INSTANCE_VERSION" \
      --project="$LUZ_GCP_PROJECT_ID" --network=projects/"$LUZ_GCP_PROJECT_ID"/global/networks/"$LUZ_GCP_CUSTOM_NETWORK" \
      --enable-auth

      # Get authentication code of redis server
      REDIS_AUTHEN_CODE=$(gcloud beta redis instances get-auth-string "$REDIS_INSTANCE_NAME" --region="$LUZ_GCP_REGION")
      echo "Creating redis server COMPLETED"
    else
      echo "Redis server already setup. Then, do nothing"
    fi
fi

# create dns zone if not exists
RESULT=$(($(gcloud dns --project="$LUZ_GCP_PROJECT_ID" managed-zones list | grep "$PRIVATE_ZONE_NAME" -c)))
if [ 0 -eq "$RESULT" ]; then
  echo "Creating DNS zone for internal communicate with Redis server..."
  gcloud dns --project="$LUZ_GCP_PROJECT_ID" managed-zones create "$PRIVATE_ZONE_NAME" --description="$PRIVATE_ZONE_NAME" \
  --dns-name="$PRIVATE_DNS_NAME" --visibility="private" --networks="$LUZ_GCP_CUSTOM_NETWORK"
  echo "Creating DNS zone for internal communicate with Redis server COMPLETED"
else
  echo "DNS zone for internal communicate with Redis server EXISTED"
fi

# get redis instance ip
REDIS_INSTANCE_IP=$(gcloud redis instances describe "$REDIS_INSTANCE_NAME" --region="$LUZ_GCP_REGION" \
--project="$LUZ_GCP_PROJECT_ID" | grep host | awk '{print $2}')

# create mapping for redis if it's not exists. Otherwise, update existing mapping
# Not exists. Then, create
echo "Adding mapping of redis server to DNS zone..."
RESULT=$(($(gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets list --zone="$PRIVATE_ZONE_NAME" \
| grep "$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME." -c)))
if [ "$RESULT" -gt 0 ]; then
  echo "Mapping existed. Updating..."

  CURRENT_MAPPING_IP=$(gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets list --zone="$PRIVATE_ZONE_NAME" \
     | grep "$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME" | awk '{print $4}')
  if [ "$REDIS_INSTANCE_IP" != "$CURRENT_MAPPING_IP" ]; then
    # this is catered for cases that previous transactions failed. If no transaction then skip the error
    gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction abort --zone="$PRIVATE_ZONE_NAME"

    gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction start --zone="$PRIVATE_ZONE_NAME"

    gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction add "$REDIS_INSTANCE_IP" \
    --name="$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME" --ttl=300 --type=A --zone="$PRIVATE_ZONE_NAME"

    # remove the old one
    gcloud beta dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction remove "$CURRENT_MAPPING_IP" \
    --name="$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME" --ttl="300" --type="A" --zone="$PRIVATE_ZONE_NAME"

    gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction execute --zone="$PRIVATE_ZONE_NAME"
  fi
  # Nothing to do with the case the mapping ip the same
  echo "Adding/Updating mapping of redis server to DNS zone COMPLETED"
else
  echo "Mapping has not existed. Creating new one..."
  # this is catered for cases that previous transactions failed. If no transaction then skip the error
  gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction abort --zone="$PRIVATE_ZONE_NAME"

  gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction start --zone="$PRIVATE_ZONE_NAME"

  gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction add "$REDIS_INSTANCE_IP" \
  --name="$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME" --ttl=300 --type=A --zone="$PRIVATE_ZONE_NAME"

  gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction execute --zone="$PRIVATE_ZONE_NAME"
  echo "Creating new DNS COMPLETED"
fi

if [ -n "$REDIS_AUTHEN_CODE" ]; then
  echo The authentication code for redis server is ["$REDIS_AUTHEN_CODE"]
fi
```

</div>

</div>

## Setup on PROD

Please run below command for both setting up Redis server, getting authentication code and establishing DNS:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e3d9bbc6-cfdb-4b9c-ac7d-d3631bd30800" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
export LUZ_GCP_REGION=europe-west6
export LUZ_GCP_STORAGE_REGION=$(echo $LUZ_GCP_REGION | tr '[:lower:]' '[:upper:]')
export LUZ_GCP_ZONE=${LUZ_GCP_REGION}-a
export LUZ_GCP_PROJECT_SUFFIX=prod
export LUZ_GCP_PROJECT_ID=klara-prod

export LUZ_GCP_CUSTOM_NETWORK=luz-custom-network-cloud-nat

export REDIS_INSTANCE_NAME=luz-redis
export REDIS_INSTANCE_TIER=standard
export REDIS_INSTANCE_SIZE=1
export REDIS_INSTANCE_VERSION=redis_5_0
export PRIVATE_ZONE_NAME=prod-internal
export PRIVATE_DNS_NAME=prod.internal.

# create memory store for redis on GCP
REDIS_INSTANCE_IP=127.0.0.1
CURRENT_MAPPING_IP=""
REDIS_AUTHEN_CODE=""
if [ "x" != "x$REDIS_INSTANCE_NAME" ]; then
  # create redis instance if it's not exists
  RESULT=$(($(gcloud --quiet --project "$LUZ_GCP_PROJECT_ID" redis instances list --region "$LUZ_GCP_REGION" | grep "$REDIS_INSTANCE_NAME" -c)))
    if [ 0 -eq "$RESULT" ]; then
      # create redis instance
      echo "Creating redis server..."
      gcloud redis instances create "$REDIS_INSTANCE_NAME" --tier="$REDIS_INSTANCE_TIER" --size="$REDIS_INSTANCE_SIZE" \
      --region="$LUZ_GCP_REGION" --zone="$LUZ_GCP_ZONE" --redis-version="$REDIS_INSTANCE_VERSION" \
      --project="$LUZ_GCP_PROJECT_ID" --network=projects/"$LUZ_GCP_PROJECT_ID"/global/networks/"$LUZ_GCP_CUSTOM_NETWORK" \
      --enable-auth

      # Get authentication code of redis server
      REDIS_AUTHEN_CODE=$(gcloud beta redis instances get-auth-string "$REDIS_INSTANCE_NAME" --region="$LUZ_GCP_REGION")
      echo "Creating redis server COMPLETED"
    else
      echo "Redis server already setup. Then, do nothing"
    fi
fi

# create dns zone if not exists
RESULT=$(($(gcloud dns --project="$LUZ_GCP_PROJECT_ID" managed-zones list | grep "$PRIVATE_ZONE_NAME" -c)))
if [ 0 -eq "$RESULT" ]; then
  echo "Creating DNS zone for internal communicate with Redis server..."
  gcloud dns --project="$LUZ_GCP_PROJECT_ID" managed-zones create "$PRIVATE_ZONE_NAME" --description="$PRIVATE_ZONE_NAME" \
  --dns-name="$PRIVATE_DNS_NAME" --visibility="private" --networks="$LUZ_GCP_CUSTOM_NETWORK"
  echo "Creating DNS zone for internal communicate with Redis server COMPLETED"
else
  echo "DNS zone for internal communicate with Redis server EXISTED"
fi

# get redis instance ip
REDIS_INSTANCE_IP=$(gcloud redis instances describe "$REDIS_INSTANCE_NAME" --region="$LUZ_GCP_REGION" \
--project="$LUZ_GCP_PROJECT_ID" | grep host | awk '{print $2}')

# create mapping for redis if it's not exists. Otherwise, update existing mapping
# Not exists. Then, create
echo "Adding mapping of redis server to DNS zone..."
RESULT=$(($(gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets list --zone="$PRIVATE_ZONE_NAME" \
| grep "$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME." -c)))
if [ "$RESULT" -gt 0 ]; then
  echo "Mapping existed. Updating..."

  CURRENT_MAPPING_IP=$(gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets list --zone="$PRIVATE_ZONE_NAME" \
     | grep "$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME" | awk '{print $4}')
  if [ "$REDIS_INSTANCE_IP" != "$CURRENT_MAPPING_IP" ]; then
    # this is catered for cases that previous transactions failed. If no transaction then skip the error
    gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction abort --zone="$PRIVATE_ZONE_NAME"

    gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction start --zone="$PRIVATE_ZONE_NAME"

    gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction add "$REDIS_INSTANCE_IP" \
    --name="$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME" --ttl=300 --type=A --zone="$PRIVATE_ZONE_NAME"

    # remove the old one
    gcloud beta dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction remove "$CURRENT_MAPPING_IP" \
    --name="$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME" --ttl="300" --type="A" --zone="$PRIVATE_ZONE_NAME"

    gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction execute --zone="$PRIVATE_ZONE_NAME"
  fi
  # Nothing to do with the case the mapping ip the same
  echo "Adding/Updating mapping of redis server to DNS zone COMPLETED"
else
  echo "Mapping has not existed. Creating new one..."
  # this is catered for cases that previous transactions failed. If no transaction then skip the error
  gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction abort --zone="$PRIVATE_ZONE_NAME"

  gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction start --zone="$PRIVATE_ZONE_NAME"

  gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction add "$REDIS_INSTANCE_IP" \
  --name="$REDIS_INSTANCE_NAME.$PRIVATE_DNS_NAME" --ttl=300 --type=A --zone="$PRIVATE_ZONE_NAME"

  gcloud dns --project="$LUZ_GCP_PROJECT_ID" record-sets transaction execute --zone="$PRIVATE_ZONE_NAME"
  echo "Creating new DNS COMPLETED"
fi

if [ -n "$REDIS_AUTHEN_CODE" ]; then
  echo The authentication code for redis server is ["$REDIS_AUTHEN_CODE"]
fi
```

</div>

</div>

## Output samples

#### Authentication code

Where the authentication code is: `0d375083-2345-4e4b-9788-e4b6fa4d18f0`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="97a2922b-7405-4a62-a89f-2da22f6dd0d7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
The authentication code for redis server is [authString: 0d375083-2345-4e4b-9788-e4b6fa4d18f0] 
```

</div>

</div>

#### Sample Redis server


![[47102132712-image-20220427-095306.png]]



#### Sample DNS

- DNS Zone:  
  Note: For the prod the zone name should be prod-internal

  

![[47102132712-image-20220427-094653.png]]



- Zone details  
  <span class="inline-comment-marker" ref="03953299-cf8e-4476-88f9-19d5bc36ed04">There should be 3 records here including SOA, NS and A. And the string dev should be prod for production</span>

  

![[47102132712-image-20220427-094825.png]]

%% ai-graph-start %%

**Related notes:**
- [[Deploy luz-epc-redis-service on GCP]]
- [[EPC Notification]]
- [[Infrastructure]]
- [[luz-vault - How to run Vault Benchmark]]
- [[Recipe Deploy with Terraform]]

%% ai-graph-end %%