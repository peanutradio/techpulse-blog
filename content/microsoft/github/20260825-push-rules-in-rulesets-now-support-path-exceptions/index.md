---
categories:
- MS
- GitHub
date: '2026-08-25T17:40:15+00:00'
description: You can now configure push rules more granularly by exempting specific
  file paths, so a rule applies everywhere in scope except the paths you choose. Path
  excep
draft: false
original_url: https://github.blog/changelog/2026-08-25-push-rules-in-rulesets-now-support-path-exceptions
source: GitHub Changelog
tags:
- Improvement
- platform governance
title: Push rules in rulesets now support path exceptions
---

You can now configure push rules more granularly by exempting specific file paths, so a rule applies everywhere in scope except the paths you choose. Path exceptions are in public preview.
Push rules give you strong control over what enters a repository. Sometimes a rule needs a narrow exception, and until now your options were to relax it for everyone or enforce it outside of GitHub. You can now keep the rule in place and exempt only the paths that need it.
Where to find it
When you configure either supported rule, expand Allowed exceptions and add the path patterns the rule should skip. GitHub validates your patterns when you save the ruleset, so you can fix mistakes right away.
Two push rules support exceptions today:

Restrict file paths: Block a file type across the repository while allowing it at a specific path, such as blocking JAR files everywhere except **/gradle/wrapper/*.jar.
Restrict file size: Enforce a size limit while exempting paths that already exceed it, so you can adopt the limit without changing those files first.

For pattern syntax and the full list of push rules, see available rules for rulesets.
Join the community discussion to share feedback.

The post Push rules in rulesets now support path exceptions appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-25-push-rules-in-rulesets-now-support-path-exceptions](https://github.blog/changelog/2026-08-25-push-rules-in-rulesets-now-support-path-exceptions)*
