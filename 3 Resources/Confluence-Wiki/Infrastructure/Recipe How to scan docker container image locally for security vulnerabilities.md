---
title: "Recipe: How to scan docker container image locally for security vulnerabilities"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47097282643/Recipe+How+to+scan+docker+container+image+locally+for+security+vulnerabilities
space: "LUZ"
topic: infra
relevance: 0.773
depth: 3
updated: 2022-04-26
attachments: 0
tags:
  - confluence
  - infra
  - space/luz
---

# Recipe: How to scan docker container image locally for security vulnerabilities

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-04-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47097282643/Recipe+How+to+scan+docker+container+image+locally+for+security+vulnerabilities)
> Relevance 0.773 · topic `infra`

This recipe shows how to scan a docker container image locally for security vulnerabilities with grype tool.

## Installation

### MacOS and Linux

The official installation guide for macOS and Linux can be found here: <a href="https://github.com/anchore/grype" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/anchore/grype</a> .

### Windows

1.  Install WSL 2 for windows 10

    (Reference: <a href="https://docs.microsoft.com/en-us/windows/wsl/install-win10" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.microsoft.com/en-us/windows/wsl/install-win10</a>)

2.  Install grype via script in WSL 2 to /usr/local/bin

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="eb329491-aa57-4d0e-b254-dca3d5b05f51" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sudo sh -s -- -b /usr/local/bin
    ```

    </div>

    </div>

    (Reference: <a href="https://github.com/anchore/grype#installation" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/anchore/grype#installation</a> )

3.  Open up grype in WSL 2 linux on windows and run grype command to check the version successfully:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="050343d3-cf75-4a75-9d17-8148a50b916c" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    grype version

    # Sample output:
    #
    # Application:          grype
    # Version:              0.13.0
    # BuildDate:            2021-06-02T01:57:12Z
    # GitCommit:            3d21b8397d65770d292184b09a4f676bce6f3ec8
    # GitTreeState:         clean
    # Platform:             linux/amd64
    # GoVersion:            go1.16.4
    # Compiler:             gc
    # Supported DB Schema:  3
    ```

    </div>

    </div>

4.  As grype will call docker from inside WSL you must login to your gcp account again (even if you can already pull docker images from our registry using cmd/PowerShell). First install the gcloud-cli (e.g. using snap):

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1af3f52c-c694-49de-9d95-d9fe0127a918" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    sudo snap install google-cloud-cli --classic
    ```

    </div>

    </div>

5.  Run `gcloud auth login` in your WSL shell. This will complain about not having access to a web-browser:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bbfadd0a-66ae-4e19-bdc2-9a075eb1079f" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    You are authorizing gcloud CLI without access to a web browser. 
    Please run the following command on a machine with a web browser and copy 
    its output back here. Make sure the installed gcloud version is 372.0.0 or newer.

    gcloud auth login --remote-bootstrap="https://accounts.google.com/o/oauth2/auth?[...]"

    Enter the output of the above command:
    ```

    </div>

    </div>

6.  Copy the command above and run it in a Windows **Command Prompt**. This will open a browser window, prompt you to login an then output a message such as:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f4cc43c3-dcfe-4acf-aea6-4eafeecbe93c" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    C:\Users\lt>gcloud auth login --remote-bootstrap="https://accounts.google.com/o/oauth2/auth?[...]"
    DO NOT PROCEED UNLESS YOU ARE BOOTSTRAPPING GCLOUD ON A TRUSTED MACHINE WITHOUT A 
    WEB BROWSER AND THE ABOVE COMMAND WAS THE OUTPUT OF `gcloud auth login --no-browser` 
    FROM THE TRUSTED MACHINE.

    Proceed (y/N)?  y

    Your browser has been opened to visit:

        https://accounts.google.com/o/oauth2/auth?[...]

    Copy the following line back to the gcloud CLI waiting to continue the login flow. 
    WARNING: THE FOLLOWING LINE ENABLES ACCESS TO YOUR GCP RESOURCES. ONLY COPY IT TO A 
    MACHINE YOU TRUST AND RAN `gcloud auth login --no-browser` ON EARLIER.

    https://localhost:8085/?state=[...]
    ```

    </div>

    </div>

7.  Copy the link at the bottom of the output, go back into the **WSL** shell, paste it and hit enter. You should see something like:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a6d3f700-ed20-4089-b0f1-f49b6d76093c" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    You are now logged in as [your.user@klara.ch].
    Your current project is [None].  You can change this setting by running:
      $ gcloud config set project PROJECT_ID
    ```

    </div>

    </div>

8.  Select a project: `gcloud config set project klara-nonprod` and configure docker: `gcloud auth configure-docker`. Now grype will be able to pull images from our gcr.io registry.

## Scan docker container image

Sample command

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9f2b7822-928b-4ba8-a124-09dc2cdbba88" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
grype <image>:<version> -o <output_format> --scope all-layers --only-fixed

# Sample command: 
# grype gcr.io/klara-repo/quarkus-adapter:v1.10 -o table --scope all-layers --only-fixed
```

</div>

</div>

- -o (output format):

  - json (more detailed view of vulnerabilities )

  - table (compact view and less detailed of vulnerabilities)

(Reference: <a href="https://github.com/anchore/grype" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/anchore/grype</a> )
