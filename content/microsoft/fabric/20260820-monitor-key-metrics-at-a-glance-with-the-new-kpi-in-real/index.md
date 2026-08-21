---
categories:
- MS
- Fabric
date: '2026-08-20T17:05:26+00:00'
description: 'Operational dashboards often need to answer a direct question: Is this
  metric healthy, approaching a limit, or already in a critical range? A number alone
  shows'
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Monitor-key-metrics-at-a-glance-with-the-new-KPI-visualization/ba-p/5360033
source: Microsoft Fabric Blog
tags: []
title: Monitor key metrics at a glance with the new KPI visualization in Real-Time
  Dashboards (Generally Available)
---

Operational dashboards often need to answer a direct question: Is this metric healthy, approaching a limit, or already in a critical range? A number alone shows the current value, but it does not always provide the context needed to interpret that value quickly.
The new KPI visualization is now generally available in Microsoft Fabric Real-Time Dashboards. It combines a query-based numeric value with a scale, status ranges, formatting options, and reference indicators so you can communicate metric health in a single responsive visual.
Choose the right KPI display
You can display a KPI in four modes: Number, Bar, Gauge, or Donut. Each mode uses the same underlying value and configuration while supporting a different dashboard layout.

Number emphasizes the formatted value and works well in compact layouts.
Bar shows progress across a horizontal scale and fits wide or narrow tiles.
Gauge presents the value on an arc for operational monitoring.
Donut uses a circular layout that works well in square dashboard tiles.

You can select the numeric source column returned by your Kusto Query Language (KQL) query. This makes the visual useful for metrics such as CPU usage, latency, error rate, availability, queue size, ingestion delay, orders per minute, or other operational and business measures.
Bike-sharing dashboard showing KPI visuals in Number, Bar, Donut, and Gauge modes for available bikes, dock utilization, station occupancy, and bikes in use.
Define healthy, warning, and critical ranges
The KPI visualization includes a configurable minimum and maximum scale. By default, the scale runs from 0 to 100, but you can adjust it to match the metric you are monitoring.
Conditional-formatting ranges add status context to the current value. The default configuration includes OK below 33, Warning from 33, and Danger from 67. You can change the range boundaries, colors, and tag text to match the terminology and operating limits used by your team.
You can update a boundary by dragging its threshold marker on the colored range bar or by entering the value manually. You can also control the threshold direction. For example, high availability might be healthy, while high latency or error rates might indicate a problem.
Range marks can be displayed directly on the visual, helping dashboard viewers understand where the current value sits within the configured scale.
Format values for your dashboard
Value formatting helps you balance precision and readability. The Automatic format abbreviates large values, such as displaying 12,340 as 12.34K. The Full format displays the complete value.
You can also control the maximum number of decimal places. The unit label is enabled by default and initially set to %, but you can customize it for values such as ms, req/s, or another unit. You can also hide the unit when the metric is self-explanatory.
Add targets and operational context
Reference lines let you mark specific values on the KPI scale. You can use them to show a service-level agreement (SLA) target, an expected value, a maximum acceptable latency, a capacity limit, or another benchmark.
By combining the current value with status ranges and a reference line, viewers can see both the present condition and its relationship to an important operating target without moving to another visual.
Why it matters
A KPI visual reduces the work required to interpret an operational metric. Instead of reading a number and remembering its acceptable range, viewers can use the scale, color, status tag, and reference indicators together.
This is particularly useful for dashboards that teams monitor continuously. A dashboard author can apply consistent definitions for healthy, warning, and critical states, while viewers can identify metrics that need attention at a glance.
Get started
To configure a KPI visualization in a Real-Time Dashboard:

Open the dashboard in Editing mode and edit an existing tile or add a tile with a KQL query that returns a numeric value.
In the visual formatting pane, select KPI as the visual type.
Select the numeric source column and choose Number, Bar, Gauge, or Donut.
Configure the minimum and maximum scale, value format, decimal places, and unit label.
Adjust the conditional-formatting ranges, colors, status tags, and threshold direction.
Optionally, show range marks and add reference lines for targets or limits.
Apply the changes and resize the tile to fit your dashboard layout.

To learn more, explore&nbsp;Customize Real-Time Dashboard visuals and Apply conditional formatting in Real-Time Dashboard visuals.
Cost and requirements
To create or edit the visualization, you need a workspace with Microsoft Fabric-enabled capacity and Editor permissions on the Real-Time Dashboard.
Next steps
Open a Real-Time Dashboard, choose a metric with known operating limits, and configure the KPI visual with the scale, status ranges, and reference line that match how your team evaluates that metric.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Monitor-key-metrics-at-a-glance-with-the-new-KPI-visualization/ba-p/5360033](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Monitor-key-metrics-at-a-glance-with-the-new-KPI-visualization/ba-p/5360033)*
