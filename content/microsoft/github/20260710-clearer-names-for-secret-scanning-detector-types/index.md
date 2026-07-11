---
categories:
- MS
- GitHub
date: '2026-07-10T20:06:10+00:00'
description: To make secret scanning easier to understand, we&rsquo;re updating the
  names we use for our detector types to better reflect how each one finds secrets.
  This is
draft: false
original_url: https://github.blog/changelog/2026-07-10-clearer-names-for-secret-scanning-detector-types
source: GitHub Changelog
tags:
- Improvement
- application security
title: Clearer names for secret scanning detector types
---

To make secret scanning easier to understand, we&rsquo;re updating the names we use for our detector types to better reflect how each one finds secrets. This is a naming change only; detection behavior is exactly the same.



Before
Now




Non-provider patterns
Generic patterns


Copilot secret scanning
AI-detected secrets



All existing product documentation links continue to work. We&rsquo;ve added redirects and updated the terminology across our documentation. There are no changes to webhook events, audit log events, or the REST API.
There are two kinds of secrets we detect:

Provider secrets are issued by a specific service (e.g., an AWS key, a Stripe token).
Generic secrets aren&rsquo;t tied to any provider (e.g., private keys, connection strings, passwords).

There are two ways we detect them:

Patterns use deterministic detection (i.e., regular expressions combined with additional checks like entropy analysis). Patterns reliably catch secrets with a recognizable structure and include both provider patterns for provider secrets, as well as generic patterns like private keys and connection strings.
AI-detected secrets use AI to catch generic secrets that don&rsquo;t follow a predictable format (e.g., passwords). The model reads the surrounding code to find harder-to-detect unstructured secrets.

Learn more
Learn more about secret scanning and see the full list of supported secrets in our documentation. Let us know what you think in the community discussion.

The post Clearer names for secret scanning detector types appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-10-clearer-names-for-secret-scanning-detector-types](https://github.blog/changelog/2026-07-10-clearer-names-for-secret-scanning-detector-types)*
