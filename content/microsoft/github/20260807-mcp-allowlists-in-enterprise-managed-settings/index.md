---
categories:
- MS
- GitHub
date: '2026-08-07T00:11:19+00:00'
description: Enterprise owners can now centrally control which Model Context Protocol
  (MCP) servers GitHub Copilot clients are allowed to run by using the new allowedMcpServ
draft: false
original_url: https://github.blog/changelog/2026-08-06-mcp-allowlists-in-enterprise-managed-settings
source: GitHub Changelog
tags:
- Release
- copilot
title: MCP allowlists in enterprise managed settings
---

Enterprise owners can now centrally control which Model Context Protocol (MCP) servers GitHub Copilot clients are allowed to run by using the new allowedMcpServers and deniedMcpServers keys in enterprise managed settings. Approve the MCP servers your developers depend on and block untrusted or non-compliant ones across your enterprise. This capability is generally available.
How it works
Add either or both keys to copilot/managed-settings.json. Each key is a list of matchers that identify MCP servers by remote URL, local command, or name:

serverUrl: Matches remote (HTTP/SSE) servers. It supports * wildcards and canonicalizes URLs to prevent evasion.
serverCommand: Matches local (stdio) servers by exact command and arguments.
serverName: Matches the user-assigned label. This is only supplied as a convenience, not a security control, since users can rename servers.

Policies fail closed, meaning a malformed or unverifiable configuration is blocked rather than allowed. When policies come from multiple layers, a server must pass every layer.
In server-managed deployments, both keys can be marked overridable so enterprise teams can define their own allow and deny lists on top of your baseline. To learn more, see our docs on overriding settings for individual teams.
For configuration examples and the full matcher syntax, see Enterprise managed settings reference.
Supported clients
MCP allowlists are currently enforced on the GitHub Copilot app, Copilot CLI and VS Code.
Getting started
In your source organization&rsquo;s .github-private repository, add the keys to copilot/managed-settings.json and commit to the default branch.
To learn more, see configuring enterprise managed settings.

The post MCP allowlists in enterprise managed settings appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-06-mcp-allowlists-in-enterprise-managed-settings](https://github.blog/changelog/2026-08-06-mcp-allowlists-in-enterprise-managed-settings)*
