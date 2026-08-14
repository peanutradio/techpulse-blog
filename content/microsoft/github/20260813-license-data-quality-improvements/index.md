---
categories:
- MS
- GitHub
date: '2026-08-13T19:14:12+00:00'
description: GitHub now uses package registries like npmjs.org and PyPI to determine
  license information for software components in the dependency graph. This improves
  the a
draft: false
original_url: https://github.blog/changelog/2026-08-13-license-data-quality-improvements
source: GitHub Changelog
tags:
- Improvement
- supply chain security
title: License data quality improvements
---

GitHub now uses package registries like npmjs.org and PyPI to determine license information for software components in the dependency graph. This improves the accuracy and completeness of the licenses shown in dependency insights, software bills of materials (SBOMs), the open source license compliance feature in GitHub Advanced Security, and the dependency review action.
Previously, the primary source of license information on GitHub was the ClearlyDefined service. While we still use and contribute to ClearlyDefined, we&rsquo;ve found that its focus on depth-first file scanning led to complex results that users found confusing. We&rsquo;ll still fall back to ClearlyDefined data, but will now prioritize license information from the package registries. Early results show that we&rsquo;ve cut the number of missing licenses in half, from 45% of the 170 million packages in the dependency graph down to 24%. Additionally, the system now tracks version ranges instead of requiring a specific database entry for every version, so the actual coverage will be higher.
To explain further, the dependency graph service now uses metadata from the canonical registry for a given package ecosystem, as described in the following table.



Package Manager
Registry




npm
npmjs.org


NuGet
nuget.org


Python
pypi.org


Rubygems
rubygems.org


Rust
crates.io


Go
pkg.go.dev


Maven
deps.dev


Dart
pub.dev


PHP
packagist.org



The dependency graph service keeps license history based on version ranges. For example, Grafana, which relicensed from Apache to AGPL, has two entries: one covering 1.0.0 through 7.5.17 for Apache-2.0, and one from 8.0.0 or newer for AGPLv3. This both reduces the complexity of the database and provides license information for new versions without requiring each one to be added explicitly.
Updated license information is available now across all of GitHub.
Join the discussion within GitHub Community.

The post License data quality improvements appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-13-license-data-quality-improvements](https://github.blog/changelog/2026-08-13-license-data-quality-improvements)*
