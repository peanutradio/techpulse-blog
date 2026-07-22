---
categories:
- MS
- GitHub
date: '2026-07-20T13:01:24+00:00'
description: 'GitHub Code Quality is now generally available on GitHub Enterprise
  Cloud and GitHub Team. It solves an emerging challenge for software development:
  AI accelera'
draft: false
original_url: https://github.blog/changelog/2026-07-20-github-code-quality-is-now-generally-available
source: GitHub Changelog
tags:
- Release
- application security
title: GitHub Code Quality is now generally available
---

GitHub Code Quality is now generally available on GitHub Enterprise Cloud and GitHub Team. It solves an emerging challenge for software development: AI accelerates code output, and Code Quality helps teams ship code they trust. Code Quality pairs CodeQL&rsquo;s deterministic analysis with AI-assisted detection to catch maintainability and reliability issues in your pull requests, and Copilot Autofix suggests fixes for you to review before you merge. In GitHub&rsquo;s own engineering organization, teams resolve 67.3% of Code Quality findings before merging pull requests.

New at general availability
Since the public preview, Code Quality has added:

Organization-wide enablement, with org-level dashboards that show maintainability and reliability scores across your repositories.
Code coverage metrics rendered from your existing test reports (in Cobertura XML format) directly on pull requests.
Quality gates through GitHub rulesets, including coverage thresholds, with an evaluate mode for gradual rollout.
APIs to manage repository enablement and fetch findings.

What it costs
Code Quality is a standalone paid product, complementary to GitHub Advanced Security rather than bundled with it. It isn&rsquo;t available on GitHub Enterprise Server at launch. You pay a predictable base license plus metered usage:

$10 per active committer, per month. A committer is considered active when they&rsquo;ve pushed a commit to a repository with Code Quality enabled in the last 90 days. Each active committer is counted only once across your organization no matter how many repositories they contribute to. Bot accounts are not charged.
Usage-based billing for AI-powered work, including AI-assisted detection and Copilot Autofix. You don&rsquo;t need a GitHub Copilot subscription to use them.
Compute costs for running deterministic CodeQL analysis on GitHub Actions. GitHub-hosted and self-hosted runners are supported.

Billing begins automatically at general availability (July 20, 2026).
If you used Code Quality in public preview
More than 10,000 enterprises ran Code Quality during the public preview. If you&rsquo;re one of them, there&rsquo;s nothing to migrate or reconfigure. Code Quality keeps running on the GitHub agreement you already have, now as a paid product. With billing in effect, review where you have Code Quality enabled. To stop future scans and charges, disable Code Quality for a repository or organization.
To learn more, see About GitHub Code Quality and GitHub Code Quality billing. To learn how to enable or disable it, see Enabling GitHub Code Quality.
Join the discussion within GitHub Community.

The post GitHub Code Quality is now generally available appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-20-github-code-quality-is-now-generally-available](https://github.blog/changelog/2026-07-20-github-code-quality-is-now-generally-available)*
