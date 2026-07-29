---
categories:
- MS
- GitHub
date: '2026-07-28T23:35:01+00:00'
description: 'Copilot app usage is now reported across much more of the Copilot usage
  metrics API.

  Individual Copilot app activity is now attributed to users in the enterpris'
draft: false
original_url: https://github.blog/changelog/2026-07-28-github-copilot-app-usage-metrics-now-expand-across-report-rollups
source: GitHub Changelog
tags:
- Improvement
- account management
- copilot
- enterprise management tools
title: GitHub Copilot app usage metrics now expand across report rollups
---

Copilot app usage is now reported across much more of the Copilot usage metrics API.
Individual Copilot app activity is now attributed to users in the enterprise-user and organization-user reports. In addition, Copilot app coding activity is now broken out in the feature, model, and language rollups alongside every other Copilot surface.
This builds on the earlier release that brought the Copilot app into the usage metrics API with enterprise-level Copilot app totals.
What&rsquo;s new

used_copilot_app: Whether a user was active in the Copilot app on a given day.
totals_by_copilot_app: A per-user section reporting session_count, request_count, prompt_count, and a token_usage breakdown of output_tokens_sum, prompt_tokens_sum, and avg_tokens_per_request.
copilot_app feature value: Copilot app activity now appears in totals_by_feature, totals_by_model_feature, totals_by_language_feature, and totals_by_language_model, so you can see which models and languages Copilot app work happens in.
Code activity and lines-of-code metrics: Top-level code generation, code acceptance, lines added, and lines deleted totals now include Copilot app activity.
daily_active_users: Now counts users who were only active in the Copilot app.

Why this matters
Copilot app usage was previously only visible as a standalone enterprise and organization-level total, so you could see that the Copilot app was being used but not who was using it or what it produced. With Copilot app activity attributed to individual users and folded into the standard breakdowns, you can identify your Copilot app adopters, and measure the code the Copilot app generates.
You can also compare the Copilot app against the IDE, chat, code review, and coding agent surfaces using the same fields you already consume.
Important notes

used_copilot_app and the user-level totals_by_copilot_app section are available in the enterprise-user and organization-user 1-day and 28-day reports. The copilot_app feature value and the code activity, lines-of-code, and daily_active_users changes apply to the enterprise, organization, and user reports for both the 1-day and 28-day windows.
These metrics are available to enterprise owners and billing managers, organization owners, and anyone with a custom organization or enterprise role that grants the View Copilot Metrics permission. The Copilot usage metrics policy must be enabled.
The changes are backward compatible. Users and entities with no Copilot app activity omit totals_by_copilot_app and produce no copilot_app breakdown entries, and existing fields keep their current shape.

Visit the Copilot usage metrics API documentation to get started.

The post GitHub Copilot app usage metrics now expand across report rollups appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-28-github-copilot-app-usage-metrics-now-expand-across-report-rollups](https://github.blog/changelog/2026-07-28-github-copilot-app-usage-metrics-now-expand-across-report-rollups)*
