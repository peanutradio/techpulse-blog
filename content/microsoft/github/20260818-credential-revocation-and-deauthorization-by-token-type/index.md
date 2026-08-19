---
categories:
- MS
- GitHub
date: '2026-08-18T21:57:30+00:00'
description: Building on our self-service credential revocation experiences for incident
  response, you can now take token-type and user-specific actions to deauthorize and
  r
draft: false
original_url: https://github.blog/changelog/2026-08-18-credential-revocation-and-deauthorization-by-token-type
source: GitHub Changelog
tags:
- Improvement
- application security
- enterprise management tools
title: Credential revocation and deauthorization by token type
---

Building on our self-service credential revocation experiences for incident response, you can now take token-type and user-specific actions to deauthorize and revoke user credentials during a security incident. This gives you finer-grained control when responding to a compromise.
Previously, credential kill-switch actions applied to all of a user&rsquo;s credentials at once. Now, enterprise owners, organization admins, and members with the Manage enterprise credentials permission can revoke all tokens of a specific credential type (e.g., Personal access tokens, SSH keys, OAuth app tokens, or GitHub App user access tokens) so you can contain the blast radius of a compromise without revoking credentials that remain trusted.
What&rsquo;s new

Token-type-specific bulk deauthorization: Revoke all SSO authorizations for a specific credential type across your enterprise or for a specific user from the UI or the enterprise REST APIs, rather than all types at once.
Token-type specific bulk revocation: Delete or revoke all user-level credentials of a specific type. For example, delete all personal access tokens for an individual EMU user without touching their SSH keys.
Organization-level parity, in the UI and API: All bulk credential-revocation actions previously available at the enterprise level are now also available at the organization level through both the web UI and the organization REST APIs, enabling incident response for organizations.
Auditing and visibility: All deauthorization and revocation actions are captured in the audit log, with notifications to affected users via email.

To learn more, see our documentation around revoking your credentials and how to respond to security incidents in your enterprise.

The post Credential revocation and deauthorization by token type appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-18-credential-revocation-and-deauthorization-by-token-type](https://github.blog/changelog/2026-08-18-credential-revocation-and-deauthorization-by-token-type)*
