---
categories:
- MS
- GitHub
date: '2026-09-03T14:04:59+00:00'
description: 'CodeQL is the static analysis engine behind GitHub code scanning, which
  finds and remediates security issues in your code. We&rsquo;ve recently released
  CodeQL '
draft: false
original_url: https://github.blog/changelog/2026-09-03-codeql-2-26-4-improves-github-actions-security-detections
source: GitHub Changelog
tags:
- Improvement
- application security
title: CodeQL 2.26.4 improves GitHub actions security detections
---

CodeQL is the static analysis engine behind GitHub code scanning, which finds and remediates security issues in your code. We&rsquo;ve recently released CodeQL 2.26.4, which adds support for Go 1.27, improves alert locations for Rust data flow queries, and includes accuracy improvements across C#, Java/Kotlin, and GitHub Actions.
Language and framework support
Go

CodeQL now supports Go 1.27.

Rust

Alert locations for data flow queries are now more precise and are based on the actual source and sink nodes. Some alerts will change location, so they&rsquo;ll appear as new alerts while the previous alerts close.

Java/Kotlin

We&rsquo;ve added SQL injection sink models for Spring R2DBC DatabaseClient and the R2DBC SPI.
Taint now propagates through calls to String.valueOf(Object) when the argument is a CharSequence (e.g., a String or a StringBuilder).

JavaScript/TypeScript

We&rsquo;ve added support for regular expressions using the d flag and for the React Native Worklets 'worklet' directive.

Python

We&rsquo;ve added taint flow through list.extend and list.insert, matching the existing taint flow through list.append.

Query changes
C#

The cs/web/missing-token-validation query now recognizes enabled ASP.NET Core RequireAntiforgeryToken attributes when antiforgery middleware is used.
The cs/virtual-call-in-constructor query no longer reports uses of virtual members in nameof expressions, since they aren&rsquo;t calls.
The cs/useless-cast-to-self and cs/simplifiable-boolean-expression queries produce fewer false positives in build-mode: none databases.

GitHub Actions

Checks on actor fields read from the event payload (e.g., github.event.pull_request.user.login) now only count as protection for events that actually populate that field. This may produce more alerts for queries that use the ControlCheck class.
The actions/unpinned-tag query now detects mutable references to reusable workflows.
You can now specify EnvironmentCheck through a models-as-data model. Queries using ControlCheck may find more results when an environment is no longer a sufficient sanitizer.

For a full list of changes, please refer to the complete changelog for version 2.26.4. Every new version of CodeQL is automatically deployed to users of GitHub code scanning on github.com. The new functionality in CodeQL 2.26.4 will also be included in a future GitHub Enterprise Server (GHES) release. If you use an older version of GHES, you can manually upgrade your CodeQL version.

The post CodeQL 2.26.4 improves GitHub actions security detections appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-09-03-codeql-2-26-4-improves-github-actions-security-detections](https://github.blog/changelog/2026-09-03-codeql-2-26-4-improves-github-actions-security-detections)*
