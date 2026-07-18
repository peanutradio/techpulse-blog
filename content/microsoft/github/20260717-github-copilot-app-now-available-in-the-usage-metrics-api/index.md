---
categories:
- MS
- GitHub
date: '2026-07-17T22:05:11+00:00'
description: The Copilot usage metrics API now reports the GitHub Copilot app usage
  in the enterprise and organization 1-day and 28-day reports. This gives enterprise
  and or
draft: false
original_url: https://github.blog/changelog/2026-07-17-github-copilot-app-now-available-in-the-usage-metrics-api
source: GitHub Changelog
tags:
- Improvement
- account management
- copilot
- enterprise management tools
title: GitHub Copilot app now available in the usage metrics API
---

The Copilot usage metrics API now reports the GitHub Copilot app usage in the enterprise and organization 1-day and 28-day reports. This gives enterprise and organization admins visibility into the app&rsquo;s activity alongside the IDE, chat, code review, and coding agent metrics they already retrieve.
What&rsquo;s new
The enterprise and organization reports now include two new fields:

daily_active_copilot_app_users: The number of distinct users active in the Copilot app on a given day.
totals_by_copilot_app: A dedicated GitHub Copilot app section reporting session_count, request_count, prompt_count, and a token_usage breakdown (i.e., output_tokens_sum, prompt_tokens_sum, and avg_tokens_per_request).

Why this matters
The GitHub Copilot app activity was not previously represented in usage reporting. With these fields, enterprise and organization admins can see how broadly the app is being adopted (e.g., distinct active users, session and request volume, and token consumption) in the same API they already use for the rest of their Copilot usage metrics.
Important notes

These fields appear in the enterprise and organization 1-day and 28-day reports.
The GitHub Copilot app usage is reported in its own totals_by_copilot_app section and is kept separate from the generic feature, model, and language totals, as well as from lines-of-code metrics.
Enterprises or organizations with no GitHub Copilot app activity report null for both daily_active_copilot_app_users and totals_by_copilot_app, so existing integrations are unaffected.

Visit the Copilot usage metrics API documentation to get started.

The post GitHub Copilot app now available in the usage metrics API appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-17-github-copilot-app-now-available-in-the-usage-metrics-api](https://github.blog/changelog/2026-07-17-github-copilot-app-now-available-in-the-usage-metrics-api)*
