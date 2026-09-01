---
categories:
- MS
- GitHub
date: '2026-08-31T08:39:54+00:00'
description: 'This changelog covers VS Code v1.132 through v1.135, shipped throughout
  August 2026.

  These releases make it easier to organize agent sessions, review changes, a'
draft: false
original_url: https://github.blog/changelog/2026-08-31-github-copilot-in-vs-code-august-2026-releases
source: GitHub Changelog
tags:
- Release
- copilot
title: GitHub Copilot in VS Code, August 2026 releases
---

This changelog covers VS Code v1.132 through v1.135, shipped throughout August 2026.
These releases make it easier to organize agent sessions, review changes, and navigate long conversations. Agent Host, the integrated browser, and dictation also get updates to support more ways of working in VS Code.
Agent sessions and workflows
The Agents window adds more ways to organize related chats, follow agent activity, and move between prompts and their file changes.

Arrange chats side by side: Keep multiple chats visible in horizontal or vertical groups to compare results or follow your agent&rsquo;s work, with the layout restored when you return to the session.
Open a side-conversation: Type /btw to open a side chat that shares the primary chat&rsquo;s context and prompt cache while it continues running.
Navigate the chat conversation: Use the prompt timeline control from the transcript gutter to easily navigate to specific prompts in chat and review the related file changes.
Install portable agent plugins: Install agent customizations from plugins that follow the Agent Plugins 1.0 standard across VS Code and other compatible agent clients.
Open the Agents window without GitHub sign-in: Enable the experimental setting to open the Agents window without GitHub sign-in when Claude is configured with an API key.
Switch model providers in Claude sessions: Choose between models from your Anthropic subscription and Copilot subscription at any time.
Continue external agent sessions: View and continue recent Copilot or Claude agent sessions created in other applications from the Sessions list in VS Code.
Connect windows to the same session: Use the Agent Host to connect to the same agent session from multiple VS Code windows.
Get a second opinion: Try the experimental /rubber-duck command in a Copilot Agent Host session to ask a complementary model to surface missed details and edge cases.
Review session details next to chat: Use the default single-pane layout to open session details and editors in one side pane, with diffs that adapt to the available width.

See it in action in our Agent Host introduction YouTube video.

		
			
		
Chat and review
Chat updates make it easier to find information in long conversations and review changes in agent-generated content.

Find text across a conversation: Avoid endless scrolling to find a search term with text search across the complete chat transcript with options for case matching, whole words, and regular expressions.
Keep the current prompt visible: Don&rsquo;t lose track of which prompt a response belongs to with chat sticky scroll, which keeps the current prompt pinned while you scroll through a long conversation.
Review rendered Markdown diffs: Combine viewing diffs and editing Markdown in a single experience with the experimental hybrid Markdown editor.
Switch editor types from the breadcrumb bar: Switch between regular and diff editors from the breadcrumb bar in the Agents window.
Resize terminal output in chat: Expanded terminal output in Chat now reflows as you resize the view, making it easier to read at different widths.
View token usage by model: Hover over the response footer to see input, cached input, and output token usage for each model in a chat turn.


Integrated browser
The integrated browser makes it easier to give agents feedback on web pages and see local HTML changes as you work.

Comment on web page elements: Select and annotate several HTML elements on a page to provide targeted UI feedback, which the agent can then address in batch.
Reload HTML files automatically: See saved changes and agent edits immediately as local HTML files automatically reload when they change on disk.
Open HTML files in the browser by default: If you often preview local HTML files instead of editing them, you can now set the integrated browser as their default editor by configuring the editor associations setting.


Dictation
Built-in dictation now supports multiple languages, project-specific instructions, and shell-aware cleanup for terminal commands.

Dictate in multiple languages: Dictate in multiple languages with an on-device model. Choose your language or let VS Code automatically detect it.
Customize transcript cleanup: Add user-level or workspace-level instructions so dictation follows your project terminology and formatting preferences.
Improve dictation for terminal commands: Shell-aware cleanup preserves command syntax instead of inserting spoken punctuation literally.


Browse the full release notes for VS Code 1.132, 1.133, 1.134 and 1.135 to explore everything that&rsquo;s new.
Download the latest version of VS Code and, as always, happy coding!

The post GitHub Copilot in VS Code, August 2026 releases appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-31-github-copilot-in-vs-code-august-2026-releases](https://github.blog/changelog/2026-08-31-github-copilot-in-vs-code-august-2026-releases)*
