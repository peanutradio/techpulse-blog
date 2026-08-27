---
categories:
- MS
- GitHub
date: '2026-08-26T12:18:59+00:00'
description: Enterprise owners can now grant a GitHub App access to enterprise billing
  data. When you create or configure a GitHub App, you can select the enterprise billing
draft: false
original_url: https://github.blog/changelog/2026-08-26-github-apps-can-now-access-enterprise-billing-data
source: GitHub Changelog
tags:
- Release
- account management
- ecosystem &amp; accessibility
- enterprise management tools
title: GitHub Apps can now access enterprise billing data
---

Enterprise owners can now grant a GitHub App access to enterprise billing data. When you create or configure a GitHub App, you can select the enterprise billing permission and choose one of two levels: read or read and write.
Previously, the only way to read usage or manage budgets and cost centers through the API was a personal access token belonging to an enterprise owner or billing manager. That tied your billing automation to one person&rsquo;s token, so it broke when that person changed roles or left.
Now an app installation access token that carries the enterprise billing permission can call the enterprise billing REST API endpoints. Use it to pull usage data into your finance and BI systems, reconcile invoices, and manage budgets and cost centers, all without relying on an individual&rsquo;s token. As an added benefit, you also get higher rate limits than a personal access token.
The enterprise billing permission is available on GitHub Enterprise Cloud. To learn more, see Choosing permissions for a GitHub App.

The post GitHub Apps can now access enterprise billing data appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-26-github-apps-can-now-access-enterprise-billing-data](https://github.blog/changelog/2026-08-26-github-apps-can-now-access-enterprise-billing-data)*
