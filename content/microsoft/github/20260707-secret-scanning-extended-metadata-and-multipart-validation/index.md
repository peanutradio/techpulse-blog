---
categories:
- MS
- GitHub
date: '2026-07-07T16:43:04+00:00'
description: 'To help you understand ownership and impact of a leaked secret, GitHub
  secret scanning surfaces enriched metadata for supported secret types. Extended
  metadata '
draft: false
original_url: https://github.blog/changelog/2026-07-07-secret-scanning-extended-metadata-and-multipart-validation
source: GitHub Changelog
tags:
- Release
- application security
title: Secret scanning extended metadata and multipart validation
---

To help you understand ownership and impact of a leaked secret, GitHub secret scanning surfaces enriched metadata for supported secret types. Extended metadata checks are now generally available, including support for multipart validators with supplementary metadata.
What&rsquo;s new?
These updates expand on existing validity checks to give more actionable context for triage and remediation, enabling development and security teams to assess exposure faster and prioritize remediation.

Secret scanning already performs validity checks for most secret providers, verifying whether a detected secret is active. You can now extend those checks to include additional metadata from supported providers, including details about a secret&rsquo;s owner, secret creation and expiry dates, and project or organization context.
Metadata is surfaced across list and detail views, including alert list filters, security campaign creation, webhook events, and the REST API. GitHub makes a best effort to display metadata for the secret&mdash;metadata availability can vary by secret provider, token type, and possibly even for a specific secret at different points in time.
In addition, some secret types require more than the secret literal itself to determine its validity. When possible, GitHub will leverage supplementary metadata in order to determine if a secret is still active. Multipart validity checks now cover key credential formats across major providers.
Coverage includes Alibaba Cloud, Databricks token and workspace URL combinations, multiple Microsoft Azure key and host or endpoint pairs, and more.
GitHub will continually add support for additional secret types on a rolling basis.
Learn more
Learn more about securing your repositories with secret scanning and see the full list of supported secrets in our documentation.

The post Secret scanning extended metadata and multipart validation appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-07-secret-scanning-extended-metadata-and-multipart-validation](https://github.blog/changelog/2026-07-07-secret-scanning-extended-metadata-and-multipart-validation)*
