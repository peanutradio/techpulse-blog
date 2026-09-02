---
categories:
- MS
- GitHub
date: '2026-09-01T12:35:59+00:00'
description: 'GitHub CLI now has a repeatable --attach flag that uploads a local image
  or video and references it inline in an issue, pull request, or comment body.

  This is n'
draft: false
original_url: https://github.blog/changelog/2026-09-01-github-cli-media-in-issues-pull-requests-and-comments
source: GitHub Changelog
tags:
- Release
- client apps
title: 'GitHub CLI: Media in issues, pull requests, and comments'
---

GitHub CLI now has a repeatable --attach flag that uploads a local image or video and references it inline in an issue, pull request, or comment body.
This is now generally available to all users on GitHub across all plans. You just need to update gh to v2.99.0.
Why this matters?
Some things are easier to show than to describe (i.e., a UI bug, a rendered result, an error on screen). Previously, gh only wrote text, so adding an image or video to an issue or pull request meant stopping to open the browser, drag the file into the comment box, and copy the result back. Now, gh can attach images and videos directly from the command line.
--attach puts the image in the same command that writes Markdown. File the bug, and the screenshot lands with it, so the issue shows the actual problem the first time. Open the pull request with a before and after, and the reviewer sees the change instead of pulling the branch to check. Your coding agents get this too, so they can show a result rather than describe it.
Capabilities

Attach from commands that write Markdown text: --attach is repeatable and works on gh issue create, gh issue edit, gh issue comment, gh pr create, gh pr edit, and gh pr comment.
Keep your Markdown as written: A local path already referenced in the body is rewritten in place, so ![alt](./login.png) keeps its alt text and becomes the uploaded asset. Anything attached but never referenced is appended at the end.
Post images and video: PNG, JPEG, GIF, WebP, SVG, MP4, MOV, and WebM are all supported.
Describe what you attach: Alt text follows the path after #, as in --attach './login.png#The login error state'. Omit it and gh falls back to the filename.

Security and access controls
Uploads authenticate with the common token types gh already uses, either the OAuth token from gh auth login or a classic personal access token. Write access to the repository you are attaching to is required for image uploading.
--attach is generally available to all users on GitHub across all plans, with no preview period. Size limits match the web upload flow: 10 MB for images and GIFs, 10 MB for video on Free plans, and 100 MB for video on paid plans. GitHub Enterprise Server is not supported in this release.
Getting started
Update gh to v2.99.0, then run gh issue comment  --attach ./screenshot.png. --attach works the same way on gh issue and gh pr create, edit, and comment. Run gh help for the full flag reference.

Join the discussion on the GitHub Community and read the docs to learn more about attaching files from the command line.
Editor&rsquo;s note (September 1, 2026): Updated the doc link.

The post GitHub CLI: Media in issues, pull requests, and comments appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-09-01-github-cli-media-in-issues-pull-requests-and-comments](https://github.blog/changelog/2026-09-01-github-cli-media-in-issues-pull-requests-and-comments)*
