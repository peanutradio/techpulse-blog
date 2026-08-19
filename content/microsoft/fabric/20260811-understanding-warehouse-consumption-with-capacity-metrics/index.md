---
categories:
- MS
- Fabric
date: '2026-08-11T14:55:50+00:00'
description: 'When reviewing warehouse usage, customers often start with the same
  question: which workloads contributed to the consumption I’m seeing in the Fabric
  Capacity M'
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Understanding-Warehouse-Consumption-with-Capacity-Metrics-and/ba-p/5357458
source: Microsoft Fabric Blog
tags: []
title: Understanding Warehouse Consumption with Capacity Metrics and Query Insights
---

When reviewing warehouse usage, customers often start with the same question: which workloads contributed to the consumption I’m seeing in the Fabric Capacity Metrics app?With the updated consumption accrual model for Fabric Data Warehouse and the SQL analytics endpoint, the best way to answer that question is to use Capacity Metrics and Query Insights together. Capacity Metrics shows warehouse-level CU consumption over time based on the virtual nodes allocated to the warehouse, while Query Insights helps you understand the queries, users, and workloads active during that same period.This blog walks through a practical approach for applying query-level weighted attribution from warehouse-level consumption data.Start with the Capacity Metrics AppThe Fabric Capacity Metrics app remains the source for getting the Capacity Units (CUs) consumed by the warehouse. It shows how many CUs were consumed by a warehouse during a given time range.Use the Metrics app to identify:Which warehouses are consuming the most CUsWhen consumption spikes occurredWhich workloads are contributing most to capacity usageHow much Warehouse consumption was attributed to user initiated workloads vs system initiated workloads.The following example shows the Capacity Metrics app timepoint detail view used to identify a period of warehouse consumption for further analysis.Capacity Metrics identifies when consumption occurred, but it doesn't explain which specific queries contributed to that consumption. To answer that question, use Query Insights.Drill into Query InsightsAfter identifying a time window of interest, Query Insights can help explain what drove that consumption.The queryinsights.exec_requests_history view captures the allocated CPU time, or vCore seconds, for each query, enabling you to analyze which users, workloads, and queries are consuming compute resources.DECLARE start_Time DATETIME2(0) = '2026-08-10 8:00:00'
        ,@End_Time DATETIME2(0) = '2026-08-10 9:00:00'

SELECT
        [database_name],
        sql_pool_name,
        distributed_statement_id,
        login_name,
        allocated_cpu_time_ms / 1000.0 AS vcore_seconds
FROM queryinsights.exec_requests_history
WHERE start_time &lt; @End_Time
AND end_time &gt; start_TimeApplying Weighted Consumption by QueryNow that we have data for the workloads that were running for the period of interest, we can combine these two datasets to get the weighted consumption of each workload's contribution to overall warehouse consumption.For a given period:Capture the total CU consumption from the Capacity Metrics app.Sum the vCore seconds of all the queries from Query Insights.Allocate CU consumption proportionally based on each query’s percentage of total vCore seconds.For example, if a query represents 25% of the total vCore seconds consumed during a period, you can attribute approximately 25% of the warehouse CU consumption to that query.Because Capacity Metrics reports warehouse-level consumption and Query Insights reports query-level activity, the resulting attribution is a weighed attribution of warehouse consumption. You can further analyze the data by attributing cost to certain SQL pool usage or by users.Practical ScenariosThis approach can help answer questions such as:Which users contributed most to warehouse consumption during a peak period?Which workloads should be prioritized for optimization?Which SQL pools or warehouses are driving the highest capacity usage?How did a recent deployment or workload pattern change affect consumption?Bringing It TogetherThink of the two tools as answering different questions:Capacity Metrics: How much consumption occurred over timeQuery Insights: What was running during that timeBy using Capacity Metrics and Query Insights together, you can move from simply seeing warehouse consumption to understanding what drove it. While attribution remains weighted attribution of the workloads executed, this approach helps identify expensive workloads, prioritize optimization efforts, and make more informed capacity decisions.Learn MoreTo learn more about monitoring and understanding warehouse consumption, see:How to Observe Fabric Data Warehouse utilization trendsBilling and utilization reporting in Fabric Data WarehouseFabric Operations

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Understanding-Warehouse-Consumption-with-Capacity-Metrics-and/ba-p/5357458](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Understanding-Warehouse-Consumption-with-Capacity-Metrics-and/ba-p/5357458)*
