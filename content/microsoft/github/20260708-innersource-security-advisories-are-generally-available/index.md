---
categories:
- MS
- GitHub
date: '2026-07-08T21:30:35+00:00'
description: 'GitHub Advanced Security enterprise customers can now publish internal
  security advisories. Innersource advisories work similarly to GitHub&rsquo;s open
  source '
draft: false
original_url: https://github.blog/changelog/2026-07-08-innersource-security-advisories-are-generally-available
source: GitHub Changelog
tags:
- Release
- supply chain security
title: Innersource security advisories are generally available
---

GitHub Advanced Security enterprise customers can now publish internal security advisories. Innersource advisories work similarly to GitHub&rsquo;s open source advisories, but their visibility is restricted to repositories owned by the enterprise.
There is a new REST API endpoint to manage innersource vulnerabilities, including operations to create, update, or withdraw vulnerabilities. Once you use the API to create an advisory about a component, GitHub uses Dependabot to notify repositories inside the enterprise that use the component. Notifications can include security alerts and version updates. When a version upgrade is needed, Dependabot will open a pull request to upgrade a vulnerable version of the component to one with a fix. For more information, see Creating and using innersource advisories.


The post Innersource security advisories are generally available appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-08-innersource-security-advisories-are-generally-available](https://github.blog/changelog/2026-07-08-innersource-security-advisories-are-generally-available)*
