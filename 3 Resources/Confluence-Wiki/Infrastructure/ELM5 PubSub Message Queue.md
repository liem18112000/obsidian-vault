---
title: "ELM5 PubSub Message Queue"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47577465300/ELM5+PubSub+Message+Queue
space: "LUZ"
topic: infra
relevance: 0.716
depth: 2.67
updated: 2023-12-04
attachments: 4
tags:
  - confluence
  - infra
  - space/luz
---

# ELM5 PubSub Message Queue

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-12-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47577465300/ELM5+PubSub+Message+Queue)
> Relevance 0.716 · topic `infra`

![[47577465300-check.png]]

 Environment preparation:  
- Docker  
- Command terminal  
- Kubectl client and connect to the gcloud.  
- Script: `/luz_kubernetes/cluster/gcp/manual_script/add_luz_elm5_pubsub_queue.sh`  
  
**Step 1:**  
- Open the command terminal then execute the  
*\* SourcePath = Negigate the path to your luz_kubernetes project (such as:* `c:/work/workspace/projects/ `*)*

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0192afcf-a58c-4194-92ff-5b736200d332" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker run -it -v "SourcePath/luz_kubernetes:/root/development/luz_kubernetes" gcr.io/klara-repo/luz-deploy:0.0.1 bash
```

</div>

</div>

  
Step 2:  
- After step 1, the running terminal executes these commands as priority below:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="351b65b1-5ea1-4ed3-bf6d-e3ce0fa5b2d2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Convert the files EOL to Unix format
  $ find ./ -path "*/.git" -prune -o -path "*/*" \( -name "*.sh" -o -name "SopsSecretGenerator" \) -type f -exec sed -i -e 's/\r$//' {} \;

# Login to Google Cloud
  $ gcloud auth login

# Go to luz_kubernetes project
  $ cd root/development/luz_kubernetes

#  Activate the expected scripts
  $ cluster/gcp/<script-path> gcp-nonprod <dev|dev-vn|dev-staging>
  (ex: cluster/gcp/manual_script/add_luz_elm5_pubsub_queue.sh gcp-nonprod dev-vn)
```

</div>

</div>

  
**Step 3:**  
- Checking the topic and subscription on the GCP.


![[47577465300-image-20231204-015648.png]]

![[47577465300-image-20231204-015722.png]]



- **Where is the ELM5 PubSub**

  - luz_kubernetes\cluster\gcp\create_environment.sh

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1c549255-fe4f-44e6-8fe5-0d384365d4ca" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre

# Create a LUZ_ELM5 PUB/SUB topic for institution run
RESULT=$(($(gcloud pubsub topics list --filter=$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID --project=$LUZ_GCP_PROJECT_ID | wc -l)))
if [ 0 -ne "$RESULT" ]; then
   echo "Topic [$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID] already exists. Skipping pubsub topic creation."
else
   gcloud pubsub topics create $LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID --project=$LUZ_GCP_PROJECT_ID
   if [ 0 -ne $? ]; then
       echo "Failed to create topic [$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID] to project [$LUZ_GCP_PROJECT_ID]. Exiting."
      exit 1
   fi
   echo "Created topic [$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID] to project [$LUZ_GCP_PROJECT_ID]!."
fi

# create a LUZ_ELM5 PUB/SUB pull subscription for institution run
RESULT=$(($(gcloud pubsub topics list-subscriptions $LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID --filter=$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_SUBSCRIPTION_ID --project=$LUZ_GCP_PROJECT_ID | wc -l)))
if [ 0 -ne "$RESULT" ]; then
echo "Pull subscription [$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_SUBSCRIPTION_ID] already exists. Skipping pull subscription creation."
else
   gcloud beta pubsub subscriptions create $LUZ_ELM5_INSTITUTION_RUN_PUBSUB_SUBSCRIPTION_ID --topic=$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID --project=$LUZ_GCP_PROJECT_ID \
       --ack-deadline=30 \
       --min-retry-delay="10s" \
       --max-retry-delay="20s" \
       --expiration-period="never"\
       --message-filter='attributes.delivery_size = "SMALL"'
   if [ 0 -ne $? ]; then
       echo "Failed to create pull subscription [$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_SUBSCRIPTION_ID] to topic [$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID]. Exiting."
       exit 1
   fi

   echo "Created pull subscription [$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_SUBSCRIPTION_ID] to topic [$LUZ_ELM5_INSTITUTION_RUN_PUBSUB_TOPIC_ID]!"
fi

# Create a LUZ_ELM5 PUB/SUB topic for gather person data run
RESULT=$(($(gcloud pubsub topics list --filter=$LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID --project=$LUZ_GCP_PROJECT_ID | wc -l)))
if [ 0 -ne "$RESULT" ]; then
   echo "Topic [$LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID] already exists. Skipping pubsub topic creation."
else
   gcloud pubsub topics create $LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID --project=$LUZ_GCP_PROJECT_ID
   if [ 0 -ne $? ]; then
       echo "Failed to create topic [$LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID] to project [$LUZ_GCP_PROJECT_ID]. Exiting."
      exit 1
   fi
   echo "Created topic [$LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID] to project [$LUZ_GCP_PROJECT_ID]!."
fi

# create a LUZ_ELM5 PUB/SUB pull subscription for gather person data run
RESULT=$(($(gcloud pubsub topics list-subscriptions $LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID --filter=$LUZ_ELM5_PERSON_DATA_PUBSUB_SUBSCRIPTION_ID --project=$LUZ_GCP_PROJECT_ID | wc -l)))
if [ 0 -ne "$RESULT" ]; then
echo "Pull subscription [$LUZ_ELM5_PERSON_DATA_PUBSUB_SUBSCRIPTION_ID] already exists. Skipping pull subscription creation."
else
   gcloud beta pubsub subscriptions create $LUZ_ELM5_PERSON_DATA_PUBSUB_SUBSCRIPTION_ID --topic=$LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID --project=$LUZ_GCP_PROJECT_ID \
       --ack-deadline=30 \
       --min-retry-delay="10s" \
       --max-retry-delay="20s" \
       --expiration-period="never"\
       --message-filter='attributes.delivery_size = "SMALL"'
   if [ 0 -ne $? ]; then
       echo "Failed to create pull subscription [$LUZ_ELM5_PERSON_DATA_PUBSUB_SUBSCRIPTION_ID] to topic [$LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID]. Exiting."
       exit 1
   fi

   echo "Created pull subscription [$LUZ_ELM5_PERSON_DATA_PUBSUB_SUBSCRIPTION_ID] to topic [$LUZ_ELM5_PERSON_DATA_PUBSUB_TOPIC_ID]!"
fi
```

</div>

</div>
