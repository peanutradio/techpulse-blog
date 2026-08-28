---
categories:
- MS
- GitHub
date: '2026-08-27T21:50:39+00:00'
description: Starting October 1, 2026, checks, workflow runs, and statuses will be
  governed by the same Actions retention setting that already controls how long artifacts
  an
draft: false
original_url: https://github.blog/changelog/2026-08-27-actions-retention-will-cover-checks-workflow-runs-and-statuses
source: GitHub Changelog
tags:
- Retired
- actions
title: Actions retention will cover checks, workflow runs, and statuses
---

Starting October 1, 2026, checks, workflow runs, and statuses will be governed by the same Actions retention setting that already controls how long artifacts and logs are kept, with a default of 90 days. Until now, checks, workflow runs, and statuses were retained for 400+ days regardless of your retention configuration. After this change, they will automatically be cleaned up once they pass the artifact and log retention period configured for your repository, organization, or enterprise.
This change reduces the amount of stale data stored across GitHub Actions, helping keep checks, workflow runs, and statuses fast and reliable for everyone.
What&rsquo;s changing

Checks, workflow runs, and statuses will follow your Actions retention setting. They will be cleaned up once they exceed your configured artifact and log retention period, instead of sticking around for 400+ days.
The retention setting now has a broader scope. The setting label in the UI will be updated to read &ldquo;Check, workflow run, status, artifact and log retention&rdquo; to reflect that it now governs all five.
Retention caps still apply. A repository can raise its retention only up to the cap configured at the organization and enterprise level. For public repositories, the maximum retention for checks, workflow runs, and statuses is 90 days, matching the existing limit for artifacts and logs.
The change is not retroactive. Adjusting your retention setting will not restore data that was previously evicted due to a retention policy. This matches how artifact and log retention behaves today.

Billing and storage implications
Artifacts and logs count toward your billable GitHub Actions storage. Because this change cleans up data once it passes your retention period, repositories that previously kept artifacts and logs around longer than their configured setting may see a reduction in billable Actions storage usage.
Keep this in mind when you review your retention setting:

Lowering your retention period removes artifacts and logs sooner, which can reduce your Actions storage costs.
Raising your retention period keeps artifacts and logs available longer, which may increase your billable Actions storage. Checks, workflow run, and statuses metadata are not billed for storage, but the artifacts and logs associated with them are.

What you need to do
For most repositories, no action is required. Starting on October 1, checks, workflow runs, and statuses will be cleaned up according to the retention setting you already have configured.
To prepare:

Review your Actions retention setting for your repository, organization, or enterprise before October 1, 2026. Confirm it reflects how long you need checks, workflow runs, and statuses to remain available.
Increase the retention period if you need a longer window, up to the maximum allowed by your organization and enterprise caps (90 days for public repositories). Note that a longer retention period keeps artifacts and logs longer and may increase your billable Actions storage.
Export or archive anything you need to keep beyond your configured retention period, since older checks, workflow runs, and statuses will be automatically removed once this change takes effect.


The post Actions retention will cover checks, workflow runs, and statuses appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-27-actions-retention-will-cover-checks-workflow-runs-and-statuses](https://github.blog/changelog/2026-08-27-actions-retention-will-cover-checks-workflow-runs-and-statuses)*
