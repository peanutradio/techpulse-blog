---
categories:
- MS
- GitHub
date: '2026-07-30T08:00:00+00:00'
description: 'This changelog covers VS Code v1.127 through v1.131, shipped throughout
  July 2026.

  These releases improve how you work with agents, review changes, use chat, an'
draft: false
original_url: https://github.blog/changelog/2026-07-30-github-copilot-in-visual-studio-code-july-2026-releases
source: GitHub Changelog
tags:
- Release
- copilot
title: GitHub Copilot in Visual Studio Code, July 2026 releases
---

This changelog covers VS Code v1.127 through v1.131, shipped throughout July 2026.
These releases improve how you work with agents, review changes, use chat, and navigate VS Code. They also add new browser, accessibility, and dictation features.
Agents window improvements
The Agents window, currently in public preview, adds a redesigned layout, faster review workflows, and improvements for managing multiple sessions.

Review code alongside chat: The editor panel has a new design. Open files and diffs next to your conversation. A shared tab bar keeps everything in one place.
Review changes faster: See the addition and deletion counts for each file, use a more compact diff view, and switch between inline and side-by-side diffs.
Use worktrees with any harness: Start Copilot, Claude, or Codex sessions in a Git worktree, so each session can work in an isolated copy of your repository.
Start a quick chat: Ask a question without opening a workspace. Quick chats stay separate from your project sessions.
Group and reorder sessions: Group related sessions and reorder the list by dragging items, groups, and workspace headers.
Track running subagents: See each subagent&rsquo;s model, elapsed time, and active tool call, then open its conversation without losing the parent conversation.
Handle pull request updates from chat: Act on failed CI checks and new review comments from a banner above the chat input.


Multi-chat sessions
You can now keep related conversations in one session. This makes it easier to compare ideas, split up work, and explore other approaches and work in parallel.

Multi-chat support for Claude: A single agent session can now hold multiple related chats, each with its own history, title, and model, instead of just one linear conversation.
Fork into a peer chat: Explore a different approach from any point in your conversation. The new peer chat keeps the original context, so you don&rsquo;t have to repeat it.
Close, reopen, and delete chats: Hide a chat from its tab, bring it back from the Conversations dropdown, or remove it for good from the tab menu.
Keyboard-driven navigation: Create, switch, reopen, and close chats without leaving the keyboard, with shortcuts scoped to the Agents window.


Chat and model updates
Chat now shows more information about your conversations and gives you more control over the models you use.

Add images and PDFs to chat: Copilot vision is now generally available. Paste a file, drag it into chat, or add it from the context menu.
Check your AI credit usage: Copilot Business and Enterprise users can see their total credit usage for the current billing cycle in the Copilot status menu.
Use BYOK models in the Agents window: BYOK models have been available in the VS Code editor since the 1.99 (March 2025) release. Now you can also use them with the Copilot agent in the Agents window.
Run commands with the ! prefix: Start a chat message with ! to run its contents as a terminal command in the Agents window.


Editor, terminal, and browser
Editor, terminal, and browser updates improve navigation, customization, and everyday workflows across VS Code.

Modern UI preview: Try the modernized VS Code user interface, available as an experimental feature in Stable and enabled by default in Insiders. Share your feedback with us.
Open files from git diff: Select a file path in terminal diff output to open it directly. VS Code now recognizes Git-prefixed paths such as i/ and w/.
Configurable browser tab placement: Choose where integrated browser tabs open: the active group, a dedicated side group, or a separate window.
Use shortcuts when VS Code is in the background: Create OS-level keyboard shortcuts that can run VS Code commands even when another app is in focus.
Migrate prompt files to skills: Turn existing prompt files into reusable skills from the AI Customizations overview.
Switch editor type from the toolbar: Quickly switch between different editor views, such as previewing a diff in the Markdown editor instead of the default diff view.
Edit Markdown with agents: This experimental feature lets you view and edit Markdown files in place in the Agents window, and add comments that an agent can act on.


Accessibility and dictation
Accessibility updates bring built-in dictation and improved screen reader support for the integrated terminal.

Built-in dictation: Enable the experimental dictation.enabled setting to use dictation in chat, the editor, and the integrated terminal.
Optional transcript cleanup: Enable the experimental dictation.experimental.llmCleanup setting to let Copilot tidy your transcript as you speak by adding formatting and removing filler words.
Terminal screen reader control: Keep the cursor in place in the terminal Accessible View as new output arrives, and read at your own pace without repeated interruptions.


Browse the full release notes for Visual Studio Code 1.127, 1.128, 1.129, 1.130, and 1.131 to explore everything that&rsquo;s new.
Download the latest version of Visual Studio Code and, as always, happy coding!

The post GitHub Copilot in Visual Studio Code, July 2026 releases appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-30-github-copilot-in-visual-studio-code-july-2026-releases](https://github.blog/changelog/2026-07-30-github-copilot-in-visual-studio-code-july-2026-releases)*
