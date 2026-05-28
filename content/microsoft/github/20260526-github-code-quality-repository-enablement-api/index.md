---
categories:
- MS
- GitHub
date: '2026-05-26T20:35:06+00:00'
description: You can now programmatically enable and configure GitHub Code Quality
  on individual repositories using the new Repository Enablement API, available today
  in pub
draft: false
original_url: https://github.blog/changelog/2026-05-26-github-code-quality-repository-enablement-api
source: GitHub Changelog
tags:
- Release
- application security
- enterprise management tools
title: 'GitHub Code Quality: Repository Enablement API'
---

You can now programmatically enable and configure GitHub Code Quality on individual repositories using the new Repository Enablement API, available today in public preview.
Two new endpoints are now available:

PATCH /repos/{owner}/{repo}/code-quality/setup: Enable or disable Code Quality default setup for a repository, configure the languages to analyze, and specify the runner type.
GET /repos/{owner}/{repo}/code-quality/setup: Retrieve the current Code Quality configuration for a repository, including state, languages, runner type, and analysis schedule.

Supported languages include csharp, go, java-kotlin, javascript-typescript, python, and ruby.
This feature is available in public preview on github.com and is not available on Enterprise Server.
To learn more about GitHub Code Quality, check out the documentation for code quality.

The post GitHub Code Quality: Repository Enablement API appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-05-26-github-code-quality-repository-enablement-api](https://github.blog/changelog/2026-05-26-github-code-quality-repository-enablement-api)*
