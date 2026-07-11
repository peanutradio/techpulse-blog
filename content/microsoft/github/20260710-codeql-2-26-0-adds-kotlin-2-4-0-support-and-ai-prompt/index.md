---
categories:
- MS
- GitHub
date: '2026-07-10T20:40:56+00:00'
description: 'CodeQL is the static analysis engine behind GitHub code scanning, which
  finds and remediates security issues in your code. We&rsquo;ve recently released
  CodeQL '
draft: false
original_url: https://github.blog/changelog/2026-07-10-codeql-2-26-0-adds-kotlin-2-4-0-support-and-ai-prompt-injection-detection
source: GitHub Changelog
tags:
- Improvement
- application security
title: CodeQL 2.26.0 adds Kotlin 2.4.0 support and AI prompt injection detection
---

CodeQL is the static analysis engine behind GitHub code scanning, which finds and remediates security issues in your code. We&rsquo;ve recently released CodeQL 2.26.0, which adds support for Kotlin 2.4.0, introduces a JavaScript and TypeScript query for system prompt injection, and improves analysis accuracy across multiple languages.
Language and framework support
Kotlin: CodeQL now supports Kotlin versions up to 2.4.0.
C#: We&rsquo;ve added Razor Page handler method parameters, such as parameters for OnGet, OnPost, and OnPostAsync, as remote flow sources. Security queries such as cs/sql-injection can now detect vulnerabilities involving these parameters in PageModel subclasses.
Go: We&rsquo;ve added models for the log/slog package introduced in Go 1.21. The go/log-injection and go/clear-text-logging queries can now detect issues in code that uses slog package functions and slog.Logger methods.
JavaScript/TypeScript: We&rsquo;ve added prompt injection sinks for additional OpenAI, Anthropic, and Google GenAI SDK APIs, including Sora prompts, OpenAI Realtime session instructions, Anthropic legacy completion prompts, and Google GenAI cached content and system instructions.
Query changes
JavaScript/TypeScript

We&rsquo;ve added the js/system-prompt-injection query to detect when untrusted, user-provided values flow into an AI model&rsquo;s system prompt, allowing an attacker to manipulate the model&rsquo;s behavior.
We&rsquo;ve added the experimental javascript/ssrf-ipv6-transition-incomplete-guard query to detect server-side request forgery (SSRF) guards that reject private IPv4 ranges but can be bypassed with IPv6 transition address formats.

Go

The go/unhandled-writable-file-close query now produces fewer false positives. It no longer flags a deferred call to Close when every execution path first handles a call to Sync on the same file handle.

Python

The py/modification-of-locals query no longer flags modifications to a locals() dictionary after it has passed out of the scope where it was created, reducing false positives.

Swift

We&rsquo;ve improved CryptoKit modeling for the swift/weak-sensitive-data-hashing and swift/weak-password-hashing queries. These queries may now detect additional results.

GitHub Actions

We&rsquo;ve updated the actions/pr-on-self-hosted-runner query to recognize the latest standard runner labels, reducing false positives.
We&rsquo;ve corrected the name, description, and alert message for actions/untrusted-checkout/medium to clarify that it applies to a nonprivileged context.

For a full list of changes, please refer to the complete changelog for version 2.26.0. Every new version of CodeQL is automatically deployed to users of GitHub code scanning on github.com. The new functionality in CodeQL 2.26.0 will also be included in a future GitHub Enterprise Server (GHES) release. If you use an older version of GHES, you can manually upgrade your CodeQL version.

The post CodeQL 2.26.0 adds Kotlin 2.4.0 support and AI prompt injection detection appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-10-codeql-2-26-0-adds-kotlin-2-4-0-support-and-ai-prompt-injection-detection](https://github.blog/changelog/2026-07-10-codeql-2-26-0-adds-kotlin-2-4-0-support-and-ai-prompt-injection-detection)*
