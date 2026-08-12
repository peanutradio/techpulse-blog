---
categories:
- MS
- GitHub
date: '2026-08-11T20:15:55+00:00'
description: This update brings persistent memory, local model access, and more enterprise
  controls to GitHub Copilot for JetBrains. It also improves everyday chat workflows
draft: false
original_url: https://github.blog/changelog/2026-08-11-copilot-memory-and-ollama-in-github-copilot-for-jetbrains
source: GitHub Changelog
tags:
- Release
- copilot
- enterprise management tools
title: Copilot memory and Ollama in GitHub Copilot for JetBrains
---

This update brings persistent memory, local model access, and more enterprise controls to GitHub Copilot for JetBrains. It also improves everyday chat workflows and resolves reliability issues across MCP servers, terminals, customizations, and cloud agents.
What&rsquo;s new
Enterprise managed settings
Administrators have more server-based controls for managing GitHub Copilot across their organizations. These enterprise managed settings cover plugin availability, MCP server access, permission bypass behavior, and OpenTelemetry settings.
Read more about enterprise managed settings.
Copilot memory across chat sessions
Copilot memory can now retain and recall useful information across agent chat sessions. This helps you maintain context between conversations instead of repeatedly providing the same project details or preferences.
You can manage the feature with the Copilot Memory toggle in the Copilot settings portal. For more information, see About Copilot memory.

Ollama as a BYOK provider
You can now use Ollama as a BYOK provider in GitHub Copilot for JetBrains. The integration supports provider configuration and model selection throughout the JetBrains experience, giving you another way to work with models that fit your development environment.

Expanded Codex workflows
Codex sessions are now visible in agent debug logs. Codex workflows also support updated permission modes and customizations through instructions and skills, helping you adapt agent behavior to your project.

Easier Copilot CLI setup
GitHub Copilot for JetBrains can now automatically install Copilot CLI from integrated terminals on macOS, Linux, and Windows. This reduces setup steps and makes it easier to start using terminal-based agent workflows directly from your IDE.
User experience enhancements
This release improves common account, model, and agent interactions:

Account management: Improved account management flows by supporting account switch/removal and polishing account-deletion interactions.
Model and settings views: Improved model and settings usability by capping long model-name width in the model picker and refining model view/todo list panel behavior.
Chat references: Restored file and folder # references in Copilot, Claude, and Codex chat inputs.
Customization: Moved the customization button to the top of Copilot chat.
Agent debug logs: Moved the agent debug logs button to the Options dropdown at the top of Copilot chat.


Quality improvements
This release improves reliability for MCP execution and approvals, terminal output and auto-approval, customizations, cloud agents, and diff-based editing. It also fixes ANSI escape rendering and makes terminal scrollbars more predictable.
Changed
User-facing product strings now use &ldquo;Copilot&rdquo; instead of &ldquo;Copilot CLI.&rdquo;
Try it out
Try the latest version of the GitHub Copilot plugin and share your feedback.
Share your feedback
Your feedback drives improvements. We&rsquo;d love to hear about your experience in the following channels:

In-product feedback: Use the feedback options within your IDE.
Feedback repository: Share your thoughts in the GitHub Copilot for JetBrains IDEs issues.


The post Copilot memory and Ollama in GitHub Copilot for JetBrains appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-11-copilot-memory-and-ollama-in-github-copilot-for-jetbrains](https://github.blog/changelog/2026-08-11-copilot-memory-and-ollama-in-github-copilot-for-jetbrains)*
