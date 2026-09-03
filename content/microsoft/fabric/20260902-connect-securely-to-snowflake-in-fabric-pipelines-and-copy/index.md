---
categories:
- MS
- Fabric
date: '2026-09-02T19:00:00+00:00'
description: Snowflake is a common source and sink for enterprise data movement, and
  Microsoft Fabric Data Factory supports it in both pipelines and Copy Jobs. For organizat
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Connect-securely-to-Snowflake-in-Fabric-Pipelines-and-Copy-Jobs/ba-p/5363159
source: Microsoft Fabric Blog
tags: []
title: Connect securely to Snowflake in Fabric Pipelines and Copy Jobs
---

Snowflake is a common source and sink for enterprise data movement, and Microsoft Fabric Data Factory supports it in both pipelines and Copy Jobs. For organizations that restrict public network access, some additional considerations are needed to meet that requirement. You can have private inbound and outbound access to Fabric, a private staging storage account, and a private Snowflake instance.
By combining Workspace-level Private Link, a private Azure Storage account, and Snowflake's storage integration with USE_PRIVATELINK_ENDPOINT, you can keep the path entirely private. This post explains how those controls fit together for Fabric Data Factory pipelines and Copy Jobs.
When this might apply
Your organization might require private networking, where every hop needs to be inspectable and privately routed. Regulated workloads in financial services, healthcare, and public sector regularly require that analytics platforms have no public endpoints, that inbound and outbound traffic to storage stays inside a VNet, and that any third-party SaaS integration uses private links where supported. Snowflake supports Azure Private Link for storage endpoints and Fabric supports Workspace-level Private Link for inbound workspace access which can be combined to achieve a private path. Using each control for its intended direction avoids public exposure.
The challenge
The Snowflake connector in Fabric Data Factory uses Snowflake's native COPY command under the hood and requires an interim storage account for data staging. Currently, staging is not supported by Onelake when using Workspace-level Private Link, so an external Azure Storage account can be used instead. Staged copy supports source or destination shapes that aren't natively compatible with Snowflake's COPY command and can improve throughput. The catch is the Azure Storage account sits between two systems that each have their own network story: Fabric on one side and Snowflake on the other. Locking down the copy path end-to-end means securing all three surfaces — the Fabric workspace, Azure Storage staging account, and Snowflake.
The core model
Think of the topology as two independent network paths that meet at the staging account.

Figure: Private Path for Snowflake in Fabric Pipelines and Copy Jobs.


Inbound Access to the Fabric Workspace — Workspace-level Private Link maps the workspace to an approved VNet and lets you deny public inbound access. It doesn't place the Fabric workspace inside your VNet or govern outbound data-source traffic.
Azure Blob Staging — Trusted Workspace Access provides a private connection from the Fabric Workspace to your storage account. Not only is it private, but it uses a Workspace Identity to authenticate and a Resource Instance rule to only allow specific workspaces.
Snowflake to Staging Storage — Snowflake provisions a private endpoint into your Azure tenant against the same staging account. The Snowflake storage integration is created with USE_PRIVATELINK_ENDPOINT = TRUE so Snowflake's own copy operations honor the private path.
Public Internet Access — Public network access on the storage account and Fabric Workspace is blocked.

Network access and authorization are separate. Snowflake's storage integration provisions a service principal in Azure. This SPN will need Storage Blob Data Contributor permission on the staging storage account. The Fabric Azure Blob connection uses the authentication configured for that connection, such as workspace identity. That identity will need Storage Blob Data Reader on the staging storage account.
Setup
The full configuration breaks into five phases:

Build the Network Foundation. Create a workspace, Azure private link service, virtual network, virtual machine, private endpoint from the workspace to the private link service and finally turn off public networking to the workspace. Complete instructions for all these items can be found in the Set up and use Workspace-level Private Links documentation.
Create Staging Storage Account. Use these instructions to create an Azure storage account. Once you have an account you need to configure Trusted Workspace Access for private networking between your Fabric Workspace and the staging storage account.
Configure the Snowflake side. In Snowflake, create a private connectivity endpoint that points at the external staging container and set USE_PRIVATELINK_ENDPOINT = TRUE. Snowflake provisions a service principal in your Azure tenant on your behalf. Grant that service principal the Storage Blob Data Contributor role on the staging account. This lets Snowflake read from and write to staging over the private endpoint using its own identity.
Close the public paths. You should have already denied public inbound access to the Fabric workspace as the last part of the instructions provided in step 1. You also need to disable public network access to the staging account by setting the default public network access rule to disabled.
Create Copy Activity or Copy Job. Create your pipeline with Copy Activity or, even better, create a Copy Job. Then select Snowflake as the source or destination, enable external staging, select the staging storage account, enter the required storage path, choose a destination, and run the item.

Boundaries and things to check
A few constraints are worth calling out before you plan a rollout:

Workspace-level Private Link prerequisites. Confirm that your Fabric capacity SKU and region support Workspace-level Private Link, and that your tenant admin has enabled the required tenant settings.
Staging is Azure Blob. External staging for Snowflake in the Copy Activity uses an Azure Blob container. You don't need to enable hierarchical namespace on the account for staging to function.
Snowflake USE_PRIVATELINK_ENDPOINT requires a supported Snowflake account. Snowflake-managed outbound private connectivity endpoints require Business Critical edition or higher. Confirm that the feature is enabled for your Snowflake account before provisioning the endpoint.
Private Endpoint Approval. The private endpoint from Snowflake into your storage account will be in a pending state and must be approved on the storage account.

Next steps
If you're evaluating Snowflake as a source or sink in Fabric today, deploy this topology end-to-end in a non-production workspace before you promote it. The moving parts are simple individually, but each one must be in place for the private path to hold.
Learn more from the following documentation:

Set up and use Workspace-level Private Links in Microsoft Fabric
Configure Snowflake in a copy activity in Fabric Data Factory
Use workspace identity in Fabric Data Factory
Configure trusted workspace access in Microsoft Fabric
Snowflake: Configuring an Azure container for loading data
Snowflake: Managing Azure Private Link endpoints

&nbsp;

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Connect-securely-to-Snowflake-in-Fabric-Pipelines-and-Copy-Jobs/ba-p/5363159](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Connect-securely-to-Snowflake-in-Fabric-Pipelines-and-Copy-Jobs/ba-p/5363159)*
