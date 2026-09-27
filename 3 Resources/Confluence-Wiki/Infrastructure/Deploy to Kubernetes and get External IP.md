---
title: "Deploy to Kubernetes and get External IP"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675014564/Deploy+to+Kubernetes+and+get+External+IP
space: "LUZCOMP"
topic: infra
relevance: 0.741
depth: 2.66
updated: 2016-05-18
attachments: 0
tags:
  - confluence
  - infra
  - space/luzcomp
---

# Deploy to Kubernetes and get External IP

> [!info] Imported from Confluence
> Space **LUZCOMP** · updated 2016-05-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675014564/Deploy+to+Kubernetes+and+get+External+IP)
> Relevance 0.741 · topic `infra`

1.  Install Kubernetes  
    1.  If you are using virtual docker machine, you have to login to this machine first by command

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="07bcaab4-21b6-47d2-9971-77d034e9016c" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        docker-machine ssh default
        ```

        </div>

        </div>

    2.  Download kubectl tool from official website by command:

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9b56b844-48df-494f-a1ec-1614f7b59567" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        wget https://storage.googleapis.com/kubernetes-release/release/v1.2.4/bin/linux/amd64/kubectl
        ```

        </div>

        </div>

    3.  Change permission for this file

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="58e63f6e-b677-4899-be45-a0794ac94505" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        chmod +x kubectl
        ```

        </div>

        </div>

    4.  Copy it to {{/usr/local/bin/kubectl}} by command:

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7311e86b-9b3f-4c38-8f32-78620e6695e2" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        sudo cp kubectl /usr/local/bin/kubectl
        ```

        </div>

        </div>

    5.  Finall you can get benefit from kubectl tool by checking this command:

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3d8f7732-74ca-4668-adc1-7daa73d77159" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        kubectl
        ```

        </div>

        </div>

2.  Create yaml files for setup deployment to Kubernetes cluster (for example: I would like to setup postgre database server on Kubenetes cluster)
    1.  Create postgres-database-server-controller.yaml

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5748613c-7e91-4aa8-ab91-366a2e17607d" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **postgres-database-server-controller.yaml**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        apiVersion: v1
        kind: ReplicationController
        metadata:
          name: postgres-database-server-rc
          labels:
            name: postgres-database-server-rc-lbl
            env : test-integration
        spec:
          replicas: 1
          selector:
            name: postgres-database-server-rc
          template:
            metadata:
              labels:
                name: postgres-database-server-rc
            spec:
              containers:
              - name: postgres-database-server
                image: gcr.io/luzcomp-integration/postgres-database-server:latest
                env:
                - name: GET_HOSTS_FROM
                  value: dns
                ports:
                - containerPort: 5432
        ```

        </div>

        </div>

    2.  Create postgres-database-server-service.yaml

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4a6f5cd5-d403-4134-bbb8-bf987249f5bb" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **postgres-database-server-service.yaml**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        apiVersion: v1
        kind: Service
        metadata:
          name: postgres-server-service
          labels:
            name: postgres-server-service
        spec:
          type: LoadBalancer
          ports:
            - port: 5432
              targetPort: 5432
              protocol: TCP
          selector:
            name: postgres-database-server-rc
        ```

        </div>

        </div>

3.  Create Dockerfile with the content below:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bb1428c7-5f2b-412c-bc48-9b4072a1985d" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

    **Dockerfile**

    </div>

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    #
    # example Dockerfile for https://docs.docker.com/examples/postgresql_service/
    #

    FROM ubuntu
    MAINTAINER SvenDowideit@docker.com

    # Add the PostgreSQL PGP key to verify their Debian packages.
    # It should be the same key as https://www.postgresql.org/media/keys/ACCC4CF8.asc
    RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8

    # Add PostgreSQL's repository. It contains the most recent stable release
    #     of PostgreSQL, ``9.3``.
    RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list

    # Install ``python-software-properties``, ``software-properties-common`` and PostgreSQL 9.3
    #  There are some warnings (in red) that show up during the build. You can hide
    #  them by prefixing each apt-get statement with DEBIAN_FRONTEND=noninteractive
    RUN apt-get update && apt-get install -y python-software-properties software-properties-common postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3

    # Note: The official Debian and Ubuntu images automatically ``apt-get clean``
    # after each ``apt-get``

    # Run the rest of the commands as the ``postgres`` user created by the ``postgres-9.3`` package when it was ``apt-get installed``
    USER postgres

    # Create a PostgreSQL role named ``docker`` with ``docker`` as the password and
    # then create a database `docker` owned by the ``docker`` role.
    # Note: here we use ``&&\`` to run commands one after the other - the ``\``
    #       allows the RUN command to span multiple lines.
    RUN    /etc/init.d/postgresql start &&\
        psql --command "CREATE USER admin WITH SUPERUSER PASSWORD 'admin';" &&\
        createdb -O admin luz_person && createdb -O admin luz_compensation

    # Adjust PostgreSQL configuration so that remote connections to the
    # database are possible.
    RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.3/main/pg_hba.conf

    # And add ``listen_addresses`` to ``/etc/postgresql/9.3/main/postgresql.conf``
    RUN echo "listen_addresses='*'" >> /etc/postgresql/9.3/main/postgresql.conf

    # Expose the PostgreSQL port
    EXPOSE 5432

    # Add VOLUMEs to allow backup of config, logs and databases
    VOLUME  ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]

    # Set the default command to run when starting the container
    CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]
    ```

    </div>

    </div>

4.  Get credentials to log in google cloud, currently we use an json file GoogleServiceAccount.json like below:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1b5a5cd6-9a98-4880-bdb9-3de9c547a749" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

    **GoogleServiceAccount.json**

    </div>

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    {
      "type": "service_account",
      "project_id": "luzcomp-integration",
      "private_key_id": "<REDACTED>",
      "private_key": "<REDACTED — rotate this key>",
      "client_email": "smurfs-deploy@luzcomp-integration.iam.gserviceaccount.com",
      "client_id": "101428299019982679319",
      "auth_uri": "https://accounts.google.com/o/oauth2/auth",
      "token_uri": "https://accounts.google.com/o/oauth2/token",
      "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
      "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/smurfs-deploy%40luzcomp-integration.iam.gserviceaccount.com"
    }
    ```

    </div>

    </div>

5.  Create job in Jenkins Execute Shell as below:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b9e5a8aa-87a2-4a31-b50d-2371b2b1dcbd" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    docker -H tcp://192.168.73.128:4444 build -t gcr.io/luzcomp-integration/postgres-database-server:latest -f Dockerfile .
    docker -H tcp://192.168.73.128:4444 login -e smurfs-deploy@luzcomp-integration.iam.gserviceaccount.com -u _json_key -p "$(cat GoogleServiceAccount.json)" https://gcr.io
    docker -H tcp://192.168.73.128:4444 push gcr.io/luzcomp-integration/postgres-database-server:latest

    export KUBERNETES_SERVER=https://104.155.225.209
    export KUBERNETES_USER=admin
    export KUBERNETES_PASS=GTC7rGj114IbFwC3

    kubectl -s $KUBERNETES_SERVER --insecure-skip-tls-verify --username $KUBERNETES_USER --password $KUBERNETES_PASS stop rc -l "name in (postgres-database-server-rc-lbl)"
    kubectl -s $KUBERNETES_SERVER --insecure-skip-tls-verify --username $KUBERNETES_USER --password $KUBERNETES_PASS delete service -l "name in (postgres-server-service)"

    kubectl -s $KUBERNETES_SERVER --insecure-skip-tls-verify --username $KUBERNETES_USER --password $KUBERNETES_PASS create -f ./postgres-database-server-controller.yaml
    sleep 15
    kubectl -s $KUBERNETES_SERVER --insecure-skip-tls-verify --username $KUBERNETES_USER --password $KUBERNETES_PASS create -f ./postgres-database-server-service.yaml
    ```

    </div>

    </div>

6.  Get external IP by command below

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e54a6029-7f17-4ccf-9de9-9ac37799b8dc" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    kubectl -s https://104.155.225.209 --insecure-skip-tls-verify --username admin --password GTC7rGj114IbFwC3 get services
    ```

    </div>

    </div>

    <span class="legacy-color-text-red2">Remember it takes more than 1 minute to get external IP successfully.</span>
