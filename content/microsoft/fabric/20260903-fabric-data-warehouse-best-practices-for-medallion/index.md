---
categories:
- MS
- Fabric
date: '2026-09-03T21:00:00+00:00'
description: 'Part three of a series on medallion architecture with Fabric Data Warehouse.

  Good medallion architecture is mostly operational discipline.

  In&nbsp;part&nbsp;one'
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Data-Warehouse-best-practices-for-medallion-architectures/ba-p/5364080
source: Microsoft Fabric Blog
tags: []
title: Fabric Data Warehouse best practices for medallion architectures
---

Part three of a series on medallion architecture with Fabric Data Warehouse.
Good medallion architecture is mostly operational discipline.
In&nbsp;part&nbsp;one&nbsp;of this series, we chose the pattern, and in&nbsp;part&nbsp;two, we filled in the Bronze, Silver, and Gold layers. Now comes the part that usually determines whether the architecture holds up in production: the operating rules. Many medallion architectures look great on paper but become difficult to maintain as data volumes, business requirements, and consumers grow.&nbsp;
A medallion pipeline is easy to explain and easy to demo. It is more challenging to keep clean over time. The challenges usually start small: row-by-row loads, report-specific logic in the wrong place, transformations that cannot be safely rerun, or Gold tables that slowly become another staging layer.&nbsp;
Why this matters&nbsp;
Most medallion problems are not caused by the&nbsp;names&nbsp;Bronze, Silver, and Gold. They happen because the pipeline stops behaving like a pipeline.&nbsp;
Bronze starts cleaning. Silver starts serving dashboards. Gold starts compensating for upstream data quality. Before long, nobody&nbsp;knows&nbsp;where a rule belongs, and every change feels risky.&nbsp;
The goal of best practices is not to add ceremony. The goal is to make the pipeline predictable: predictable loads, repeatable transformations, trusted outputs, and clear places to look when something breaks.&nbsp;
Best practice 1:&nbsp;Batch the writes&nbsp;
Fabric DW is built for set-based work. Treat ingestion and transformations as batches, not as a stream of tiny row-by-row operations.&nbsp;
In Bronze, that usually means using COPY INTO, Fabric Pipelines, or other bulk-loading patterns to land data in raw tables. If you are&nbsp;loading from&nbsp;files, aim for fewer well-sized files instead of many tiny ones. When practical, files in the 100 MB to 1 GB range are a healthier starting point than a long tail of small files.&nbsp;
In Silver and Gold, the same idea&nbsp;applies:&nbsp;prefer set-based T-SQL transformations, CTAS, INSERT...SELECT, and MERGE patterns over procedural row-at-a-time logic.&nbsp;
Do this well

Load Bronze in&nbsp;batches, and&nbsp;avoid trickle inserts when the source can be staged first.&nbsp;
Add ingestion metadata, such as source file name and load timestamp, so every batch is traceable.&nbsp;
Keep operational logging lightweight. If you need very high-write audit events, do not turn the warehouse into a single-row logging engine.&nbsp;
Let Bronze preserve the batch; let Silver decide what is valid.&nbsp;


Rule of thumb: if a load pattern creates a large number of tiny writes, fix the load pattern before tuning the query.&nbsp;

Best practice 2:&nbsp;Make Silver rerunnable&nbsp;
Silver is where the pipeline earns trust. That means&nbsp;Silver&nbsp;transformations need to be repeatable, testable, and safe to rerun.&nbsp;
If an upstream source reloads, or a cleansing rule changes, you should know how to rebuild the affected&nbsp;Silver&nbsp;tables without guessing which reports need to be patched. This is where idempotent design matters: a transformation should produce the same result when&nbsp;run&nbsp;again against the same inputs.&nbsp;
In Fabric DW, use CTAS when you want to materialize a clean table from a query, INSERT...SELECT for controlled incremental loads, and MERGE when&nbsp;late-arriving&nbsp;or changed data needs to update existing&nbsp;Silver&nbsp;rows.&nbsp;
Do this well

Design transformations so they can run again without duplicating or corrupting data.&nbsp;
Use staging tables when the logic is complex. A few clear steps are easier to&nbsp;operate&nbsp;than one unreadable query.&nbsp;
Put quality gates in Silver:&nbsp;required&nbsp;fields, valid formats, duplicate handling, and reason codes for rejected records.&nbsp;
Choose precise data types and lengths. Silver is the right place to turn loose source data into reliable analytical data.&nbsp;


Rule of thumb: if a report needs to clean the data again, Silver did not finish its job.

Best practice 3:&nbsp;Shape Gold for consumption&nbsp;
Gold is not just “the final table.” Gold is the business-facing serving layer. It should be modeled around how people ask questions, not around how the source systems store data.&nbsp;
For some workloads, that means a star schema with fact and dimension tables. For others, it means a data mart, a wide reporting table, or a pre-aggregated summary. The pattern matters less than the principle: Gold should make the common analytical path simple, fast, and trustworthy.&nbsp;
This is also where you should be careful not to let Gold become a junk drawer. If Gold is full of one-off fixes, report-specific exceptions, and raw technical fields, the layer is doing too much.&nbsp;
Do this well

Model the grain explicitly. A fact table without a clear grain becomes hard to explain and harder to debug.&nbsp;
Pre-aggregate where the business repeatedly asks the same question.&nbsp;
Hide technical fields that helped the pipeline but do not help the consumer.&nbsp;
Keep Gold dependent on Silver by default. Direct Gold-to-Bronze&nbsp;dependencies should be rare and deliberate.&nbsp;


Rule of thumb: Gold should answer the business question quickly without making the report author rediscover the pipeline.&nbsp;

Best practice 4: Use Fabric DW defaults, but do not fight the engine&nbsp;
Fabric DW gives you a SQL warehouse over Delta data in&nbsp;OneLake. That means you get transactional behavior, optimized storage patterns, and a managed engine that handles many physical decisions for you.&nbsp;
The practical&nbsp;advice: do not bring every habit from traditional data warehousing with you. You do not need to micromanage distribution or&nbsp;indexing&nbsp;the same way you would in older platforms. Focus first on healthy data layout, set-based transformations, good table design, and predictable query patterns.&nbsp;
At the same time, do not ignore the basics. Query performance still&nbsp;benefits&nbsp;from clean data types, useful statistics, well-shaped&nbsp;Gold&nbsp;tables, and avoiding unnecessary scans.&nbsp;
Do this well

Keep V-Order and platform&nbsp;optimizations on&nbsp;unless you have a measured reason to change them.&nbsp;


Use the performance guidance for Fabric Data Warehouse before inventing custom tuning patterns.&nbsp;


Check query behavior when a&nbsp;Gold&nbsp;table becomes critical to many reports.&nbsp;


Treat advanced exceptions as exceptions. Most teams should start with the defaults and tune only when evidence says to tune.&nbsp;


Rule of thumb: tune from evidence, not from habit.&nbsp;

Best practice 5: Monitor by layer&nbsp;
A medallion pipeline should be observable at each layer. If a dashboard is wrong or slow, you should be able to tell whether the issue started in&nbsp;Bronze&nbsp;ingestion, Silver transformation, or Gold serving.&nbsp;
In Fabric DW, use Query Insights and the warehouse monitoring views to understand query behavior, expensive operations, and refresh patterns. Pair that with pipeline-level monitoring so you can see not only whether a job failed, but where the failure happened.&nbsp;
Measure the pipeline in terms the team can act on:&nbsp;Bronze&nbsp;load duration,&nbsp;Silver&nbsp;transformation duration, rejected-record counts,&nbsp;Gold&nbsp;refresh duration, and&nbsp;Gold&nbsp;query performance.&nbsp;
Do this well

Track load and refresh duration by layer.&nbsp;
Log the number of records received, accepted, rejected, and published.&nbsp;
Watch critical&nbsp;Gold&nbsp;queries after&nbsp;refresh, especially the ones that feed executive dashboards or widely used semantic models.&nbsp;
Keep operational alerts tied to business impact. A failed&nbsp;Gold&nbsp;refresh matters differently from a delayed&nbsp;Bronze&nbsp;load.&nbsp;


Rule of thumb: if you cannot tell which layer failed, your monitoring is not layer-aware enough.&nbsp;

Before moving a medallion pipeline into production, use the following checklist to verify that each layer is&nbsp;operating&nbsp;as intended.&nbsp;
The best-practice checklist&nbsp;

Area&nbsp;

Bronze&nbsp;

Silver&nbsp;

Gold&nbsp;

Write pattern&nbsp;

Batch&nbsp;ingest&nbsp;with metadata&nbsp;

Set-based transformations&nbsp;

Scheduled refreshes&nbsp;

Quality rule&nbsp;

Preserve what arrived&nbsp;

Validate, conform, and flag&nbsp;

Expose trusted fields only&nbsp;

Performance focus&nbsp;

Avoid tiny writes&nbsp;

Keep logic rerunnable&nbsp;

Shape for common queries&nbsp;

Takeaway&nbsp;
Part&nbsp;two&nbsp;of this series&nbsp;was about one job per layer. Part&nbsp;three&nbsp;is about&nbsp;operating&nbsp;each job&nbsp;like&nbsp;it matters.&nbsp;
Batch the&nbsp;writes. Make Silver rerunnable. Shape Gold for consumption. Use Fabric DW’s managed engine instead of fighting it. Monitor the pipeline by layer so failures are easy to&nbsp;locate&nbsp;and fixes happen in the right place.&nbsp;
If you follow those rules, your medallion architecture becomes less fragile over time, not more.&nbsp;
This post is part of our Medallion Architecture on Fabric Data Warehouse series:&nbsp;

Choosing your medallion pattern in Fabric Data Warehouse&nbsp;
Building the Bronze → Silver → Gold layers&nbsp;
Fabric DW best practices for medallion architectures&nbsp;
Securing and governing your layers&nbsp;
Performance tuning your medallion pipeline&nbsp;

Ready to go deeper? Explore the&nbsp;Microsoft Fabric Data Warehouse performance guidelines&nbsp;and&nbsp;ingestion guidance, then stay tuned for Part four of this series, where we’ll cover securing and governing your layers.&nbsp;

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Data-Warehouse-best-practices-for-medallion-architectures/ba-p/5364080](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Data-Warehouse-best-practices-for-medallion-architectures/ba-p/5364080)*
