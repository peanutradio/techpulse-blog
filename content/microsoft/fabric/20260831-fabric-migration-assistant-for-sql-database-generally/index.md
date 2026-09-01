---
categories:
- MS
- Fabric
date: '2026-08-31T19:00:00+00:00'
description: Migrating a SQL Server database to the cloud rarely fails on the data
  — it fails on everything around it. Schema objects that use unsupported features
  surface o
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Migration-Assistant-for-SQL-database-Generally-Available/ba-p/5363606
source: Microsoft Fabric Blog
tags: []
title: Fabric Migration Assistant for SQL database (Generally Available)
---

Migrating a SQL Server database to the cloud rarely fails on the data — it fails on everything around it. Schema objects that use unsupported features surface only after you've provisioned a target and started deploying. Remediation turns into a manual pass-through error logs. Schema and data move through separate tools, on separate timelines, with separate things to go wrong. A migration most teams could scope in a day ends up planned like a quarter-long project.
What makes that work worth doing is where it lands. SQL database in Fabric provisions in seconds, with no networking, storage, or sizing decisions to make — the serverless architecture scales compute and storage automatically, and high availability, disaster recovery, Microsoft Entra authentication, customer-managed keys and SQL auditing are built in rather than configured. Your existing T-SQL, SSMS and Visual Studio Code skills carry over unchanged. And the part customers tend to notice first: near real-time replication to OneLake makes your operational data available to Power BI, Spark notebooks and AI workloads without a separate pipeline.
The Fabric Migration Assistant for SQL database is what gets you there — one guided workflow that assesses your schema, provisions the target, deploys with AI-assisted fixes for what doesn't translate, and copies your data without leaving the wizard. Now generally available and supported for production migrations, and it ships with the capability the preview community asked for most: the ability to validate your schema before you create anything in Fabric.

Migration Assistant for SQL database in Fabric landing page.

Validate before you provision
The new Validate function gives early signals of incompatibilities. Upload a DACPAC generated from your source database and the Migration Assistant analyzes it on its own, before a target database exists. You get the compatibility picture up front: which schema objects deploy cleanly, which ones use features that SQL database in Fabric does not support, why each incompatible object fails, and how the objects depend on one another. Nothing is provisioned, nothing is consumed, and no capacity is committed while you assess.
That changes how a migration gets planned. Architects can scope effort and risk during the assessment phase, hand a concrete remediation list to the team that owns the source database and walk into a change-advisory conversation with evidence instead of an estimate. Teams evaluating several candidate databases can validate all of them in an afternoon and sequence the easy wins first. The migration itself starts only once you already know how it will end.
Validate results view showing compatible and incompatible schema objects before database creation.
Getting Started
The new validation experience changes the migration conversation from reactive to proactive. Instead of discovering compatibility issues after you've created a target and started deploying, you can assess your schema upfront, understand exactly what needs attention, and move forward with a migration plan grounded in real results. When you're ready, the Migration Assistant guides you through assessment, deployment, and data movement in a single workflow.

Export your source schema as a DACPAC using SQL Server Management Studio, the MSSQL extension for Visual Studio Code, or SqlPackage. Complete the prerequisites. From your Fabric workspace, select Migrate and choose Migrate to SQL database in Fabric.
Run Validate first. Upload the DACPAC and review the compatibility report: migrated objects, failed objects, the reason behind each failure, and the dependency chain between them. Resolve what you can at the source or plan the remediation.
Provision the target when you are ready. Name your SQL database in Fabric and the wizard creates it in seconds, with no networking, storage, or sizing decisions to make.
Deploy the schema. The assistant applies the schema and surfaces any remaining incompatibilities, with Copilot-powered fix suggestions you can accept or reject interactively. Primary objects are resolved before dependent objects follow.
Copy the data using built-in Fabric Copy Jobs powered by Data Factory. The assistant integrates with the Fabric Data Gateway to reach on-premises sources securely and supports pre-deployment and post-deployment scripts so constraints can be disabled and re-enabled around the load.
Validate and go live. Review results in the Migration Assistant panel inside the SQL editor, verify your data, update application connection strings, and complete the migration session.

Learn more
Migrating a database shouldn't require weeks of investigation before you can determine whether the move is viable. With Fabric Migration Assistant, you can assess compatibility, understand remediation requirements, deploy your schema, and move your data through a single guided workflow.
Download a DACPAC from an existing SQL Server database and run a validation assessment today. You'll get immediate insight into compatibility and migration readiness, helping you prioritize the right databases and accelerate your move to SQL database in Microsoft Fabric. For detailed guidance, review the migration documentation.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Migration-Assistant-for-SQL-database-Generally-Available/ba-p/5363606](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Migration-Assistant-for-SQL-database-Generally-Available/ba-p/5363606)*
