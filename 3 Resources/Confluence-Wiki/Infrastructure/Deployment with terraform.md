---
title: "Deployment with terraform"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47973925376/Deployment+with+terraform
space: "FUT"
topic: infra
relevance: 0.832
depth: 3
updated: 2024-09-20
attachments: 1
tags:
  - confluence
  - infra
  - space/fut
---

# Deployment with terraform

> [!info] Imported from Confluence
> Space **FUT** · updated 2024-09-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47973925376/Deployment+with+terraform)
> Relevance 0.832 · topic `infra`

# Introducti<span class="inline-comment-marker" ref="2004f99d-2d63-4934-8008-b5d2206ef563">on to terr</span>aform

<span class="inline-comment-marker" ref="a2160d9e-4b01-41cb-a4a2-199fc984714d">Terraform is an infrastructure</span> as code tool that lets you build, change, and version infrastructure safely and efficiently. This includes low-level components like compute instances, storage, and networking; and high-level components like DNS entries and SaaS features.

In Klara/ePost, normally, a module is typically deploy on Kubernetes, which requires a yaml file, placed in the luz_kubernetes repository.

For the common service architecture, we are dealing with Cloud Run, which is a resource of Google cloud, and not running on the Google kubernetes engine.  
Traditionally, to create these Google cloud resources (e.g. PubSub topics/subscriptions, GCS buckets,…), we write shell scripts containing gcloud commands. But that can get complicated since you need to check whether the resources are already created or not, if already created then skip the creation.

Terraform can help us replace those scripts to create Google cloud resources.

## Resource

This represents an individual infrastructure component, such as a virtual machine, network interface, or storage bucket. Terraform provides resources for a wide range of cloud providers and services.

e.g.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6c02963f-a40a-4308-9740-bffe41b4702e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
resource "google_compute_subnetwork" "cloudrun_subnet" {
  project       = var.project_id
  region        = var.location
  name          = "subnet-${var.location}-cloudrun"
  network       = data.google_compute_network.vpc_network.id
  ip_cidr_range = local.ip_range
}
```

</div>

</div>

## Variable

Used to store values that can be referenced within your Terraform configuration. This allows for customization and flexibility.

e.g.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7f338af2-4dac-4cf8-8c8d-95ce9abb0e43" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#Project ID
variable "project_id" {
  description = "The project id"
  type        = string
}

variable "location" {
  description = "The location to deploy to"
  type        = string
}
```

</div>

</div>

## Module

A reusable collection of resources and variables that can be used to create complex infrastructure components. It promotes code reusability and modularity.

e.g.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b67f77b9-3804-4188-8735-4dfbdbbb3962" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// cloudsql/main.tf
resource "google_sql_database" "common_service" {
...
}

resource "google_sql_user" "postgres_user" {
...
}

// module use resources in ./cloudsql
module "common-service-cloudsql" {
  source                     = "./cloudsql"
  project_id                 = var.project_id
  location                   = lookup(local.zones, local.location, "${local.location}-a") # Kubernetes cluster zone
  region                     = local.location
  vpc_network                = module.network.cloudrun_vpc_network_id
  instance_name              = local.cloudsql_instance_name
  db_name_secret_version     = module.common-service-secret.secret_versions["db-name"]
  db_username_secret_version = module.common-service-secret.secret_versions["db-username"]
  db_password_secret_version = module.common-service-secret.secret_versions["db-password"]
}
```

</div>

</div>

## Output

Provides a way to export values from your Terraform configuration for use by other systems or scripts. This can be useful for sharing information about the provisioned infrastructure.

e.g.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c60599a0-618b-4728-9ee4-405d8885f938" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
output "private_ip_address" {
    value       = google_sql_database_instance.common_service.private_ip_address
    description = "The private IP address of the common service CloudSQL instance"
}
output "internal_dns_name" {
    value       = google_dns_record_set.db-instance.name
    description = "The private internal DNS name of the common service CloudSQL instance"
}
```

</div>

</div>

# Terraform in luz_kubernetes

In the project luz_kubernetes the Terrform scripts are in the root direct named `terraform` with the structure as below


![[47973925376-image-20240813-034220.png]]



# How to deploy a cloud run service using terraform

Refer to session Deployment of `README` in the generated project.

References

<a href="https://developer.hashicorp.com/terraform?product_intent=terraform" class="external-link" data-card-appearance="inline" rel="nofollow">https://developer.hashicorp.com/terraform?product_intent=terraform</a>
