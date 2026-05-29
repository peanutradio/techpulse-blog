---
categories:
- MS
- GitHub
date: '2026-05-28T21:09:44+00:00'
description: 'CodeQL is the static analysis engine behind GitHub code scanning, which
  finds and remediates security issues in your code. We&rsquo;ve recently released
  CodeQL '
draft: false
original_url: https://github.blog/changelog/2026-05-28-codeql-2-25-5-improves-query-accuracy-for-github-actions
source: GitHub Changelog
tags:
- Improvement
- application security
title: CodeQL 2.25.5 improves query accuracy for GitHub Actions
---

CodeQL is the static analysis engine behind GitHub code scanning, which finds and remediates security issues in your code. We&rsquo;ve recently released CodeQL 2.25.5, which includes accuracy improvements across C/C++, Java/Kotlin, and GitHub Actions queries.
Language and framework support
Java/Kotlin

We&rsquo;ve introduced a new sink kind, path-injection[read], for Models-as-Data rows that only read from a path (such as ClassLoader.getResource, FileInputStream, and FileReader). This helps queries distinguish read-only path sinks from more dangerous ones.

GitHub Actions

We&rsquo;ve extended the poisonable_steps modeling to detect additional sinks, including scripts executed via Python modules and go run in directories.

Query changes
C/C++

The cpp/cleartext-transmission query no longer raises an alert on calls to fscanf (and variants) when the call reads from an input that isn&rsquo;t a socket, reducing false positives.

Java/Kotlin

The java/zipslip query no longer reports archive entry names that flow only to read-only path sinks such as ClassLoader.getResource, FileInputStream, and FileReader, reducing false positives.

GitHub Actions

The actions/unpinned-tag query now analyzes composite action metadata (action.yml and action.yaml files) in addition to workflow files, providing more comprehensive detection.
We&rsquo;ve fixed the help file descriptions for the actions/untrusted-checkout/critical, actions/untrusted-checkout/high, and actions/untrusted-checkout/medium queries.
We&rsquo;ve renamed actions/untrusted-checkout/high to more clearly describe which parts of the scenario run in a privileged context.

For a full list of changes, please refer to the complete changelog for version 2.25.5. Every new version of CodeQL is automatically deployed to users of GitHub code scanning on github.com. The new functionality in CodeQL 2.25.5 will also be included in GitHub Enterprise Server (GHES) release 3.22. If you use an older version of GHES, you can manually upgrade your CodeQL version.

The post CodeQL 2.25.5 improves query accuracy for GitHub Actions appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-05-28-codeql-2-25-5-improves-query-accuracy-for-github-actions](https://github.blog/changelog/2026-05-28-codeql-2-25-5-improves-query-accuracy-for-github-actions)*
