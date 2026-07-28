---
categories:
- MS
- GitHub
date: '2026-07-27T17:00:35+00:00'
description: You can now govern the GitHub Copilot app and Copilot cloud agent with
  enterprise managed settings, the same centrally managed policies you use to control
  Copil
draft: false
original_url: https://github.blog/changelog/2026-07-27-enterprise-managed-settings-now-apply-to-the-github-copilot-app
source: GitHub Changelog
tags:
- Release
- client apps
- copilot
- enterprise management tools
title: Enterprise managed settings in the GitHub Copilot app and Copilot cloud agent
---

You can now govern the GitHub Copilot app and Copilot cloud agent with enterprise managed settings, the same centrally managed policies you use to control Copilot across your enterprise. With a managed-settings.json file, enterprise owners define one set of guardrails, such as which plugins and marketplaces developers can use and whether they can bypass approval prompts. Copilot clients automatically enforce these settings for everyone on your enterprise&rsquo;s Copilot plan.
As your developers adopt Copilot across more surfaces, you&rsquo;re accountable for applying the same governance everywhere they work. Any client that sits outside your policy is a gap, a place where someone could install a plugin you haven&rsquo;t vetted or run a command you&rsquo;d normally gate. Your governance is only as strong as its least-covered surface.
The Copilot app and cloud agent now join Copilot CLI and VS Code as supported clients for enterprise managed settings, so your guardrails follow your developers into the app and cloud agent tasks. You define your policy once, and it&rsquo;s enforced consistently wherever your teams build. That&rsquo;s the cross-client consistency and high-trust teams need to adopt Copilot with confidence.
Bring every client under the same guardrails
The Copilot app reads the same managed-settings.json you already use for your other clients. You can govern things like:

Which plugins are available.
Which plugin marketplaces developers can install from.
Whether developers can bypass approval prompts before Copilot runs commands, accesses files, or fetches URLs.
Setting auto model selection as the default for new conversations.

The Copilot cloud agent reads the applicable managed settings, including those for plugins and marketplace controls. It only uses the plugins and marketplaces you&rsquo;ve approved. Bypass-prompt controls only apply to the interactive clients (i.e., the app, Copilot CLI, and VS Code).
For each supported key, your managed value takes precedence over anything a developer sets locally. See the full list of options in the enterprise managed settings reference.
If you already deploy managed-settings.json for Copilot CLI and VS Code, there&rsquo;s nothing new to set up. The Copilot app automatically picks up your existing configuration the next time a developer signs in or restarts the app, and the cloud agent observes changes on the next task assignment.
Getting started
If you&rsquo;re setting up enterprise managed settings for the first time, the default approach is server-managed deployment:

Create and configure a .github-private repository in your enterprise. For more information, see our guide.
In that repository, create or update copilot/managed-settings.json.
Add your enterprise policy keys and values in JSON, then commit and push to the default branch.

Supported clients apply updated settings within about an hour, immediately after a developer restarts the client, or when a developer signs back in. You can also deploy through MDM or a distributed file.
To learn more, see configuring enterprise managed settings.
Join the discussion within GitHub Community.

The post Enterprise managed settings in the GitHub Copilot app and Copilot cloud agent appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-27-enterprise-managed-settings-now-apply-to-the-github-copilot-app](https://github.blog/changelog/2026-07-27-enterprise-managed-settings-now-apply-to-the-github-copilot-app)*
