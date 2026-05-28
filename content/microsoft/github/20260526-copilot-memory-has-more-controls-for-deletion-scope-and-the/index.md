---
categories:
- MS
- GitHub
date: '2026-05-26T21:05:28+00:00'
description: Copilot Memory now includes improved memory deletion, adds a repository-level
  off switch, and brings further memory controls into the Copilot CLI. Copilot Memor
draft: false
original_url: https://github.blog/changelog/2026-05-26-copilot-memory-has-more-controls-for-deletion-scope-and-the-copilot-cli
source: GitHub Changelog
tags:
- Improvement
- copilot
title: Copilot Memory has more controls for deletion, scope, and the Copilot CLI
---

Copilot Memory now includes improved memory deletion, adds a repository-level off switch, and brings further memory controls into the Copilot CLI. Copilot Memory is in public preview and available to all paid Copilot plans.
What&rsquo;s changing

Deletion guidance: When you ask Copilot to forget something, it now points you to the right place to remove the memory and down-votes the memory where voting is available.
Repository-level off switch: Repository admins can now disable Copilot Memory for a repository from the existing Copilot feature controls in repository settings. Repository-level facts will no longer be stored or read. Preexisting facts are not deleted. User-level preferences are unaffected.
/memory command in the Copilot CLI: Use /memory on to enable Copilot Memory, /memory off to disable it, and /memory show to check its current status. Your choice persists across sessions.
Clearer scope at capture time: The store_memory permission prompt now explicitly states whether the entry will be a user-level preference (i.e., visible only to you, used in your sessions across repositories) or a repository-level fact (i.e., visible to all contributors on the repository).

Managing memories

Your user-level preferences: Review or delete them in your personal Copilot Memory settings.
Your repository&rsquo;s facts: Repository owners can review, delete, or turn off Copilot Memory for the repository under Repository Settings &gt; Copilot &gt; Memory.

Learn more about managing stored memories.
Share your feedback
For more information, see About GitHub Copilot Memory.
We&rsquo;re continuing to evolve Copilot Memory and plan to bring it to more features in the future. To share feedback or join the discussion, visit the GitHub Community.

The post Copilot Memory has more controls for deletion, scope, and the Copilot CLI appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-05-26-copilot-memory-has-more-controls-for-deletion-scope-and-the-copilot-cli](https://github.blog/changelog/2026-05-26-copilot-memory-has-more-controls-for-deletion-scope-and-the-copilot-cli)*
