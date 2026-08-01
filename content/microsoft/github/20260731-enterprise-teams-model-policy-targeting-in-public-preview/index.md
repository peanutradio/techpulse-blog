---
categories:
- MS
- GitHub
date: '2026-07-31T18:11:50+00:00'
description: You can now take advantage of user-based model policy targeting for GitHub
  Enterprise customers with Copilot Business or Copilot Enterprise licenses. This
  featu
draft: false
original_url: https://github.blog/changelog/2026-07-31-enterprise-teams-model-policy-targeting-in-public-preview
source: GitHub Changelog
tags:
- Improvement
- copilot
- enterprise management tools
title: Enterprise teams model policy targeting in public preview
---

You can now take advantage of user-based model policy targeting for GitHub Enterprise customers with Copilot Business or Copilot Enterprise licenses. This feature empowers AI administrators to set a baseline of models for the entire enterprise and then grant additional models to specific enterprise teams. This enables admins to tailor access by job role or early experimentation by frontier teams, without deferring entirely to organization-level decisions.
This public preview is the first step in a broader shift toward team-level governance. Historically, model access has been managed at the organization (i.e., resource) level, but enterprise teams give you finer grained, user-based control that better maps to how people actually work: by role, training level, or function. Over time, we&rsquo;ll continue adding controls to the team level, giving admins better granularity and precision than org-level settings can offer.
We are rolling this feature out gradually, with most enterprise customers gaining access to the preview opt-in on August 3rd.
How it works
At the enterprise level, you can now set models to:

Enabled: Available to all members of the enterprise.
Disabled: Not available to any member of the enterprise.
Optional: Available to assign to enterprise teams.

Enterprise teams model access evaluates with a least-restrictive strategy. If a user gets a model from any one enterprise team, that user has access to the model everywhere. In order to opt into this preview, turn on the Enterprise teams mode toggle found in the Copilot page under &ldquo;Models&rdquo;.

When enabled, model access is managed at the enterprise level and through enterprise teams, and organization-level model settings no longer apply. During the preview, you may roll back to reset your policy to its previous configuration. To learn more, see our docs about managing model availability for your enterprise.
Migrating to enterprise teams mode
As part of the preview, you can start creating enterprise teams and assigning Optional models to those teams before enabling enterprise teams mode. This allows you to prepare model-to-user assignments similar to those you may have used with organization delegation. Review your enterprise model policy and create enterprise teams to grant additional model access before opting into the preview.
Behavior for users in multiple enterprises
If your enterprise does not use Enterprise Managed Users, you may have enterprise members who have been granted a Copilot license from additional enterprises. In enterprise teams mode, only the model policies from your enterprise apply to users if they are using a Copilot license assigned from your enterprise. Restrictions from any other enterprise will not apply.
Join the discussion within GitHub Community.

The post Enterprise teams model policy targeting in public preview appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-31-enterprise-teams-model-policy-targeting-in-public-preview](https://github.blog/changelog/2026-07-31-enterprise-teams-model-policy-targeting-in-public-preview)*
