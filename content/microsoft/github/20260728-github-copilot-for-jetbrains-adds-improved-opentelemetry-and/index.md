---
categories:
- MS
- GitHub
date: '2026-07-28T01:23:10+00:00'
description: This update brings more control and clarity to your GitHub Copilot for
  JetBrains workflows. You can now connect MCP servers and custom agents in Claude
  agent fl
draft: false
original_url: https://github.blog/changelog/2026-07-27-github-copilot-for-jetbrains-adds-improvved-opentelemetry-configuration-and-model-management
source: GitHub Changelog
tags:
- Release
- copilot
- enterprise management tools
title: GitHub Copilot for JetBrains adds improved OpenTelemetry configuration and
  model management
---

This update brings more control and clarity to your GitHub Copilot for JetBrains workflows. You can now connect MCP servers and custom agents in Claude agent flows, tune telemetry and token settings for advanced scenarios, and work with a cleaner chat and model-selection experience.
What&rsquo;s new
OpenTelemetry export for agent workflows
You can now configure OpenTelemetry export settings for agent workflows. This makes it easier to align plugin behavior with your organization&rsquo;s requirements for observability. You can configure this under Settings &gt; Tools &gt; GitHub Copilot &gt; Chat.

More control over model behavior
You can now set default token limits, including maxInputToken and maxOutputToken, for BYOK and custom endpoints. You can also disable or enable all built-in Copilot models from model-management controls.
These options make it easier to align plugin behavior with your organization&rsquo;s requirements for cost control and model governance.

MCP servers and custom agents in Claude agent flows
You can now use MCP servers and custom agents directly in Claude agent flows. This gives you more flexibility when you need specialized tools, custom instructions, or team-specific workflows in your IDE.
If you rely on shared agent setups across projects, this update helps you keep your flow consistent while still adapting to repository-specific needs.

More Copilot CLI session capabilities
Copilot CLI sessions now support forks, include the /rubber-duck command, and show a todo list in the harness. These additions help you break down work, reason through implementation ideas, and keep progress visible while you iterate.
Cost efficiency
For enterprise users, we now display the number of AI credits consumed when their organization has not configured a user-level budget. For more information, see our docs about user-level budgets.
User experience enhancements
We are also enhancing day-to-day experience across chat, inline chat, and model selection.

Model and action picker: Improved consistency so controls are easier to predict and use.
Customization flows: Improved usability so creating and managing setup details takes less effort.
Session prompts: Improved clarity by rendering ask-user questions as Markdown and adding explicit user-attention notifications.
Inline chat and model picker layout: Improved layout behavior for cleaner interactions.
MCP diagnostics: Improved diagnostics to help you understand configuration and runtime issues faster.
URL rendering in Copilot CLI harness: Improved bare URL display for better readability in chat output.

Quality improvements
This release also improves path handling and session recording. Copilot CLI now preserves path capitalization more reliably in working sets and snapshots on macOS and Linux.
Try it out
We encourage you to try out the latest version of the GitHub Copilot plugin and share your feedback. Your input is invaluable in helping us refine and improve the product.
Share your feedback
Your feedback drives improvements. We&rsquo;d love to hear about your experience in the following channels:

Private survey: Share your feedback in this short survey.
Feedback repository: Share your thoughts in the GitHub Copilot for JetBrains IDEs issues.


The post GitHub Copilot for JetBrains adds improved OpenTelemetry configuration and model management appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-27-github-copilot-for-jetbrains-adds-improvved-opentelemetry-configuration-and-model-management](https://github.blog/changelog/2026-07-27-github-copilot-for-jetbrains-adds-improvved-opentelemetry-configuration-and-model-management)*
