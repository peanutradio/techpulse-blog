---
categories:
- MS
- Fabric
date: '2026-08-18T13:27:49+00:00'
description: 'One of the most common questions I hear from customers as they move
  to an allocation-based billing model is:

  "What levers do I actually have if I want to contro'
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Using-Custom-SQL-Pools-to-balance-performance-and-cost-in-Fabric/ba-p/5359735
source: Microsoft Fabric Blog
tags: []
title: Using Custom SQL Pools to balance performance and cost in Fabric Data Warehouse
---

One of the most common questions I hear from customers as they move to an allocation-based billing model is:
"What levers do I actually have if I want to control how many resources my workloads consume?"
Many customers are looking for a way to prevent specific workloads from scaling aggressively and consuming more resources than they're comfortable with. That's where Custom SQL Pools can help significantly.
Custom SQL Pools, currently in preview and soon to be generally available, were designed to give customers more control over how resources are allocated across workloads. Their primary purpose is workload isolation and governance, but with allocation-based billing, governing resource allocation can have a direct impact on overall consumption.
Figure 1: Custom SQL Pools - application based classification
Understanding the tradeoff
Every workload wants resources.
Your Power BI dashboards want low latency. Your ETL workloads want throughput. Your ad hoc analysts want concurrency.
Without workload governance, multiple workloads can compete for the same resources and can scale aggressively when they become active. While that's desirable for some latency-sensitive workloads, customers often want different workloads isolated from one another. A business-critical Power BI dashboard may require priority access to resources, while an ETL process may be allowed to run longer in exchange for consuming fewer resources. Custom SQL Pools provide those isolation boundaries and governance controls, allowing customers to make intentional performance versus consumption tradeoffs.
A simple example
Imagine a bursty reporting workload. Without a Custom SQL Pool, a query might:

Scale to 10 vNodes.
Run for 10 seconds.
Trigger billing on 10 allocated vNodes, each subject to the one-minute minimum allocation window.

In that scenario, the workload would be billed against a maximum allocation of 10 vNodes during the billing interval. Under the one-minute minimum, that's approximately 600 billed vNode-seconds.
Now imagine that same workload is assigned to a Custom SQL Pool limited to 50% of the warehouse resources.
The query might:

Scale to only 5 vNodes.
Run for 20 seconds instead of 10.
Trigger billing on only 5 allocated vNodes during the billing interval.

In that scenario, the workload would be billed against a maximum allocation of 5 vNodes, or approximately 300 billed vNode-seconds under the one-minute minimum.
The query took longer to complete, but it reached a lower allocation peak and consumed fewer billed resources.
In other words:
You're trading performance for a smaller resource footprint. For bursty workloads that frequently trigger the one-minute minimum allocation window, limiting how aggressively a workload can scale may reduce overall consumption. For latency-sensitive workloads, that tradeoff may not make sense. For others, it's an intentional and useful optimization.
The real-world scenario
Numerous customers use Custom SQL Pools effectively to control how resources are allocated to their workloads. &nbsp;
One customer had a long-running ETL process that wasn't latency sensitive. For this customer, they didn't care whether the load finished in 20 minutes or 30 minutes.
What they did care about was controlling how aggressively the workload consumed resources. They used SQL Pools to restrict the throughput available to that ETL process, effectively smoothing resource usage over a longer period of time. The workload ran longer, but consumed resources at a more controlled rate to further utilize the 24-hour smoothing behavior of background operations.
That's a great example of where workload governance and consumption governance become closely related.
Good Candidates for SQL Pools
Custom SQL Pools are particularly useful when you have workloads that:

Are not latency sensitive.
Can tolerate longer runtimes.
Generate large bursts of activity.
Compete with other workloads for resources.
Need more predictable resource consumption.

Examples include:
ETL and Data Loading
Nightly data loads often care more about successful completion than finishing a few minutes faster.
Restricting resource allocation may be a perfectly reasonable tradeoff.
Background Processing
Data preparation, maintenance operations, and other non-interactive workloads can often run with fewer resources.
Power BI Reporting
Many organizations want to ensure reporting workloads cannot consume all available warehouse resources during business hours.
Using Custom SQL Pools to allocate a fixed percentage of resources to reporting workloads provides predictable behavior while preventing contention with other workloads.
What Custom SQL Pools are not
It's equally important to be clear about what SQL Pools do not do.
They are not:

A spending limit.
A monthly budget cap.
A mechanism to stop charges after a threshold.
A "$100/month maximum" control.

Instead, they serve as guardrails that define the maximum share of warehouse resources each workload can access. While they are not a cost optimization feature in themselves, the governance they provide can indirectly affect overall consumption.
Looking ahead
We're continuing to invest in workload management capabilities.
A common ask we hear is for resource governance to follow the identity rather than the application. For example, a customer may want executive reporting users, production service principals, or critical business processes to receive different resource allocations than ad hoc analyst workloads.
Identity-based classifiers are one example of where we're headed, but the broader goal is to give customers more flexibility in how workloads are governed. We're also working toward making the built-in workload boundaries configurable, allowing customers to adjust resource allocation between the default SELECT and NONSELECT workload groups without needing to define custom classifiers. For customers that simply want different resource allocations for reporting and ingestion workloads, this provides a simpler path to workload governance.
Ultimately, the goal is to give customers more explicit control over how resources are allocated and governed, making it easier to align resource allocation with business priorities while managing performance and consumption in a predictable way. 
Takeaway
Custom SQL Pools is primarily a workload management feature, not a cost-control feature. However, in an allocation-based billing model, governing resource allocation and governing consumption become closely related.
If you're willing to let a workload run a little longer, Custom SQL Pools can provide a practical mechanism for reducing the resources that workload is allowed to consume.
For many customers, that's exactly the lever they're looking for. Get started with Custom SQL Pools today! Learn more in the documentation: Configure Custom SQL Pools in the Fabric Portal.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Using-Custom-SQL-Pools-to-balance-performance-and-cost-in-Fabric/ba-p/5359735](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Using-Custom-SQL-Pools-to-balance-performance-and-cost-in-Fabric/ba-p/5359735)*
