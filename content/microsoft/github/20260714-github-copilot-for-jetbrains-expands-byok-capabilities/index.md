---
categories:
- MS
- GitHub
date: '2026-07-14T23:37:29+00:00'
description: This update brings major advances in customization and model provider
  flexibility to all tiers of GitHub Copilot for JetBrains IDEs. With richer plugin
  and prov
draft: false
original_url: https://github.blog/changelog/2026-07-14-github-copilot-for-jetbrains-expands-byok-capabilities
source: GitHub Changelog
tags:
- Improvement
- copilot
title: GitHub Copilot for JetBrains expands BYOK capabilities
---

This update brings major advances in customization and model provider flexibility to all tiers of GitHub Copilot for JetBrains IDEs. With richer plugin and provider experiences, improved conversational interactions, and stronger reliability, teams can more confidently tailor Copilot to how they build software.
What&rsquo;s new
Bring your own key custom endpoint support
We&rsquo;ve expanded bring your own key support with custom endpoints. You can now configure OpenAI-compatible custom endpoints with API keys to use your own models.

Expanded customizations with plugin management
GitHub Copilot for JetBrains now includes a more complete plugin management experience in customizations. You can browse and install plugins through the marketplace or from the source repository.
This makes it easier to shape Copilot around team-specific workflows without jumping between disconnected setup surfaces.

Claude agent provider customizations support
Customizations now support Claude agent provider, allowing you to set up custom agents, skills, and instructions. Available for GitHub Copilot Pro and higher plans in public preview.

Local sandboxing support
This release adds support for local sandboxing, including new sandbox settings and configuration flows in the JetBrains plugin. This feature is in public preview. For more information, see About cloud and local sandboxes.

Built-in debugger skill for Copilot CLI
This release adds a built-in debugger skill for Copilot CLI sessions, enabling agent-driven debugging workflows directly in your development environment. It helps you investigate issues step by step with guided debugging support. This feature is in public preview.
User experience enhancements
This version also improves usability across chat, model selection, customization views, and CLI sessions.

Improved model picker controls in chat and inline chat
Improved UI clarity for custom agents, customizations, and provider settings
Improved authentication UX messaging
Added message re-edit support in inline and CLI experiences for faster prompt iteration and follow-up requests

Quality improvements
We&rsquo;ve improved reliability and stability across authentication recovery, account switching, provider and session persistence, and editor interaction paths. These fixes reduce friction in long-running sessions and make Copilot behavior more consistent when switching modes, providers, or work contexts.
Changed
We&rsquo;ve adjusted Copilot CLI provider policy handling for CLI-as-default scenarios. Disabling Copilot CLI by policy no longer affects Copilot CLI provider in JetBrains IDEs.
Try it out
We encourage you to try out the latest version of the GitHub Copilot plugin and share your feedback. Your input is invaluable in helping us refine and improve the product.
Share your feedback
Your feedback drives improvements. We&rsquo;d love to hear about your experience in the following channels:

In-product feedback: Use the feedback options within your IDE.
Feedback repository: Share your thoughts in the GitHub Copilot for JetBrains IDEs issues.


The post GitHub Copilot for JetBrains expands BYOK capabilities appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-14-github-copilot-for-jetbrains-expands-byok-capabilities](https://github.blog/changelog/2026-07-14-github-copilot-for-jetbrains-expands-byok-capabilities)*
