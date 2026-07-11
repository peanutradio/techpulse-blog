---
categories:
- MS
- GitHub
date: '2026-07-10T15:07:23+00:00'
description: 'You can now retrieve every user&rsquo;s progress against a multi-user
  budget from a single REST API endpoint. This makes it much faster to find who is
  close to '
draft: false
original_url: https://github.blog/changelog/2026-07-10-per-user-states-for-multi-user-budgets-in-the-rest-api
source: GitHub Changelog
tags:
- Release
- account management
- enterprise management tools
title: Per-user states for multi-user budgets in the REST API
---

You can now retrieve every user&rsquo;s progress against a multi-user budget from a single REST API endpoint. This makes it much faster to find who is close to their limit across a large enterprise.
Previously, you had to make one API call per user to check per-user consumption, and you couldn&rsquo;t filter by how much of the budget someone had used. Reviewing spend across a large budget was slow and pushed teams to build their own scripts. Now you can page through all users from one endpoint and go straight to the people who need attention.
With this endpoint, you can:

Get how much each user has consumed and their allocated limit for a multi-user budget, one page at a time.
Filter by how much of the budget each user has used, so you can pull everyone at or above a certain percentage of their limit in one request.
Filter to a single user or sort the results by how much of the budget each person has consumed.
See when a user has an individual budget override that changes the limit applied to them.

This works for both types of multi-user budget: a universal budget that applies to every user in your enterprise and a per-user budget scoped to a cost center.
Who can use this
Enterprise owners and billing managers on GitHub Enterprise Cloud can call this endpoint for any multi-user budget in their enterprise.
To learn more, see Get user states for a multi-user budget in the REST API documentation.

The post Per-user states for multi-user budgets in the REST API appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-10-per-user-states-for-multi-user-budgets-in-the-rest-api](https://github.blog/changelog/2026-07-10-per-user-states-for-multi-user-budgets-in-the-rest-api)*
