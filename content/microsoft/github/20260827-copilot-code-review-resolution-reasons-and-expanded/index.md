---
categories:
- MS
- GitHub
date: '2026-08-27T22:46:04+00:00'
description: 'Copilot code review can now review two types of pull requests it didn&rsquo;t
  cover before:


  Reviews requested automatically on pull requests authored by bots, '
draft: false
original_url: https://github.blog/changelog/2026-08-27-copilot-code-review-resolution-reasons-and-expanded-capabilities
source: GitHub Changelog
tags:
- Improvement
- copilot
title: 'Copilot code review: Resolution reasons and expanded capabilities'
---

Copilot code review can now review two types of pull requests it didn&rsquo;t cover before:

Reviews requested automatically on pull requests authored by bots, including Copilot cloud agent
Very large pull requests

Additionally, you can now submit the reason for why you&rsquo;re resolving a particular Copilot code review comment.
&#129302; Expanded capabilities
Pull requests authored by bots
When a pull request is authored by a bot and requested automatically, there&rsquo;s no Copilot-licensed account to attribute the review to. With the &ldquo;Allow members without a Copilot license to use Copilot code review in GitHub.com&rdquo; policy enabled, Copilot code review can now review these pull requests and bill the usage directly to your organization. To learn more, see Copilot code review without a Copilot license.
Copilot cloud agent pull requests
Previously, when Copilot code review was automatically requested due to automatic review settings on pull requests authored by Copilot cloud agent, Copilot code review would fall back to a limited experience. Copilot code review can now give pull requests opened by Copilot cloud agent a full agentic review.
Large pull requests
Copilot code review previously had a 300 file or 20,000 lines of code limit on the size of a pull request it could review. This limitation no longer applies.
&#9745;&#65039; Comment resolution reasons
You can now specify the reason for resolving a Copilot code review comment by either selecting &ldquo;Addressed&rdquo;, &ldquo;Won&rsquo;t fix&rdquo;, or &ldquo;Incorrect&rdquo;, after clicking the new dropdown next to the &ldquo;Resolve conversation&rdquo; button, located at the bottom of any Copilot code review comment. Selecting one of these options provides valuable feedback to the product team and helps improve the product.


The post Copilot code review: Resolution reasons and expanded capabilities appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-27-copilot-code-review-resolution-reasons-and-expanded-capabilities](https://github.blog/changelog/2026-08-27-copilot-code-review-resolution-reasons-and-expanded-capabilities)*
