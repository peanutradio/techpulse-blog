---
categories:
- MS
- Fabric
date: '2026-08-19T20:25:31+00:00'
description: Enterprise Planning is cyclical, and Fabric Planning is built for that
  reality. Instead of per-user licenses or a separate subscription, Fabric Plan uses
  an act
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Billing-personas-for-PowerTable-within-Planning-in-Fabric-IQ/ba-p/5360168
source: Microsoft Fabric Blog
tags: []
title: Billing personas for PowerTable within Planning in Fabric IQ (Generally Available)
---

Enterprise Planning is cyclical, and Fabric Planning is built for that reality. Instead of per-user licenses or a separate subscription, Fabric Plan uses an activity-based, session-driven model that bills only against your Fabric capacity. You avoid license commitments and administration; seasonal and ad-hoc contributors can participate without inflating annual costs; and spend is eligible for Microsoft Azure Consumption Commitment (MACC) credits where applicable. Unused capacity remains available to other Fabric workloads.&nbsp;
Now, that model has been refined for customers who come to Fabric Plan for PowerTable, our data-management capability and for Intelligence.&nbsp;
You can now provision Fabric Plan with no Planning sheets configured. In this configuration the item is oriented towards PowerTable and Intelligence data-management use, and all editors bill at the Stakeholder tier, alongside read-only Viewers.&nbsp;
What's changing&nbsp;
You can now provision Fabric Plan with no Planning sheets configured. In this configuration the item is oriented towards PowerTable and Intelligence data-management use, and all editors bill at the Stakeholder tier, alongside read-only Viewers.&nbsp;

Figure: Three user personas with access permissions.&nbsp;
The moment a Planning sheet is created and accessed, the session upgrades to the Planner tier, with notification to the user before the higher-tier session begins, so there are no billing surprises.&nbsp;
This is not a new pricing model. There is no new meter, no new rate, and no new SKU. The same billing meters and 30-day session mechanics you use today continue to apply.&nbsp;
How roles and metering work&nbsp;
With this update, PowerTable and Intelligence expose two personas: Stakeholder as editor and Viewer as read-only consumer. Planning sheets retain all three. Editing only PowerTable or Intelligence content makes you a Stakeholder; only editing a Planning sheet makes you a Planner.

Figure: PowerTable and Intelligence user personas and the capabilities granted at each level&nbsp;
A user's role is assigned dynamically by what they do in a session. Everyone starts as a Viewer, writing back data promotes them to Stakeholder, and authoring or modeling a Planning sheet promotes them to Planner. Each role meters as its own 30-day session type, consumption is attributed by role in the Capacity Metrics app, and a user holding multiple roles in the same capacity is billed once, at their highest-value role.&nbsp;
Each role meters at a different rate to align costs with business value:
RoleWhat they do
Metered consumption (30-day session)
Planner
Author, modeler, and admin. Builds data models, configures Planning logic, authors artifacts, full admin permissions
847 CUStakeholder
Contributes to the Plan. Data entry, approvals, collaboration; authors PowerTable and Intelligence content
168 CUViewer
Consumer with read-only access
37 CU
Consumption of other Fabric resources, including Fabric SQL, OneLake, the Power BI XMLA API, and other native Fabric items, meters separately, and we recommend keeping roughly a 30% buffer. Job-based billing continues to apply to Connected Planning and PowerTable Automation.
What this looks like in practice
PowerTable-only data management: You adopt Fabric Plan primarily for PowerTable and provision with no Planning sheets. Editors are billed as Stakeholders and consumers as Viewers, so you get full data-management value without paying for a Planner session.&nbsp;
Growing from PowerTable into Planning: Later, you decide to build a plan. When the first Planning sheet is created and accessed, the session upgrades to the Planner tier with clear notice, while your existing PowerTable and Viewer usage continues to meter as before.&nbsp;
Mixed usage in one capacity: Within a single capacity, some users only enter data in PowerTable and bill as Stakeholders, some only read and bill as Viewers, and some author Planning logic and bill as Planners. Each user is billed once at their highest-value role, with consumption attributed by role.&nbsp;
For more information on how Billing for Fabric Planning works, refer to:&nbsp;&nbsp;

Billing for Microsoft Fabric Planning (Preview) - Microsoft Fabric Community&nbsp;


Fabric Plan Billing and Pricing Model - Microsoft Fabric | Microsoft Learn

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Billing-personas-for-PowerTable-within-Planning-in-Fabric-IQ/ba-p/5360168](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Billing-personas-for-PowerTable-within-Planning-in-Fabric-IQ/ba-p/5360168)*
