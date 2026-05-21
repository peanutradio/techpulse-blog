---
categories:
- MS
- GitHub
date: '2026-05-20T21:07:38+00:00'
description: On Monday May 18, we detected and contained a compromise of an employee
  device involving a poisoned VS Code extension published by a third party. We removed
  the
draft: false
original_url: https://github.blog/security/investigating-unauthorized-access-to-githubs-internal-repositories/
source: GitHub Blog
tags:
- Security
title: Investigating unauthorized access to GitHub-owned repositories
---

On Monday May 18, we detected and contained a compromise of an employee device involving a poisoned VS Code extension published by a third party. We removed the malicious extension version, isolated the endpoint, and began incident response immediately.



Our current assessment is that the activity involved exfiltration of GitHub-internal repositories only. The attacker’s current claims of ~3,800 repositories are directionally consistent with our investigation so far.



We have no evidence of impact to customer information stored outside of GitHub’s internal repositories, such as our customer’s own enterprises, organizations, and repositories. Some of GitHub’s internal repositories contain information from customers, for example, excerpts of support interactions. If any impact is discovered, we will notify customers via established incident response and notification channels.



We moved quickly to reduce risk. We rotated critical secrets Monday and into Tuesday with the highest-impact credentials prioritized first.



We continue to analyze logs, validate secret rotation, and monitor our infrastructure for any follow-on activity. We will take additional action as the investigation warrants.



We will publish a fuller report once the investigation is complete.
The post Investigating unauthorized access to GitHub-owned repositories appeared first on The GitHub Blog.

---
*원문: [https://github.blog/security/investigating-unauthorized-access-to-githubs-internal-repositories/](https://github.blog/security/investigating-unauthorized-access-to-githubs-internal-repositories/)*
