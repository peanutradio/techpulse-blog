---
categories:
- MS
- Fabric
date: '2026-09-03T19:50:43+00:00'
description: 'Understand Connection Recency

  Connections are shared infrastructure in Microsoft Fabric. Pipelines, dataflows,
  semantic models, and other Fabric items use them '
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Manage-Fabric-connections-at-scale-with-connection-recency-in/ba-p/5363147
source: Microsoft Fabric Blog
tags: []
title: Manage Fabric connections at scale with connection recency in Fabric REST APIs
---

Understand Connection Recency
Connections are shared infrastructure in Microsoft Fabric. Pipelines, dataflows, semantic models, and other Fabric items use them to reach data sources without storing connection details and credentials in every item. Connection Recency adds the context administrators need to understand how each connection is being used. If you go to “Manage Connections and Gateways” in Fabric settings and open the settings of any connection, the following information card will populate:
Figure: Connection Recency shows when a connection was created, last linked to an item, and last used to access credentials.
The card provides three insights:&nbsp;

Created on shows when the connection was created.&nbsp;
Last Bound to items shows when an item was most recently associated with the connection. By any user reflects activity across the tenant, and&nbsp;By me is specific to the viewer.&nbsp;
Last credentials used shows when a workload most recently used the connection credentials. It also separates activity&nbsp;By any user from activity&nbsp;By me, which is the current user.&nbsp;

Linking and usage are different events. A connection can be linked but never used, or it could have been linked a long time ago to an item that still runs every day. Credential-use information can also be up to 30 minutes behind real time, so use it for governance and investigation rather than immediate usage monitoring.&nbsp;
Learn more in the Data source management documentation.&nbsp;
Why enterprise connection management becomes difficult&nbsp;
Connections can grow exponentially across a large organization. Every team, project, environment, gateway, and data source can introduce more connections. Over time, names become inconsistent, employees move on, and multiple connections can point to the same endpoint. The Fabric interface is useful for inspecting one connection, but reviewing recency and ownership one connection at a time does not scale to an enterprise inventory.&nbsp;
This creates three common problems:&nbsp;

Stale connections: A connection may no longer support an active workload, but its name, creation date, and Last Bound Date do not prove that it is unused. Administrators need both Last Bound Date and Last Credential Use signals before deciding what to review or retire.&nbsp;
Duplicate connections: Different display names can hide equivalent connection definitions. Duplicates increase credential maintenance, complicate troubleshooting, and make it harder to establish a preferred connection which dilutes the advantage of connection sharing and reuse across a tenant.&nbsp;
Ownership risk: A connection with only one person as an owner can become orphaned when that person leaves the organization. The connection might then be unavailable for administration or no longer usable by dependent workloads.&nbsp;

The REST APIs make it possible to evaluate these risks across every connection the caller can access. In the case of cloud connections, you can only see connections where you are the owner, but the admin for a gateway can see all connections on the gateway. This provides better scalability than the UI.&nbsp;
Using the APIs to manage connections&nbsp;
You can use the APIs to identify stale and duplicate connections for removal and to help mitigate the issue of single user ownership. Gateways in Fabric have a limit of 1,000 connections, so managing and governing your connections will help you stay under that limit.&nbsp;&nbsp;
Now that we understand each of the three scenarios, let’s see what the API can provide to help us handle each scenario:&nbsp;

Stale connections: 

LastBoundDateTime is the first of two signals we can use. If this is NULL, then it is currently not being used by a Fabric item and would be a candidate for removal. The one catch is that connections bound prior to recency being introduced will also show NULL, so we must filter out anything with a CreatedDateTime earlier than the introduction of the recency feature. Recency came out in preview at the end of March 2026, so my example code will filter anything created prior to May 1, 2026, to play it safe.&nbsp;
The second signal we have is the LastCredentialUsedDateTime, which indicates the last time the connection was used by any item. You can change this in the example code, but I’m going to use 90 days. You might have connections that legitimately get used less frequently, such as year-end runs or only during the holiday season, so keep that in mind when reviewing and adjust accordingly.&nbsp;


Duplicate connections:

It is possible for two connections to have identical configurations except for the DisplayName. This defeats the purpose of having shareable connections, so we’ll need to use the&nbsp;LastCredentialUsedDateTime to determine which one was most recently used and flag any others to be considered for removal or consolidation. We’ll use ConnectionType, ConnectionPath, ConnectivityType, and GatewayID to identify duplicates. There are other columns you may want to add, such as CredentialType or ConnectionEncryption, depending on what you consider duplicate in your specific environment.&nbsp;


Ownership risk:

For ownership continuity, you want to identify connections whose only&nbsp;Owner role assignment is a User, then add another approved owner. This helps avoid having an orphaned connection if an owner gets deleted. Adding a Microsoft Entra group is preferred because group membership can be maintained as people join, leave, or change responsibilities. A second individual owner is better than a single owner, but it does not provide the same durable operating model as group ownership.&nbsp;




MYTH: A common misconception is that a connection fails when its owner account is deleted. In reality, what matters is the credential being used for authentication. When you create a connection that uses OAuth, you become the owner and your OAuth credential is typically used. If your account is later deleted, the connection fails because it can no longer authenticate with that credential, not because ownership changed. If the connection instead uses an SPN and is shared with another user, it will continue to work.&nbsp;

Explore the List Connections, List Connection Role Assignments, and Add Connection Role Assignment documentation for the complete request, pagination, identity, and permission details.
Next steps&nbsp;
Run the companion notebook first. The notebook is designed as a Python notebook to be run in the Fabric Python runtime and not the PySpark Python runtime. There will be a section for each of the previous scenarios. Stale and duplicate connections will be shown so you can review before deciding what to remove, but that part will be up to you. You can use the data frame and pass the connection ID values to the Delete Connection API. If you don’t know how to write that part, then it’s a great opportunity to use Copilot.&nbsp;&nbsp;
The ownership section will show you all connections with a single owner where you will want to add another owner. As a best practice, we’ll assume you are adding a group, but that part will be commented out as you very likely do not want to blatantly add the same group to every connection across the board. You can use the Add Connection Role Assignment API to make those additions as you need.&nbsp;
Connection recency data gives you the signals you need to manage connections at scale, but it should be the starting point for review rather than the basis for automatic removal. Use the companion notebook to identify potentially stale or duplicate connections and connections with a single owner, then apply your organization’s requirements before taking action. Regularly reviewing these signals can help you reduce unnecessary connections, improve ownership continuity, and establish a more manageable connection inventory.&nbsp;

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Manage-Fabric-connections-at-scale-with-connection-recency-in/ba-p/5363147](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Manage-Fabric-connections-at-scale-with-connection-recency-in/ba-p/5363147)*
