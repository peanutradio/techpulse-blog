---
categories:
- MS
- GitHub
date: '2026-07-28T11:57:19+00:00'
description: 'Recent supply chain attacks use compromised GitHub credentials to push
  malicious GitHub Actions workflows that steal CI/CD credentials and carry out additional '
draft: false
original_url: https://github.blog/changelog/2026-07-28-github-actions-holds-potentially-malicious-workflows-for-approval
source: GitHub Changelog
tags:
- Improvement
- actions
- supply chain security
title: GitHub Actions holds potentially malicious workflows for approval
---

Recent supply chain attacks use compromised GitHub credentials to push malicious GitHub Actions workflows that steal CI/CD credentials and carry out additional attacks. To help protect public repositories from these attacks, GitHub Actions now holds certain workflow runs for approval before they start.
When a workflow run is identified as potentially malicious and held, the workflow won&rsquo;t execute until a repository collaborator with write access reviews and approves it. The approval must be submitted through an authenticated web session. Once approved, the workflow continues normally.
You don&rsquo;t need to configure this protection; GitHub applies it automatically.
This protection currently applies to public repositories on github.com only. GitHub Enterprise Server doesn&rsquo;t add this protection at this time.

The post GitHub Actions holds potentially malicious workflows for approval appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-28-github-actions-holds-potentially-malicious-workflows-for-approval](https://github.blog/changelog/2026-07-28-github-actions-holds-potentially-malicious-workflows-for-approval)*
