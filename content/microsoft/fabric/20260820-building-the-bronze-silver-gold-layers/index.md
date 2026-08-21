---
categories:
- MS
- Fabric
date: '2026-08-20T16:00:00+00:00'
description: 'Part two of a series on medallion architecture with Fabric Data Warehouse

  One job per layer — with enough implementation detail to make it real&nbsp;

  In Part 1 '
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Building-the-Bronze-Silver-Gold-layers/ba-p/5360201
source: Microsoft Fabric Blog
tags: []
title: Building the Bronze → Silver → Gold layers
---

Part two of a series on medallion architecture with Fabric Data Warehouse
One job per layer — with enough implementation detail to make it real&nbsp;
In Part 1 of this series, you picked your pattern. Now, let’s fill in the layers. The single most useful mental model here: each layer has exactly one job. Most medallion messes come from a layer doing another’s work — cleaning data in Bronze or letting business logic creep into Gold.&nbsp;
In this post, we’ll look at hose Bronze, Silver, and Gold layers are implemented in Fabric Data Warehouse (DW), the T-SQL patterns commonly used in each layer, and the practices that help keep your architecture maintainable as it grows. We’ll also cover which practices are worth calling out before we move deeper into best practices in Part 3 of this series.&nbsp;
Here’s what “one job” means in Fabric DW.&nbsp;
Bronze — land it, don’t touch it&nbsp;
Bronze has one job: ingest all source data in its original raw form, with no business logic or cleansing applied — “write everything down first” — so you preserve a source-of-truth copy you can always refer back to.&nbsp;
In Fabric DW, that usually means staging tables that closely mirror the source structure, placed in a dedicated Bronze schema, such as Bronze.SalesOrdersRaw, to clearly mark them as raw. If the source is a relational extract, the Bronze table often follows the source columns. If the source is semi-structured, keep parsing to the minimum needed to land and trace the data.&nbsp;
For loading, Fabric DW supports Data Factory Pipelines, Dataflows, COPY INTO, T-SQL ingestion, OPENROWSET, and Spark-based patterns. A common and efficient path is the T-SQL COPY INTO command to bulk-load files from OneLake or external storage into a DW table.&nbsp;
Implementation sketch:
CREATE SCHEMA Bronze; CREATE TABLE Bronze.CustomerRaw (CustomerID INT NULL, Name VARCHAR(100) NULL, Email VARCHAR(100) NULL, CreatedDate DATETIME2 NULL, RawFileName VARCHAR(255) NULL ); COPY INTO Bronze.CustomerRaw FROM 'https://&lt;storage&gt;/exports/customers/*.csv' WITH (FILE_TYPE = 'CSV', FIRSTROW = 2, FIELDTERMINATOR = ','); 
Hybrid note: if the raw data needs Spark-heavy preparation, land it in a Lakehouse for Bronze and then either expose it to the warehouse or begin Silver in Fabric DW. The rule does not change: Bronze is still raw, traceable, and rebuildable.
Do this well:&nbsp;

Preserve raw data. Do not filter out “bad” records in Bronze; all cleaning happens in Silver.&nbsp;


Use batch loads, not row-by-row inserts. Small trickle inserts create many small Delta files and hurt performance.&nbsp;


Keep file and schema discipline. Match the source structure and handle schema evolution by adding nullable columns rather than dropping source information.&nbsp;


Add metadata columns like ingestion timestamp or source filename for traceability and auditing.&nbsp;


Automate recurring loads with Fabric Pipelines or scheduled SQL scripts, so Bronze remains repeatable.

Silver — Clean it once, for everyone&nbsp;
Silver’s one job is to take raw Bronze data and apply cleaning, validation, and integration. This is where you remove duplicates, standardize formats, handle missing or invalid values, join related sources, and apply shared business rules.&nbsp;
In Fabric DW, Silver is typically implemented with T-SQL transformations from Bronze into curated Silver tables. Use CTAS, or CREATE TABLE AS SELECT, when you want to materialize a clean table from a query. Use INSERT...SELECT or MERGE when the pattern is incremental.&nbsp;
The important design point: Silver should become the single source of truth for cleansed data in the pipeline. You might keep one Silver table per Bronze source or combine multiple Bronze inputs into one conformed Silver table, such as a consolidated customer table.&nbsp;
Implementation sketch:&nbsp;
CREATE SCHEMA Silver; CREATE TABLE Silver.CustomerCleaned AS SELECT CustomerID, Name, IIF(Email NOT LIKE '%@%.%', NULL, Email) AS Email_Valid, CONVERT(DATETIME2(6), CreatedDate) AS CreatedDate FROM ( SELECT *, ROW_NUMBER() OVER (PARTITION BY CustomerID ORDER BY CreatedDate DESC) AS rn FROM Bronze.CustomerRaw ) AS t WHERE rn = 1;
Hybrid note: if Bronze lives in a Lakehouse, Silver can be handled with Spark, Dataflows, or T-SQL after the data is exposed to the warehouse. Use Dataflows for lighter citizen-developer transformations; use Spark or T-SQL when scale, repeatability, or engineering complexity matters.&nbsp;
Do this well:&nbsp;

Make transformations idempotent, so they can run repeatedly without damaging data.&nbsp;


Validate and enforce quality here. Silver is the gate where bad data gets stopped, fixed, or flagged.&nbsp;


Use MERGE for incremental upserts when late-arriving data needs to update Silver instead of forcing a full reload.&nbsp;


Use staging or temporary tables when the logic gets complex; simpler modular SQL is easier to maintain.&nbsp;


Keep performance visible. Complex joins and aggregations belong here, but they should be written in a way the warehouse can optimize and the team can reason about.

Gold — Shape it for the question&nbsp;
Gold’s one job: present business-ready data that Power BI, dashboards, and downstream analytics can use directly. This is usually where you shape the model into facts and dimensions, data marts, wide reporting tables, or pre-aggregated summaries.&nbsp;
In Fabric DW, this is where the warehouse shines. Gold tables are built with SQL transformations from Silver, often involving joins, calculated fields, and aggregations. The payoff is that report authors and business users do not need to repeat heavy transformations in every semantic model or dashboard.&nbsp;
To create Gold tables, start from clean Silver data and optimize for the business question. For example, transaction-level Silver data can become a daily sales summary table used directly by Power BI.
Implementation sketch:
CREATE SCHEMA Gold; CREATE OR ALTER VIEW Gold.v_Customer AS SELECT CustomerID, Name, Email_Valid AS Email, CreatedDate FROM Silver.CustomerCleaned; CREATE TABLE Gold.DailySalesSummary AS SELECT CAST(s.OrderDate AS date) AS OrderDate, COUNT(DISTINCT s.OrderID) AS TotalOrders, SUM(s.TotalAmount) AS TotalSalesAmount, COUNT(DISTINCT s.CustomerID) AS UniqueCustomers FROM Silver.SalesCleaned AS s GROUP BY CAST(s.OrderDate AS date);
Consumption: once Gold tables or views are created in Fabric DW, they are directly queryable by BI tools. Because the data is clean, aggregated, and shaped for use, consumers can treat Gold as the trusted version of the truth for analytics.&nbsp;
Do this well:&nbsp;

Model for analytics. If using a star schema, define the right grain for facts and use clean dimension tables.&nbsp;


Use aggregations to reduce data volume and make common queries fast.&nbsp;


By Gold, sensitive data should be removed, masked, or protected with row-level or column-level security.&nbsp;


Document lineage, especially how Gold fields are derived from Silver.&nbsp;


Decide a refresh strategy. Gold is usually rebuilt or incrementally updated on a schedule.&nbsp;


Encapsulate repeatable refresh logic in stored procedures when that makes the pipeline easier to operate.

The layers at a glance&nbsp;
Use this as a quick health check for your Fabric DW medallion design.
LayerIts one jobIn Fabric DWWatch out forBronzePreserve raw source data
Staging tables, COPY INTO, metadata columns

Cleaning too early; tiny files
SilverClean, validate, and conform
CTAS, INSERT...SELECT, MERGE, quality gates

Non-repeatable transformations
GoldServe trusted analytics
Facts, dimensions, marts, aggregations

Business logic leaking in late

Takeaway&nbsp;
Data flows Bronze (raw) → Silver (cleaned) → Gold (curated) — and the discipline is in the arrows. If you can point at any table and say which single job it serves, your medallion architecture is healthy.&nbsp;
When something breaks in Gold, a clean Bronze layer and a repeatable Silver layer let you recompute from scratch instead of reverse-engineering business logic from reports.&nbsp;
This post is part of our Medallion Architecture on Fabric Data Warehouse series: &nbsp;

Choosing your medallion pattern in Fabric Data Warehouse
Building the Bronze → Silver → Gold layers
Fabric DW best practices for medallion architectures 
Securing and governing your layers 
Performance tuning your medallion pipeline &nbsp;

In the next post, we'll explore the Fabric DW-specific best practices that keep all three layers fast, reliable, and governable.&nbsp;

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Building-the-Bronze-Silver-Gold-layers/ba-p/5360201](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Building-the-Bronze-Silver-Gold-layers/ba-p/5360201)*
