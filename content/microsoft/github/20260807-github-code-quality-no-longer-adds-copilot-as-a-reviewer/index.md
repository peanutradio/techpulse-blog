---
categories:
- MS
- GitHub
date: '2026-08-07T15:07:25+00:00'
description: Enabling GitHub Code Quality on a repository no longer creates a ruleset
  that automatically requests a code review from GitHub Copilot on your pull requests.
  In
draft: false
original_url: https://github.blog/changelog/2026-08-07-github-code-quality-no-longer-adds-copilot-as-a-reviewer
source: GitHub Changelog
tags:
- Retired
- application security
- copilot
title: GitHub Code Quality no longer adds Copilot as a reviewer
---

Enabling GitHub Code Quality on a repository no longer creates a ruleset that automatically requests a code review from GitHub Copilot on your pull requests. In repositories that already have that ruleset, we&rsquo;ve turned off the settings we enabled.
When Code Quality became generally available on July 20, 2026, enabling it created a repository ruleset named Code Quality Copilot review for default branch that targeted your default branch. You told us that adding a reviewer should be your choice, so we&rsquo;ve reversed that.
What we&rsquo;ve turned off
We&rsquo;ve disabled the three settings we enabled in that ruleset:

Automatically request Copilot code review, which requested a Copilot review on every pull request.
Review new pushes, which requested another review each time you pushed to a pull request.
Review draft pull requests, which requested a review before you marked a pull request ready.

We only change the ruleset where it still matches what we created. If you&rsquo;ve edited it, we leave it as you set it, and we never touch a ruleset you wrote yourself. The ruleset stays in your repository with these settings off, so you can delete it whenever you want.
How to keep automatic Copilot code review
Copilot code review itself hasn&rsquo;t changed, and you can turn it back on at any time. Add or edit a ruleset that enables Automatically request Copilot code review for the branches you choose, at either the repository or organization level. For the steps, see configuring automatic code review by Copilot. Copilot code review continues to bill to your Copilot plan.
This change applies to Code Quality on GitHub Enterprise Cloud and GitHub Team.

The post GitHub Code Quality no longer adds Copilot as a reviewer appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-07-github-code-quality-no-longer-adds-copilot-as-a-reviewer](https://github.blog/changelog/2026-08-07-github-code-quality-no-longer-adds-copilot-as-a-reviewer)*
