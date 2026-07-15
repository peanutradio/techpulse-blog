---
categories:
- MS
- GitHub
date: '2026-07-15T00:01:24+00:00'
description: June 2026 is about visibility and trust with a clearer view of your GitHub
  Copilot usage, a new trust layer for MCP servers, and the first C++ scenarios for
  the
draft: false
original_url: https://github.blog/changelog/2026-07-14-github-copilot-in-visual-studio-june-update
source: GitHub Changelog
tags:
- Release
- copilot
title: GitHub Copilot in Visual Studio — June update
---

June 2026 is about visibility and trust with a clearer view of your GitHub Copilot usage, a new trust layer for MCP servers, and the first C++ scenarios for the modernization agent reaching general availability.
Highlights
Here&rsquo;s what&rsquo;s new with GitHub Copilot in Visual Studio 2026. Check the Insiders channel for the latest:

Copilot usage tracking and alerts: The refreshed Copilot Usage window reflects GitHub Copilot&rsquo;s usage-based billing model with real-time updates. In addition, proactive alerts let you know when you&rsquo;re approaching your limit, when you&rsquo;ve hit it, and when overages activate. Open it from Copilot badge menu &gt; Copilot Usage and tune the warning threshold in settings to decide how early you get the notification.
Trust validation for MCP servers: Visual Studio now compares an MCP server&rsquo;s configuration and asset fingerprint against a trusted baseline at startup. If anything has changed, a trust dialog asks you to review and approve the change before the server runs. This feature is on by default and lives under Tools &gt; Options &gt; GitHub &gt; Copilot &gt; Copilot Chat &gt; Show trust dialog before running tools from an updated MCP server.
GitHub Copilot modernization agent for C++ is generally available: The MSVC upgrade scenarios for the modernization agent have graduated from preview. Run it in Automated mode for end-to-end upgrades or in Guided mode to review the assessment, plan, and execution before each step. Right-click a project in Solution Explorer and select Modernize or use @Modernize in Copilot Chat.
Long-distance next edit suggestions: Copilot&rsquo;s next edit suggestions can now predict and propose follow-up edits anywhere in the active file, not just near your cursor. Turn it on under Tools &gt; Options &gt; Text Editor &gt; Inline Suggestions by selecting Enable extended range suggestions.
Add pull requests to Copilot Chat: Right-click a pull request in the Git Repository window and select Add to Copilot Chat. Copilot will then pick up the pull request description, changed files, and comments as context. You can also reference a pull request inline by typing # followed by the pull request ID. This functionality requires View pull requests for a Git repository under Preview Features.
Review and approve pull requests in the IDE: The new in-IDE pull request review experience pairs naturally with Add to Copilot Chat. Browse, comment, approve, and complete pull requests from GitHub or Azure DevOps without leaving Visual Studio, then pull any pull request into Copilot Chat when you want help triaging or summarizing.

This update is available to users on all GitHub Copilot plans, including Copilot Free, Student, Pro, Pro+, Max, Business, and Enterprise.
Download Visual Studio 2026 to experience all the new Copilot features today. To learn more about what&rsquo;s new, check out the Visual Studio blog and release notes.
What&rsquo;s next for Copilot in Visual Studio
Stay up to date on the latest Copilot features by following the Visual Studio blog, where you&rsquo;ll find roadmap updates and opportunities to share feedback.

The post GitHub Copilot in Visual Studio — June update appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-14-github-copilot-in-visual-studio-june-update](https://github.blog/changelog/2026-07-14-github-copilot-in-visual-studio-june-update)*
