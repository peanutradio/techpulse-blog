---
categories:
- MS
- GitHub
date: '2026-07-02T23:19:06+00:00'
description: 'We&rsquo;ve made three improvements to the Copilot usage metrics API
  that make its reports more complete and accurate: GitHub Copilot CLI now reports
  suggested '
draft: false
original_url: https://github.blog/changelog/2026-07-02-improved-accuracy-and-coverage-in-copilot-usage-metrics-reports
source: GitHub Changelog
tags:
- Improvement
- account management
- copilot
- enterprise management tools
title: Improved accuracy and coverage in Copilot usage metrics reports
---

We&rsquo;ve made three improvements to the Copilot usage metrics API that make its reports more complete and accurate: GitHub Copilot CLI now reports suggested lines of code, users seen only through server-side telemetry now have their IDE identified, and AI credit consumption is now attributed more completely.
What&rsquo;s new

GitHub Copilot CLI now reports suggested lines of code. CLI activity now contributes to the loc_suggested_to_add_sum and loc_suggested_to_delete_sum fields, which previously always reported 0 for the CLI. Code generation counts are also more accurate on newer CLI versions, where suggested and accepted edits are de-duplicated so the same edit isn&rsquo;t counted twice.
IDE identified for more users. Users who were previously visible only through server-side telemetry now have their IDE and plugin versions surfaced in totals_by_ide, so totals_by_ide reflects more of your Copilot users.
AI credits attributed more accurately. We fixed two issues that caused some users to show 0.0 AI credits despite real usage. First, AI credit consumption not associated with an organization was being dropped. It&rsquo;s now attributed to the correct organization or enterprise. Second, users seen only through server-side telemetry were not being matched to their billing data. Their consumption is now included. Thanks to these updates, ai_credits_used totals more completely reflect actual consumption.

Why this matters

More complete coverage: Surfacing CLI suggested lines of code and identifying IDEs for server-side-only users means fewer blind spots in who is using Copilot and how.
More trustworthy consumption data: Correcting AI credit attribution means ai_credits_used totals more accurately reflect what your users actually consumed.
Consistent analysis across surfaces: As Copilot usage spans the IDE, CLI, and server-side surfaces, these updates keep the reports aligned with real activity.

Important notes

These metrics are available to enterprise administrators and organization owners who have access to Copilot usage metrics through the REST API.
Copilot CLI reports suggested lines of code from CLI version 1.0.57 onward. Code generation de-duplication applies from version 1.0.64 onward. Between 1.0.57 and 1.0.64, code generation activity may be slightly undercounted for the CLI.
AI credit totals for previously-missed usage will increase as a result of these attribution fixes&mdash;values that were already reported are unchanged.

Visit the Copilot usage metrics API documentation to learn more.

The post Improved accuracy and coverage in Copilot usage metrics reports appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-02-improved-accuracy-and-coverage-in-copilot-usage-metrics-reports](https://github.blog/changelog/2026-07-02-improved-accuracy-and-coverage-in-copilot-usage-metrics-reports)*
