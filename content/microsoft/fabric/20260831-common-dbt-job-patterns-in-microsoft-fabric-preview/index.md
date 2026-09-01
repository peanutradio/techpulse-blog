---
categories:
- MS
- Fabric
date: '2026-08-31T21:00:00+00:00'
description: 'One of the most common questions we hear from customers adopting dbt
  job in Microsoft Fabric isn''t how to create models, it''s how to structure them.&nbsp;


  Shou'
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Common-dbt-job-patterns-in-Microsoft-Fabric-Preview/ba-p/5361480
source: Microsoft Fabric Blog
tags: []
title: Common dbt job patterns in Microsoft Fabric (Preview)
---

One of the most common questions we hear from customers adopting dbt job in Microsoft Fabric isn't how to create models, it's how to structure them.&nbsp;

Should Bronze, Silver, and Gold live in a Lakehouse or a Warehouse?
Should dbt own Silver, Gold, or both?
When does a Fabric pipeline make sense?

To help answer these questions,&nbsp;we've&nbsp;published new guidance:&nbsp;Common dbt job patterns in Microsoft Fabric (Preview). The guide walks through common architecture choices and explains where dbt fits within a Fabric medallion architecture.&nbsp;
Figure: Four documented patterns show how Warehouse and Lakehouse can support each medallion layer.
Four common patterns
The guidance highlights four common approaches:&nbsp;

Warehouse-only medallion for SQL-first teams that want to keep Bronze, Silver, and Gold in Fabric Data Warehouse.
Lakehouse landing with Warehouse transformation for teams that land raw data in OneLake but prefer Warehouse for transformation and serving.
Lakehouse refinement with Warehouse serving for organizations where data engineering and BI teams have distinct ownership boundaries.
Lakehouse-only medallion for Delta-first architectures that keep all layers in Lakehouse and use Spark-based execution.

There isn't a single right answer. The best pattern depends on where data lands, which engine performs transformations, and how curated data is consumed.&nbsp;
Orchestration is a separate decision
A key takeaway from the guidance is that architecture and orchestration should be considered separately.&nbsp;
After choosing where your data and transformations live, you can decide whether to run dbt jobs through native scheduling or orchestrate them as part of a broader Fabric pipeline workflow.&nbsp;
Fabric pipelines manage workflow dependencies and end-to-end orchestration, while dbt continues to own transformation logic, testing, dependencies, and materialization.&nbsp;
Learn more&nbsp;
If&nbsp;you're&nbsp;planning a new implementation, modernizing an existing architecture, or evaluating how&nbsp;dbt&nbsp;fits into your Fabric environment, start with the new documentation:&nbsp;Common dbt job patterns in Microsoft Fabric (Preview). &nbsp;
We'd love to hear which pattern best matches your architecture and where you'd like to see additional guidance in the future. Please share your feedback, questions, and use cases in the comments section.&nbsp;&nbsp;

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Common-dbt-job-patterns-in-Microsoft-Fabric-Preview/ba-p/5361480](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Common-dbt-job-patterns-in-Microsoft-Fabric-Preview/ba-p/5361480)*
