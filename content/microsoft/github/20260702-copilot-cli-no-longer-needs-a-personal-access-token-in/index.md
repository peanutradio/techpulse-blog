---
categories:
- MS
- GitHub
date: '2026-07-02T20:25:32+00:00'
description: 'You can now run GitHub Copilot CLI in GitHub Actions using the built-in
  GITHUB_TOKEN.

  This means that you no longer need to create and store a personal access t'
draft: false
original_url: https://github.blog/changelog/2026-07-02-copilot-cli-no-longer-needs-a-personal-access-token-in-github-actions
source: GitHub Changelog
tags:
- Release
- copilot
title: Copilot CLI no longer needs a personal access token in GitHub Actions
---

You can now run GitHub Copilot CLI in GitHub Actions using the built-in GITHUB_TOKEN.
This means that you no longer need to create and store a personal access token (PAT), eliminating the operational and security risks of managing long-lived PATs for automations at scale.
When you run Copilot CLI with the Actions token in an organization-owned repository, AI credits consumed by the CLI are billed directly to the organization.
Configuring organization billing for Copilot CLI in GitHub Actions
In order to use this feature, you must enable the &ldquo;Allow use of Copilot CLI billed to the organization&rdquo; Copilot policy. This is enabled by default if you have the existing &ldquo;Copilot CLI&rdquo; policy enabled.
Once enabled, workflows just need the copilot-requests: write permission and can authenticate with the workflow&rsquo;s built-in GITHUB_TOKEN. No additional secrets are required.
To learn more, see &ldquo;Using Copilot CLI in GitHub Actions with GITHUB_TOKEN&rdquo; in the GitHub Docs.

  Note: You must be on a recent version of Copilot CLI. Update with copilot update, or reinstall the latest version with npm install -g @github/copilot.

Controlling cost while billing to your organization
User-level budgets are not considered when billing directly to the organization because the cost is not attributed to a user. There are multiple ways to manage spend when using this billing method:

Configure cost centers for the relevant organizations. Cost centers allow cost attribution to groups of organizations, and budgets can be applied to cost centers.
Monitor Copilot usage from your organization&rsquo;s billing and usage dashboards to track consumption over time.
Set a session limit to configure a maximum amount of AI credits that will be used by a workflow.


The post Copilot CLI no longer needs a personal access token in GitHub Actions appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-02-copilot-cli-no-longer-needs-a-personal-access-token-in-github-actions](https://github.blog/changelog/2026-07-02-copilot-cli-no-longer-needs-a-personal-access-token-in-github-actions)*
