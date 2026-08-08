---
categories:
- MS
- GitHub
date: '2026-08-07T18:20:47+00:00'
description: Since agent apps arrived on GitHub, teams have been able to run agents
  from partners like Claude and Codex directly in their GitHub workflows. The Copilot
  usage
draft: false
original_url: https://github.blog/changelog/2026-08-07-copilot-usage-metrics-api-adds-agent-app-activity
source: GitHub Changelog
tags:
- Improvement
- account management
- copilot
- enterprise management tools
title: Copilot usage metrics API adds agent app activity
---

Since agent apps arrived on GitHub, teams have been able to run agents from partners like Claude and Codex directly in their GitHub workflows. The Copilot usage metrics API now reports that activity, broken out by individual agent. The usage metrics API does this in the enterprise, organization, enterprise-user, and organization-user 1-day and 28-day reports.
What&rsquo;s new
A new optional totals_by_3rd_party_agent array contains one entry per recognized agent app. Each entry includes:

agent_name: The agent&rsquo;s display name. Display names can change, so group on agent_id rather than this field.
agent_id: The agent&rsquo;s stable identifier, and the right key for joining across reporting periods.
user_initiated_interaction_count: The number of user-initiated agent app job starts.
session_count: The number of agent app sessions. Included in the aggregated enterprise and organization reports only. Per-user entries omit this field.

Why this matters
Until now, agent activity in your usage metrics was effectively a single bucket, so there was no way to tell Copilot coding agent work apart from work done through other agents. As teams adopt more than one agent, that made it hard to answer basic questions: which agents are actually being used, by how many people, and how does adoption of a newly rolled-out agent compare to the one it was meant to supplement.
Breaking activity out per agent lets you distinctly track each agent app, compare adoption across them, and ground rollout and licensing decisions in real usage rather than assumption.
Important notes

The nested user_initiated_interaction_count counts agent app job starts. It is distinct from the top-level field of the same name, which counts explicit prompts from other supported telemetry. Do not sum the two or treat them as interchangeable.
Activity is aggregated by agent, so multiple apps belonging to the same agent are collapsed into one entry. Activity from agents that cannot be identified is omitted.
These metrics are available to enterprise owners and billing managers, organization owners, and anyone with a custom organization or enterprise role that grants the View Copilot Metrics permission. The Copilot usage metrics policy must be enabled.
The change is backward compatible. Existing fields keep their current shape, and reports omit totals_by_3rd_party_agent entirely when there is no recognized agent app activity for the reporting period.

Visit the Copilot usage metrics API documentation to get started, or see Agent apps metrics fields for the full field definitions.

The post Copilot usage metrics API adds agent app activity appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-07-copilot-usage-metrics-api-adds-agent-app-activity](https://github.blog/changelog/2026-08-07-copilot-usage-metrics-api-adds-agent-app-activity)*
