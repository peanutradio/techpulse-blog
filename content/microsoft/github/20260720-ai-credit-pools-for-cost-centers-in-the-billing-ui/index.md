---
categories:
- MS
- GitHub
date: '2026-07-20T18:24:14+00:00'
description: You can now manage a cost center&rsquo;s AI credit pool directly in the
  billing UI where you create and edit cost centers. Previously, you could only manage
  thi
draft: false
original_url: https://github.blog/changelog/2026-07-20-ai-credit-pools-for-cost-centers-in-the-billing-ui
source: GitHub Changelog
tags:
- Improvement
- copilot
- enterprise management tools
title: AI credit pools for cost centers in the billing UI
---

You can now manage a cost center&rsquo;s AI credit pool directly in the billing UI where you create and edit cost centers. Previously, you could only manage this through the GitHub REST API. This feature is available for Copilot Business and Copilot Enterprise on GitHub Enterprise Cloud.
Toggle the AI credit pool on when you create or edit a cost center. GitHub automatically calculates the pool limit from the assigned licenses and adjusts it as you add or remove them. Because of this, you never set a number yourself. You can also choose what happens at the limit: block further included usage or let it continue as additional spend if your enterprise allows overages.
An AI credit pool keeps a cost center from using more included AI credits than its own Copilot licenses fund, so each group stays within what it paid for. It&rsquo;s separate from a cost center budget, which caps metered charges after the pool is exhausted. You can set both on the same cost center.
To learn more, see Control GitHub costs at scale.

The post AI credit pools for cost centers in the billing UI appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-20-ai-credit-pools-for-cost-centers-in-the-billing-ui](https://github.blog/changelog/2026-07-20-ai-credit-pools-for-cost-centers-in-the-billing-ui)*
