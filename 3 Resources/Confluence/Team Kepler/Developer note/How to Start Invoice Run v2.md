---
ai_hash: de025d54d1611fd3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48979247109'
confluence_path: Team Kepler > Developer note
created: 2025-12-16
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- invoice-run
title: How to Start Invoice Run v2
type: source
updated: 2025-12-18
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48979247109/How+to+Start+Invoice+Run+v2
---

# How to Start Invoice Run v2

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48979247109/How+to+Start+Invoice+Run+v2) · updated 2025-12-18*

Forward ports:

```
kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev
kubectl port-forward --address 0.0.0.0 services/luz-webclient 8083:8081 -n dev
kubectl port-forward service/luz-database 5432:5432 -n dev
kubectl port-forward --address 0.0.0.0 services/luz-docs-creator-client 8084:8080 -n dev
```

Using you DB Client (I use the tool in IntelliJ):

- As we have forward port of “luz-database“ to localhost so the configuration in your local

![[image-20251218-015202.png]]

- When we connect successfully, we connect to database “luz-store“ and notice 2 table “invoice_run_v2“ and “invoice_item“

- Select any invoice run V2 with status “INVOICES_CALCUALTED“

Select an invoices run v2 and its item:

```
select *
from invoice_item
where invoice_run_uuid='{your-uuid}';

select *
from invoice_run_v2
where invoice_run_uuid='{your-uuid}';
```

Edit data in dev database:

```
UPDATE invoice_item
SET state='19', invoice_number=NULL
WHERE invoice_run_uuid='{your-uuid}';

UPDATE invoice_run_v2
SET state='INVOICES_CALCULATED'
WHERE invoice_run_uuid='{your-uuid}';
```

![[image-20251217-045942.png]]

![[image-20251217-050456.png]]

```
# Replace on your local:
#   host.docker.internal = local ip address
#   ports for database and rest apis
#   database name, user/pass

version: '3.9'
services:
  luz-store:
    build:
      context: '.'
    command: >
      bash -c "/opt/jboss/wildfly/bin/add-user.sh admin admin --silent
      && /opt/jboss/wildfly/bin/standalone.sh -c=standalone.xml -b 0.0.0.0 -bmanagement 0.0.0.0 --debug *:8788"

    environment:
      JAVA_TOOL_OPTIONS:

      CH_KLARA_RUN_AS_PUBLIC_KEY: 'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAhet2cNCjaqxui+7qJC6BJmtk9vb7Bx3Wnd2PmhPrtWK6ObGXwk8N0kaJcy5jzSAXtOevd8ZDxDfgXfqDIULkx5kNw82m8DoRp1SfPz5YuUEMCuAkMXaHxTKNrJmUCbRopLiwl1ri4J94HdBvEnBZ+Mf39u678ItEFjBSBx3XBSKToSviHrKa/UkJVhkQZ8S/2anZRAxe0U4dKymcDXNy7M/CoDw31VyWHBKO2ufat7oHdjUE347dIlnAIZ6uN76B2O3KSO5Zc5eI02UkzZ8BvBkiyWSGAFJPxtla/tE5gIn7affaLvDQe7+pbzE5yeb/Q5+tFpJIOKDTj+F3kxMLmwIDAQAB'
      CH_KLARA_RUN_AS_TOKEN: 'eyJhbGciOiJSUzUxMiJ9.eyJydW5Bc1JvbGUiOiJzeXN0ZW0iLCJpc3MiOiJjb20uYXhvbml2eSIsImV4cCI6MTU0ODMyNTg0OCwiaWF0IjoxNTQ4MzI0MDQ4fQ.giemeLOmVABg4bJL_kjXWVEnJy8IWnhkZjnc20T0j5mPa6LtafP1trgbxOLn3P5jZrpyM8hmH1B7wimCCDvwTx3b46XuwPI61DMtT5s2Lyx9wb12rVKss_MDAu632zndFJXCKafkHGvqD4h7-E2jLHXF0cEkqfqQO_O-6qCGoIZTId8r0QQPa2dthZ5W22D8DdyvwjYx_ZmRgsqeKRiilVz_im6SSoJTkstrKOAUAqCurLWhp-ukII-x-a8Npo8EHf3-40CtHJw-89HdqADK63Mjsp_aT6tntp2jSAJ2qVnMDDsN7eJ1QF8I82GmB8nLSBaqAtpes1f8GqOoYk8eZg'

      LUZ_STORE_JDBC: 'jdbc:postgresql://host.docker.internal:5555/luzstore'
      LUZ_STORE_DB_USER: 'postgres'
      LUZ_STORE_DB_PASS: 'postgres'
      #      LUZ_STORE_JDBC: 'jdbc:postgresql://host.docker.internal:5432/postgres'
      #      LUZ_STORE_DB_USER: 'postgres'
      #      LUZ_STORE_DB_PASS: '123456789'

      JWT_SERVICE_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_PERSON_HOST_PORT: 'http://host.docker.internal:8080'
      LUZFIN_FINANCE_HOST_PORT: 'http://host.docker.internal:8080'
      #      LUZFIN_FINANCE_HOST_PORT: 'http://host.docker.internal:8103'
      LUZ_COMPENSATION_HOST_PORT: 'http://host.docker.internal:8080'
      LUZTENANT_SERVICE_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_STORE_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_HUBSPOT_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_PROFILE_SERVICE_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_REPORTING_SERVICE_KEY: 'http://host.docker.internal:5000'
      LUZ_ADMIN_SERVICE_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_TENANT_DELETION_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_KEYVALUESTORE_HOST_PORT_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_keyvaluestore/api/'
      JWT_SERVICE_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luzsec/api/'
      LUZ_COMPENSATION_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_compensation/api/'
      LUZFIN_FINANCE_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luzfin_finance/api/'
      #      LUZFIN_FINANCE_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8103/luzfin_finance/api/'
      LUZ_PERSON_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_person/api/'
      LUZ_PROFILE_V2_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_profile/api/v2/'
      LUZ_TENANT_SERVICE_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luztenant/api/'
      LUZ_HUBSPOT_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_hubspot/api/'
      LUZ_ONLINE_PAYMENT_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_online_payment/api/'
      LUZ_MESSAGE_BROKER_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_message_broker/api/'
      #      LUZ_MESSAGE_BROKER_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8138/luz_message_broker/api/'
      LUZ_REPORTING_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_reporting'
      LUZ_ARTICLE_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_article/api/'
      LUZ_ACCOUTING_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_accounting/api/'
      LUZ_TENANT_DIR_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_tenant_dir/api/'
      LUZ_DOC_MANAGER_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_doc_manager/api/'
      LUZ_DOCS_STATISTIC_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_docs_statistic/api/'
      LUZ_WEB_CLIENT_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8083/luz/api/'
      LUZ_DOCS_VIEW_CONTROLLER_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_docs_view_controller/api/'
      LUZ_WEB_CLIENT_ENGINE_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8084/luz/api/'
      LUZ_ELETTER_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_eletter/api/'
      #      LUZ_ELETTER_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8137/luz_eletter/api/'
      LUZ_ADMIN_SERVICE_KEY_MP_REST_URL: 'http://host.docker.internal:8080/luz_admin_service/api/'

      KLARA_BUSINESS_AG_COMPANY_URL: '/luz_compensation/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/companies/1'
      KLARA_BUSINESS_AG_IBAN: 'CH78002352357E6731580'
      KLARA_BUSINESS_AG_ARTICLE_CODE: 'P0000016169'
      KLARA_BUSINESS_AG_ARTICLE_NO_VAT_RATE: 'P0000000009'

      KIE_USERNAME: 'admin'
      #      KIE_PASSWORD: 'Kie@KlaraDev'
      KIE_PASSWORD: 'admin'
      KIE_RULE_ENGINE_HOST_PORT: 'http://host.docker.internal:8080'
      KIE_CONTAINER_NAME: 'luz-store'
      KIE_SESSION_NAME: 'defaultKieSession'
      LUZ_ONLINE_PAYMENT_SERVICE_TENANT_CLIENT_ID: '437c9844-691c-4429-adfd-855ccee80292'
      LUZ_ONLINE_PAYMENT_SERVICE_TENANT_CLIENT_SECRET: 'fcd56103-7192-4700-b583-54b78190f67e'
      LUZ_DOCS_SERVICE_TENANT_CLIENT_ID: '814d88bd-d1be-421f-9929-be03e7871420'
      LUZ_DOCS_SERVICE_TENANT_CLIENT_SECRET: '2ae0c19c-d055-43a1-be35-7085fee9e359'
      CRON_USER: 'admin'
      CRON_PASSWORD: 'nDVOE2cf3bAytJbw48jN7w=='
      CH_KLARA_MAIL_HOST: mail.axonivy.io
      CH_KLARA_MAIL_PORT: 465
      CH_KLARA_MAIL_SSL: 'true'
      CH_KLARA_MAIL_TLS: 'false'
      CH_KLARA_EMAIL_SERVER_MAILADDRESS: 'noreply@axonivy.io'
      CH_KLARA_MAIL_SENDER_USERNAME: 'klara-dev@axonivy.io'
      CH_KLARA_MAIL_SENDER_PASSWORD: 'axonivy'
      CH_KLARA_CONTRACT_RECEIVER_EMAIL: 'test.contract@axonivy.io'
      CH_KLARA_MAX_NUMBER_INVOICED_CUSTOMER: 50

      MAX_POST_SIZE: 1073741274
      REQUEST_TIMEOUT: 7200000
      CH_KLARA_BUSINESS_AG_MAILADDRESS: 'noreply@axonivy.io'
      CH_KLARA_BUSINESS_AG_SENDER_NAME: 'ePost Service AG'

      SWISS_BANKERS_CARD_CLEAN_UP_ENABLED: 'false'

      TENANT_DUNNING_JOB_SCHEDULE: '{year_:"*", month_:"*", dayOfMonth_:"*", dayOfWeek_:"*", hour_:"1", minute_:"30", second_:"0"}'

      #this for dev-vn env TESTING PUBSUB

      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_INVOICE_GROUP: 'thanh-invoice-group-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_INVOICE_GROUP: 'thanh-invoice-group-topic-company-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_INVOICE_GROUP: 'thanh-invoice-group-topic-individual-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_INVOICE_ID: 'thanh-invoice-id-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_INVOICE_ID: 'thanh-invoice-id-topic-company-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_INVOICE_ID: 'thanh-invoice-id-topic-individual-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_SENDING_MAIL_GROUP: 'thanh-invoice-group-sending-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_SENDING_GROUP: 'thanh-invoice-group-sending-topic-company-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_SENDING_GROUP: 'thanh-invoice-group-sending-topic-individual-sub'

      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_INVOICE_GROUP: 'thanh-luz-store-invoice-run-invoice-group-topic'
      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_INVOICE_GROUP: 'thanh-luz-store-invoice-run-company-invoice-group-sub'
      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_INVOICE_GROUP: 'thanh-luz-store-invoice-run-individual-invoice-group-sub'
      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_INVOICE_ID: 'thanh-luz-store-invoice-run-single-topic'
      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_INVOICE_ID: 'thanh-luz-store-invoice-run-company-single-sub'
      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_INVOICE_ID: 'thanh-luz-store-invoice-run-individual-single-sub'
      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_SENDING_MAIL_GROUP: 'thanh-luz-store-invoice-run-sending-group-topic'
      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_SENDING_GROUP: 'thanh-luz-store-invoice-run-company-sending-group-sub'
      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_SENDING_GROUP: 'thanh-luz-store-invoice-run-individual-sending-group-sub'
      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_TRIGGER_PROCESS: 'thanh-luz-store-invoice-run-trigger-process-topic'
      LUZ_STORE_PUBSUB_INVOICE_RUN_TRIGGER_PROCESS_SUBSCRIPTION_ID: 'thanh-luz-store-invoice-run-trigger-process-sub'

      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_INVOICE_GROUP: 'dev-vn-luz-store-invoice-run-invoice-group-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_INVOICE_GROUP: 'dev-vn-luz-store-invoice-run-company-invoice-group-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_INVOICE_GROUP: 'dev-vn-luz-store-invoice-run-individual-invoice-group-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_INVOICE_ID: 'dev-vn-luz-store-invoice-run-single-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_INVOICE_ID: 'dev-vn-luz-store-invoice-run-company-single-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_INVOICE_ID: 'dev-vn-luz-store-invoice-run-individual-single-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_SENDING_MAIL_GROUP: 'dev-vn-luz-store-invoice-run-sending-group-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_SENDING_GROUP: 'dev-vn-luz-store-invoice-run-company-sending-group-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_SENDING_GROUP: 'dev-vn-luz-store-invoice-run-individual-sending-group-sub'

      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_INVOICE_GROUP: 'dev-vn-luz-store-invoice-run-invoice-group-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_INVOICE_GROUP: 'dev-vn-luz-store-invoice-run-company-invoice-group-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_INVOICE_GROUP: 'dev-vn-luz-store-invoice-run-individual-invoice-group-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_INVOICE_ID: 'dev-vn-luz-store-invoice-run-single-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_INVOICE_ID: 'dev-vn-luz-store-invoice-run-company-single-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_INVOICE_ID: 'dev-vn-luz-store-invoice-run-individual-single-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_TOPIC_SENDING_MAIL_GROUP: 'dev-vn-luz-store-invoice-run-sending-group-topic'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_COMPANY_SUBSCRIPTION_SENDING_GROUP: 'dev-vn-luz-store-invoice-run-company-sending-group-sub'
      #      LUZ_STORE_PUBSUB_INVOICE_RUN_INDIVIDUAL_SUBSCRIPTION_SENDING_GROUP: 'dev-vn-luz-store-invoice-run-individual-sending-group-sub'

      LUZ_STORE_PUBSUB_EARCHIVE_SUBSCRIPTION_EVENT_TOPIC_ID: 'local-earchive-subscription-event-topic'
      LUZ_STORE_PUBSUB_EARCHIVE_UNSUBSCRIPTION_EVENT_TOPIC_ID: 'local-topic-unsubscribe-earchive-subscription-event'

      LUZ_GCP_PROJECT_ID: 'klara-nonprod'
      INVOICE_GROUP_MESSAGE_MAX_OUTSTANDING_ELEMENT_COUNT: 30
      INVOICE_MESSAGE_MAX_OUTSTANDING_ELEMENT_COUNT: 250
      SENDING_GROUP_MESSAGE_MAX_OUTSTANDING_ELEMENT_COUNT: 50
      EXECUTOR_THREAD_COUNT: 4
      INVOICE_MESSAGE_PARALLEL_PULL_COUNT: 1
      CREDIT_CARD_CHARGE_SEMAPHORE_PERMITS: 1
      COM_AXONIVY_LUZ_STORAGE_IS_ENABLE: 'false'
      pubsub-adminsdk-key.json: '{
                "type": "service_account",
                "project_id": "klara-nonprod",
                "private_key_id": "<REDACTED>",
                "private_key": "<REDACTED — rotate this key>",
                "client_email": "klara-dev-vn-pubsub@klara-nonprod.iam.gserviceaccount.com",
                "client_id": "105105867379155648927",
                "auth_uri": "https://accounts.google.com/o/oauth2/auth",
                "token_uri": "https://oauth2.googleapis.com/token",
                "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
                "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/klara-dev-vn-pubsub%40klara-nonprod.iam.gserviceaccount.com"
              }'
      FILESTORE_TENANT_CACHE_MEMORY_EXPIRY_SECONDS: 36000
      FILESTORE_TENANT_MEMORY_CAPACITY_INITIAL: 100
      FILESTORE_TENANT_MEMORY_CAPACITY_MAXIMUM: 50000
      FILE_TENANT_WRITE_FILE_EXPIRY_THRESHOLD_MINUTES: 5
      FILESTORE_TENANT_PATH: '/mnt/individual'
      FILESTORE_TENANT_RESOURCE_COMPANY_DOCUMENT_NAME: 'company_documents'

    volumes:
      - ./log:/opt/jboss/wildfly/standalone/log
    ports:
      - 8104:8080
      - 8788:8788

#      - "8104-8106:8080"
#      - "8788-8790:8788"

volumes:
  redis-data:
    driver: local

```

- After setup to run the luz-store

%% ai-graph-start %%

**Related notes:**
- [[Port forward and Docker compose]]
- [[Invoice Run V2UAT - No error when luz-store is running with multiple pods]]
- [[Port Forward to call GCP API in localhost]]
- [[Call API trigger Vacuum POS schema on PROD]]
- [[How to call generic interface document API on dev]]

%% ai-graph-end %%