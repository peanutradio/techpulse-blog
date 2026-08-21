---
categories:
- MS
- GitHub
date: '2026-08-20T14:28:41+00:00'
description: GitHub Code Quality now writes an audit log event whenever someone enables,
  disables, or changes its settings on a repository. Three new events give you that
  hi
draft: false
original_url: https://github.blog/changelog/2026-08-20-track-github-code-quality-enablement-changes-in-the-audit-log
source: GitHub Changelog
tags:
- Improvement
- application security
- platform governance
title: Track GitHub Code Quality enablement changes in the audit log
---

GitHub Code Quality now writes an audit log event whenever someone enables, disables, or changes its settings on a repository. Three new events give you that history:

repo.code_quality_enabled records when someone turns on Code Quality for a repository.
repo.code_quality_disabled records when someone turns it off.
repo.code_quality_updated records when someone changes the configuration of a repository that already has Code Quality enabled.

Each event captures the repository, the actor who made the change, and when it happened. Because Code Quality billing counts active committers on enabled repositories, you can now see exactly when a repository entered or left that scope.
You&rsquo;ll find these events in your organization audit log and enterprise audit log, and you can query them with the audit log API.
Code Quality is available on GitHub Enterprise Cloud, GitHub Enterprise Cloud with data residency, and GitHub Team. To review or change where you&rsquo;ve turned it on, see enabling GitHub Code Quality.
Join the discussion within GitHub Community.

The post Track GitHub Code Quality enablement changes in the audit log appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-20-track-github-code-quality-enablement-changes-in-the-audit-log](https://github.blog/changelog/2026-08-20-track-github-code-quality-enablement-changes-in-the-audit-log)*
