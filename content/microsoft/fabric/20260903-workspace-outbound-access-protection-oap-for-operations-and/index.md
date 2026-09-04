---
categories:
- MS
- Fabric
date: '2026-09-03T20:00:00+00:00'
description: 'Co-authors: Andre Terceros and Bodhisatva Gautam

  Workspace Outbound Access Protection (OAP) in Microsoft Fabric helps WS admins secure
  outbound connections from'
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Workspace-Outbound-Access-Protection-OAP-for-Operations-Agent/ba-p/5363817
source: Microsoft Fabric Blog
tags: []
title: Workspace Outbound Access Protection (OAP) for Operations Agent and Fabric
  Maps (Preview)
---

Co-authors: Andre Terceros and Bodhisatva Gautam
Workspace Outbound Access Protection (OAP) in Microsoft Fabric helps WS admins secure outbound connections from workspace items to external resources. Administrators can control outbound access by blocking unwanted connections by default and allowing only approved connections through configured rules.&nbsp;
As organizations adopt AI-powered operations at scale, governance and security remain at critical requirements. With this preview release, Microsoft Fabric introduces Outbound Access Protection (OAP) for Operations Agent and Fabric Maps, enabling workspace administrators to control the outbound actions an agent can perform.&nbsp;
OAP for Operations Agent
When OAP is enabled, Operations Agent continues to perform core functions including reasoning, recommendation generation, rule evaluation, and telemetry collection. However, outbound actions are governed by the workspace's configured access policies. Administrators gain greater visibility through in-product notifications, Teams messaging experiences, and the Operations Agent Activity Log, making it easier to identify and troubleshoot blocked actions.

Figure: Outbound Access Protection helps workspace administrators govern outbound Operations Agent actions while providing clear visibility when actions are blocked.

What's new with Operations Agent and OAP?
With Outbound Access Protection support for Operations Agent, workspace administrators gain more control over how agent-initiated actions interact with external services and resources. The following updates improve governance, visibility, and operational oversight while helping organizations continue to automate with confidence.

Govern outbound agent actions through workspace-level OAP policies.
Control whether Operations Agent can send Teams notifications based on allowed connections.
Prevent unauthorized cross-workspace actions when OAP policies restrict outbound access.
Receive clear visibility when actions are blocked through in-product notifications and Teams messaging experiences.
Monitor agent activity and OAP-related outcomes through the Operations Agent Activity Log.


Figure: Activity Log Operations Details show the steps the agent completed and each action’s status, including when an action is blocked.

OAP for Maps
Fabric Maps can connect to a variety of data sources, including Lakehouse across Fabric workspaces, Kusto databases (KQL), Ontologies as well as external geospatial services such as Web Map Services (WMS), Web Map Tile Services (WMTS), and Web Feature Services (WFS).
Workspace Outbound Access Protection helps organizations maintain security and compliance by governing outbound connections and requiring explicit approval before a map can access external resources.
With OAP enabled, Fabric Maps follows a "default deny" model, ensuring only approved destinations can be accessed.
How it Works
When Workspace Outbound Access Protection is enabled on a workspace, Fabric Maps evaluates outbound connectivity at multiple stages:
Save and Load Operations
Whenever a map is created, updated, or loaded, Fabric evaluates all referenced data sources against the workspace policy.
If a data source isn't permitted:

References are blocked
Disallowed sources are redacted when the map is opened
Attempts to save unsupported references are rejected

Runtime Data Access
Map visuals continuously retrieve data such as:

Map tiles
Geospatial features
Query results

Before each outbound request is executed, Fabric validates the destination against the workspace's outbound access protection policy.
External Service Validation
External geospatial services such as WMS, WMTS, and WFS are validated through Data Movement and Transformation Services (DMTS). DMTS acts as a policy enforcement layer and ensures only explicitly approved external endpoints can be reached.
Supported Connection Scenarios
Lakehouse
Lakehouse connectivity receives the most flexibility under the current release.

Lakehouse within the same workspace are always allowed.
Lakehouse in other workspaces can be allowed using Data Connection Rules.
Administrators maintain granular control over cross-workspace access.

External Geospatial Services
Organizations can securely connect to approved WMS, WMTS, and WFS services.
By using Data Connection Rules with the Geospatial Web Services connection type, administrators can create an allow list of approved endpoints.
This enables scenarios such as:

Accessing enterprise GIS systems
Consuming approved mapping services
Integrating with external geospatial data providers

Kusto Databases and Ontologies
Connections to Kusto databases and Ontologies within the same workspace continue to function normally.
Cross-workspace connectivity for these sources is currently blocked when OAP is enabled. Future releases will introduce additional policy controls for these connection types.
Configuring Fabric Maps with Workspace OAP
Enabling protection is straightforward:

Enable Workspace Outbound Access Protection for the workspace.
Create Data Connection Rules that define approved destinations.
Add approved external geospatial services using the Fabric connection experience.
Configure Geospatial Web Services rules to authorize required endpoints.

Once configured, Fabric Maps can access only the approved destinations specified by policy.
Security by Design
Workspace OAP for Fabric Maps follows several key security principles:

Explicit Allow Lists: Only approved destinations can be reached. All other outbound connectivity is automatically blocked.
Consistent Enforcement: Policies are applied during authoring, loading, and runtime execution, ensuring continuous protection throughout the lifecycle of a map.
Fail-Closed Protection: If policy validation services become unavailable, Fabric denies cross-workspace connections by default. This fail-closed behavior helps prevent unintended data exposure during service disruptions.

What’s next?
We are actively working to expand OAP support for additional experiences and plan to add support for Power BI Semantic Models and Reports soon in GA soon.
Your feedback is essential! Let us know how we can make Fabric even more secure and flexible for your workloads by sharing your feedback at Fabric Ideas – Microsoft Fabric Community
Learn more

Workspace outbound access protection overview
Outbound Access Protection for Operations Agent (Preview)
Workspace Outbound Access Protection for Fabric Maps

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Workspace-Outbound-Access-Protection-OAP-for-Operations-Agent/ba-p/5363817](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Workspace-Outbound-Access-Protection-OAP-for-Operations-Agent/ba-p/5363817)*
