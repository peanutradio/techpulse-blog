---
categories:
- MS
- Fabric
date: '2026-08-26T16:00:00+00:00'
description: IDENTITY columns in Fabric Data Warehouse are now generally available.
  Since preview, thousands of customers have adopted IDENTITY columns to simplify
  data ware
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/IDENTITY-columns-support-for-Identity-Insert-Generally-Available/ba-p/5361138
source: Microsoft Fabric Blog
tags: []
title: IDENTITY columns support for Identity Insert (Generally Available)
---

IDENTITY columns in Fabric Data Warehouse are now generally available. Since preview, thousands of customers have adopted IDENTITY columns to simplify data warehouse design, reduce custom key-generation logic, and make it easier to build scalable dimensional models in Microsoft Fabric.
IDENTITY columns automatically generate unique numeric values when rows are inserted into a table. This is especially useful for surrogate keys in dimension tables, fact-to-dimension relationships, migration scenarios, and any workload where stable system-generated identifiers are needed. Instead of maintaining custom logic such as MAX(ID) + 1, ROW_NUMBER(), control tables, hashes, or application-side scripts, data teams can let the Warehouse engine generate unique values during ingestion.
What’s new
We have expanded IDENTITY column capabilities to support more enterprise data warehousing and migration patterns. The key additions are:

IDENTITY_INSERT support, so customers can explicitly insert values into an IDENTITY column when needed, such as when migrating existing warehouse tables or preserving existing surrogate key values.
Reseed operations, so customers can reset or advance the next generated identity value after data movement, backfill, reload, or administrative operations.
Production-ready support for the core IDENTITY column experience, enabling automatic, system-managed surrogate key generation in Fabric Data Warehouse.

Figure: Managing identity values and reseeding a table for future inserts.
These additions are particularly important for customers migrating from existing SQL-based warehouses, where preserving identity values can be necessary to maintain relationships across dimension and fact tables. They also help simplify operational workflows where teams need more control over identity value generation after loading historical data or performing controlled backfills.
Why IDENTITY columns matter
Surrogate keys are foundational in data warehousing because they provide stable identifiers that are independent of source-system business keys. IDENTITY columns make surrogate key creation simpler and more reliable by generating unique values automatically as data is inserted. This reduces ETL complexity, avoids duplicate-key risks from concurrent loads, and removes repetitive key-generation logic from pipelines and scripts.
Fabric Data Warehouse is designed for distributed, parallel execution. IDENTITY values are generated in a way that supports scale-out ingestion, which means values are guaranteed to be unique but are not guaranteed to be sequential or gap-free. This behavior is expected and enables the Warehouse engine to preserve high-throughput load performance while still providing system-managed unique identifiers.
Get started
You can use IDENTITY columns in Fabric Data Warehouse by defining a BIGINT IDENTITY column in your table schema and omitting that column from inserts when you want Fabric to generate values automatically. For migration or controlled load scenarios, use IDENTITY_INSERT to preserve existing values, then reseed the table when needed so new values continue from the expected point.
To learn more, explore the tutorial and the documentation.
Thank you to the thousands of customers who adopted IDENTITY columns during preview and shared feedback with us. Your input helped shape the general availability release and the additional capabilities we are introducing today.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/IDENTITY-columns-support-for-Identity-Insert-Generally-Available/ba-p/5361138](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/IDENTITY-columns-support-for-Identity-Insert-Generally-Available/ba-p/5361138)*
