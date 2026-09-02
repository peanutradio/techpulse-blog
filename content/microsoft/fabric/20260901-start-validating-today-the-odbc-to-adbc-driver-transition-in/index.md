---
categories:
- MS
- Fabric
date: '2026-09-01T20:00:00+00:00'
description: Power BI and Microsoft Fabric are transitioning supported data source
  connections from legacy embedded ODBC drivers to&nbsp;Apache Arrow Database Connectivity
  (
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Start-validating-today-The-ODBC-to-ADBC-driver-transition-in/ba-p/5362917
source: Microsoft Fabric Blog
tags: []
title: 'Start validating today: The ODBC to ADBC driver transition in Power BI and
  Microsoft Fabric (Generally Available)'
---

Power BI and Microsoft Fabric are transitioning supported data source connections from legacy embedded ODBC drivers to&nbsp;Apache Arrow Database Connectivity (ADBC) drivers. ADBC provides standard interfaces for interacting with Arrow data, delivers significant performance improvements when fetching large datasets, and incorporates memory-safety and security enhancements over the embedded drivers we ship today.&nbsp;
In this post, we share what’s changing, and how you can start validating today — well before the change becomes the default for your tenant.&nbsp;
If you use Snowflake, Databricks, Azure Databricks, Google BigQuery, Google BigQuery (Microsoft Entra ID), Impala, Spark, or Dremio connectors in Power BI or Fabric, this transition affects you. To learn more about each connector, please reference our migration documentation.&nbsp;&nbsp;
Why we’re making this change&nbsp;
We’re making the change for several reasons:&nbsp;

Efficient transport for large result sets. ADBC provides standard interfaces for interacting with Arrow data, especially efficient at fetching large datasets with minimal overhead and no serialization or copying. That matters most for wide table Import refresh and DirectQuery over large result sets.&nbsp;
Security posture. The ADBC drivers incorporate security enhancements. Collaboration with the open-source community also enables more rapid updates, utilizing modern tools and secure development lifecycle (SDL) processes.&nbsp;

Connectors and driver changes&nbsp;
The following connectors are transitioning from embedded ODBC drivers to replacement drivers. In most cases, the replacement driver is an ADBC driver.&nbsp;

Snowflake&nbsp;
Google BigQuery&nbsp;
Google BigQuery (Microsoft Entra ID)&nbsp;
Databricks&nbsp;
Azure Databricks&nbsp;
Impala&nbsp;
Spark&nbsp;
Dremio&nbsp;

To learn more, please reference our migration documentation.&nbsp;&nbsp;
The two events that matter&nbsp;&nbsp;
Event 1 - Default enablement:&nbsp;ADBC becomes the default for new connections. Existing connections in your semantic models, dataflows, and paginated reports continue to use ODBC unless you edit the connection or set Implementation="2.0". Refresh behavior does not change for artifacts you don’t touch. This is your validation window.&nbsp;
Event 2 - Cutover: ODBC is disabled in the service. Every connection — new and existing — uses ADBC from this point on. The tenant setting no longer applies. There is no per-tenant opt-out.&nbsp;
The window between those two events is where customers need to start validating. Please use this gap to validate your production reports.&nbsp;
Key dates by connector&nbsp;
For Snowflake, Google BigQuery, Spark / HDInsight Spark, Databricks / Azure Databricks, and Dremio the default switch will occur in Fall 2026 and ODBC disabled in service will occur in Early 2027. Dates may adjust as engineering learns from each connector — the authoritative source is Transition from ODBC to ADBC drivers in Power BI and Microsoft Fabric on Microsoft Learn.&nbsp;
You do not need to wait for the default flip on your tenant. The ADBC drivers are already available, and you can opt in per-connection to validate specific datasets while everything else keeps running on ODBC.&nbsp;
You can start validating in one of three ways, ordered by how much control you want:&nbsp;
Opt in a single connection using Implementation = “2.0”
Refresh the dataset and compare the results against your production baseline. To roll back a specific report to ODBC while you validate, remove Implementation="2.0" from the M expression and make sure the workspace ADBC setting is off. Don't hardcode Implementation="1.0" as a fallback — it will fail once ODBC is disabled in the service.&nbsp;
&nbsp;Override the setting for a validation workspace
If you want to validate several reports together, the workspace override is designed for this: a workspace admin can turn ADBC on for a single workspace and leave the rest of the tenant untouched. Publish a copy of the reports you want to test into that workspace, run the same refresh schedule, and compare.&nbsp;
Enable the tenant setting when you’re ready
When your validation is complete, the tenant admin can enable Users can connect to data sources by using Apache Arrow database connectivity (ADBC) in the Admin portal to move the entire tenant to ADBC ahead of the default flip. This is the recommended state for the validation window because it aligns Desktop and the service on the same driver.&nbsp;
What to expect during validation&nbsp;
The ADBC drivers are designed as compatibility-preserving replacements for the embedded ODBC drivers. Test your key reports in Desktop before enabling ADBC at the tenant level. For the current list of known behavioral differences per connector, see the Transition from ODBC to ADBC drivers resource on Microsoft Learn. If you hit a regression, email adbcmigration@microsoft.com with the tenant ID, connector, and repro.&nbsp;
Recommended migration checklist&nbsp;

Pick a pilot workspace and enable ADBC there first using the workspace override to validate key datasets and refresh scenarios.&nbsp;
For any critical connections you want to validate immediately, opt in per-connection with Implementation="2.0".&nbsp;
Once validation is complete, enable ADBC by default at the tenant level.&nbsp;
If you find a genuine regression, share it with us — see the next section.&nbsp;

We want your feedback before the cutover&nbsp;
If you find behavior that looks like a bug, a refresh that fails, a total that changed unexpectedly, a performance regression, please tell us early. Send the tenant ID, the connector, the M expression, and what you observed to&nbsp;adbcmigration@microsoft.com.&nbsp;
Get started

Read the full Learn documentation: Transition from ODBC to ADBC drivers in Power BI and Microsoft Fabric.&nbsp;
Install the latest Power BI Desktop.&nbsp;
Select one production report on an in-scope connector and add Implementation="2.0" to its source step.
Compare row counts, column data types, and key totals against your production baseline.&nbsp;
Please report anything unexpected to adbcmigration@microsoft.com.&nbsp;

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Start-validating-today-The-ODBC-to-ADBC-driver-transition-in/ba-p/5362917](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Start-validating-today-The-ODBC-to-ADBC-driver-transition-in/ba-p/5362917)*
