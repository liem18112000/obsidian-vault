---
title: "Use Case: Run by Test Set - Complete Process Flow"
created: 2026-03-13
updated: 2026-03-13
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49229463570/Use+Case+Run+by+Test+Set+-+Complete+Process+Flow
confluence_id: "49229463570"
confluence_path: "Team Kepler > Developer note > Integration Test with Xray and Cucumber"
tags: [confluence, xray]
---

# Use Case: Run by Test Set - Complete Process Flow

*Confluence source · Team Kepler › Developer note › Integration Test with Xray and Cucumber · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49229463570/Use+Case+Run+by+Test+Set+-+Complete+Process+Flow) · updated 2026-03-13*

![[image-20260313-020345.png]]

### Phase 1 — CI/CD Trigger & Infrastructure Setup

A push to the `master` branch on Bitbucket triggers Cloud Build:

- Connects to the GKE cluster

- Creates a Kubernetes Job

- Copies the source code into the pod

- Sends a Slack notification with links to the build and GKE logs.

> [!note]- Sequence Diagram
>
> ![[image-20260313-010043.png]]
>

> [!note]- Sample Log: CI/CD Trigger & Infrastructure Setup
>
> `Execution logs: `[https://console.cloud.google.com/logs/query;query=resource.labels.project_id%3D%22klara-nonprod%22%0Aresource.labels.location%3D%22europe-west6-a%22%0Aresource.labels.cluster_name%3D%22klara-nonprod%22%0Aresource.labels.namespace_name%3D%22dev%22%0Alabels.k8s-pod%2Fbatch_kubernetes_io%2Fcontroller-uid%3D%2229f6109c-d519-4e9f-9be1-b76215c8d515%22;timeRange=2026-03-12T09:38:59Z%2F2026-03-12T10:43:59Z?project=klara-nonprod](https://console.cloud.google.com/logs/query;query=resource.labels.project_id%3D%22klara-nonprod%22%0Aresource.labels.location%3D%22europe-west6-a%22%0Aresource.labels.cluster_name%3D%22klara-nonprod%22%0Aresource.labels.namespace_name%3D%22dev%22%0Alabels.k8s-pod%2Fbatch_kubernetes_io%2Fcontroller-uid%3D%2229f6109c-d519-4e9f-9be1-b76215c8d515%22;timeRange=2026-03-12T09:38:59Z%2F2026-03-12T10:43:59Z?project=klara-nonprod)
>
> ![[image-20260313-014436.png]]
>

![[image-20260313-075729.png]]

### Phase 2 — Pod Bootstrap

- The pod starts with a polling loop waiting for the source code (signaled by a `/workspace/ready` file).

- Once ready, it creates a Python virtual environment, installs dependencies and launches the Use Case 2 entry point.

> [!note]- Sequence Diagram
>
> ![[image-20260313-010128.png]]
>

> [!note]- Configuration: cloudbuild-xray.yml and gke-xray.yml
>
>
>
> ```
> steps:
>   - id: "run-integration-tests"
>     name: "europe-west6-docker.pkg.dev/klara-infra/google-cloud-build/gcloud"
>     entrypoint: "bash"
>     args:
>       - "-c"
>       - |
>         set -e
>         gcloud container clusters get-credentials "${_GKE_CLUSTER}" --zone "${_GCP_LOCATION}" --project "${_GCP_PROJECT_ID}"
>         BUILD_TRIGGER_URL=$$(gcloud builds describe "${BUILD_ID}" --project "${PROJECT_ID}" --region "${LOCATION}" --format="value(logUrl)")
>
>         # Delete existing job if it exists and create a new one
>         kubectl delete job "${_GKE_JOB_NAME}" -n "${_GKE_NAMESPACE}" --ignore-not-found=true
>         sed \
>           -e "s/name: luz-docs-integration-test-job/name: ${_GKE_JOB_NAME}/" \
>           -e "s/memory: .* # limit/memory: \"${_GKE_JOB_POD_LIM_MEM}\" # limit/" \
>           -e "s/cpu: .* # limit/cpu: ${_GKE_JOB_POD_LIM_CPU} # limit/" \
>           -e "s~build-trigger: \".*\"~build-trigger: \"$$BUILD_TRIGGER_URL\"~" \
>           -e "s~\$${_DEFAULT_CMD}~${_DEFAULT_CMD}~" \
>           -e "s~\$${_ENV_VARS}~${_ENV_VARS}~" \
>           -e "s~\$${_XRAY_CLIENT_ID}~${_XRAY_CLIENT_ID}~" \
>           -e "s~\$${_XRAY_CLIENT_SECRET}~${_XRAY_CLIENT_SECRET}~" \
>           -e "s~\$${_XRAY_MAX_WORKERS}~${_XRAY_MAX_WORKERS}~" \
>           -e "s~\$${_JIRA_BASE_URL}~${_JIRA_BASE_URL}~" \
>           -e "s~\$${_JIRA_EMAIL}~${_JIRA_EMAIL}~" \
>           -e "s~\$${_JIRA_API_TOKEN}~${_JIRA_API_TOKEN}~" \
>           -e "s~\$${_XRAY_PROJECT_KEY}~${_XRAY_PROJECT_KEY}~" gke/k8s-xray.yaml | kubectl apply -f - -n "${_GKE_NAMESPACE}"
>
>         # Wait for the pod to be created and ready
>         until kubectl get pod -l job-name=${_GKE_JOB_NAME} -n ${_GKE_NAMESPACE} --no-headers | grep -q .; do sleep 2; done
>         kubectl wait --for=condition=Ready pod -l job-name=${_GKE_JOB_NAME} -n ${_GKE_NAMESPACE} --timeout=60s
>
>         # Transfer src to the job pod
>         JOB_POD_NAME=$(kubectl get pod -l job-name=${_GKE_JOB_NAME} -n ${_GKE_NAMESPACE} -o jsonpath='{.items[0].metadata.name}')
>         kubectl cp . ${_GKE_NAMESPACE}/$$JOB_POD_NAME:/workspace
>         kubectl exec -n ${_GKE_NAMESPACE} $$JOB_POD_NAME -- touch /workspace/ready
>
>         # Direct link to job logs with time range
>         GKE_JOB_UID=$(kubectl get job ${_GKE_JOB_NAME} -n ${_GKE_NAMESPACE} -o jsonpath='{.metadata.uid}')
>         LOG_START_TIME=$$(awk 'BEGIN {print strftime("%Y-%m-%dT%H:%M:%SZ", systime() - 300, 1)}')
>         LOG_END_TIME=$$(awk 'BEGIN {print strftime("%Y-%m-%dT%H:%M:%SZ", systime() + 3600, 1)}')
>         LOG_TIME_RANGE="$$LOG_START_TIME%2F$$LOG_END_TIME"
>         GKE_JOB_LOG_LINK="https://console.cloud.google.com/logs/query;query=resource.labels.project_id%3D%22${_GCP_PROJECT_ID}%22%0Aresource.labels.location%3D%22${_GCP_LOCATION}%22%0Aresource.labels.cluster_name%3D%22${_GKE_CLUSTER}%22%0Aresource.labels.namespace_name%3D%22${_GKE_NAMESPACE}%22%0Alabels.k8s-pod%2Fbatch_kubernetes_io%2Fcontroller-uid%3D%22$$GKE_JOB_UID%22;timeRange=$$LOG_TIME_RANGE?project=${_GCP_PROJECT_ID}"
>         echo "Execution logs: $$GKE_JOB_LOG_LINK"
>
>         if [ "${_SLACK_NOTIFICATION_ENABLE}" = "true" ]; then
>           send_slack_msg() {
>             curl -s -X POST -H "Content-type: application/json" \
>               --data "{\"channel\":\"${_SLACK_CHANNEL}\",\"icon_url\":\"${_SLACK_ICON_URL}\",\"username\":\"Cloud Build\",\"attachments\":[{\"color\":\"$1\",\"title\":\"${_SLACK_MESSAGE_TITLE}\",\"text\":\"$2\",\"title_link\":\"https://console.cloud.google.com/cloud-build/builds;region=europe-west6/${BUILD_ID}?project=${PROJECT_ID}\"}]}" \
>               "$$SLACK_WEBHOOK_URL" > /dev/null
>           }
>           send_slack_msg "good" "GKE integration tests for job \`${_GKE_JOB_NAME}\` have triggered.\nBranch: \`${BRANCH_NAME}\`.\nCommit: \`${COMMIT_SHA}\`.\nView logs: <$$GKE_JOB_LOG_LINK|GKE Job Logs>."
>         fi
>
>     secretEnv: ["SLACK_WEBHOOK_URL"]
>
> availableSecrets:
>   secretManager:
>     - env: SLACK_WEBHOOK_URL
>       versionName: projects/518364204428/secrets/slack-webhook-url/versions/1
>
> substitutions:
>   _GCP_PROJECT_ID: klara-nonprod
>   _GCP_LOCATION: europe-west6-a
>   _GKE_CLUSTER: klara-nonprod
>   _GKE_NAMESPACE: dev
>   _GKE_JOB_NAME: "${_GKE_NAMESPACE}-luz-docs-it-${BUILD_ID}"
>   _GKE_JOB_POD_LIM_CPU: "1"
>   _GKE_JOB_POD_LIM_MEM: "2Gi"
>   _ENV_VARS: ""
>   _TEST_SET_KEY: "LUZ-149913"
>   _XRAY_CLIENT_ID: ""
>   _XRAY_CLIENT_SECRET: ""
>   _XRAY_PROJECT_KEY: ""
>   _XRAY_MAX_WORKERS: "8"
>   _JIRA_BASE_URL: "https://axonivy.atlassian.net"
>   _JIRA_EMAIL: "liem.doanvanthanh@axonactive.com"
>   _JIRA_API_TOKEN: ""
>   _DEFAULT_CMD: "python -m helpers.xray.service ${_TEST_SET_KEY}"
>   _SLACK_CHANNEL: luz-docs-integration-test
>   _SLACK_NOTIFICATION_ENABLE: "true"
>   _SLACK_ICON_URL: data:image/png;base64,iVBORw0
>   _SLACK_MESSAGE_TITLE: "${_GKE_NAMESPACE}-luz-docs-integration-test"
>
> options:
>   logging: CLOUD_LOGGING_ONLY
> ```
>
>
>
>
>
> ```
> apiVersion: batch/v1
> kind: Job
> metadata:
>   name: luz-docs-integration-test-job
>   annotations:
>     owner: "kepler"
>     build-trigger: ""
>     description: "test luz-docs API functions"
>     repository: "https://bitbucket.org/axonivy-prod/luz_docs_integration_test/src/master/"
> spec:
>   ttlSecondsAfterFinished: 10
>   template:
>     metadata:
>       name: luz-docs-integration-test
>     spec:
>       containers:
>         - name: python
>           image: python:slim
>           imagePullPolicy: Always
>           env:
>             - name: PYTHONUNBUFFERED
>               value: "1"
>             - name: XRAY_CLIENT_ID
>               value: "${_XRAY_CLIENT_ID}"
>             - name: XRAY_CLIENT_SECRET
>               value: "${_XRAY_CLIENT_SECRET}"
>             - name: XRAY_PROJECT_KEY
>               value: "${_XRAY_PROJECT_KEY}"
>             - name: XRAY_MAX_WORKERS
>               value: "${_XRAY_MAX_WORKERS}"
>             - name: JIRA_BASE_URL
>               value: "${_JIRA_BASE_URL}"
>             - name: JIRA_EMAIL
>               value: "${_JIRA_EMAIL}"
>             - name: JIRA_API_TOKEN
>               value: "${_JIRA_API_TOKEN}"
>           command:
>             [
>               "/bin/bash",
>               "-c",
>               "echo 'waiting for source'; for i in {1..150}; do [ -f /workspace/ready ] && { cd /workspace && python -m venv venv && . venv/bin/activate && pip install -q -r requirements.txt && exec env ${_ENV_VARS} ${_DEFAULT_CMD}; }; sleep 2; done; echo 'timeout'; exit 1",
>             ]
>           resources:
>             requests:
>               cpu: 10m
>               memory: "0.5Gi"
>             limits:
>               cpu: 1 # limit
>               memory: "2Gi" # limit
>       restartPolicy: Never
> ```
>
>
>

### Phase 3 — Step 1: Export .feature files from Xray

- Authenticates with Xray Cloud to get a Bearer token (cached for all subsequent calls)

- Downloads the `.feature` files for the given Test Set key as a ZIP

- Extracts them into `features_export/`.

> [!note]- Sequence Diagram
>
> ![[image-20260313-010243.png]]
>

> [!note]- Sample Log : Export .feature files from Xray
>
> ![[image-20260313-012707.png]]
>

### Phase 4 — Step 2: Execute per feature file (parallel)

- Prepares the execution environment (creates `results/`, symlinks Behave support files)

- Pre-warms the Xray token

> [!note]- Xray Token Cache and Pre-warm
>
> **Xray Token Cache**
>
> - Authenticate with Xray Cloud once and reuse the Bearer token for the entire run.
>
> - The cache is thread-safe so parallel workers never race to authenticate simultaneously.
>
>
>
> > [!note]- How it works
>>
>>
>>
> > ```
> > _cached_token: str | None = None
> > _token_lock = threading.Lock()
>>
> > def get_xray_token() -> str:
> >     global _cached_token
> >     with _token_lock:
> >         if _cached_token:
> >             return _cached_token
>>
> >         response = requests.post(...)
> >         response.raise_for_status()
> >         _cached_token = response.json()
> >         return _cached_token
> > ```
>>
>>
>>
> > Two module-level variables hold the cache:
>>
> > - `_cached_token` — stores the token after the first successful authentication, starts as `None`.
>>
> > - `_token_lock` — a `threading.Lock()` that serializes access so only one thread can authenticate at a time.
>>
> >   - `threading.Lock()` ensures mutual exclusion (apply in (1))
>>
>>
>>
> > ```
> > Thread worker_0: acquire lock → check cache (None) → POST /authenticate → store token → release lock
> > Thread worker_1: acquire lock → check cache (token exists) → return cached → release lock
> > Thread worker_2: acquire lock → check cache (token exists) → return cached → release lock
> > Thread worker_N: acquire lock → check cache (token exists) → return cached → release lock
> > ```
>>
>>
>>
> > - Only the first thread to acquire the lock makes the HTTP request. All subsequent threads find the cached token and return immediately.
>>
> > - The lock protects both the **read** (`if _cached_token`) and the **write** (`_cached_token = response.json()`).
>>
> > - If only the write were locked, two threads could both read `None`, both pass the check, and both send authentication requests (a race condition known as "check-then-act").
>>
>
>
>
>
> > [!note]- Sequence Diagram
>>
> > ![[image-20260313-055307.png]]
>>
>
>
> **Pre-warm strategy**
>
> - The token is explicitly pre-warmed before the thread pool starts
>
>
>
> > [!note]- How it work
>>
>>
>>
> > ```
> > # Pre-warm: cache Xray token before spawning workers
> > get_xray_token()
> > with ThreadPoolExecutor(max_workers=max_workers, thread_name_prefix="worker") as executor:
> >     ...
> > ```
>>
>>
>>
> > Since Phase 3 (export) already called `get_xray_token()`, the token is already cached at this point. The pre-warm call returns immediately from cache. However, if `execute_tests()` were ever called independently (without export), this ensures the token is ready before threads start, avoiding the scenario where all 8 threads race to authenticate at once.
>>
>
>

- Spawns a thread pool where each `.feature` file runs stages 4a through 4e independently.

> [!note]- Thread Pool Execution Mechanism
>
> Submit tasks to the thread pool:
>
> - Creates a pool of reusable threads (up to `max_workers` - this can be configured, default 8).
>
> - Schedules the function to run on the next available thread
>
> - **All tasks are submitted at once.**
>
> - The executor queues them internally and excess tasks wait in the queue until a thread becomes available.
>
> - Each thread operates on **its own feature file** with **its own output paths** (timestamped).
>
> - There is no shared mutable state between threads except:
>
>   - Xray token
>
>   - HTTP session
>
>   - Jira auth (HTTPBasicAuth)
>
> Yielding futures in the order they finish, not the order they were submitted.
>
>
>
> ```
> for future in as_completed(futures):
>     try:
>         result = future.result()
>         ...
>     except Exception as e:
>         ...
> ```
>
>
>
> - Fast-completing features are handled first. (by using `as_completed(futures)`)
>
>   - The code uses `as_completed` rather than `executor.map`
>
>
>
> ```
> # as_completed — used in this project
> for future in as_completed(futures):
>     result = future.result()  # yields in completion order
>
> # map — alternative (not used)
> for result in executor.map(_run_and_import_feature, feature_files):
>     ...  # yields in submission order
> ```
>
>
>
> - The main thread is never blocked waiting for a slow feature while fast ones are done.
>
> - Results appear in non-deterministic order.
>
> - If raises an unhandled exception, `future.result()` re-raises it in the main thread, caught by the `except` block =\> The other features continue unaffected.
>
> Why threads (not processes) =\> The workload is **I/O-bound**, not CPU-bound:
>
>
>
> > [!note]- Key Differences: Thread vs. Process
>>
> > In Python, the primary difference is that 
>>
> > - **Threads run within a single process and share memory, but are limited by the Global Interpreter Lock (GIL)** 
>>
> > - **Processes run in separate memory spaces** and thus bypass the GIL to achieve true parallelism on multi-core machines. 
>>
> > The choice between threads and processes depends on the nature of the task:
>>
> > - **Use Threads for I/O-bound tasks**:
>>
> >   - Ideal for operations that involve waiting for external systems, such as network requests, file I/O, or database queries.
>>
> >   - The GIL is released during these waiting periods, allowing other threads to run concurrently and making the application more responsive.
>>
> > - **Use Processes for CPU-bound tasks**:
>>
> >   - Best for tasks that require intensive computation and fully utilize the CPU cores, such as data processing, scientific calculations, or image manipulation.
>>
> >   - Since each process has its own Python interpreter and GIL, they can run in parallel on multiple cores to significantly speed up execution.
>>
>
>
>
>
> > [!note]- Example log output
>>
>>
>>
> > ```
> > Found 3 feature file(s) for LUZ-149913
> > Running with 3 parallel worker(s)
>>
> >   [worker_0] Starting create_document
> >   [worker_1] Starting delete_document
> >   [worker_2] Starting search_document
> >   [worker_0] Pre-execute: transitioning tests to In Progress
> >   [worker_1] Pre-execute: transitioning tests to In Progress
> >   [worker_2] Pre-execute: transitioning tests to In Progress
> >   [worker_1] Converting Behave JSON to Cucumber JSON
> >   [worker_0] Converting Behave JSON to Cucumber JSON
> >   [worker_1] Importing result to Xray
> >   [worker_0] Importing result to Xray
> >   [worker_2] Converting Behave JSON to Cucumber JSON
> >   [worker_1] Post-execute: adding automation comment
> >   [worker_1] Transitioning 1 passed test(s) to Done: ['LUZ-12346']
>>
> > ============================================================
> > Feature: delete_document.feature [PASSED]
> > ============================================================
> > Feature: Delete document ...
>>
> >   [worker_0] Post-execute: adding automation comment
> >   [worker_0] Transitioning 1 passed test(s) to Done: ['LUZ-12345']
>>
> > ============================================================
> > Feature: create_document.feature [PASSED]
> > ============================================================
> > Feature: Create document ...
>>
> >   [worker_2] Importing result to Xray
> >   [worker_2] Post-execute: adding automation comment
> >   [worker_2] No passed test keys found in LUZ-149913_search_document_...json
>>
> > ============================================================
> > Feature: search_document.feature [PASSED]
> > ============================================================
> > Feature: Search document ...
>>
> > ============================================================
> > Completed 3/3 feature(s)
> > ============================================================
> > ```
>>
>>
>>
>
>

#### Stage 4a — Pre-Execute: Transition tests to In Progress

- Scans the feature file for `@TEST_*` tags using regex,

- Calls the Jira REST API to transition each matched test issue to "In Progress" (GET available transitions, POST the matching one).

- This marks tests as actively running before Behave starts.

> [!note]- Sequence Diagram
>
> ![[image-20260313-011712.png]]
>

> [!note]- Sample Log: Pre-Execute: Transition tests to In Progress
>
> ![[image-20260313-013001.png]]
>

#### Stage 4b — Run Behave

Runs `behave` as a subprocess with two formatters:

- Behave JSON: `results/LUZ-149913_create_document_13-03-2026_10-30-00_behave.json`

- Cucumber JSON: `results/LUZ-149913_create_document_13-03-2026_10-30-00.json`

```
behave {feature_file} -f json -o {behave_json_path} -f pretty --no-capture
```

- Example:

```
behave features_export/1_LUZ-12345_create_document.feature -f json -o results/LUZ-149913_create_document_13-03-2026_10-30-00_behave.json -f pretty --no-capture
```

> [!note]- Sequence Diagram
>
> ![[image-20260313-011747.png]]
>

> [!note]- Sample Log: Run Behave
>
> ![[image-20260313-013201.png]]
>

#### Stage 4c — Convert Behave JSON to Cucumber JSON

Uses the `behave2cucumber` library to convert Behave's JSON schema to Cucumber format :

> [!note]- Transformation summary
>
>
> |  |  |  |
> |----|----|----|
> | Field | Behave format | Cucumber format |
> | Location | `"location": "file.feature:10"` | `"uri": "file.feature"` + `"line": 10` |
> | Tags | `["smoke", "regression"]` | `[{"name": "@smoke", "line": 9}, ...]` |
> | Status | `"status": "passed"` at feature/element level | Removed |
> | Step type | `"step_type": "given"` | Removed |
> | Skipped result | No `result` object | `{"status": "skipped", "duration": 0}` |
> | Error message | Full string | Truncated to 2000 chars |
> | Duration | Seconds (float) | Nanoseconds (int) — only with `duration_format=True` |
> | Table | `{"headings": [...], "rows": [...]}` | `[{"cells": [...], "line": N}, ...]` |
> | ID | Not present | Integer counter |
> | Description | Not present | Empty string `""` |
> | URI | Inside `location` | Extracted to separate `"uri"` field |
>
>

Then applies Xray-specific post-processing:

- Ensures feature/scenario IDs are strings

- Converts step durations from float seconds to integer nanoseconds.

- Deletes the intermediate Behave JSON file afterward.

> [!note]- Sequence Diagram
>
> ![[image-20260313-011817.png]]
>

> [!note]- Examples
>
> ### Behave JSON (input)
>
>
>
> ```
> [{
>   "keyword": "Feature",
>   "name": "Create Document",
>   "location": "features_export/1_LUZ-12345_create_document.feature:1",
>   "status": "passed",
>   "tags": ["TEST_LUZ-12345"],
>   "elements": [{
>     "keyword": "Scenario",
>     "name": "Verify creation",
>     "location": "features_export/1_LUZ-12345_create_document.feature:4",
>     "tags": ["TEST_LUZ-12345"],
>     "steps": [{
>       "keyword": "Given",
>       "name": "a valid request",
>       "location": "features/steps/create_document_steps.py:10",
>       "step_type": "given",
>       "result": {"status": "passed", "duration": 0.052}
>     }]
>   }]
> }]
> ```
>
>
>
> ### Cucumber JSON (output)
>
>
>
> ```
> [{
>   "keyword": "Feature",
>   "name": "Create Document",
>   "id": "create-document",
>   "uri": "features_export/1_LUZ-12345_create_document.feature",
>   "line": 1,
>   "description": "",
>   "tags": [{"name": "@TEST_LUZ-12345", "line": 0}],
>   "elements": [{
>     "keyword": "Scenario",
>     "name": "Verify creation",
>     "id": "create-document;verify-creation",
>     "uri": "features_export/1_LUZ-12345_create_document.feature",
>     "line": 4,
>     "description": "",
>     "tags": [{"name": "@TEST_LUZ-12345", "line": 3}],
>     "steps": [{
>       "keyword": "Given",
>       "name": "a valid request",
>       "line": 10,
>       "result": {"status": "passed", "duration": 52000000}
>     }]
>   }]
> }]
> ```
>
>
>
> Key differences:
>
> - `location` split into `uri` + `line`
>
> - `status` and `step_type` removed
>
> - `id` assigned as string (feature-name and feature-name;scenario-name)
>
> - `description` added as empty string
>
> - `tags` converted to objects with `@` prefix
>
> - `duration` converted from `0.052` (seconds) to `52000000` (nanoseconds)
>

#### Stage 4d — Import Result to Xray

- Uploads the Cucumber JSON result to Xray Cloud via `POST /import/execution/cucumber/multipart` with two parts:

  - results.json: Cucumber JSON file (binary read)

  - info.json: Test Execution metadata (JSON string)

<!-- -->

- Xray creates a Test Execution issue: `Test Run - {test_set_key}_{datetime.now().strftime('%d-%m-%Y_%H-%M-%S')}`

  - **Example**: `"Test Run - LUZ-149913_13-03-2026_10-30-00"` =\> Need to be improve for meaningful name for test execution

- Links test results by matching `@TEST_*` tags

> [!note]- Sequence Diagram
>
> ![[image-20260313-011852.png]]
>

> [!note]- Sample Log: Import Result to Xray
>
> ![[image-20260313-013356.png]]![[image-20260313-013957.png]]
>

#### Stage 4e — Post-Execute: Comment & Transition

Two actions:

1.  Posts an automation comment on the Test Execution issue via Jira REST API using Atlassian Document Format.

2.  Reads the Cucumber JSON:

    - Where every step passed, extracts their `@TEST_*` keys, and transitions those test issues to "Done" in Jira.

    - Failed tests stay "In Progress".

> [!note]- Sequence Diagram
>
> ![[image-20260313-011937.png]]
>

> [!note]- Sample Log: Post-Execute: Comment & Transition
>
> ![[image-20260313-013541.png]]![[image-20260313-014114.png]]
>

### Phase 5 — Step 3: Cleanup & Finish

- Removes all files from `features_export/` and `results/` directories (including symlinked support files).

- After cleanup, the service prints completion and the pod exits.

- The Kubernetes Job is auto-deleted after 10 seconds (`ttlSecondsAfterFinished: 10`).

> [!note]- Sequence Diagram
>
> ![[image-20260313-010449.png]]
>

> [!note]- Sample Log: Cleanup & Finish
>
> ![[image-20260313-013735.png]]
>
