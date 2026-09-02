---
categories:
- MS
- GitHub
date: '2026-09-01T22:24:27+00:00'
description: Enterprise Live Migrations (ELM) is now generally available, enabling
  near-zero-downtime repository migrations from GitHub Enterprise Server (GHES) to
  GitHub En
draft: false
original_url: https://github.blog/changelog/2026-09-01-enterprise-live-migrations-from-ghes-to-ghe-com-generally-available
source: GitHub Changelog
tags:
- Release
- enterprise management tools
title: Enterprise Live Migrations from GHES to ghe.com generally available
---

Enterprise Live Migrations (ELM) is now generally available, enabling near-zero-downtime repository migrations from GitHub Enterprise Server (GHES) to GitHub Enterprise Cloud with Data Residency (GHEC DR), so you can move even your largest, most active repositories while the contributions keep flowing.
Key features of ELM to consider when selecting a tool for your next repository migration are:

Cutover in minutes, not days: ELM continuously syncs data from source to target, so developers keep working in their repositories throughout the migration. When you&rsquo;re ready, the cutover only requires the time to drain remaining in-flight changes. This is especially valuable for business-critical repositories contributed to by geographically dispersed teams where there is no convenient downtime window to fit a migration into.


Built for the largest monorepos: ELM was purpose-built to handle the repositories that push existing tooling to its limits (i.e., massive monorepos with deep git history, large volumes of issues and pull requests, and constant activity around the clock). Resource-level progress tracking surfaces failures before cutover, so you can make an informed decision about when to proceed.


Use ELM alongside GitHub Enterprise Importer (GEI): ELM complements GEI, giving you flexibility to choose the right tool for each repository based on its size, shape, and activity. Use GEI for straightforward migrations where brief downtime is acceptable, and ELM for the repositories that need a zero-disruption approach. Run them concurrently as part of the same migration strategy.


Manage migrations with the gh elm extension: Install the GitHub CLI extension to configure credentials and manage the ELM lifecycle &mdash; from creating and monitoring a migration to initiating cutover &mdash; through the GHES REST API. Human-readable output helps you track progress, while JSON output supports scripting and automation.



ELM runs as a service on your GHES appliance, driven by the gh elm CLI. ELM supports migrations from GHES versions 3.17.18+, 3.18.12+, 3.19.9+, 3.20.3+, 3.21.3+, and 3.22.0+. This release ships with the most recent GHES patch releases, with plans to expand to additional migration paths.
To get started, check out the Enterprise Live Migrations documentation.


The post Enterprise Live Migrations from GHES to ghe.com generally available appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-09-01-enterprise-live-migrations-from-ghes-to-ghe-com-generally-available](https://github.blog/changelog/2026-09-01-enterprise-live-migrations-from-ghes-to-ghe-com-generally-available)*
