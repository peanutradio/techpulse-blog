---
categories:
- MS
- GitHub
date: '2026-07-14T19:12:48+00:00'
description: GitHub code scanning now surfaces AI-powered security detections directly
  on pull requests, expanding vulnerability coverage to languages and frameworks not
  cur
draft: false
original_url: https://github.blog/changelog/2026-07-14-code-scanning-shows-ai-security-detections-on-pull-requests
source: GitHub Changelog
tags:
- Release
- application security
title: Code scanning shows AI security detections on pull requests
---

GitHub code scanning now surfaces AI-powered security detections directly on pull requests, expanding vulnerability coverage to languages and frameworks not currently supported by CodeQL. These detections help teams identify and address potential issues in parts of the codebase that previously had no native scanning coverage, all before code is merged.
What&rsquo;s included

Broader language coverage: AI-powered detections extend code scanning to languages and frameworks beyond those supported by CodeQL&rsquo;s built-in analysis, reducing blind spots across your codebase.
Native pull request integration: Findings appear directly in pull requests, so developers can review and address issues as part of their existing workflow before merging code. Alerts generated using AI will be labeled with AI so you can easily distinguish them from CodeQL results.
Easy to enable: Once allowed at the enterprise level, you can enable AI security detections for any repository or organization that have GitHub code security and CodeQL default setup turned on.

How it works
AI security detections are powered by GitHub&rsquo;s AI detection engine and automatically run when a pull request is opened or updated. Results appear as the engine returns them. There&rsquo;s no need to wait for all analysis sources to complete. These findings are informational and won&rsquo;t block pull request merges.
To use the feature, AI security detections must be allowed by your Enterprise policy and enabled at the organization level. Additionally, CodeQL default analysis must be enabled on the repository. While CodeQL is not the tool performing the AI analysis, the AI detection engine relies on it to function.
Availability
This feature is now in public preview on github.com for customers with GitHub Code Security (GitHub Advanced Security). It can be enabled for repositories or organizations that use CodeQL default setup, after being allowed by an enterprise owner at the enterprise level.
Billing
During public preview, AI security detections require a GitHub Copilot license and draw down your organization&rsquo;s AI credits. Usage consumes AI credits only when detections run. Learn more about AI credits billing.
To learn more, check out our GitHub Code Scanning documentation. Join the discussion and leave feedback on the Code scanning shows AI security detections on PRs announcement in the GitHub Community.

The post Code scanning shows AI security detections on pull requests appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-14-code-scanning-shows-ai-security-detections-on-pull-requests](https://github.blog/changelog/2026-07-14-code-scanning-shows-ai-security-detections-on-pull-requests)*
