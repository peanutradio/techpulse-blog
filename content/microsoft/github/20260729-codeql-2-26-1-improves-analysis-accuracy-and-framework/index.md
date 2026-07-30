---
categories:
- MS
- GitHub
date: '2026-07-29T09:45:11+00:00'
description: 'CodeQL is the static analysis engine behind GitHub code scanning, which
  finds and remediates security issues in your code. We&rsquo;ve recently released
  CodeQL '
draft: false
original_url: https://github.blog/changelog/2026-07-29-codeql-2-26-1-improves-analysis-accuracy-and-framework-coverage
source: GitHub Changelog
tags:
- Improvement
- application security
title: CodeQL 2.26.1 improves analysis accuracy and framework coverage
---

CodeQL is the static analysis engine behind GitHub code scanning, which finds and remediates security issues in your code. We&rsquo;ve recently released CodeQL 2.26.1, which improves framework coverage for Go, Java/Kotlin, and JavaScript/TypeScript, and reduces false positives in Rust analysis.
Language and framework support
C/C++

Models-as-data flow summaries now use fully qualified field names, such as MyNamespace::MyStruct::myField. Unqualified field names remain supported but will be removed in 12 months.

Go

We&rsquo;ve improved modeling for the log/slog package, including slog.Logger methods, With, WithGroup, Attr, and Value. This expands coverage for applications using structured logging.

Java/Kotlin

We&rsquo;ve added source, sink, and flow summary models for org.apache.poi.

JavaScript/TypeScript

We&rsquo;ve added support for Angular&rsquo;s @HostListener('window:message', ...) and @HostListener('document:message', ...) decorators. CodeQL now recognizes the decorated method&rsquo;s event parameter as a client-side remote flow source.

Query changes
Go

The improved log/slog modeling expands coverage for the go/log-injection and go/clear-text-logging queries.

Java/Kotlin

The java/path-injection query now recognizes input validated with @javax.validation.constraints.Pattern as sanitized, reducing false positives.
The java/ssrf query now treats the first argument of Spring WebFlux&rsquo;s WebClient.UriSpec.uri method as a request forgery sink, which may produce additional valid alerts.

JavaScript/TypeScript

The js/missing-origin-check query now analyzes Angular message event handlers declared with @HostListener.

Rust

The rust/hard-coded-cryptographic-value query now treats arithmetic, bitwise, and string append operations as barriers. This reduces false positives when code combines hard-coded constants with nonconstant data, such as when incrementing a nonce or appending variable data to a constant prefix.

For a full list of changes, please refer to the complete changelog for version 2.26.1. Every new version of CodeQL is automatically deployed to users of GitHub code scanning on github.com. The new functionality in CodeQL 2.26.1 will also be included in a future GitHub Enterprise Server (GHES) release. If you use an older version of GHES, you can manually upgrade your CodeQL version.

The post CodeQL 2.26.1 improves analysis accuracy and framework coverage appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-29-codeql-2-26-1-improves-analysis-accuracy-and-framework-coverage](https://github.blog/changelog/2026-07-29-codeql-2-26-1-improves-analysis-accuracy-and-framework-coverage)*
