---
categories:
- MS
- GitHub
date: '2026-09-01T20:23:13+00:00'
description: You can now set an optional expiration date on an individual user budget,
  and GitHub removes the budget on that date. This capability is generally available.
  Th
draft: false
original_url: https://github.blog/changelog/2026-09-01-set-an-expiration-date-for-individual-user-budgets
source: GitHub Changelog
tags:
- Improvement
- copilot
- enterprise management tools
title: Set an expiration date for individual user budgets
---

You can now set an optional expiration date on an individual user budget, and GitHub removes the budget on that date. This capability is generally available. The user then falls back to the next budget that applies to them (i.e., their cost center per-user budget or otherwise your universal budget).
Previously, you had to remove every temporary budget override by hand, creating recurring cleanup work at enterprise scale.
When you create or edit an individual user budget, you can choose:

No expiration, which is the default and matches current behavior.
Expiration at the start of the next billing cycle.
Expiration on a specific date.


You can change or clear an expiration date at any time.
Set an expiration date in your billing settings, or use the expires_at field in the Budgets REST API when you create or update a budget.
Expiration dates are available for the GitHub Copilot Business and Copilot Enterprise plans.
Learn more about setting up budgets and the Budgets REST API.

The post Set an expiration date for individual user budgets appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-09-01-set-an-expiration-date-for-individual-user-budgets](https://github.blog/changelog/2026-09-01-set-an-expiration-date-for-individual-user-budgets)*
