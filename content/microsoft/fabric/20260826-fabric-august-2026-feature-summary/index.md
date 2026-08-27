---
categories:
- MS
- Fabric
date: '2026-08-26T16:06:14+00:00'
description: 'Welcome to the August 2026 Fabric update!

  Microsoft Fabric continues to evolve with new capabilities that help organizations
  build, manage, and scale their data'
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-August-2026-Feature-Summary/ba-p/5325824
source: Microsoft Fabric Blog
tags: []
title: Fabric August 2026 Feature Summary
---

Welcome to the August 2026 Fabric update!
Microsoft Fabric continues to evolve with new capabilities that help organizations build, manage, and scale their data and AI solutions more efficiently. This month's updates introduce enhancements across Fabric Platform, OneLake, Data Engineering, Conversational Analytics, Data Warehouse, Real-Time Intelligence, and Data Factory.&nbsp;
Whether you're strengthening governance, improving performance, streamlining development workflows, or expanding AI-powered experiences, these updates are designed to help you get more value from your data while simplifying day-to-day operations. Explore the following highlights to see what's new in Microsoft Fabric this month.

Table of Contents

Events and Announcements

Join us for FABCON and SQLCON in Barcelona, September 28 – October 1, 2026


Fabric Platform

KQL-Dashboard Embed in Fabric (Preview)
Git Integration – Workspace Relation API (Preview)
Git Integration and Deployment Pipeline – Item level permission restriction


OneLake

Resource instance rules for OneLake (Generally Available)


Data Engineering

Native Execution Engine performance improvements
Fabric Runtime 2.0 (Generally Available)
Enhanced Spark Properties Support in Notebook and Spark Job Definition


Conversational Analytics

Enhanced Data Agent Visualizations with Fabric Visuals
Advanced DAX Generation for Semantic Models in Data Agents
Data Agent orchestrator upgraded to GPT 5.1
Example Query Usability Improvements in Data Agent
Add Schema Descriptions for SQL Sources in Data Agent
Data Agent is migrating from Assistants API to Responses API
Fabric data agents now return up to 1000 rows
Fabric Data Agents in Microsoft Copilot Studio (Generally Available)
Fabric data agents in Microsoft Foundry: Easier to connect, easier to trust
Add co-publishers for data agents in Microsoft 365 Copilot


Data Warehouse

Identity columns with identity insert (Generally Available)
CI/CD 2.0 with DacFx (Preview)
Simplify Fabric Warehouse deployments with Schema Compare in VS Code
GPU Query Acceleration (Preview)
Metadata Sync supports Delta Checkpoint V2 (Generally Available)
Secure data ingestion with COPY INTO and Workspace Identity (Generally Available)
SQL Audit Logs: More Signal, Less Noise with Predicate Filtering (Generally Available)
OneLake security improvements for SQL analytics endpoints (Generally Available)


Real-Time Intelligence

Set Alerts Directly from Anomaly Detector (Generally Available)
Anomaly detector supports Eventhouse shortcut tables
Operations Agent Activity Log
Eventstream MQTT connector (Generally Available)
Reference data enrichment in Eventstream (Preview)
Eventstream observability in Workspace Monitoring re-enabled with per-Eventstream control (Preview)
Eventstream UI editor improvements (Preview)
Secure Azure Event Hubs Connections in Eventstream with Workspace Identity (Preview)
Schema Registry and Event SchemaSet Region Availability
Data-driven styling and UX improvements for Maps (Generally Available)
Workspace Outbound Access Protection (OAP) for Operations Agent (Preview)
Configure and manage Activator rules directly in Eventstream (Generally Available)


Data Factory

Introducing Hierarchical Navigation in Monitoring Hub for Fabric Pipelines
Explore the modern Fabric Pipeline canvas (Preview)
Upgrade Dataflow Gen1 to Dataflow Gen2 (CI/CD) using the Upgrade Wizard (Preview)
Extended watermark support in Copy job
Enable Change data feed during Lakehouse table creation in Copy job
Amazon Redshift as new source in Copy job
Copy job supports timestamps without time zone in Lakehouse
Migration Assistant for SQL database in Fabric (Generally Available)





Events and Announcements
Join us for FABCON and SQLCON in Barcelona, September 28 – October 1, 2026
Explore what’s possible with Microsoft Fabric and get up to speed on the latest in SQL, analytics, and AI.
From 130 sessions and 4 keynotes to workshops, the expo, community spaces, and the Power BI DataViz World Championships, this is where the data community comes together. Learn directly from Microsoft and community experts shaping the future of Fabric and SQL.

Register now and save €200 with code FABCMTY200.

Fabric Platform
KQL-Dashboard Embed in Fabric (Preview)
A new capability that lets you add interactive KQL-Dashboard content directly into your own browser-based web applications is available. Now, you can bring Fabric analytics into the apps, portals, and workflows your users already use.
Figure: A Real-Time Dashboard embedded in a web application.
Analytics are most useful when they are available where decisions happen. With Fabric Embed, you can place Fabric content inside a custom application or internal portal instead of requiring users to switch to the Fabric portal.
Fabric Embed can help you:

Bring interactive analytics into an existing business workflow.
Explore Fabric analytics without switching between your application and the Fabric portal.
Keep Fabric workspace permissions and Microsoft Entra ID identities at the center of access control.
Build user-based embedded experiences for people who already have access to the underlying Fabric item.

The embedded experience complements the Fabric portal. Content owners can continue to create and manage analytics in Fabric while application developers present that content in the context most useful to their users.
To learn more, refer to the Microsoft Fabric Embed documentation.
Git Integration – Workspace Relation API (Preview)
Following our recent announcement of branch workspaces and the relationship that’s automatically created when a user performs a branch-out operation, we’re introducing the new Workspace Relations API. These endpoints let you create, query, update, and delete relationships between Fabric workspaces programmatically — and for anyone building automation around Git integration, branched workspaces, and CI/CD, it’s a big deal!
The workflow is straightforward. A developer creates a feature branch. An ADO pipeline or GitHub Action then provisions a new feature workspace, configures Git, applies the right settings and permissions, and synchronizes the workspace. Finally, the Workspace Relations API links that feature workspace back to its parent — closing the loop and giving your automation a first-class, queryable connection between parent and branched workspaces.

Figure: Branch workspace relation.

To learn more, refer to Development process using Branch-Out experience.
Git Integration and Deployment Pipeline – Item level permission restriction
Starting December 1, 2026, users without read-write permissions on workspace items can't use Git integration and won’t be able to deploy a workspace or assign workspace to a stage via Deployment Pipeline. This restriction can result in loss of access to certain items because of sensitivity labels and protection policies applied to those items.
Figure: Git integration - item level permission warning.
To learn more, refer to Information Protection in Microsoft Fabric.
OneLake
Resource instance rules for OneLake (Generally Available)
Resource instance rules for OneLake are ready for production workloads across enterprise analytics environments. They give workspace admins a precise way to allow access from trusted Azure resource instances while continuing to enforce network and data-level protections.
Use resource instance rules when you need to enable secure service-to-service access to OneLake without relying solely on IP allowlists or requiring private networking for every integration. Combined with Private Link, IP firewall rules, and identity-based permissions, they help organizations apply layered security based on the needs of each workspace and workload.
Resource instance rules support a broad set of Azure services that can present a verifiable Azure resource identity, including Azure Databricks, Azure SQL Server, Azure Data Factory, Azure Event Grid, Azure Machine Learning, and more. Configuration requires only the Azure resource ID, simplifying setup while maintaining control over which Azure resources can access OneLake.
Figure: Configure resource instance rules to allow approved Azure resource instances to access data in OneLake.
To learn more, refer to Manage&nbsp;inbound access to OneLake with Resource Instance Rules.
Data Engineering
Native Execution Engine performance improvements
This month, we continued to improve the Native Execution Engine (NEE) with a new set of query execution optimizations designed to accelerate Spark workloads while reducing compute consumption. Key enhancements include broadcast joining reuse across queries, native acceleration for ranking window functions such as RANK and DENSE_RANK, and automatic materialization of reused Common Table Expressions (CTEs).
Together, these optimizations eliminate redundant computation, keep more processing within the engine's vectorized execution path, and improve performance for common data engineering patterns including large joins, analytical reporting, ranking workloads, and complex transformation pipelines. Because these improvements are enabled automatically when NEE is available, customers benefit from faster execution times, lower Fabric capacity consumption, and improved price-performance without requiring code changes to existing notebooks, Spark Job Definitions, or pipelines.
These optimizations are enabled by default once the Native Execution Engine is turned on, allowing customers to realize performance gains immediately without additional configuration or tuning.
Customers can enable NEE at the workspace environment level by navigating to Environment &gt; Acceleration and turning on Native Execution Engine, ensuring it is available for all Spark sessions using that environment. It can also be enabled at the session level by setting spark.native.enabled=true in Spark configuration.
With NEE enabled, customers can seamlessly take advantage of the latest runtime innovations to process data faster, improve resource efficiency, and maximize the value of their Fabric capacity investments.
To learn more about Native Execution Engine explore our documentation Native execution engine for Fabric Data Engineering.
Fabric Runtime 2.0 (Generally Available)
As the execution foundation for Microsoft Fabric's Data Engineering and Data Science experiences, Runtime 2.0 delivers a modern, high-performance platform built on Apache Spark and deeply integrated across the Fabric ecosystem.
Purpose-built for large-scale data processing and analytics workloads, Runtime 2.0 represents a major advancement in performance, reliability, security, and future readiness. Built on the latest open-source innovations, it enables customers to accelerate data processing, simplify operations, and take advantage of the newest capabilities across Microsoft Fabric.
This release includes significant platform upgrades, including Apache Spark 4.1, Delta Lake 4.2, Python 3.13, Java 21, Scala 2.13, and Azure Linux 3.0, providing a modern and enterprise-ready foundation for the next generation of data engineering, data science, and analytics workloads.
These enhancements enable customers to take advantage of the latest open-source innovations while continuing to benefit from a fully managed, enterprise-grade experience in Microsoft Fabric. Whether you're building data pipelines, developing AI and machine learning solutions, processing streaming workloads, or powering enterprise analytics, Runtime 2.0 provides a more capable, scalable, and performant platform for your workloads.
Figure: Change Runtime at the workspace settings level.
With improved performance, updated open-source foundations, and continued investment in capabilities such as the Native Execution Engine, Runtime 2.0 provides a modern platform for data engineering, data science, and analytics workloads in Microsoft Fabric.
Explore the full documentation and start using Runtime 2.0 in production Runtime 2.0 in Fabric.
Enhanced Spark Properties Support in Notebook and Spark Job Definition
Notebook and Spark Job Definition (SJD) activities now enable users to specify Spark properties directly within the data integration pipeline. This enhancement allows Spark properties to be set inside the activity panel, ensuring that the values provided are used for activity execution. If an Environment item is linked to the Notebook or SJD and both the activity panel and Environment item define the same property, the value specified in the activity panel will take precedence and overwrite the Environment value. In the case of Notebooks, if the %%configure command is used within the notebook code to set a Spark property, the value set using %%configure will be applied for execution.
This update offers users maximum flexibility, allowing Spark properties to be defined at different layers based on their specific use cases. By supporting property configuration in the activity panel, Environment item, and notebook code, users can tailor property values to meet the unique requirements of each execution of Notebook/SJD.
Figure: Spark Property in Notebook activity.
To learn more on Transform data by running notebook and Transform data by running a Spark Job Definition activity.
Conversational Analytics
Enhanced Data Agent Visualizations with Fabric Visuals
The data agent now uses Fabric visuals to render the charts it returns, bringing higher-quality, more consistent visualizations into your conversations with your data. When you ask a question like "Generate a bar chart of revenue by region" or "Show me my top 10 customers by sales," the data agent responds with an interactive, polished visual alongside its text and table answers, so you can spot trends, comparisons, and outliers. Because the data agent now shares the same visual foundation as Fabric Apps, charts look and behave consistently with AI-generated visuals in Fabric, with refinements to formatting, legends, tooltips, and axis scaling. Supported chart types include line, bar, stacked bar, pie, scatter, and area charts.
Figure: Animated GIF - The data agent uses Fabric visuals to return interactive charts alongside its responses, making it easier to spot trends and compare performance across categories.
To learn more, refer to the&nbsp;Get visual responses from a Fabric data agent documentation.
Advanced DAX Generation for Semantic Models in Data Agents
Advanced DAX generation for Power BI semantic models is now available in Fabric data agents when you use the Preview runtime. Instead of generating a DAX query in a single pass, the new system works iteratively as a specialized sub-agent that can use tools, inspect results, and refine its approach across multiple steps, providing significant improvements in response accuracy. It also uses instance value indexing to resolve values from the semantic model before generating a query, resulting in more accurate and reliable filters.
This update is built on the same semantic-model query engine used across Fabric Skills, Power BI, and M365 Copilot, providing more consistent answers across Microsoft experiences. To use the new experience, open the Runtime dropdown in the data agent ribbon and switch from Standard to Preview. More improvements for semantic models in data agents are coming soon, including data source description and instructions, granular schema selection, and example queries.
Figure: Animated GIF - In the Preview runtime, the data agent uses advanced DAX generation to answer a question over a connected semantic model, enabling more accurate DAX generation and responses.
To learn more, refer to the Semantic model best practices for data agent documentation.
Data Agent orchestrator upgraded to GPT 5.1
The data agent orchestrator now runs on GPT-5.1, across both the standard and preview runtimes. The orchestrator handles how questions are rephrased, how work is planned across your data sources, and how the final answer is composed — so this upgrade changes behavior in all three. Most of what you see should be an improvement in answer quality and planning, but the change is not behavior-neutral: prompts tuned against the previous model may produce different results. We recommend re-running your evaluations, reviewing the results against your saved baselines, and updating your agent instructions and prompts where the new behavior doesn't match what your scripts expect.
To learn more, refer to the data agent runtimes documentation.
Example Query Usability Improvements in Data Agent
We've made several usability improvements, for example queries. Errors now surface inline, directly alongside the query, so you can identify and correct issues without leaving the editor. The editor also resizes automatically based on the length of your query, removing the need to adjust the pane manually as you write.
Figure: Author and validate Data Agent example queries directly in Fabric, with inline SQL editing, testing, and error diagnostics for faster iteration.
To learn more, refer to the&nbsp;example queries in data agent documentation.
Add Schema Descriptions for SQL Sources in Data Agent
Users can now provide tailored schema descriptions through the new schema description editor, available for SQL sources on the Preview runtime. Schema descriptions improve query generation and accuracy by giving the agent more context about what everything in your data means — use them to resolve ambiguous columns, or to give a table or field a more precise meaning than its name conveys. Instead of inferring intent from column names alone, the agent works from what your data team documented, so it selects the right tables and interprets fields the way you intended.
Figure: Add rich business context to your data by authoring schema descriptions that help Data Agents better understand tables, columns, and business semantics.
To learn more, refer to the&nbsp;schema descriptions documentation.
Data Agent is migrating from Assistants API to Responses API
The OpenAI Assistants API that powers the orchestration layer for the Microsoft Fabric data agent, is currently scheduled to be shut down by&nbsp;OpenAI&nbsp;on&nbsp;August 26, 2026. After that date, direct calls to the Assistants API will stop working. If you connect to a Fabric data agent programmatically through the Assistants API, you need to migrate to the data agent Model Context Protocol (MCP) endpoint. SDK and Fabric portal users require little or no action because Microsoft will migrate those experiences internally, although conversation history may reset once. Existing agent data sources, instructions, and tools remain unchanged.
To learn more, refer to Prepare your Fabric Data Agent integrations for Assistants API retirement.
Fabric data agents now return up to 1000 rows
One of the most common requests we hear is about row limits. Until now, a Fabric data agent returned at most 25 rows with its answer, which is fine for a quick number but not enough when you want to see the whole result. Customers told us they want the full response so they can review it offline or run their own analysis on top of it. That limit is now lifted in the preview runtime. Switch your agent to the preview runtime and the response comes back as a summary followed by a tabular preview of up to 1000 rows.
This is the first step, and more is coming. Next, we plan to let you download the full response as an Excel file, with sensitivity labels carried through to the file. We also want this experience to be consistent across all integrations and consumption channels, so you get the same behavior no matter where you use your data agent. These improvements are landing over the next few months, so stay tuned.
Fabric Data Agents in Microsoft Copilot Studio (Generally Available)
Now, you can bring governed business data from Microsoft Fabric into Copilot Studio agents, so those agents can answer questions and support business processes using trusted enterprise data. Since preview, the integration has moved to the new tool-based experience: select Add a tool, search for Fabric, and add Fabric IQ Data MCP, and your agent can call the Fabric data agent like any other tool. The Fabric data agent still runs in Fabric and respects permissions on the underlying data sources. You can also publish your agent to Microsoft Teams and Microsoft 365 Copilot, so business users get data-grounded answers where they already work.
Figure: Animated GIF -Add Fabric data agent as a tool to your Copilot Studio agent.
To learn more, refer to the Fabric Data Agent MCS GitHub documentation for setup steps and join the community discussion to share feedback.
Fabric data agents in Microsoft Foundry: Easier to connect, easier to trust
Fabric data agents in Microsoft Foundry are now easier to connect and easier to monitor. The integration moves to Model Context Protocol, so your Fabric data agents appear as tools that Foundry agents can invoke when they need enterprise data in OneLake. Connecting them no longer means hunting for workspace and artifact IDs. You add the Fabric IQ (OneLake Catalog) tool, filter for data agents, and pick the ones you want by name. You can also connect more than one Fabric data agent to a single Foundry agent, so an agent can draw on a sales agent, a supply chain agent, and a customer support agent and choose the right one for the question.
On the operations side, you can now view logs and traces for Fabric data agents through Foundry Observability. Traces show which tools were invoked, how long each step took, and what came back, which makes it much easier to troubleshoot an answer that looks wrong or a workflow that runs slow. This is the visibility teams need to move agents from experiments to production. The update is rolling out to all regions over the coming days.
Figure: Animated GIF - Add Fabric data agent as part of Fabric IQ to your Foundry agent.
To learn more, refer to the Observability for Fabric data agents in Microsoft Foundry documentation.
Add co-publishers for data agents in Microsoft 365 Copilot
When you publish a Fabric data agent to Microsoft 365 Copilot, the Microsoft 365 agent platform registers the agent and records you as its only owner. That created a problem for teams. Your co-creators could still edit the data agent in Fabric, but when they tried to republish it, the operation failed, because Microsoft 365 only lets registered owners publish. Fabric access and Microsoft 365 ownership are two separate lists, so giving someone edit rights in Fabric was never enough.
Now, you can avoid this with co-publishers. After you publish the data agent, open Settings, go to the Publishing pane, and add your Fabric co-creators under Microsoft 365 Copilot co-publishers. Each person you add is registered as a co-owner on the Microsoft 365 agent platform, so anyone on that list can republish the agent. Add co-publishers right after your first publish so no one hits a failure in the meantime.
Figure: Publishing pane in the Fabric data agent settings, showing where you add Microsoft 365 co-publishers.
To learn more, refer to the Consume a data agent from Microsoft 365 Copilot (preview) documentation.
Data Warehouse
Identity columns with identity insert (Generally Available)
Since preview, thousands of customers have adopted IDENTITY to auto-generate surrogate keys and streamline migrations from SQL Server, Azure SQL Database, and Azure Synapse.
Now we're introducing support for IDENTITY_INSERT and reseed operations - two highly needed additions - so you can insert explicit key values, migrate data in bulk with COPY INTO, and safely realign identity ranges with DBCC CHECKIDENT.

Figure: Using identity insert on Fabric Data Warehouse.

IDENTITY columns are available now in every Fabric Data Warehouse. To learn more, check our updated tutorial and documentation.
CI/CD 2.0 with DacFx (Preview)
Microsoft Fabric Data Warehouse is introducing a major update to the DacFx engine that powers schema comparison, Git integration, and deployment pipelines. DacFx builds a declarative model of your warehouse and determines the schema changes required to move safely between development, test, and production environments.
Figure: Git-integrated CI/CD workflow for Microsoft Fabric, showing feature workspace synchronization, branch merging, and deployment pipeline promotion across development, test, and production workspaces.
With this update, Git integration uses DacFx-based incremental extraction to produce cleaner, more focused commits. Deployment pipelines also use the updated model to generate more accurate comparisons and&nbsp;smarter deployment plans, with settings tuned for schema evolution.
Figure: Infrastructure-as-code workflow for Microsoft Fabric, showing how shared workspace configuration creates feature workspaces that branch from main, synchronize through Git, and return through pull requests and merges
The new warehouse&nbsp;item definition version 2.0 updates the SQL project SDK, moves shared queries into a .sharedqueries folder, adds project-level Git configuration, and re-extracts object definitions to support constraints, identity columns, clustering, and consistent formatting. These changes make future commits easier to review and reduce noisy diffs. For more information, refer to the Upgrade Fabric Data Warehouse System File Version in a Git Integrated Fabric workspace documentation.
Figure: Microsoft Fabric source control notification prompting users to apply the latest Warehouse system update, with a warning that the update will introduce differences between the workspace and its connected Git repository.
The update also improves comparison accuracy. Git-connected workspaces can adopt the update when ready through the&nbsp;System update available experience, giving teams control over upgrade timing. Review and commit the generated changes before continuing normal development and deployment workflows.
Figure: Microsoft Fabric Git integration displaying Warehouse system-update differences for review before committing the updated item definition and metadata to source control.
To learn more, refer to the&nbsp;Development and Deployment Overview documentation.
Simplify Fabric Warehouse deployments with Schema Compare in VS Code
Database deployments should not feel like a guessing game. With Schema Compare in Visual Studio Code, developers can see exactly what changed before those changes reach a Fabric Warehouse—bringing clarity and control to every release.
Compare a Fabric Warehouse with another warehouse or a SQL database project, then review differences across tables, views, stored procedures, functions, and other database objects in a clear, object-by-object view. Choose the changes you want, update the project from the warehouse, or deploy selected changes to the target—without manually assembling and reviewing every deployment script.
By keeping database projects synchronized in Git, teams gain a reliable source of truth and can bring schema changes into familiar pull-request and CI/CD workflows. The result is a safer, more intentional path from development to production, with fewer surprises at deployment time.
Before applying changes, review the generated script for unsupported operations and potential data loss.
Figure: Schema Compare identifying differences between source and target warehouses, with selectable actions to add or update warehouse objects and generate or apply the synchronization script.
To learn more, refer to the Develop warehouse projects in Visual Studio Code and Schema Compare in the MSSQL extension documentation.
GPU Query Acceleration (Preview)
Query Acceleration brings GPU-powered performance directly to Fabric Data Warehouse, enabling eligible analytical queries to run faster without query rewrites, special syntax, or additional systems to manage.
Figure: Turn on Query Acceleration in Workspace Settings to enable for all SQL analytics endpoints and warehouses in the workspace. Simply run a query, and eligible queries will be accelerated.
Query Acceleration in Fabric Data Warehouse uses GPUs to accelerate the most compute-intensive portions of analytical queries, helping overcome the limits of CPU-only execution. It works transparently with existing T-SQL, Direct Query reports, applications, and tools, automatically offloading eligible operations such as scans, filters, joins, and aggregations to GPUs while the CPU continues to manage the rest of the execution pipeline.
Customers can use Query Insights, Data Warehouse Monitoring, and SQL Server Management Studio (SSMS) for query execution plans to identify accelerated queries and understand how Query Acceleration is applied during query execution.
Designed for analytical and high-concurrency workloads, Query Acceleration can improve throughput, reduce query latency, and deliver more consistent performance for dashboards and interactive analytics. Acceleration is applied selectively, enabling performance gains even when only part of a query is eligible for GPU execution.
The capability is built with reliability in mind. Unsupported operations or runtime constraints can seamlessly fall back to CPU execution without affecting query correctness. Performance improvements depend on workload characteristics, but Microsoft benchmarks have demonstrated gains of up to 7× across reporting, application, and AI-driven analytics scenarios.
Query Acceleration builds on Microsoft's Tensor Query Processor research, described in CoddSpeed: Hardware Accelerated Query Processing in Microsoft Fabric, which was selected as the SIGMOD Companion 2026 Best Industry Paper.
To sign up for the Preview, please fill out the form.
Metadata Sync supports Delta Checkpoint V2 (Generally Available)
Metadata Sync (MD Sync) now supports Delta Checkpoint V2, enabling synchronization of modern Delta tables across both MD Sync (Legacy) and MD Sync (New).
Delta Checkpoint V2 is a Delta Lake enhancement designed to improve scalability for large tables through a more efficient checkpoint structure. Previously, tables using Checkpoint V2 couldn't be synchronized and were reported as unsupported. With this release, MD Sync can discover and synchronize Delta tables that use the Checkpoint V2 format.
This enhancement helps customers:

Synchronize Delta tables that use Checkpoint V2.
Improve interoperability with Spark, Databricks, and other Delta-based platforms.
Support metadata synchronization for large-scale Delta tables more efficiently.
Continue using existing checkpoint formats without any changes.

MD Sync support for Delta Checkpoint V2 is available in both MD Sync (Legacy) and MD Sync (New), helping ensure consistent access to Delta tables across Fabric experiences.
Secure data ingestion with COPY INTO and Workspace Identity (Generally Available)
COPY INTO in Fabric Data Warehouse now supports Workspace Identity, enabling users to load approved data from OneLake or ADLS Gen2 without requiring direct access to the source files.
Previously, ingestion users often needed permissions to both the target warehouse and the source storage location, or teams relied on SAS tokens, account keys, or service principals. With this release, source access can be centrally assigned to the workspace identity, while users retain only the SQL permissions required to load data into the target table.
Key Capabilities:

Load approved data without granting users direct access to raw storage.
Use managed identity-based authentication for OneLake and ADLS Gen2 sources.
Reduce reliance on SAS tokens, shared keys, and service principal secrets.
Maintain separate authorization boundaries for source access and target-table permissions.
Support least-privilege ingestion and separation of duties between storage and warehouse administrators.

Figure: Animated GIF - Using COPY INTO with Workspace Identity.
Workspace Identity support for COPY INTO is generally available in Fabric Data Warehouse, providing a simpler and more governed approach to secure data ingestion.
To learn more, refer to the Ingest Data into Your Warehouse Using the COPY Statement and COPY INTO (Transact-SQL) documentation.
SQL Audit Logs: More Signal, Less Noise with Predicate Filtering (Generally Available)
SQL Audit Logs in Fabric Data Warehouse and SQL Analytics Endpoint now support identity-based predicate exclusion filtering, enabling administrators to reduce repetitive audit events generated by selected users and service principals.
Previously, expected activity from automation identities, scheduled processes, metadata synchronization jobs, and other operational actors could create significant audit noise. With this release, administrators can configure exclusions through the API or SQL Audit Logs user experience, while activity from identities that do not match the exclusion predicate continues to be audited normally.
Figure: Animated GIF - Configuring SQL Audit Logs.
Key Capabilities:

Reduce repetitive audit events from known users and service principals.
Focus investigations on higher-value and unexpected activity.
Lower the storage, processing, export, and query burden associated with low-value events.
Manage identity exclusions through either automated APIs or the user experience.
Apply a governed audit policy aligned with organizational monitoring and compliance requirements.

Identity-based predicate exclusion filtering is generally available for SQL Audit Logs in Fabric Data Warehouse and SQL Analytics Endpoint, providing a cleaner audit stream, less operational overhead, and more focused investigations.
To learn more, refer to the SQL Audit Logs in Fabric Data Warehouse documentation.
OneLake security improvements for SQL analytics endpoints (Generally Available)
OneLake Security for SQL analytics endpoints now includes improvements for nested groups, shortcut-backed tables, column-level security, and service principals, enabling more consistent enforcement of OneLake security policies across enterprise Fabric environments.
Previously, limitations with group expansion, shortcut scenarios, and service principal ownership could make centralized security difficult to apply at scale. With these improvements, customers can define security at the source lakehouse and rely on the SQL analytics endpoint to honor those policies across producer and consumer workspaces.
Key Capabilities:

Manage access through nested Microsoft Entra group hierarchies.
Honor source-side OneLake Security policies for shortcut-backed tables in hub-and-spoke architectures.
Apply column-level security consistently when users receive access through groups.
Use service principals for automated deployments, pipelines, and application-owned data products, including service principal-owned lakehouses.
Define security once in OneLake and reduce the need to duplicate permissions across consumer workspaces and Fabric engines.

These OneLake Security improvements help make security synchronization more practical for enterprise architectures while providing consistent access control across lakehouses and SQL analytics endpoints. Microsoft is also continuing to improve security sync notifications, error handling, and permission propagation across Fabric experiences.
To learn more, refer to the OneLake Security for SQL analytics endpoints documentation.
Real-Time Intelligence
Set Alerts Directly from Anomaly Detector (Generally Available)
Detecting anomalies becomes more valuable if you can act on them. Previously, after publishing an anomaly detector configuration, you had to leave Anomaly Detector and navigate to Real-Time Hub to create an alert. This added extra steps and interrupted your workflow right after completing your configuration.
With this update, you can now create alerts directly from Anomaly Detector. Once you publish a configuration, use the Set alert button in the ribbon to launch the alert creation pane without leaving Anomaly detector. If your configuration hasn't been published yet, you'll be guided through publishing first and then taken directly to the alert setup experience. This helps you move seamlessly from configuring anomaly detection to monitoring it in production.
The integrated experience allows you to monitor your anomalies on each event, helping you get notified as soon as anomalies are detected. If you have more complex business logic, select on each event when to add in additional logic to your conditions. Whether you're monitoring operational metrics, business KPIs, or real-time telemetry, you can now complete the entire workflow in one place and start acting on detected anomalies faster with fewer clicks.
Figure: Create alerts directly from your anomaly detector configuration and continue your workflow without navigating to another experience.
&nbsp;
Figure: Configure notifications for anomaly detector events directly within Anomaly Detector and start monitoring your published configuration immediately.
Anomaly detector supports Eventhouse shortcut tables
Anomaly Detector now supports Eventhouse shortcut tables, making it possible to analyze data without first copying or moving it into a dedicated Eventhouse table. You can create anomaly detectors directly on supported shortcut tables and use the same analysis, model recommendations, and continuous monitoring experiences available for native Eventhouse data sources.
This expands anomaly detection to a broader range of data already connected through Eventhouse shortcuts, helping teams monitor external and federated data sources with less setup and duplication. By enabling anomaly detection directly on shortcut tables, you can move more quickly from connecting data to detecting issues, while continuing to work within a unified Real-Time Intelligence experience.
To learn more, refer to the Anomaly Detection in Real-Time Intelligence documentation.
Operations Agent Activity Log
Understanding what your agent is doing and why is key to building trust and improving outcomes. The activity log is designed to provide that transparency.
It gives you a clear view into the agent’s behavior, including the conditions it evaluated, the recommendations it generated, and how those recommendations were handled. Whether you are validating results, troubleshooting unexpected behavior, or refining your configuration, the activity log helps you better understand how decisions are being made.
You can access the activity log from the Activity log section in the side navigation. It presents a chronological timeline of events with timestamps and relevant context for each entry. Selecting any event allows you to explore additional details and understand what happened at each step.
Figure: The Activity Log provides a chronological view of agent activity, making it easy to track rule evaluations, recommendations, approvals, and outcomes over time.
In the Operation details page, you can view the operation details and status.
Figure: Drill into a specific operation to view the full execution history, including triggered rules, generated insights, user interactions, and actions taken.
To learn more, refer to the Create and Configure Operations Agents documentation.
Eventstream MQTT connector (Generally Available)
It is now easier than ever to ingest real-time data from MQTT brokers directly into Microsoft Fabric Real-Time Intelligence. MQTT is one of the most widely adopted messaging protocols for lightweight, low-bandwidth event driven messaging scenarios. Eventstream MQTT connector simplifies the ingestion of operational and IoT data into Microsoft Fabric, helping organizations turn real-time device events into actionable insights.
Key Benefits:

Connect to any MQTT broker and ingest messages directly into Fabric Eventstream.
Production-ready reliability and support with General Availability readiness.
Enterprise-grade security with support for TLS, mutual TLS (mTLS), and custom certificate authorities managed through Azure Key Vault.
Private network connectivity through Eventstream's streaming connector virtual network capabilities, enabling secure access to brokers hosted in private and on-premises environments.

Figure: Get data from MQTT brokers into fabric using Eventstream MQTT connector.
To learn more, refer to the Add MQTT source to an eventstream documentation.
Reference data enrichment in Eventstream (Preview)
Eventstream now enables you to enrich real-time event streams with contextual business data using Reference Data Join. Simply add a Reference Data node to your Eventstream, select a Delta table from a Fabric Lakehouse, and use it to enrich streaming events with lookup, metadata, or reference information. You can also leverage Lakehouse shortcuts to access Delta tables across OneLake, making it easy to bring contextual data from anywhere in your Fabric environment into your real-time processing pipelines.
Reference Data Join supports both no-code and SQL-based enrichment experiences. Use the built-in Join operator to visually configure INNER and LEFT OUTER joins or use the SQL operator for advanced scenarios. Select only the columns you need from the reference dataset and configure optional refresh intervals to keep slowly changing reference data up to date. This enables Eventstream to continuously use the latest lookup information for real-time enrichment, without requiring additional data movement or downstream processing pipelines.
You can easily add multiple reference data sources to a single Eventstream and combine them with streaming data to create richer, more contextual event pipelines. Developers and data engineers can test and validate join conditions, preview join results, and verify SQL-based enrichment queries before deploying them into production, helping ensure accuracy and confidence in real-time data processing workflows.
Reference Data Join unlocks powerful real-time enrichment scenarios in Eventstream. Users can enrich IoT telemetry with device metadata, correlate operational events with customer and product information, perform lookups against business reference datasets, and add contextual information to streaming data in flight. By bringing reference data and event processing together in a single experience, Eventstream enables customers to transform raw events into actionable business insights in real time.
Figure: Reference data source added and joined with input stream using both built in join operator and SQL operator.
To learn more, refer to the Reference data join in Eventstream using Lakehouse documentation.
Eventstream observability in Workspace Monitoring re-enabled with per-Eventstream control (Preview)
Eventstream observability in Workspace Monitoring is back — now with granular control over which Eventstreams emit monitoring data. A new ‘Log Eventstream activity’ toggle in Eventstream Settings lets you enable or disable observability per Eventstream, so you can balance monitoring coverage with capacity consumption.
When enabled, your Eventstream emits performance metrics, error counts, and health status to three tables in your Workspace Monitoring Eventhouse:

EventStreamMetrics: throughput, backlog, and watermark delay
EventStreamErrorMetrics: deserialization, conversion, and runtime error counts
EventStreamNodeStatus: node health (Running / Failed)

The toggle defaults to OFF for all Eventstreams. To get started, open any Eventstream, go to Settings, and turn on Log Eventstream activity. Your monitoring data will appear in the Workspace Monitoring database within minutes.
Figure: Configuration settings for "Log Eventstream activity" within Monitoring. The panel highlights an active toggle switch, a description explaining that enabling this feature emits performance and error metrics to a monitoring database.
To learn more, refer to the Monitor Eventstream data flows in Workspace Monitoring documentation.
Eventstream UI editor improvements (Preview)
We've redesigned key parts of the Eventstream editor to make building and troubleshooting faster and more intuitive.

Always Publish: No more blocked publish buttons. Publish your work at any stage, the editor gives you clear, contextual guidance on what still needs attention instead of preventing you from moving forward.
Inline error indicators: Errors now appear directly on the node that needs fixing, with actionable guidance on click. No more hunting through a detached error list to find what's broken.
Operator and destination descriptions: Each option now includes an inline description explaining what it does, so you can build confidently without switching to docs.

These changes reduce friction during authoring and make it easier to go from idea to running your pipelines.
Figure: Inline descriptions in the configuration panel for transforming event data.
Secure Azure Event Hubs Connections in Eventstream with Workspace Identity (Preview)
Bringing real time event data into Microsoft Fabric is now simpler and more secure with Azure Event Hubs integration for Eventstream. Organizations can connect Event Hubs directly to Eventstream and start routing event data to destinations such as Eventhouse for analytics and operational insights.
A key capability is Workspace Identity, which removes the need to manage shared access keys. Instead, Eventstream can authenticate to Azure Event Hubs using the Fabric workspace identity. Administrators simply grant the workspace the Azure Event Hubs Data Receiver role, enabling secure access through Microsoft Entra based permissions. This approach improves security, simplifies credential management, and aligns with enterprise governance requirements.
The integration supports both public and private network deployments. For Event Hubs hosted in private networks, organizations can connect through a streaming virtual network gateway while continuing to use Workspace Identity for authentication.
For advanced event processing scenarios, users can enable schema support, associate schemas from the event schema registry, and route structured events to destinations such as Eventhouse. Combined with Workspace Identity, this provides a secure and scalable foundation for building real time data pipelines without the operational overhead of managing secrets or credentials.
Schema Registry and Event SchemaSet Region Availability
Previously, Schema Registry feature and the Event SchemaSet artifact was available for preview in 31 regions.
Expanded to 10 additional regions:

Geography

Region

Americas

Central US

Americas

Mexico Central

Americas

West US 3

Europe

Italy North

Europe

Poland Central

Europe

Spain Central

Europe

West Europe

Asia Pacific

Australia Southeast

Asia Pacific

Israel Central

Asia Pacific

Japan West

If you were previously blocked from trying SchemaSets and schema-based Eventstream data ingestion, you can now do so in these regions.
For more information on Event SchemaSets and how you can create and manage them, visit Schema Registry Overview. To learn more about configuring Eventstreams with schema-based sources, visit Use schemas in Eventstreams.
Data-driven styling and UX improvements for Maps (Generally Available)
Maps become most valuable when they help users understand not just where things are, but what the data means. Now, Data-Driven Styling, along with Markers Rotate by Data, Traffic Flow visualization, additional Map View options, and an improved Layer Settings experience. Together, these enhancements help organizations transform raw geospatial data into intuitive, actionable business insights.
Let Your Data Tell the Story
Understanding patterns hidden within geographic data can be challenging when every feature on a map looks the same. With Data-Driven Styling, Fabric Maps enables map builders to visually represent business data directly on the map, helping viewers quickly identify trends, hotspots, and outliers without inspecting individual records.
The new Color by Value Range capability allows authors to style map layers using numeric measures such as revenue, utilization, sensor readings, environmental measurements, or operational KPIs. Instead of applying a single color to an entire layer, Fabric Maps visualizes value distributions chromatically, making important differences immediately visible.
Organizations can choose between two visualization approaches:

Gradient Styling uses continuous color transitions to reveal magnitude, trends, and geographic variation across a dataset.
Step-Based Styling allows users to define custom value ranges with distinct colors, making it easy to visualize business thresholds, risk levels, performance bands, or service categories.

To make these visualizations easier to interpret, Fabric Maps automatically generates corresponding data legends that explain how colors map to underlying values. This helps viewers understand the meaning behind the visualization and make decisions with greater confidence.
Fabric Maps also includes thoughtfully designed color palettes, including options that support colorblind-friendly visualization scenarios, helping more users accurately interpret map-based insights. Combined with clear, automatically generated legends, these capabilities improve accessibility and make data-driven maps easier to understand across a wider range of audiences.
Figure: Animated GIF - Data-driven styling highlights land-value patterns per street and road and high-value exposure in flood-prone areas.
Visualize Direction and Movement with Markers Rotate by Data
Many operational scenarios involve not only location but also direction. Fabric Maps now supports Markers Rotate by Data, allowing marker symbols to automatically rotate based on values stored in a data column. This capability is available for Marker layers, enabling builders to visualize directional information directly on the map.
Whether visualizing aircraft headings, vehicle movements, equipment orientation, or other operational workflows, map authors can represent direction without requiring custom visualization development. By transforming static points into directional indicators, organizations can add valuable operational context and communicate movement patterns more effectively.
Figure: Bearing-based marker rotation makes real-time flight direction from LAX clear and intuitive at a glance.
Add Real-World Context with Traffic Flow Visualization
Location data alone doesn't always tell the full story. Fabric Maps now supports Traffic Flow overlays, allowing map builders to bring current traffic conditions into their existing map experiences.
By combining business data with real-world traffic information, organizations can gain additional situational awareness for logistics operations, field service planning, transportation monitoring, and operational decision-making. The added context helps users better understand the environment surrounding their assets and activities without leaving the map experience.
Figure: Animated GIF - Real-time traffic flow helps users quickly choose a nearby EV charging station with the clearest route.
Configure the Right Map View for Your Audience
Organizations often create maps for users across different regions and business contexts. Fabric Maps introduces additional Map View settings that allow map builders to configure how geographic information is presented, helping ensure maps are displayed in a way that aligns with organizational needs and audience expectations.
This flexibility gives authors greater control over creating a consistent and intuitive viewing experience across a variety of business scenarios.
A More Discoverable Authoring Experience
UX research and user feedback showed that some key layer settings were difficult to discover. We updated the experience by moving geometry and visualization options—including visual type, Data-Driven Styling, and marker rotation—higher in the configuration pane. We also renamed General to Visibility, making the settings clearer and map authoring more intuitive.
Turn Location Data into Business Insight
Data-Driven Styling, built-in data legends, Markers Rotate by Data, Traffic Flow overlays, enhanced Map View options, and the improved Layer Settings experience help organizations transform location data into meaningful business insight. Together, these capabilities make it easier to uncover patterns, understand operational context, and communicate geospatial insights across teams. Start building with these capabilities today: explore the customization options, apply them to your own geospatial data, and create map experiences that turn location into action.
To learn more, refer to the Customize a map in Microsoft Fabric documentation.
Workspace Outbound Access Protection (OAP) for Operations Agent (Preview)
Workspace Outbound Access Protection (OAP) in Microsoft Fabric helps admins secure outbound connections from workspace items to external resources. Admins can control outbound access by blocking unwanted connections by default and allowing only approved connections through configured rules.
As organizations adopt AI-powered operations at scale, governance and security remain critical requirements. With this preview release, Microsoft Fabric introduces Outbound Access Protection (OAP) for Operations Agent, enabling workspace administrators to control the outbound actions an agent can perform. OAP applies workspace-level policies to actions such as Teams notifications, workflow triggers, and cross-workspace operations, helping organizations enforce security and compliance requirements while continuing to benefit from AI-driven automation.
When OAP is enabled, Operations Agent continues to perform core functions including reasoning, recommendation generation, rule evaluation, and telemetry collection. However, outbound actions are governed by the workspace's configured access policies. Administrators gain greater visibility through in-product notifications, Teams messaging experiences, and the Operations Agent Activity Log, making it easier to identify and troubleshoot blocked actions
Figure: Outbound Access Protection helps workspace administrators govern outbound Operations Agent actions while providing clear visibility when actions are blocked.
What's new with Operations Agent and OAP?&nbsp;

Govern outbound agent actions through workspace-level OAP policies.
Control whether Operations Agent can send Teams notifications based on allowed connections.
Prevent unauthorized cross-workspace actions when OAP policies restrict outbound access.
Receive clear visibility when actions are blocked through in-product notifications and Teams messaging experiences.
Monitor agent activity and OAP-related outcomes through the Operations Agent Activity Log.

Figure: Activity Log Operations Details show the steps the agent completed and each action’s status, including when an action is blocked.
During the preview, some limitations apply. Cross-workspace actions are blocked when OAP is enabled. For example, Power Automate actions are not yet supported when OAP is applied, and only connectors that explicitly support OAP policies can be permitted. By extending Fabric's outbound governance framework to Operations Agent, organizations can adopt AI-powered operational automation with greater confidence while maintaining control over how and where agent-initiated actions are executed.
Resources

Workspace outbound access protection for operations agent (preview)
Workspace Outbound Access Protection (OAP) 
Workspace Outbound Access Protection for Operations Agents

Configure and manage Activator rules directly in Eventstream (Generally Available)
You can now create and manage rules directly in Eventstream. Previously, setting up an alert required switching from Eventstream to Activator. While powerful, this meant moving between experiences to complete a single workflow. Now, alert creation is embedded directly into Eventstream.
Capabilities for building or editing your Eventstream:

Select the stream you want to monitor.
Choose&nbsp;Set Alert.
Define your condition (thresholds, aggregations, patterns).
Configure the action.
Create the rule.Figure: Set alert inside Eventstream.

Capabilities for Activator destination created on Eventstream:

Select Activator node
Select Rule icon
Create the rule

Once you have the rule(s) created on your Eventstream, you can manage them by editing, deleting or opening in Activator.
Figure: Manage rules inside Eventstream.
To learn more, refer to the Add a Fabric activator destination to an eventstream documentation.
Data Factory
Introducing Hierarchical Navigation in Monitoring Hub for Fabric Pipelines
Modern data estates rarely consist of a single job running in isolation. Pipelines trigger notebooks, notebooks invoke other workloads, and business processes span multiple interconnected executions. When troubleshooting a failure or understanding lineage, customers often need visibility into how these executions relate to one another.
Hierarchical Navigation in Monitoring Hub, is a new capability that helps you understand the relationships between runs and quickly navigate across upstream and downstream executions.
Figure: View hierarchical runs via Upstream and Downstream runs in Monitoring Hub.
With Hierarchical Navigation enabled, Monitoring Hub can display:

Upstream runs that initiated a workload
Downstream runs triggered by a workload
Execution relationships across supported Fabric artifacts

This provides a richer observability experience by helping you move beyond individual run monitoring and understand how your workloads operate together.
This enhancement is another step toward a richer observability experience in Fabric, helping customers gain deeper insight into workload execution and dependencies at scale.
To learn more, refer to the Hierarchical Navigation for Pipelines in Monitoring Hub documentation.
Explore the modern Fabric Pipeline canvas (Preview)
The new Fabric Pipeline canvas experience is designed to make pipeline authoring easier than ever.
Key capabilities with modern canvas:

Better visibility when navigating large pipeline graphs
Cleaner, more structured layouts for complex orchestration logic
Improved responsiveness when working with enterprise-scale workflows
A more intuitive experience for pipelines with many activities and branches

Figure: The modern Fabric Pipeline canvas, showing the updated node experience and option to disable the preview if needed.
If you haven't tried it yet, now's the perfect time. The new experience is rolled out automatically and can be disabled at any time.
Whether you're building your first pipeline or managing hundreds of activities across complex workflows, the new canvas is designed to help you stay productive and focused on what matters most.
To learn more, refer to the Modern Pipeline Node Experience documentation.
Upgrade Dataflow Gen1 to Dataflow Gen2 (CI/CD) using the Upgrade Wizard (Preview)
The Dataflows Upgrade Wizard is a guided, end-to-end experience that upgrades your existing Power BI Dataflow Gen1 items to Dataflow Gen2 (CI/CD) in Microsoft Fabric with minimal effort. You can upgrade a single dataflow, or several dataflows from a workspace in a single flow.
Previously, bringing a Gen1 dataflow into Fabric meant recreating it and repointing everything that depended on it. The Upgrade Wizard upgrades in place instead. Each dataflow keeps its ID, name, schedule, and connections, so the reports and semantic models that connect to it keep working without any changes.
Figure: Start the Upgrade Wizard from the menu next to a Dataflow Gen1 in your workspace.
Before anything changes, the wizard assesses every dataflow in the workspace and tells you which ones need attention and why, such as incremental refresh settings to reconfigure or a linked entity to update, so you know what to expect before you upgrade.
Figure: The Upgrade Wizard assesses each Dataflow Gen1 and shows its status before you upgrade.
Why this matters

Upgrade in place, with nothing to rebuild and nothing to repoint.
Upgrade a whole workspace at once instead of one dataflow at a time.
See what needs attention before you upgrade.
Unlock Dataflow Gen2 innovations: improved performance, deeper Fabric integration, CI/CD and Git support, data destinations of your choice, richer diagnostics, Copilot-assisted authoring, and a modern data transformation foundation.

The wizard is available for Dataflow Gen1 items in Premium or Fabric workspaces and requires Fabric to be enabled.
To learn more, refer to the Upgrade Dataflow Gen1 to Dataflow Gen2 (CI/CD) using the Upgrade Wizard documentation.
Extended watermark support in Copy job
Watermark-based incremental load support enables Copy Job to efficiently ingest only new or changed data from key enterprise and SaaS data sources, avoiding costly full reloads. This reduces source-system impact, network traffic, and runtime while improving scalability for production analytics workloads. Customers can now use incremental loading across more of their critical data sources, including Salesforce, Informix, Cassandra, Greenplum, Presto, and Databricks, by leveraging Copy job’s built-in watermark mechanism and without building custom ingestion logic.
To learn more, refer to the Incremental copy in Copy job documentation.
Enable Change data feed during Lakehouse table creation in Copy job
Copy job can now create Lakehouse tables with Change Data Feed (CDF) enabled automatically. There’s no longer a need to pre-create destination tables or manually configure Delta table properties. Simply select Enable CDF on destination and Copy job takes care of the setup for you. This ensures the table is immediately ready for incremental processing and downstream CDC scenarios, helping reduce data movement and improve replication efficiency. By eliminating manual configuration steps, it makes advanced data integration patterns much easier to adopt and operate at scale.

To learn more, refer to the&nbsp;Automatic table creation and truncation on destination documentation.
Amazon Redshift as new source in Copy job
As part of our mission to enable multi-cloud data movement at petabyte scale with Copy job, we are bringing Amazon Redshift support as a source. This enables customers to seamlessly ingest data from one of AWS's most widely adopted data warehouse platforms directly into Fabric. Redshift support further strengthens Fabric's vision of delivering an open, connected, and multi-cloud data platform.

To learn more, refer to the Connectors for Copy Job documentation.
Copy job supports timestamps without time zone in Lakehouse
Support for timestamps without time zones (timestamp_ntz) allows Fabric Lakehouse tables to preserve date and time values exactly as stored, without applying time zone conversions. Copy job can now automatically map timezone-independent datetime values to Delta Lake timestamp_ntz, ensuring greater compatibility with source systems that allow storing date and time values without time zone information.
Migration Assistant for SQL database in Fabric (Generally Available)
The guided, Fabric-native wizard takes you from a source SQL Server schema to a running SQL database in Fabric: upload a DACPAC, review compatibility results, deploy the schema with Copilot-assisted fix suggestions, and copy your data using built-in Fabric Copy Jobs.
We've added the capability preview customers asked for most:&nbsp;Validate. You can now check out a DACPAC for compatibility before creating a SQL database in Fabric. Upload the file and the assistant reports which schema objects will deploy cleanly, which ones use features that aren't supported, the reason behind each failure, and the dependencies between objects — with nothing provisioned and no capacity consumed. That means you can scope migration effort, plan remediation, and get change-approval sign-off before you commit to a target database.
Figure: Validate a DACPAC and review compatibility results before creating a SQL database in Fabric.
To get started, select Migrate in your Fabric workspace and choose Migrate to SQL database in Fabric. To learn more, refer to the Fabric Migration Assistant documentation.
&nbsp;
Until next month
That's a wrap for the August 2026 Microsoft Fabric Monthly Update.
As always, we'll continue sharing new capabilities, enhancements, and improvements across Microsoft Fabric in future monthly updates.
Thank you for being part of the Fabric community!

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-August-2026-Feature-Summary/ba-p/5325824](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-August-2026-Feature-Summary/ba-p/5325824)*
