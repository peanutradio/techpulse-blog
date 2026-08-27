---
categories:
- MS
- GitHub
date: '2026-08-26T22:08:38+00:00'
description: In July, we announced a default model policy for generally available
  GitHub Copilot models on Copilot Business and Copilot Enterprise plans. Starting
  today, we&
draft: false
original_url: https://github.blog/changelog/2026-08-26-global-model-policy-generally-available
source: GitHub Changelog
tags:
- Improvement
- copilot
- enterprise management tools
title: Global model policy generally available
---

In July, we announced a default model policy for generally available GitHub Copilot models on Copilot Business and Copilot Enterprise plans. Starting today, we&rsquo;re gradually rolling out enforcement of the policy through September 1, so it will take effect at different times for different enterprises. Previously unconfigured and new generally available models will inherit the global policy state. Administrators can make durable decisions for individual models at their discretion. Open-weight models and any models that require data retention are disabled by default.
What&rsquo;s changing
Once the policy takes effect for your organization or enterprise:

Models you haven&rsquo;t previously configured will change their state to &ldquo;Delegate to default policy&rdquo;, and they&rsquo;ll begin following your policy setting. If your policy is enabled &mdash; which is the default &mdash; those models will become available to your users. 
&ldquo;Delegate to default policy&rdquo; is a live, dynamic state that always tracks your policy. You can change the policy at any time, and all applicable models follow that state change.
We always preserve explicit choices. If you&rsquo;ve deliberately enabled or disabled a specific model, we do not change that setting.
We exclude open-weight models (e.g., DeepSeek and Kimi K2) and models not covered by GitHub&rsquo;s data retention agreement (e.g., Fable 5) from default enablement, regardless of your policy.

After the rollout, each model in your settings shows one of four states:

Enabled: You&rsquo;ve explicitly turned the model on.
Disabled: You&rsquo;ve explicitly turned the model off.
Delegate to enterprise teams/apps or organizations: The model follows a setting inherited from your enterprise team or organization.
Delegate to default policy: The model follows your default enablement policy.

To learn more, see default model availability.
What&rsquo;s next
We are looking for your feedback in the community discussion below. We&rsquo;re evaluating making the global model policy state an explicit decision and removing the &ldquo;Delegate to default policy&rdquo; state. This would help ensure that every policy reflects an explicit, intentional choice rather than an inferred one.
Join the discussion within GitHub Community.

The post Global model policy generally available appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-26-global-model-policy-generally-available](https://github.blog/changelog/2026-08-26-global-model-policy-generally-available)*
