---
categories:
- MS
- Fabric
date: '2026-09-03T16:29:18+00:00'
description: Capacity Operation Events in Real-Time Hub is now available as a preview,
  giving Microsoft Fabric capacity administrators access to detailed, operation-level
  te
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Capacity-Operation-Events-in-Real-Time-Hub-Preview/ba-p/5364092
source: Microsoft Fabric Blog
tags: []
title: Capacity Operation Events in Real-Time Hub (Preview)
---

Capacity Operation Events in Real-Time Hub is now available as a preview, giving Microsoft Fabric capacity administrators access to detailed, operation-level telemetry in near real time.
Capacity Operation Events build on&nbsp;Capacity Overview Events by helping administrators move beyond understanding overall capacity health&nbsp;to&nbsp;identifying&nbsp;the specific operations, workspaces, and artifacts contributing to capacity consumption, throttling, and performance issues.&nbsp;&nbsp;
From capacity health to root cause analysis
Capacity administrators have long relied on tools such as the Capacity Metrics App to understand utilization patterns and investigate capacity-related issues. This augments those capabilities by providing direct, near real-time access to the underlying operational signals. &nbsp;It exposes granular telemetry for individual operations running on a Fabric capacity, enabling organizations to:

Identify the operations driving capacity consumption.
Investigate throttling delays and performance bottlenecks.
Understand which workspaces and artifacts contribute to capacity pressure.
Correlate workload activity with capacity utilization trends.
Build custom monitoring, governance, and automation solutions using Fabric Real-Time Intelligence, including the use of Microsoft Activator for designing automated triggers with no code.

Together with Capacity Overview Events, administrators now gain both a high-level and detailed view of capacity behavior:

Capacity Overview Events help answer:&nbsp;What is happening to my capacity?
Capacity Operation Events help answer: Which operations are causing it?

Deep operational visibility
Capacity Operation Events provide detailed telemetry for individual operations executed on a Fabric capacity. Each event includes information such as workspace and item details; capacity unit consumption; operation duration; operation start time and smoothing window attribution.&nbsp;
This enables administrators to quickly answer questions such as

Which refreshes consumed the most capacity yesterday?
Which operations experienced throttling delays?
Which operations are contributing most to capacity pressure?

Administrators can in turn translate these insights into actions:

Create alerts when high-cost operations exceed defined thresholds.
Detect and investigate throttled workloads in near real time.
Trigger automated notification and/or remediation workflows.
Stream operational telemetry into Eventhouse for historical analysis and retention.
Build custom dashboards tailored to your monitoring and governance needs.
Integrate capacity telemetry into existing operational platforms and workflows.

Getting started
For complete schema and event details for operation events, see the Explore Fabric capacity operation events in Fabric Real-Time hub documentation.
You can also learn more about overview events in the Explore Fabric capacity overview events in Fabric Real-Time hub documentation.&nbsp; The Capacity Events Accelerator GitHub repository is also available as an even easier way to get started with capacity events.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Capacity-Operation-Events-in-Real-Time-Hub-Preview/ba-p/5364092](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Capacity-Operation-Events-in-Real-Time-Hub-Preview/ba-p/5364092)*
