---
categories:
- MS
- Fabric
date: '2026-08-24T21:00:00+00:00'
description: Microsoft OneLake, the single, unified, logical lake for all of your
  organization’s data, can now work with more data sources! We’re excited to announce
  the new
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Bring-your-Google-Lakehouse-Runtime-Catalog-data-to-OneLake/ba-p/5360492
source: Microsoft Fabric Blog
tags: []
title: Bring your Google Lakehouse Runtime Catalog data to OneLake (Preview)
---

Microsoft OneLake, the single, unified, logical lake for all of your organization’s data, can now work with more data sources! We’re excited to announce the new Mirrored Google Lakehouse runtime catalog item, now available in&nbsp;preview. Similar to our existing catalog mirroring sources like Mirrored AWS Glue catalog and Mirrored Azure Monitor, this new capability allows organizations using Fabric to connect Google-managed Apache Iceberg catalogs to Fabric and make those tables available throughout the Fabric experience, all without moving or duplicating underlying data. The catalog’s data remains in its existing cloud storage location, while the tables appear in OneLake for visibility and usability across Fabric experiences.
Bring more of your multi-cloud data estate into OneLake
Many organizations operate lakehouse environments across multiple clouds. While Microsoft Fabric provides a unified analytics platform, external data can remain cataloged and governed in external environments. Google Lakehouse runtime catalog can be used to organize and manage Apache Iceberg tables in Google Cloud Platform solutions.
Historically, bringing externally cataloged data into Fabric required custom ingestion pipelines, data duplication, and ongoing synchronization processes. Mirroring provides a simpler approach. Fabric connects directly to the external catalog, reflects the metadata into OneLake, and makes supported tables available across Fabric experiences without requiring data copies or ETL pipelines. This helps organizations reduce operational overhead while preserving existing investments in their data architecture.
Bring Google-cataloged Iceberg tables into Fabric
Mirrored Google Lakehouse runtime catalog is designed for organizations that use Google Lakehouse runtime catalog to organize Apache Iceberg tables.
Users connect Fabric to their external catalog, select supported Iceberg tables, and make those tables available in OneLake experiences. Once connected, Fabric continuously synchronizes catalog metadata so that users can discover and analyze data using familiar Fabric workloads.
Take a look at our documentation to get started right away!
The experience is intended to enable:

Unified discovery of Google-cataloged Iceberg tables alongside other OneLake data.
Faster onboarding of existing multi-cloud lakehouse data into Fabric analytics experiences.
Cross-cloud analytics without building complex ingestion pipelines.
Simplified access to Apache Iceberg data from Power BI, Data Engineering, Data Warehousing, Data Science, and Real-Time Intelligence experiences.
A single governance and discovery experience across data that spans cloud providers.

Zero-copy access to your existing data
A key benefit of Mirrored Google Lakehouse runtime catalog is that data remains where it already lives – data mirroring or duplication isn’t taking place.
Fabric mirrors the catalog metadata and creates the necessary OneLake shortcuts to access supported Iceberg tables. Organizations can continue using their existing storage architecture while enabling Fabric workloads to discover and analyze the same data.
This approach can help you:

Avoid duplicate storage costs.
Eliminate unnecessary ingestion pipelines.
Maintain existing governance and ownership models.
Accelerate analytics adoption without migrating large datasets.
Enable interoperability across open table formats and engines.

Analytics experiences built into Fabric
When you create a mirrored Google Lakehouse runtime catalog item, Fabric automatically creates the experiences needed to work with your cataloged data.
With this feature, many cross-workload Fabric capabilities are available to you, including:

Query through SQL: A built-in SQL analytics endpoint provides a familiar analytical interface for exploring and querying mirrored tables.
Build Power BI reports: Create Power BI reports directly on mirrored catalog data to deliver business insights without first moving information into Fabric-managed storage.
Use Data Engineering and Data Science workloads: Spark notebooks, machine learning workflows, and data preparation experiences can all leverage mirrored Iceberg tables.
Enable cross-domain analytics: Combine Google-cataloged data with data already available in OneLake, including data from Fabric lakehouses, warehouses, mirrored databases, and external sources.

Open formats, open architectures
Microsoft OneLake’s strategy is built around openness and interoperability. Mirrored Google Lakehouse runtime catalog extends that strategy by helping organizations work with more of their existing Apache Iceberg investments regardless of where those tables are stored or cataloged.
By reducing data movement and enabling direct access through a common metadata experience, Fabric helps organizations focus on delivering insights rather than maintaining integrations.
Get started today
Mirrored Google Lakehouse runtime catalog is available today in&nbsp;preview. Take a look at our documentation to get started right away.
Simply create a Mirrored Google Lakehouse runtime catalog item, connect to your external catalog, and start discovering and using your Iceberg tables directly within OneLake.
We're excited to have our customers use this feature to simplify multi-cloud analytics and bring more of their data estate together with Microsoft Fabric, without any data duplication or migration required.
We want your feedback!
Try the preview today! Share your feedback through the&nbsp;Fabric Ideas site&nbsp;and&nbsp;Microsoft Fabric Community!

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Bring-your-Google-Lakehouse-Runtime-Catalog-data-to-OneLake/ba-p/5360492](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Bring-your-Google-Lakehouse-Runtime-Catalog-data-to-OneLake/ba-p/5360492)*
