---
categories:
- MS
- GitHub
date: '2026-07-30T14:54:47+00:00'
description: Enterprises and organizations can now restrict which devices are eligible
  to host remotely controlled Copilot sessions, giving administrators fine-grained
  contr
draft: false
original_url: https://github.blog/changelog/2026-07-30-limit-remote-control-to-managed-devices
source: GitHub Changelog
tags:
- Improvement
- copilot
title: Limit remote control to managed devices
---

Enterprises and organizations can now restrict which devices are eligible to host remotely controlled Copilot sessions, giving administrators fine-grained control over where remote control access is permitted.
The new remoteControl enterprise managed setting gives administrators the ability to define exactly how remote control works on managed devices, whether that means requiring SSO authorization for specific organizations or tailoring access on a per-device basis. This setting works alongside the existing enterprise policy, which controls whether remote control is available to users at all. Together, this gives you layered control from broad access down to per-device restrictions.
Getting started
Set mode to requireSSO to enforce SSO authorization, disabled to block remote control entirely, or enabled to allow it without restriction.
You can also add the remoteControl key to your copilot-settings.json file.
You can deploy this setting through three mechanisms:

Server-managed (.github-private repository): Applies to user accounts in your enterprise.
MDM-managed: Applies to specific devices and is ideal for enforcing policies on managed machines.
File-based: Applies to any machine that receives the file.

For more information on enterprise managed settings, check out the GitHub docs.

The post Limit remote control to managed devices appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-30-limit-remote-control-to-managed-devices](https://github.blog/changelog/2026-07-30-limit-remote-control-to-managed-devices)*
