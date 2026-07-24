---
categories:
- MS
- GitHub
date: '2026-07-23T20:38:22+00:00'
description: 'The MCP protocol is going stateless on 28th July 2026, and the GitHub
  MCP Server supports the latest spec ahead of the official release.

  What&rsquo;s changing


  '
draft: false
original_url: https://github.blog/changelog/2026-07-23-github-mcp-server-supports-the-next-mcp-specification
source: GitHub Changelog
tags:
- Release
- copilot
title: GitHub MCP Server supports the next MCP specification
---

The MCP protocol is going stateless on 28th July 2026, and the GitHub MCP Server supports the latest spec ahead of the official release.
What&rsquo;s changing

The new stateless core means MCP deployments are now easy to scale.
Extensions unlock innovation (e.g., MCP apps and Enterprise Managed Auth, both of which are already supported by VS Code).
Sessions and initialize are both removed, so you can connect to servers faster and easier. Clients can also complete the handshake in parallel.
You&rsquo;ll see more remote servers supporting features like elicitation thanks to multi round-trip requests.

Since all tier 1 SDKs have preserved backwards compatibility and they have all already shipped beta support, you don&rsquo;t need to do anything to maintain support. The GitHub MCP server uses the official Go SDK.
For GitHub MCP Server, we made three changes:

Removed Redis sessions: Database writes on initialize are gone, and database reads are gone from every call, which makes things snappier without users losing anything.


Avoided deep packet inspection: We need to read some values from MCP requests for logging and secret scanning. In the new spec we can do that from HTTP headers guaranteed to be present. That means no more inspecting the payload of every single request before the SDK does.


Upgraded our elicitation implementation: Our stdio MCP server uses URL elicitation for easy user login. In the new protocol version, each step is a separate HTTP request. To make this work with old and new clients, the Go SDK provides a wrapper that makes both mechanisms work.


In addition, MCP added official conformance tests. Strict validation helps agents to verify their work. To use this, point Copilot at your codebase and provide access to:

The conformance suite
The draft spec documentation
Any tier 1 SDK implementation

This is a huge boost to all tiers of the official SDK, and to bespoke clients and servers too, because AI assisted development is much easier to verify with these tests.
GitHub support
GitHub MCP Server already supports the latest spec ahead of the official release.
Additional info
To learn more, see the blog post about this release.

The post GitHub MCP Server supports the next MCP specification appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-23-github-mcp-server-supports-the-next-mcp-specification](https://github.blog/changelog/2026-07-23-github-mcp-server-supports-the-next-mcp-specification)*
