---
categories:
- MS
- GitHub
date: '2026-07-30T17:39:46+00:00'
description: 'You can now reference an action or reusable workflow that lives in the
  same repository using the new self-repository syntax. A uses: value that starts
  with $/ r'
draft: false
original_url: https://github.blog/changelog/2026-07-30-reference-same-repository-actions-with-self-repository-syntax
source: GitHub Changelog
tags:
- Release
- actions
title: Reference same-repository actions with self-repository syntax
---

You can now reference an action or reusable workflow that lives in the same repository using the new self-repository syntax. A uses: value that starts with $/ resolves to your workflow&rsquo;s own repository at the exact commit that is running, with no checkout required. It works everywhere the workspace-relative ./ syntax works, including workflow steps, composite action steps, nested composition, and reusable workflow calls.
Before this, referencing an action defined in your own repository meant either relying on ./ and a checkout, or hardcoding a version. This was a maintenance burden and quietly defeated commit SHA pinning. With self-repository references, sibling actions and workflows automatically match the ref you are already running, so your internal references stay consistent even when callers pin to a full-length commit SHA. This also makes it possible to adopt the enterprise policy that requires actions to be pinned to a full-length commit SHA for workflows that call their own actions.
Self-repository references are now the recommended way to compose actions and reusable workflows within a repository. They are available on github.com. This feature requires the GitHub Actions runner to be on version 2.336.0 or newer.
Learn more by checking out our docs about finding and customizing actions, or join the discussion within GitHub Community.

The post Reference same-repository actions with self-repository syntax appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-07-30-reference-same-repository-actions-with-self-repository-syntax](https://github.blog/changelog/2026-07-30-reference-same-repository-actions-with-self-repository-syntax)*
