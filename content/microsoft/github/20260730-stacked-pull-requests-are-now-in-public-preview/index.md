---
categories:
- MS
- GitHub
date: '2026-07-30T16:14:29+00:00'
description: Stacked pull requests break large changes into small, reviewable pull
  requests. They&rsquo;re an ordered series of pull requests that each represent focused
  lay
draft: false
original_url: https://github.blog/changelog/2026-07-30-stacked-pull-requests-are-now-in-public-preview
source: GitHub Changelog
tags:
- Release
- collaboration tools
title: Stacked pull requests are now in public preview
---

Stacked pull requests break large changes into small, reviewable pull requests. They&rsquo;re an ordered series of pull requests that each represent focused layers of your change. With stacks, you can independently review and check each pull request, then merge everything together in one click. No more opening a single large pull request that takes forever to review, or splitting work across multiple branches you have to keep manually rebasing.

  &ldquo;We&rsquo;ve been using GitHub stacked PRs for Next.js for the past few months. It has helped us introduce smaller individual changes while shipping larger features, making it easier to review PRs. &ndash; Tim Neutkens, NextJS lead, Vercel&rdquo;



With stacked pull requests, teams can:

Keep large changes moving by reviewing short, narrowly scoped pull requests in parallel.
Maintain quality across every layer by using focused pull request reviews alongside existing branch protections to protect main.
Merge one, some, or all by landing an entire stack altogether or individual layers one at a time.

And because stacked pull requests are built into GitHub, your existing reviews, checks, and merge requirements all work out of the box.

  &ldquo;The new Github Stacked PRs preview is incredible. Landing 5 stacked PRs directly to a merge queue all at once! A+++! This removes so much friction (and the gh cli tools + agent skill help a ton)&rdquo; &ndash; John Resig, creator, jQuery

Get started with the CLI extension
Install the CLI extension and create your first stack in under a minute:
gh extension install github/gh-stack

Create stacks from your terminal or github.com
Work with stacks on github.com, the GitHub CLI, the GitHub mobile app, or with a coding agent such as GitHub Copilot using the gh-stack skill. Start with a branch and pull request for your first change. Then add branches and pull requests on top of it; each pull request targets the layer below it.


Review each layer independently
Open any pull request in the stack to review only the diff for that specific layer. Use the stack map at the top of the pull request to see how the change you&rsquo;re reviewing fits into the larger work. You and your teammates can each review different layers in parallel without blocking further work.

  &ldquo;AI has made TED&rsquo;s developers dramatically more productive, but that created a new bottleneck: PRs were growing large enough that reviewers were struggling. Stacked PRs help to solve that. By breaking large changes into small, dependency-ordered pieces, review happens in smaller logical chunks &ndash; not just faster PR reviews, but more accurate ones. Stacked PRs tighten our feedback loop and help get stable code to ted.com faster.&rdquo; &ndash; Andy Merryman, CTO, TED


Merge everything in a single click
Merge the latest ready pull request to land it and every unmerged layer below it in one single operation. To land part of a stack, merge one or more lower layers&mdash;the pull requests above it stay open and automatically rebase and retarget. Your existing branch protections and required checks still govern what reaches main.

  &ldquo;A big change used to mean one giant PR nobody wanted to review. Now it&rsquo;s a stack of small ones reviewers can actually follow, and the whole stack merges in one shot. It stopped feeling like a tool on top of GitHub and started feeling like GitHub.&rdquo; &ndash; Mayank Saini, connectivity engineer, WHOOP



Find out more and share your feedback
Stacked pull requests are rolling out in public preview to all repositories over the coming days. Merge queue support for stacked pull requests is rolling out progressively over the coming weeks.
For more information, check out the stacked pull requests documentation, and share your feedback with us in the stacks discussion.

The post Stacked pull requests are now in public preview appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-30-stacked-pull-requests-are-now-in-public-preview](https://github.blog/changelog/2026-07-30-stacked-pull-requests-are-now-in-public-preview)*
