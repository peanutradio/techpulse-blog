---
categories:
- MS
- GitHub
date: '2026-09-03T20:34:34+00:00'
description: We&rsquo;re continuing to make trusted publishing smoother for npm publishers,
  guided by maintainers feedback. Three updates to npm publishing are now generally
draft: false
original_url: https://github.blog/changelog/2026-09-03-multiple-trusted-publishing-configurations-for-npm
source: GitHub Changelog
tags:
- Release
- supply chain security
title: Multiple trusted publishing configurations for npm
---

We&rsquo;re continuing to make trusted publishing smoother for npm publishers, guided by maintainers feedback. Three updates to npm publishing are now generally available:

Multiple trusted publishing configurations per package
Staged packages can only be approved after malware scanning is complete
Maintainers can see their staged history in the package versions tab

A package can now have more than one trusted publishing (OIDC) configuration. Maintainers are no longer limited to one configuration per package to separate workflows with stable, prerelease, or staging versions. Before this, maintainers had to depend on workflow workarounds or keep a long-lived token around for the paths OIDC couldn&rsquo;t cover.
Each configuration is independent and additive, with its own repository, workflow, and environment criteria. You can add, list, and remove them from your package&rsquo;s settings page. A publish or stage is authorized if the incoming OIDC token matches any one configuration. Configurations never restrict one another, and evaluation order is not guaranteed, so don&rsquo;t build logic that depends on which configuration matches.
Every trusted publishing configuration can stage a package by default. Direct publishing is opt-in per configuration. We recommend keeping your configurations to staging only. Staged publishing adds a human approval step before a version becomes available, so a compromised workflow can&rsquo;t push straight to the registry.
Since we introduced publish-time malware scanning, packages are scanned before they become available. In the staged publishing queue, the approval button is now disabled while a package is still being scanned and becomes available once the scan completes. The page refreshes status every minute.
The versions tab on npmjs.com now shows to respective maintainers a detailed history for each version, including whether it was approved, rejected, or still staged.
Follow along and ask questions in the community discussion.

The post Multiple trusted publishing configurations for npm appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-09-03-multiple-trusted-publishing-configurations-for-npm](https://github.blog/changelog/2026-09-03-multiple-trusted-publishing-configurations-for-npm)*
