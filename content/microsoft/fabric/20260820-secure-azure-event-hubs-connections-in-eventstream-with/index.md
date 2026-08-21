---
categories:
- MS
- Fabric
date: '2026-08-20T17:35:21+00:00'
description: Connecting streaming workloads to Azure Event Hubs should not require
  teams to manage shared credentials. Organizations building real-time data solutions
  need a
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Secure-Azure-Event-Hubs-Connections-in-Eventstream-with/ba-p/5359945
source: Microsoft Fabric Blog
tags: []
title: Secure Azure Event Hubs Connections in Eventstream with Workspace Identity
  (Preview)
---

Connecting streaming workloads to Azure Event Hubs should not require teams to manage shared credentials. Organizations building real-time data solutions need a secure and manageable way to connect streaming workloads to event sources. With workspace identity support for Azure Event Hubs sources in Eventstream, now in preview, customers can use the identity associated with their Fabric workspace to authenticate.
Workspace identity support for Azure Event Hubs sources gives Eventstream an identity-based alternative to shared access keys. Eventstream authenticates using the identity associated with the Fabric workspace, while administrators manage access through Azure role-based access control (Azure RBAC). This removes the need to store a connection string containing a shared access key in the Eventstream configuration and aligns the connection with how teams manage access to other Azure resources.
Why identity-based authentication matters
Azure Event Hubs has traditionally supported shared access key authentication, which requires customers to provide credentials as part of the connection configuration. While this approach is widely used, it also means teams need to manage and maintain those credentials as part of their Eventstream deployments. Many organizations are increasingly adopting identity-based authentication across Azure services to reduce reliance on shared secrets and centralize access management through Microsoft Entra ID and Azure RBAC.
Workspace identity brings this model to Azure Event Hubs sources in Eventstream. Instead of storing shared access keys in the Eventstream configuration, customers can grant access directly to the Fabric workspace identity and use it to authenticate to Azure Event Hubs. This simplifies connection management and provides a more streamlined way to configure Azure Event Hubs sources in Eventstream.
How workspace identity helps
Workspace identity is an automatically managed service associated with a Fabric workspace. It gives the workspace its own identity for accessing supported Azure resources. Fabric manages the underlying credentials and their lifecycle, so customers do not need to create, store, or rotate those credentials themselves.
For Azure Event Hubs sources, an administrator grants the workspace identity access through Azure role-based access control by assigning the Azure Event Hubs Data Receiver role. When the source starts, Eventstream uses the workspace identity to obtain a Microsoft Entra ID token and connect to Azure Event Hubs. No shared access key or connection string needs to be stored in the Eventstream configuration.
Because the connection uses the identity of the Fabric workspace, access can be managed separately for each workspace. Administrators can grant, update, or remove permissions through the Event Hubs namespace’s Access control (IAM) settings without changing credentials used by other applications or services. This keeps authorization with the Azure resource and provides administrators with direct control over which Fabric workspaces can read from the event hub.
This approach also simplifies how teams create and manage Eventstream connections. Developers can configure an Azure Event Hubs source without obtaining or handling a shared access key, while administrators continue to manage access through Azure RBAC. When access requirements change, permissions can be updated for the workspace identity without modifying the Eventstream connection configuration.
For customers, this means:

No shared keys to store. Sensitive credentials do not need to be copied into the connection configuration.
No manual credential rotation. Fabric manages the credentials associated with the workspace identity.
Access managed through Azure RBAC.&nbsp;Administrators assign the required Event Hubs permissions directly to the workspace identity.
Identity-based access. The connection uses a specific workspace identity instead of a shared credential used by multiple applications.
Automatic credential lifecycle management. Fabric maintains the underlying identity credentials.

Workspace identity can be particularly valuable for organizations that already use Microsoft Entra ID and Azure RBAC as part of their security strategy. Instead of introducing another credential that must be managed separately, teams can extend existing identity and access management processes to Eventstream connections. This creates a more consistent approach to securing access across Azure and Fabric resources.
Real-life examples
Financial Services
Consider a payment processor that streams credit card authorization events from Azure Event Hubs into Microsoft Fabric. Its fraud operations team uses Eventstream to route declined transactions, unusual spending patterns, and other risk signals for real-time analysis. These events can help analysts identify suspicious activity and investigate potential fraud while transactions are still being processed.
Because this pipeline handles sensitive payment activity, the company needs precise control over which production workloads can access the event stream. With workspace identity, the Fabric workspace running the fraud-monitoring solution can be assigned the Azure Event Hubs Data Receiver role through Azure RBAC. The Eventstream connection does not require a stored shared access key, and administrators can manage access specifically for the workspace processing the transaction events.
This workspace-level access is also useful when the company maintains separate development, testing, and production environments. Each workspace can use its own identity and receive only the access required for that environment, helping the organization manage its fraud-monitoring pipelines without distributing the same credential across multiple teams or solutions.
Healthcare Operations
Consider a hospital that streams admissions, transfers, discharges, and bed-status updates from Azure Event Hubs into Microsoft Fabric. Operations teams use these events to track available beds, coordinate patient movement, and identify capacity constraints across emergency, inpatient, and discharge units. For example, an unexpected increase in emergency admissions or a delay in patient discharges can quickly affect bed availability across the hospital.
The Eventstream solution can route these operational events for real-time monitoring, giving hospital teams a current view of patient flow and capacity. This helps bed-management teams coordinate transfers between departments, identify units approaching capacity, and allocate beds and operational resources where they are needed.
With workspace identity, the Fabric workspace running the patient-flow solution can be granted access to Azure Event Hubs through Azure RBAC. The hospital can keep access tied to the specific workspace processing these events without storing a shared access key in the Eventstream connection. If separate workspaces are used for testing and production, administrators can manage access for each workspace independently through the Event Hubs namespace.
Set up workspace identity authentication
The setup has three main steps:
1. Enable workspace identity. In the Fabric workspace settings, enable Workspace identity for the workspace.
2. Grant access to Azure Event Hubs. Assign the Azure Event Hubs Data Receiver role to the workspace identity.
3. Configure the Eventstream source. Add an Azure Event Hubs source, choose Extended features, and select Workspace identity as the authentication method.
Figure: Workspace identity selected as the authentication method for an Azure Event Hubs source.
After these steps, Eventstream can connect to Azure Event Hubs using the workspace identity. No shared access key or connection string is required. For the complete procedure, explore Add an Azure Event Hubs source to an eventstream.&nbsp;
What this means for customers
The primary benefit of workspace identity is that customers no longer need to manage shared access keys in their Eventstream connection configuration for Azure Event Hubs sources.
Instead of obtaining a connection string that contains a shared access key and storing it as part of the source configuration, administrators can grant the Fabric workspace identity access to Azure Event Hubs through Azure RBAC. Eventstream then uses that workspace identity to authenticate and access the event source.
This simplifies connection setup and ongoing management by removing the need to handle shared credentials within Eventstream. As organizations deploy additional Eventstream solutions and onboard new Azure Event Hubs sources, teams can configure connections using the workspace identity without distributing or maintaining shared access keys across environments.
By shifting authentication from shared keys to the workspace identity, customers can manage access through Azure RBAC while reducing the operational overhead associated with credential management in Eventstream.
Get started
Workspace identity authentication for Azure Event Hubs sources is available in preview through Eventstream extended features. To try it, enable workspace identity in your Fabric workspace, assign the required Event Hubs role, and select workspace identity when you create the source connection. For detailed setup instructions, review the Azure Event Hubs source configuration guide to set up the connection and begin streaming events into Eventstream.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Secure-Azure-Event-Hubs-Connections-in-Eventstream-with/ba-p/5359945](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Secure-Azure-Event-Hubs-Connections-in-Eventstream-with/ba-p/5359945)*
