---
categories:
- MS
- GitHub
date: '2026-07-29T14:01:07+00:00'
description: We&rsquo;re introducing a global default enablement policy for generally
  available Copilot models on Copilot Business and Copilot Enterprise plans. Instead
  of r
draft: false
original_url: https://github.blog/changelog/2026-07-29-default-model-enablement-for-copilot-business-and-enterprise
source: GitHub Changelog
tags:
- Improvement
- copilot
title: Default model enablement for Copilot Business and Enterprise
---

We&rsquo;re introducing a global default enablement policy for generally available Copilot models on Copilot Business and Copilot Enterprise plans. Instead of requiring admins to manually turn on each new model as it ships, models that become generally available will now be on by default. We&rsquo;re adding a single opt-out control for organizations and enterprises that need stricter governance.
What&rsquo;s changing
Today, a new default availability for released models policy is available in your enterprise and organization model settings. For the next 28 days, this policy is configurable but has no effect on model availability &mdash; nothing changes for your users yet.
On August 26, the policy takes effect:

Models you haven&rsquo;t explicitly configured will be relabeled from &ldquo;unconfigured&rdquo; to &ldquo;inherits default&rdquo; and will begin following your policy setting. If your policy is enabled (i.e., the default), those models become available to your users. If it&rsquo;s disabled, they stay off.
&ldquo;Inherits default&rdquo; is a live, dynamic state that always tracks your policy. You can flip the policy at any time and all &ldquo;inherits default&rdquo; models follow immediately.
Explicit choices are always preserved. If you&rsquo;ve deliberately enabled or disabled a specific model, that setting is never touched.
Open-weight models (e.g., DeepSeek, Kimi K2.7) and models not covered by GitHub&rsquo;s data retention agreement (e.g., Fable 5) are excluded from default enablement regardless of your policy.

Use the next 28 days to review your model settings. If you&rsquo;re happy with models being available by default, no action is needed. If you&rsquo;d rather manually approve each model, set the policy to disabled before August 26.

The post Default model enablement for Copilot Business and Enterprise appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-29-default-model-enablement-for-copilot-business-and-enterprise](https://github.blog/changelog/2026-07-29-default-model-enablement-for-copilot-business-and-enterprise)*
