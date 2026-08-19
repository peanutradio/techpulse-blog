---
categories:
- MS
- GitHub
date: '2026-08-18T23:33:07+00:00'
description: GitHub Copilot for JetBrains now supports enterprise managed settings
  for plugin governance, MCP server access, OpenTelemetry, and permission modes. Administrat
draft: false
original_url: https://github.blog/changelog/2026-08-18-enterprise-managed-settings-in-github-copilot-for-jetbrains
source: GitHub Changelog
tags:
- Release
- copilot
- enterprise management tools
title: Enterprise managed settings in GitHub Copilot for JetBrains
---

GitHub Copilot for JetBrains now supports enterprise managed settings for plugin governance, MCP server access, OpenTelemetry, and permission modes. Administrators can now apply consistent controls for everyone on your enterprise&rsquo;s Copilot plan.
Enterprise-managed plugin governance
Administrators can manage Copilot plugins and their marketplaces in JetBrains IDEs. The supported settings provide three controls:

Enabled plugins: Use enabledPlugins to require a plugin to be enabled or disabled.
Additional marketplaces: Use extraKnownMarketplaces to make approved plugin sources available.
Restricted marketplaces: Use strictKnownMarketplaces to limit installation to approved sources.

MCP server allowlist
Administrators can use allowedMcpServers and deniedMcpServers to centrally control which MCP servers developers can connect to from GitHub Copilot for JetBrains. This brings centrally managed MCP governance into JetBrains IDEs and prevents connections to servers outside the enterprise allowlist.
Managed OpenTelemetry
Administrators can centrally configure OpenTelemetry for Copilot in JetBrains IDEs, including the collector endpoint, protocol, service name, resource attributes, and content-capture policy. Managed values take precedence over developer settings, so telemetry is consistently routed to the approved collector.
Developers can review the applied configuration under Settings &gt; Tools &gt; GitHub Copilot &gt; Chat &gt; OpenTelemetry.
Organization-controlled permission modes
Administrators can set permissions.disableBypassPermissionsMode to disable to prevent the Copilot agent in JetBrains from using Bypass Approvals or Autopilot.
Try it out
Try the latest version of the GitHub Copilot plugin for JetBrains and share your feedback. For more details on enterprise managed settings, see enterprise managed settings reference.
Share your feedback
Your feedback drives improvements. We&rsquo;d love to hear about your experience in the following channels:

In-product feedback: Use the feedback options within your JetBrains IDE.
Feedback repository: Share your experience in the Copilot IntelliJ feedback repository.
GitHub Community: Join the discussion in GitHub Community.


The post Enterprise managed settings in GitHub Copilot for JetBrains appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-18-enterprise-managed-settings-in-github-copilot-for-jetbrains](https://github.blog/changelog/2026-08-18-enterprise-managed-settings-in-github-copilot-for-jetbrains)*
