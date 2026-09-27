---
ai_hash: 198bc30ac1e6c433
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 8
depth: 2.43
entities: []
relevance: 0.762
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3555243283/Axonivycloud+-+Volumes+architecture+for+a+kubernetes+AWS+EKS
space: AII
status: reference
tags:
- confluence
- infra
- space/aii
title: Axonivycloud - Volumes architecture for a kubernetes AWS EKS
topic: infra
type: source
updated: 2019-10-02
---

# Axonivycloud - Volumes architecture for a kubernetes AWS EKS

> [!info] Imported from Confluence
> Space **AII** · updated 2019-10-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3555243283/Axonivycloud+-+Volumes+architecture+for+a+kubernetes+AWS+EKS)
> Relevance 0.762 · topic `infra`

The efs-provisioner allows you to mount EFS storage as PersistentVolumes in kubernetes. It consists of a container that has access to an AWS <a href="https://aws.amazon.com/efs/" class="external-link" rel="nofollow">EFS</a> resource. The container reads a configmap which contains the EFS filesystem ID, the AWS region and the name you want to use for your efs-provisioner. This name will be used later when you create a storage class.

## Create EFS file system

In aws, create EFS, select 2 private subnet

Once EFS created, it will automatically create security group for this service, modify it and make sure it allow worker nodes to access.

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr>
<th style="text-align: left;"><p>Type</p></th>
<th style="text-align: left;"><p>Protocol</p></th>
<th style="text-align: left;"><p>Port Range</p></th>
<th style="text-align: left;"><p>Source</p></th>
<th style="text-align: left;"><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr>
<td><p>All traffic</p></td>
<td><p>All</p></td>
<td><p>All</p></td>
<td><p>sg-07e16287f799442f6 (default)</p></td>
<td><br />
</td>
</tr>
<tr>
<td><p>NFS</p></td>
<td><p>TCP</p></td>
<td><p>2049</p></td>
<td><p>sg-08089b8f41a72ab19 (axonivycloud-eks-node-NodeSecurityGroup-14P2FY4MVI56P)</p></td>
<td><br />
</td>
</tr>
</tbody><tfoot>
<tr>
<td colspan="5">sg-fra-axonivycloud-efs</td>
</tr>
</tfoot>
&#10;</table>

</div>

Set tag for EFS service and security group

# Create efs-provisioner

In kubectl client, run yaml files, these file I already configure, you only need to change in step 2,3.

1.  kubectl create -f ns.yaml [[3555243283-ns.yaml|ns.yaml]], create k8s namespace for efs provisioner such as efs-storage-class
2.  kubectl create -f configmap.yaml [[3555243283-configmap.yaml|configmap.yaml]], create efs configmap, modify this file base on EFS information which created above step
3.  kubectl create -f deployment.yaml [[3555243283-deployment.yaml|deployment.yaml]], create service account, efs provisioner pod, edit file and change volumes.name.nfs.server with efs dns name such as <a href="http://fs-5dcd5604.efs.eu-central-1.amazonaws.com" class="external-link" rel="nofollow">fs-5dcd5604.efs.eu-central-1.amazonaws.com</a>,
4.  kubectl create -f rbac.yaml  [[3555243283-rbac.yaml|rbac.yaml]], authorize the provisioner
5.  kubectl create -f class.yaml [[3555243283-class.yaml|class.yaml]], create storage class
6.  kubectl create -f claim.yaml[[3555243283-claim.yaml|claim.yaml]],  create pvc claim for tesing

# Create gp2-eu-central-1c

Efs storageclass cannot limit pv storage size, so we created a gp2-eu-central-1c storage class, each customer will using 1 EBS disk which located in AZ eu-central-1c.

EKS worker nodes also located in eu-central-1c to allow ebs can mount volume to nodes.

Ingress storage size by manual edit EBS disk or edit a pvc

Run kubect create -f [[3555243283-sc-zone.yaml|sc-zone.yaml]] to create storage class.

Detail issue describle here : <a href="https://github.com/kubernetes/kubernetes/issues/64373" class="external-link" rel="nofollow" style="letter-spacing: 0.0px;">https://github.com/kubernetes/kubernetes/issues/64373</a>

**Notes**: Cause EBS disk created in AZ eu-central-1c, so  make sure change Autoscaling group to create worker notes in this AZ also.


![[3555243283-image2019-10-2_16-6-45.png]]



If not, you may got this error when create the pod which requited persistent storage: 1 node(s) had volume node affinity conflict.

### More detail:

<a href="https://github.com/kubernetes-incubator/external-storage/tree/master/aws/efs" class="external-link" rel="nofollow">https://github.com/kubernetes-incubator/external-storage/tree/master/aws/efs</a>

<a href="https://medium.com/devopslinks/aws-eks-volumes-architecture-in-a-statefull-app-in-multiple-azs-6ca1b05f80eb" class="external-link" rel="nofollow">https://medium.com/devopslinks/aws-eks-volumes-architecture-in-a-statefull-app-in-multiple-azs-6ca1b05f80eb</a>

%% ai-graph-start %%

**Related notes:**
- [[AxonivyCloud - Infrastructure Diagram EKS Proposal]]
- [[Axonivycloud - Monitoring EKS cluster using Prometheus and Grafana]]
- [[Axonivycloud - Deploy Nginx Ingress for EKS]]
- [[Jenkins in cluster container]]
- [[Infrastructure]]

%% ai-graph-end %%