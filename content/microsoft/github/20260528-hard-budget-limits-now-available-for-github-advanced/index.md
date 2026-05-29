---
categories:
- MS
- GitHub
date: '2026-05-28T21:59:05+00:00'
description: Enterprise administrators and billing managers can now set hard budget
  limits for GitHub Advanced Security (GHAS) SKUs, preventing teams from exceeding
  their al
draft: false
original_url: https://github.blog/changelog/2026-05-28-hard-budget-limits-now-available-for-github-advanced-security
source: GitHub Changelog
tags:
- Release
- application security
title: Hard budget limits now available for GitHub Advanced Security
---

Enterprise administrators and billing managers can now set hard budget limits for GitHub Advanced Security (GHAS) SKUs, preventing teams from exceeding their allocated license budgets.
Previously, license-based products like GHAS only supported soft budgets. Admins could set a spending target and receive email notifications at 75%, 90%, and 100% thresholds, but the product did not enforce the limit. This could lead to accidental overspending, especially during user onboarding flows such as IdP group provisioning where licenses are automatically assigned.
With hard budget limits, once a GHAS budget threshold is reached, additional license usage is blocked, and GHAS won&rsquo;t be enabled on new repositories until the budget is increased or licenses are freed. This gives enterprises precise control over their security spending at the organization level.
What&rsquo;s new

Enforceable license limits for GHAS: Set a hard budget in license count and GitHub will prevent new license assignments once the limit is met.
License-to-cost transparency: When configuring a budget, a real-time estimate shows the dollar equivalent (e.g., X licenses &asymp; ~$Y/month), so admins always know their remaining capacity, even for mid-month additions.
Smart defaults for existing usage: If your enterprise already has active GHAS licenses, the new budget floor will be set to at least your current billable license count to avoid disruption.
Continued alerting: Email notifications at 75%, 90%, and 100% thresholds remain active alongside hard limits, keeping admins informed as usage approaches the cap.
Organization-level control: Enterprises can allocate license budgets scoped to a cost center and limit spending for the organizations assigned to the cost center. Organizations on the Team plan can also allocate license budgets for the organization to limit spending.

Getting started
Hard budget limits for GHAS can be configured in your enterprise billing settings under Budgets &amp; alerts. Both new and existing customers can create hard budgets. Existing soft budgets can be migrated to the new license-based format through in-product flows.
To learn more, see Preventing overspending in the GitHub documentation.
Join the discussion within GitHub Community.

The post Hard budget limits now available for GitHub Advanced Security appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-05-28-hard-budget-limits-now-available-for-github-advanced-security](https://github.blog/changelog/2026-05-28-hard-budget-limits-now-available-for-github-advanced-security)*
