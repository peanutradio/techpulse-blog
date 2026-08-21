---
categories:
- MS
- GitHub
date: '2026-08-20T14:29:27+00:00'
description: A dedicated workflow path for code quality CodeQL actions workflows is
  now generally available. Your workflow run history and your Actions usage reports
  now tel
draft: false
original_url: https://github.blog/changelog/2026-08-20-separate-github-actions-path-for-github-code-quality
source: GitHub Changelog
tags:
- Improvement
- actions
- application security
title: Separate GitHub Actions path for GitHub Code Quality
---

A dedicated workflow path for code quality CodeQL actions workflows is now generally available. Your workflow run history and your Actions usage reports now tell GitHub Code Quality runs apart from GitHub code scanning runs. Code Quality analysis runs on dynamic/github-code-quality/codeql and shows github-code-quality as the actor, instead of sharing the dynamic/github-code-scanning/codeql path and the github-advanced-security actor with code scanning.
What you need to do
Code Quality itself doesn&rsquo;t need reconfiguration, and your enabled repositories keep scanning as they are. If you&rsquo;ve built anything on the old path or actor, you need to update it:

Change Actions usage and billing reports that filter on dynamic/github-code-scanning/codeql so they also account for dynamic/github-code-quality/codeql.
Update scripts, dashboards, or workflow run filters that identify Code Quality runs by the github-advanced-security actor.

GitHub Code Quality is available on GitHub Enterprise Cloud and GitHub Team, as well as on GitHub Enterprise Cloud with data residency. To learn more, see GitHub Code Quality billing.
Join the discussion within GitHub Community.

The post Separate GitHub Actions path for GitHub Code Quality appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-20-separate-github-actions-path-for-github-code-quality](https://github.blog/changelog/2026-08-20-separate-github-actions-path-for-github-code-quality)*
