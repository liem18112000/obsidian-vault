---
title: "AxonivyCloud - Infrastructure Diagram EKS Proposal"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3555237110/AxonivyCloud+-+Infrastructure+Diagram+EKS+Proposal
space: "AII"
topic: architecture
relevance: 0.711
depth: 2.44
updated: 2020-07-31
attachments: 4
tags:
  - confluence
  - architecture
  - space/aii
---

# AxonivyCloud - Infrastructure Diagram EKS Proposal

> [!info] Imported from Confluence
> Space **AII** · updated 2020-07-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3555237110/AxonivyCloud+-+Infrastructure+Diagram+EKS+Proposal)
> Relevance 0.711 · topic `architecture`

![[3555237110-axonivycloud-eks.png]]



  

## **Description**

The whole configuration is protected in an own VPC with 4 subnet: 2 public subnet and 2 private subnet.

Only bastion host and NAT instance have public ip to access from outsite. From bastion host, we will SSH to servers in private subnet.

All instances in private subnet will go to internet via NAT instance

EKS worker node join in EKS cluster, so ivy container and db container will be run in this nodes. Depend on how many containers, EKS cluster will auto add/remove nodes

Ivy container will be expose via Nginx.

Ansible to run automation task such as create kubenertes deployment, services, config nginx, add sub-domain....

## **How Does it work**

Each customer will have ivy container + postgresql container which created in EKS cluster

Nginx ingress inside EKS will reverse proxy to ivy container base on pod ip address. We cannot use a ipaddress of node, because ivy pod can be run in any node, so nginx ingress will not know the correct node ipaddress which ivy pod are running.

The application load balancer (ALB) will route traffic from outsite to Nginx ingress. Same with ivy container, nginx ingress is container as well.

  
**IAM Role need to created**

### EKS Service Role

Before you can create an Amazon EKS cluster, you must create an IAM role that Kubernetes can assume to create AWS resources. For example, when a load balancer is created, Kubernetes assumes the role to create an Elastic Load Balancing load balancer in your account. This only needs to be done one time and can be used for multiple EKS clusters.

1.  Open the IAM console at <a href="https://console.aws.amazon.com/iam/" class="external-link" rel="nofollow">https://console.aws.amazon.com/iam/</a>.

2.  Choose **Roles**, then **Create role**.

3.  Choose **EKS** from the list of services, then **Allows Amazon EKS to manage your clusters on your behalf** for your use case, then **Next: Permissions**.

4.  Choose **Next: Tags**.

5.  (Optional) Add metadata to the role by attaching tags as key–value pairs. For more information about using tags in IAM, see <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html" class="external-link" rel="nofollow">Tagging IAM Entities</a> in the *IAM User Guide*.

6.  Choose **Next: Review**.

7.  For **Role name**, enter a unique name for your role, such as `eksServiceRole`, then choose **Create role**.

**IAM**

The AWS credentials must be associated with a user having at least the following AWS managed IAM policies

- AmazonEKSClusterPolicy

In addition, you will need to create the following managed policies

*EKS*

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "eks:*"
                ],
                "Resource": "*"
            }
        ]
    }


![[3555237110-image2019-2-26_18-10-32.png]]



**EKS_Node**

Create IAM roles name: EKS_Node, then attach policy <span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name">AmazonEKSWorkerNodePolicy , <span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name">AmazonEKSWorkerNodePolicy, <span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name">AmazonEKS_CNI_Policy , take node Role ARN, Instance profile ARNs</span></span></span></span></span></span>

<span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name"><span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name"><span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name">Create the IAM policy to give the Ingress controller the right permissions. More detail: <a href="https://aws.amazon.com/blogs/opensource/kubernetes-ingress-aws-alb-ingress-controller/" class="external-link" rel="nofollow">https://aws.amazon.com/blogs/opensource/kubernetes-ingress-aws-alb-ingress-controller/</a></span></span></span></span></span></span>

  

1.  Go to the IAM Console and choose the section Policies.
2.  Select Create policy.
3.  Embed the contents of the template [[3555237110-iam-policy.json|iam-policy.json]] in the JSON section. 
4.  Review policy and save as “ingressController-iam-policy”

  

Attach the IAM policy to the EKS_Node IAM

<span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name"><span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name"><span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name">  
</span></span></span></span></span></span>

<span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name"><span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name"><span class="awsui-tooltip awsui-tooltip-no-slide awsui-tooltip-right awsui-tooltip-rounded awsui-tooltip-size-auto"><span class="ng-scope system-policy-name">  
</span></span></span></span></span></span>

**  **
