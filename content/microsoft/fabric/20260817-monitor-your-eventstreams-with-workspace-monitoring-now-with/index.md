---
categories:
- MS
- Fabric
date: '2026-08-17T21:00:00+00:00'
description: 'When a real-time data pipeline breaks, the first question is always
  the same: what went wrong, and where?

  Eventstream observability in Microsoft Fabric brings t'
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Monitor-your-Eventstreams-with-Workspace-Monitoring-Now-with-per/ba-p/5331185
source: Microsoft Fabric Blog
tags: []
title: Monitor your Eventstreams with Workspace Monitoring — Now with per-Eventstream
  control (Preview)
---

When a real-time data pipeline breaks, the first question is always the same: what went wrong, and where?
Eventstream observability in Microsoft Fabric brings that visibility directly into Workspace Monitoring, so you can track the health, performance, and errors of your eventstreams without leaving the Fabric portal or setting up external monitoring infrastructure.
With this update, eventstream observability returns with a redesigned approach that puts you in control — including a per-eventstream toggle that lets you decide exactly which eventstreams emit monitoring data and which don't.
Why observability matters for real-time pipelines
Real-time data pipelines operate continuously. Unlike batch jobs that run and finish, an eventstream can process data for days or weeks without anyone checking on it. When something goes wrong — a destination write error, or backlogged events — the problem often goes unnoticed until downstream consumers report stale or missing data.
Eventstream observability closes that gap. When enabled, each eventstream emits structured metrics and error data to your workspace's monitoring Eventhouse. This data lands in three dedicated KQL tables that you can query, alert on, and build dashboards against:


Table&nbsp;What it capturesEmission frequencyWhen to use itEventStreamMetricsThroughput, event counts, ingress and egress rates&nbsp;~1 minuteCapacity planning, data flow validation, throughput monitoringEventStreamErrorMetricsError category, error code, severity, affected events~1 minuteTroubleshooting failures, identifying recurring error patternsEventStreamNodeStatusNode state (running, stopped)~6 hoursHealth checks, topology status overview


&nbsp;


Once monitoring is enabled for an eventstream, these tables appear&nbsp;in your workspace's monitoring KQL database alongside existing Eventhouse and other workload tables.

&nbsp;
Figure: Eventstream monitoring tables in the Workspace Monitoring KQL database, showing EventStreamMetrics, EventStreamErrorMetrics, and EventStreamNodeStatus tables available for querying.
What changed: Per-eventstream control&nbsp;
During the initial preview, eventstream observability was enabled globally for all eventstreams in a workspace. Based on customer feedback, the feature now provides granular control — you choose exactly which eventstreams emit monitoring data, giving you flexibility to focus observability on the pipelines that matter most.
The key changes:

Per-eventstream toggle.&nbsp;A new setting — Log Eventstream activity — appears in each eventstream's settings panel. You can enable or disable observability for each eventstream individually, so a workspace with ten eventstreams can monitor only the two or three that are most critical.
Default OFF.&nbsp;All eventstreams default to monitoring OFF. You choose which ones to enable.&nbsp;

How to get started
Eventstream observability requires Workspace Monitoring to be enabled on your workspace. If you haven't set it up yet:

Open your workspace settings.
Navigate to the Monitoring section.
Enable Workspace Monitoring and wait for the monitoring Eventhouse to provision.

Once Workspace Monitoring is active:

Open any eventstream in your workspace.
Select the Settings gear icon.
Find the Log Eventstream activity toggle.
Enable it.

Within a few minutes, data begins flowing into the eventstream tables in your monitoring KQL database. You can query them directly with KQL, build Real-Time Dashboards, or use KQL querysets for deeper analysis.
What comes next
This release covers performance metrics, error metrics, and node health — the foundation for operational observability. Additional capabilities are planned for future updates:

Diagnostic logs — Error logs from the ASA processing engine, providing deeper root-cause analysis for failures like deserialization errors, runtime query errors, and output write failures.
Expanded connector coverage — Monitoring support for connector-based sources is under investigation and will be added in a future phase.

Next steps

Refer to the&nbsp;Eventstream monitoring in Workspace Monitoring&nbsp;documentation to learn more about configuration and supported tables.
Refer to the&nbsp;Workspace Monitoring overview&nbsp;documentation for setup instructions.
Share your feedback and feature requests in the&nbsp;Fabric Community Forums.
Explore the&nbsp;Create a Real-Time Dashboard&nbsp;documentation to build live dashboards from your monitoring data.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Monitor-your-Eventstreams-with-Workspace-Monitoring-Now-with-per/ba-p/5331185](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Monitor-your-Eventstreams-with-Workspace-Monitoring-Now-with-per/ba-p/5331185)*
