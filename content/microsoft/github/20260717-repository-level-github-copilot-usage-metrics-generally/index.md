---
categories:
- MS
- GitHub
date: '2026-07-17T22:05:18+00:00'
description: The Copilot usage metrics REST API now reports repository-level activity.
  Two new endpoints return a daily, per-repository breakdown of pull request activity
  fo
draft: false
original_url: https://github.blog/changelog/2026-07-17-repository-level-github-copilot-usage-metrics-generally-available
source: GitHub Changelog
tags:
- Release
- account management
- copilot
- enterprise management tools
title: Repository-level GitHub Copilot usage metrics generally available
---

The Copilot usage metrics REST API now reports repository-level activity. Two new endpoints return a daily, per-repository breakdown of pull request activity for Copilot coding agent and Copilot code review. They do this for both enterprise and organization reports.
What&rsquo;s new
Two new endpoints return a per-repository report for a single day:

GET /enterprises/{enterprise}/copilot/metrics/reports/repos-1-day?day=YYYY-MM-DD
GET /orgs/{org}/copilot/metrics/reports/repos-1-day?day=YYYY-MM-DD

Each response returns the following activity:

Pull requests created and merged by Copilot coding agent.
Pull requests reviewed by Copilot code review, with suggestion counts broken down by comment type.

Why this matters
Until now, Copilot usage metrics stopped at the organization and user level. Repository-level reporting lets you see exactly where Copilot coding agent and Copilot code review are driving pull request activity across your codebase. This is the foundation for repository insights and AI-readiness reporting, so you can target enablement at the repositories that stand to benefit most.
Important notes
Enterprise owners and billing managers, organization owners, and anyone with a custom organization or enterprise role that grants the View Copilot Metrics permission can access these reports. The Copilot usage metrics policy must be enabled to support this functionality.
Visit the Copilot usage metrics API documentation to get started.

The post Repository-level GitHub Copilot usage metrics generally available appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-17-repository-level-github-copilot-usage-metrics-generally-available](https://github.blog/changelog/2026-07-17-repository-level-github-copilot-usage-metrics-generally-available)*
