---
categories:
- MS
- GitHub
date: '2026-07-31T16:45:50+00:00'
description: npm granular access tokens (GATs) configured to bypass 2FA can no longer
  perform sensitive account, org, and package management actions. These now require
  an in
draft: false
original_url: https://github.blog/changelog/2026-07-31-restricting-npm-bypass-2fa-granular-access-tokens
source: GitHub Changelog
tags:
- Retired
- supply chain security
title: Restricting npm bypass-2FA granular access tokens
---

npm granular access tokens (GATs) configured to bypass 2FA can no longer perform sensitive account, org, and package management actions. These now require an interactive 2FA challenge, closing one of the largest credential-based attack surfaces on the registry.

  This only impacts npm granular access tokens. This does not affect GitHub personal access tokens, GitHub App tokens, or GITHUB_TOKEN in Actions.

What now requires an interactive 2FA challenge

Creating or deleting tokens
Changing package access, maintainers, or trusted publishing configuration
Managing organization/team membership and package grants

An attacker could previously use a leaked 2FA-bypass token to take over an account and mint new tokens, add a maintainer, and more. A token that skips 2FA shouldn&rsquo;t also be a way to manage your account.
How to prepare
Stop using 2FA-bypass tokens for these operations and perform them interactively (web or CLI) with a 2FA challenge.
Coming next
2FA-bypass tokens will also lose direct publish. Their publishing surface will reduce to reading private packages and staging a publish, which a maintainer approves with 2FA. We are targeting January 2027 for this update. You should move automated publishing to trusted publishing (OIDC) or staged publishing.
This continues the deprecation we announced on July 8. Have questions or a workflow that would be blocked? Tell us in the community discussion.

The post Restricting npm bypass-2FA granular access tokens appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-31-restricting-npm-bypass-2fa-granular-access-tokens](https://github.blog/changelog/2026-07-31-restricting-npm-bypass-2fa-granular-access-tokens)*
