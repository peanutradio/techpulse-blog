---
categories:
- MS
- GitHub
date: '2026-09-03T19:05:40+00:00'
description: 'The current PGP key for the GitHub CLI Linux package repositories expires
  on Saturday, September 5, 2026. Beginning with the first release after that date,
  APT '
draft: false
original_url: https://github.blog/changelog/2026-09-03-github-cli-linux-package-signing-key-expires-september-5
source: GitHub Changelog
tags:
- Retired
- client apps
title: GitHub CLI Linux package signing key expires September 5
---

The current PGP key for the GitHub CLI Linux package repositories expires on Saturday, September 5, 2026. Beginning with the first release after that date, APT and RPM repository metadata and newly published RPM packages will be signed with just the replacement key.
In April, we published a keyring containing both the current and replacement keys. If you installed gh from the official APT or RPM repositories before April 8, 2026, and have not updated the setup since, follow the instructions in the full announcement before September 5. If you installed later, do not remember, or use a custom image or automation path, follow the steps in the announcement to verify that your system trusts the replacement key.
This change does not affect you if you:

Use Windows or macOS.
Build from source.
Install gh through Homebrew, Conda, or a community package manager.
Use a direct .deb file or standalone archive binary from GitHub Releases.


The post GitHub CLI Linux package signing key expires September 5 appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-09-03-github-cli-linux-package-signing-key-expires-september-5](https://github.blog/changelog/2026-09-03-github-cli-linux-package-signing-key-expires-september-5)*
