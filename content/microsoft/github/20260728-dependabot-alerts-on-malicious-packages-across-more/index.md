---
categories:
- MS
- GitHub
date: '2026-07-28T14:55:31+00:00'
description: 'The GitHub Advisory Database now ingests malware advisories from the
  OpenSSF malicious-packages repository, significantly expanding the breadth of malware
  data '
draft: false
original_url: https://github.blog/changelog/2026-07-28-dependabot-alerts-on-malicious-packages-across-more-ecosystems
source: GitHub Changelog
tags:
- Improvement
- supply chain security
title: Dependabot alerts on malicious packages across more ecosystems
---

The GitHub Advisory Database now ingests malware advisories from the OpenSSF malicious-packages repository, significantly expanding the breadth of malware data available to you through Dependabot alerts.
What changed
With this update, advisories from the OpenSSF malicious-packages project are automatically ingested into the GitHub Advisory Database, giving you broader coverage across ecosystems including npm, PyPI, and more. You can view these using the type:malware filter.
If you have malware alerting enabled, Dependabot will now match your dependencies against this expanded set of malware advisories and alert you when a match is found.
What this means for you
You get broader ecosystem coverage. Malware advisories now cover additional ecosystems beyond npm, powered by the OpenSSF community&rsquo;s malicious-packages data.
If you already have malware alerting enabled, you will automatically benefit from the expanded coverage without any additional configuration needed. New advisories will generate alerts as they are published.
Getting started
If you haven&rsquo;t enabled malware alerting yet, navigate to your repository or organization Settings &rarr; Code security &rarr; Dependabot and enable Malware alerts under the Dependabot alerts section.
You can browse malware advisories directly at github.com/advisories.
To learn more, check out our docs about Dependabot malware alerts.

The post Dependabot alerts on malicious packages across more ecosystems appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-28-dependabot-alerts-on-malicious-packages-across-more-ecosystems](https://github.blog/changelog/2026-07-28-dependabot-alerts-on-malicious-packages-across-more-ecosystems)*
