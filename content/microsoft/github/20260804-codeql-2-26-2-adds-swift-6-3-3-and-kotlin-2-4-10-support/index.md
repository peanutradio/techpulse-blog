---
categories:
- MS
- GitHub
date: '2026-08-04T10:57:39+00:00'
description: 'CodeQL is the static analysis engine behind GitHub code scanning, which
  finds and remediates security issues in your code. We&rsquo;ve recently released
  CodeQL '
draft: false
original_url: https://github.blog/changelog/2026-08-04-codeql-2-26-2-adds-swift-6-3-3-and-kotlin-2-4-10-support
source: GitHub Changelog
tags:
- Improvement
- application security
title: CodeQL 2.26.2 adds Swift 6.3.3 and Kotlin 2.4.10 support
---

CodeQL is the static analysis engine behind GitHub code scanning, which finds and remediates security issues in your code. We&rsquo;ve recently released CodeQL 2.26.2, which adds support for Swift 6.3.3 and Kotlin 2.4.10, and improves the accuracy of path injection, URL redirection, and GitHub Actions queries.
Language and framework support
Swift: CodeQL now supports analysis of apps built with Swift 6.3.3.
Java/Kotlin: CodeQL now supports Kotlin versions up to 2.4.10.
Query changes
C#

System.Web.HttpRequest.RawUrl is no longer a sanitizer for cs/web/unvalidated-url-redirection, since it contains the unnormalized request line. This may lead to more results.
We&rsquo;ve removed the cs/useless-assignment-to-local query from the code-quality suite. It remains in the code-quality-extended suite.

Go

path/filepath.Rel is no longer a sanitizer for go/path-injection and go/zipslip, which may lead to more results.

Java/Kotlin

java.io.File.getName() is no longer a complete sanitizer for java/path-injection, since it doesn&rsquo;t remove a .. path component. This may lead to more results.

C/C++

We&rsquo;ve updated the cpp/new-free-mismatch query to use the external/cwe/cwe-762 tag instead of external/cwe/cwe-401, which better matches the query&rsquo;s behavior.

GitHub Actions

We&rsquo;ve changed the EnvironmentCheck logic so it only protects against non-TOCTOU scenarios, which surfaces more results in the untrusted checkout queries.

Breaking change

CodeQL no longer parses [[-style links in alert messages. This undocumented legacy feature let query authors embed links inline in select clause message strings. Use $@ placeholder pairs instead.

For a full list of changes, please refer to the complete changelog for version 2.26.2. Every new version of CodeQL is automatically deployed to users of GitHub code scanning on github.com. The new functionality in CodeQL 2.26.2 will also be included in a future GitHub Enterprise Server (GHES) release. If you use an older version of GHES, you can manually upgrade your CodeQL version.

The post CodeQL 2.26.2 adds Swift 6.3.3 and Kotlin 2.4.10 support appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-04-codeql-2-26-2-adds-swift-6-3-3-and-kotlin-2-4-10-support](https://github.blog/changelog/2026-08-04-codeql-2-26-2-adds-swift-6-3-3-and-kotlin-2-4-10-support)*
