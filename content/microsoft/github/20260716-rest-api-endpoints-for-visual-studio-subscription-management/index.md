---
categories:
- MS
- GitHub
date: '2026-07-16T18:06:52+00:00'
description: 'GitHub Enterprise Cloud admins can now use the following REST API endpoints
  to programmatically manage Visual Studio Subscription (VSS) assignments:


  GET /enter'
draft: false
original_url: https://github.blog/changelog/2026-07-16-rest-api-endpoints-for-visual-studio-subscription-management
source: GitHub Changelog
tags:
- Improvement
- enterprise management tools
title: REST API endpoints for Visual Studio Subscription management
---

GitHub Enterprise Cloud admins can now use the following REST API endpoints to programmatically manage Visual Studio Subscription (VSS) assignments:

GET /enterprises/{enterprise}/visual-studio-subscriptions: Returns all VSS assignments for an enterprise, including whether each assignment has been matched to a GitHub user.
PUT /enterprises/{enterprise}/visual-studio-subscriptions: Maps a VSS UPN to a GitHub handle, enabling bulk programmatic matching.
DELETE /enterprises/{enterprise}/visual-studio-subscriptions/{visual_studio_subscription_id}: Removes a manual match between a Visual Studio subscription and a GitHub user, allowing admins to correct mistaken assignments or programmatically rematch subscriptions.

These endpoints are especially useful for organizations where VSS UPN formats do not align with SCIM identities, a scenario that prevents automatic matching and previously required tedious manual resolution in the UI. Admins can now supply a UPN-to-GitHub-handle crosswalk and script bulk-matching operations.

The post REST API endpoints for Visual Studio Subscription management appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-16-rest-api-endpoints-for-visual-studio-subscription-management](https://github.blog/changelog/2026-07-16-rest-api-endpoints-for-visual-studio-subscription-management)*
