---
categories:
- MS
- GitHub
date: '2026-07-23T15:30:29+00:00'
description: Agent automations increasingly label, type, assign, and close issues
  for you. GitHub Issues now shows the reason behind each change and lets you review
  them bef
draft: false
original_url: https://github.blog/changelog/2026-07-23-agent-automation-controls-in-github-issues-in-public-preview
source: GitHub Changelog
tags:
- Release
- collaboration tools
- copilot
- projects &amp; issues
title: Agent automation controls in GitHub Issues in public preview
---

Agent automations increasingly label, type, assign, and close issues for you. GitHub Issues now shows the reason behind each change and lets you review them before they&rsquo;re applied, so you decide how much to automate and when to stay in the loop.

What&rsquo;s new
We&rsquo;ve released three new capabilities:

Approvals: Prompt your automation to suggest instead of applying, and the changes wait in a panel on the issue instead of taking effect. Accept or decline each one, or accept or decline all at once.
Confidence: Agents rate each supported action high, medium, or low confidence. High-confidence changes apply automatically. GitHub Issues holds medium and low ones as suggestions for you to review.
Rationale: Every supported action records the reason behind it, whether it applies automatically or waits for review. You get an audit trail of what changed and why, and can see the reasoning on each suggestion before making a decision.

Use has:suggestions in issue search to find issues with changes waiting for review. Repository admins can also configure the automation level to set the confidence threshold, controlling which changes apply automatically and which are held for review.
Reviewing changes is optional. If you tell your automation to apply directly, it will. If you tell it to suggest, you get a review step. A small team moving fast might let the agent run on its own, while a busy public repository can hold changes for review. Because GitHub Issues hold back low-confidence actions, you only spend time on the changes most likely to need a second look.
Approvals are a workflow convenience, not a security control. They don&rsquo;t enforce a server-side boundary, and an agent with permission to change issues can directly apply changes rather than suggest them.
Supported platforms and actions
Rationale, confidence, and approvals work with GitHub Agentic Workflows and Copilot cloud agent automations, and are also available through the REST and GraphQL APIs. At launch, they cover changes to labels, fields, type, close, and assignees.
Try it with GitHub Agentic Workflows
If you already use GitHub Agentic Workflows, upgrade your workflows to add issue intent support. After upgrading, supported safe outputs include intent information automatically while remaining backward compatible. To require intents for a specific safe output, set issue-intents: true in your workflow frontmatter.
Set issue-intents: false to explicitly opt out. Issue intents are supported for the set-issue-type, set-issue-field, add-labels, close-issue, assign-to-agent, and assign-to-user safe outputs.
Try it with Copilot cloud agent
No update is needed. Create an automation from the Automations pane in your repository&rsquo;s Agents tab and use supported tools to see confidence and rationale in action.
What you can build

Triage: Label, type, and prioritize incoming issues automatically. Each action carries its reasoning without adding a triage comment, and you can review the ones you want before they apply.
Metadata enrichment: Backfill missing labels, types, or field values on issues that were filed without them, with the reasoning attached and an optional review step.
Spam detection: Flag suspected spam with a reason and hold uncertain cases for you to review.

Rationale, confidence, and approvals are available now in public preview. Create an automation today using the supported tools to see suggestions and rationale in action, or join the discussion within GitHub Community.

The post Agent automation controls in GitHub Issues in public preview appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-23-agent-automation-controls-in-github-issues-in-public-preview](https://github.blog/changelog/2026-07-23-agent-automation-controls-in-github-issues-in-public-preview)*
