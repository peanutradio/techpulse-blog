---
categories:
- MS
- GitHub
date: '2026-07-15T22:38:16+00:00'
description: "This week, we&rsquo;re rolling out several improvements to secret scanning\
  \ and public monitoring:\n\nResend is now a GitHub secret scanning partner. \nSecret\
  \ scann"
draft: false
original_url: https://github.blog/changelog/2026-07-15-improvements-to-secret-scanning-and-public-monitoring
source: GitHub Changelog
tags:
- Improvement
- application security
title: Improvements to secret scanning and public monitoring
---

This week, we&rsquo;re rolling out several improvements to secret scanning and public monitoring:

Resend is now a GitHub secret scanning partner. 
Secret scanning now detects new secret types from APIclub and Resend.
Secret scanning now blocks VolcEngine secrets with push protection by default.
The secret_scanning_alert webhook now includes a secret_category field (i.e., default or generic) so you can distinguish between specific and generic types.
The public monitoring alert list now surfaces insight cards at the top of the page, including a breakdown of associated leaks by attribution, your enterprise member count, and your verified domains.

GitHub secret scanning partnership program
GitHub secret scanning protects users by searching repositories for known types of secrets such as tokens and private keys. By identifying and flagging these secrets, our scans help prevent data leaks and fraud.
We have partnered with Resend to scan for their tokens to help secure the development community. GitHub will forward any exposed secrets found in public repositories to Resend, who will take appropriate action, including revoking the secret or notifying respect admins.
Learn more about the secret scanning partnership program. If you are a secret issuer interested in partnering with us, you can get started by opening a ticket with GitHub support.
Detectors added
Secret scanning now automatically detects the following new secret types in your repositories.



Provider
Secret type




APIclub
apiclub_api_key


Resend
resend_api_key



Partner secrets are automatically reported to the secret issuer when found in public repositories through the secret scanning partnership program. User secrets generate secret scanning alerts when found in public or private repositories.
Push protection defaults expanded
The following detector is now included in push protection by default. Repositories with secret scanning enabled, including free public repositories, will automatically block commits containing this secret.



Provider
Secret type




VolcEngine
volcengine_ark_api_key



Distinguish secret categories in the webhook payload
The secret_scanning_alert webhook payload now includes a secret_category field, so you can tell default and generic detections apart without maintaining your own mapping of secret types:

default: provider patterns plus your custom patterns.
generic: generic patterns and AI-detected secrets.

This mirrors the Default and Generic results views in secret scanning, making it easier to filter, route, and report on alerts in your own integrations and automation.
New insights on the public monitoring alert list
The public monitoring alert list now shows insight cards at the top of the page, giving your security team at-a-glance context before digging into individual alerts:

Associated leaks by attribution: A breakdown of alert counts by how each leak was attributed to your enterprise: member activity (a commit authored by an enterprise member) and verified domain (a committer email on one of your verified domains). 
Enterprise members: The total number of members in your enterprise, matching the count of members in your enterprise People tab.
Verified domains: The verified domains associated with your enterprise, including both enterprise-owned and organization-owned domains.

Together, these cards help you gauge the scope of exposure and understand how leaks are reaching your enterprise, all without leaving the alert list.


Learn more
Learn more about secret scanning, public monitoring, and the full list of supported secrets in our documentation. Let us know what you think in the community discussion.

The post Improvements to secret scanning and public monitoring appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-15-improvements-to-secret-scanning-and-public-monitoring](https://github.blog/changelog/2026-07-15-improvements-to-secret-scanning-and-public-monitoring)*
