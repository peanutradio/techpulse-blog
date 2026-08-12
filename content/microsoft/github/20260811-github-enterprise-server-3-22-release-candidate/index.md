---
categories:
- MS
- GitHub
date: '2026-08-11T20:26:26+00:00'
description: 'GitHub Enterprise Server (GHES) 3.22 is now available and introduces
  new capabilities across the platform. Here are a few highlights in the 3.22 release:


  Admin'
draft: false
original_url: https://github.blog/changelog/2026-08-11-github-enterprise-server-3-22-release-candidate
source: GitHub Changelog
tags:
- Release
- collaboration tools
title: GitHub Enterprise Server 3.22 release candidate
---

GitHub Enterprise Server (GHES) 3.22 is now available and introduces new capabilities across the platform. Here are a few highlights in the 3.22 release:

Administrators can configure Copilot CLI to work with GHES for enterprises that operate in disconnected or air-gapped environments without connectivity to GitHub Cloud. Set up a model provider once in GHES, and your end users across the enterprise can use Copilot CLI with their GHES credentials. This capability is in technical preview and is subject to change.


Enterprise Teams, previously in public preview, is now generally available. Enterprise owners can use Enterprise Teams to manage users and their access across the entire enterprise, including organizations and repositories, from a single, centralized team structure. This reduces the operational overhead of managing user access across multiple organizations within an enterprise.


Security analysts can now sort secret scanning push protection bypass requests and secret scanning alert dismissal requests by date, in ascending or descending order, using the filter bar at the repository, organization, and enterprise levels. Previously, sort order for these requests could not be adjusted, which made it difficult for teams managing high volumes of requests to prioritize their review.


Repository rulesets now support bypassing by individual users. This gives administrators more granular control over rule bypass permissions, such as adding a service account to a bypass list without needing to create a dedicated role or team.


Organization owners and repository administrators can require specific reviewers for pull requests by adding a required reviewers rule to a repository ruleset, targeting specific branches, files, and folders using pattern matching, and can set the minimum number of required reviews per team. This rule works alongside CODEOWNERS and is particularly useful when changes need sign-off from teams beyond the code owners (e.g., security, QA, or design). For example, administrators can require a data platform team review on all *.sql changes, security team members on the default branch, or product and design reviewers on feature branches.


Developers can now see release status directly in the issue sidebar. When a linked pull request has been included in a release, the sidebar displays a &ldquo;Latest release&rdquo; or &ldquo;Pre-release&rdquo; badge, making it easy to tell whether a fix has shipped without leaving the issue.


Maintainers can now see contributor role labels, such as &ldquo;First-time contributor,&rdquo; &ldquo;Contributor,&rdquo; and &ldquo;Member,&rdquo; directly in the pull request list view for public repositories. This gives maintainers at-a-glance context about each author&rsquo;s relationship to the repository without needing to open individual pull requests to check the comment box for this information.


Release candidates are a way for you to try the latest features early and help us gather feedback. Read more about the release candidate process. To learn more about GHES 3.22, check out the release notes, or download the 3.22 release candidate now.
If you have any feedback or questions about the release candidate, please contact our support team.

The post GitHub Enterprise Server 3.22 release candidate appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-11-github-enterprise-server-3-22-release-candidate](https://github.blog/changelog/2026-08-11-github-enterprise-server-3-22-release-candidate)*
