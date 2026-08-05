---
categories:
- MS
- GitHub
date: '2026-08-04T15:54:47+00:00'
description: 'Beginning August 4, 2026, GitHub Spark no longer accepts new users or
  allows the creation of new apps. Existing users can continue to access GitHub Spark
  until '
draft: false
original_url: https://github.blog/changelog/2026-08-04-upcoming-deprecation-of-github-spark-on-github-com
source: GitHub Changelog
tags:
- Retired
- copilot
- ecosystem &amp; accessibility
title: Upcoming deprecation of GitHub Spark on github.com
---

Beginning August 4, 2026, GitHub Spark no longer accepts new users or allows the creation of new apps. Existing users can continue to access GitHub Spark until August 31, 2026 to export apps they&rsquo;ve already created. Apps you&rsquo;ve already deployed will continue to work after GitHub Spark is retired.
GitHub Spark was created to help people quickly move from an idea to a working application. Since its launch, AI models and agentic development tools have advanced significantly. Builders can now build and refine these experiences through GitHub Copilot in the environments where they already work, including VS Code, Copilot CLI, and the GitHub Copilot app. We&rsquo;ve seen builders increasingly choose these integrated workflows for application development, and we&rsquo;re aligning our experiences accordingly. This change applies specifically to the current GitHub Spark experience on github.com.
Separately, GitHub Models, the inference service used by Spark&rsquo;s llm() function, retired on July 30, 2026. As of that date, calls to llm() no longer work. Apps that don&rsquo;t use llm() aren&rsquo;t affected by the GitHub Models retirement.
What this means for you

Existing apps you&rsquo;ve already deployed will continue to work after GitHub Spark shuts down.
Export your app code before August 31, 2026 if you want to keep editing your app in the future. To do this, open the Spark workbench for your app, select &hellip;, then select Create repository.
If your app uses llm(), replace it with your own inference provider to keep AI features working.

Not all Spark apps use AI inferencing. To check whether an app is affected by the GitHub Models retirement, open the app in Spark and search the code for llm(). If there are no llm() calls found, no action is needed. If the app does use llm(), replace inferencing with a separate provider to keep the app running with AI capabilities. Users will need to supply their own API key and manage billing going forward in this instance, as GitHub will not provide the underlying model or tokens any longer.
Questions or feedback? Join the discussion within GitHub Community.

The post Upcoming deprecation of GitHub Spark on github.com appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-04-upcoming-deprecation-of-github-spark-on-github-com](https://github.blog/changelog/2026-08-04-upcoming-deprecation-of-github-spark-on-github-com)*
