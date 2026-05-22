---
categories:
- MS
- GitHub
date: '2026-05-21T16:00:00+00:00'
description: Five years ago, we stood up GitHub&rsquo;s accessibility program. What
  began as a small team addressing accessibility debt has grown into a company-wide
  discipl
draft: false
original_url: https://github.blog/open-source/building-githubs-next-chapter-in-accessibility/
source: GitHub Blog
tags:
- Open Source
- accessibility
- open source
title: Building GitHub’s next chapter in accessibility
---

Five years ago, we stood up GitHub&rsquo;s accessibility program. What began as a small team addressing accessibility debt has grown into a company-wide discipline, woven into our engineering fundamentals, our design system, our AI tools, and our culture. The five-year milestone prompted us to step back and ask a fundamental question: where do we go from here?



The answer is our strategy for accessibility, which we published earlier this year. The strategy marks a pivotal moment for our program. For the first five years, our focus was primarily internal. Going forward, we are turning outward&mdash;engaging the global community of developers as we continue to mature internally. We want to build a culture of accessibility across GitHub and include every developer, every team, and every open source project.



The strategy is organized around four priorities. On this Global Accessibility Awareness Day, we&rsquo;re sharing initial progress on each strategic priority.



Help improve the accessibility of open source at scale



Open source software powers much of the world&rsquo;s technology, yet many projects are not designed to be usable by people with disabilities. On the eve of GAAD 2025, we took a pledge to help change that. We committed to three goals: empower people with disabilities to contribute to open source, increase the availability of open source assistive technologies, and improve the accessibility of mainstream open source projects.



Last year, we launched an open source initiative that is turning our pledge into action.



Open Source Assistive Technology Hackathon



This week, we&rsquo;re hosting the first Open Source Assistive Technology Hackathon at GitHub headquarters in San Francisco. Over two days, participants will contribute to 16 featured projects that empower people with disabilities. They include projects that enable blind students to interact with graphical information on the Monarch refreshable tactile display, a project that leverages AI to convert PDF files into an accessible format, hacks for power wheelchairs, and much more. The hackathon also features Office Hours for the open source NVDA screen reader and a GitHub Learning Room to help participants learn open source workflows on GitHub.



Open Source Accessibility Summit



In October 2025, we hosted the inaugural Open Source Accessibility Summit at All Things Open in Raleigh, North Carolina. The response was overwhelming&mdash;300 people registered and more than 500 joined the waitlist. The summit brought together experts from the disability, accessibility, and open source communities to identify six priority challenge areas and draft a collaborative roadmap. That work continues in the open-source-accessibility organization on GitHub, with community discussions coordinated through a public Slack workspace.



Accessibility best practices for open source projects



Maria Lamardo collaborated with open source maintainers to publish accessibility best practices for open source projects on opensource.guide. The guide helps maintainers center people with disabilities in their development process&mdash;from writing an accessibility statement and making documentation accessible by default, to designing keyboard-navigable interfaces and using semantic HTML.



Empower developers with disabilities to build on GitHub



As the home for all developers, we want every developer to feel welcome and to be able to contribute to the future of global software development using everything GitHub has to offer. Accessibility is a GitHub Engineering Fundamental. We drive excellence by clearly defining expectations, continuously testing against them, and ensuring accountability through our engineering scorecards. Our Primer Design System provides a strong foundation, and we embed accessibility designers within product teams to ensure accessibility is considered from the first sketch to the final deployment.



Over the past year, we&rsquo;ve made substantial improvements across the platform.



A redesigned pull request experience



Pull requests are an essential part of the developer experience. This year, the pull requests team completely redesigned the files changed page and included accessibility at every step in the process. They rebuilt it with consistent keyboard navigation, landmarks, adjustable line spacing, and fewer page reloads (critical for screen reader users who lose their place when pages refresh). They shipped seven updates over seven months, and the improved experience became the default for all users in January 2026.



Enhanced contrast and themes



In June 2025, the Design team introduced enhanced contrast controls for all GitHub themes. For the first time, contrast is adjustable for logged-out users&mdash;anyone who visits GitHub can customize their visual experience without needing an account.



Smarter search



In April 2026, semantic search for GitHub Issues reached general availability. Users can now describe what they&rsquo;re looking for in natural language and find conceptually related results, reducing cognitive load and making the platform more intuitive for everyone.



Major accessibility improvements for GitHub Command Line Interfaces (CLIs)



We believe the terminal is essential to the developer experience, and it has been an underserved environment for accessibility across the industry. Over the past year, our CLI team raised the bar.



In May 2025, the CLI team launched accessibility improvements to the GitHub CLI, introducing screen reader support that replaced prompts and spinner indicators that confused speech synthesis with accessible alternatives. They added customizable color palettes aligned to ANSI 4-bit colors, giving users with low vision and colorblindness full control over their terminal experience.



They carried these principles forward into the development of GitHub Copilot CLI, which reached general availability in February 2026. Copilot CLI brings the power of GitHub Copilot coding agent directly to the terminal&mdash;and it shipped with accessibility built in from day one:




Dedicated screen reader mode (--screen-reader) with consistent dialog titles, reduced redundant announcements, and enhanced contrast.



Theme picker with colorblind-friendly variants including GitHub Dark, GitHub Light, and high-contrast options.



Full keyboard-first navigation with UNIX keybindings, quick help overlay, and configurable reasoning visibility.



Responsive layout that adapts to narrow terminals and different screen configurations.




Finally, we published a guide for using Git, GitHub CLI, and GitHub Copilot CLI with a screen reader. It includes step-by-step installer walkthroughs with screen reader&ndash;specific notes and end-to-end workflows that chain commands together, so blind developers can go from zero to productive on the command line as quickly as possible.



Empower our customers to meet their accessibility goals on GitHub



We are eager to help our customers meet their accessibility goals when they build on GitHub. Our approach is to lead by example, using the latest GitHub features to run our own accessibility program, sharing our process improvements transparently, and open-sourcing the tools we build.



Sharing what we&rsquo;ve learned



This year, we published detailed accounts of how we run our accessibility program, so our customers can learn from our experience. Janice Rimmer&rsquo;s post on automating accessibility governance with Copilot showed how a program manager&mdash;not an engineer&mdash;can build automation that transforms compliance workflows. Carie Fisher&rsquo;s post on Continuous AI for Accessibility detailed our user feedback pipeline, where GitHub Copilot analyzes incoming reports and auto-populates approximately 80% of issue metadata. That workflow has driven a 62% reduction in resolution time and ensured 89% of issues close within 90 days. Finally, Eric Bailey shared timely insights about accessibility agents in his post about Building our general-purpose accessibility agent.



Figma Annotation Toolkit



When we analyzed our accessibility audit data, we found that 48% of issues could have been prevented in the design phase. That insight led Jan Maarten and our Accessibility Design team to build the Annotation Toolkit, a comprehensive Figma library that helps designers document accessibility intent directly in their work, from heading hierarchies and keyboard navigation flows to ARIA semantics and screen reader announcements. They open-sourced the toolkit in September 2025 so any team can use it.



AI-powered accessibility scanner



Our Accessibility Engineering and State and Local Government revenue teams collaborated to build an AI-powered accessibility scanner that enables customers to find, file, and fix accessibility bugs using GitHub Copilot cloud agent. The scanner uses the highly-trusted open source axe-core library from Deque Systems to find accessibility barriers through static DOM analysis. Most recently, the team implemented a new plugin architecture and an initial built-in plugin that detects WCAG 1.4.10 Reflow violations. The scanner is available in the GitHub Marketplace and as an open-source repository that teams can fork and adapt to their unique CI/CD processes.



Accessibility guides for developers



Kendall Gassner published a guide for optimizing GitHub Copilot for accessibility using custom instructions, helping developers optimize GitHub Copilot for accessibility for their teams. Roberto Perez published a guide for creating custom agents for accessibility that can jumpstart experimentation with custom agents for accessibility.



The GitHub Enterprise Accessibility Advisory Panel (GAAP)



In April 2026, we launched the GitHub Enterprise Accessibility Advisory Panel (GAAP), a forum for regular exchange between GitHub and enterprise customers working to build accessible software on GitHub. GAAP bridges real-world accessibility challenges and GitHub&rsquo;s platform features, with an emphasis on adoption of current capabilities and identification of future needs. Membership is open to accessibility professionals from GitHub enterprise customer organizations.



Empower Hubbers with disabilities to do the best work of their lives



We know that companies that embrace best practices for employing and supporting people with disabilities outperform their peers, and we take that responsibility seriously.



All employees (we call them Hubbers) are required to complete accessibility training. We integrate accessibility into our procurement processes to continuously improve the tools and systems our employees depend on. We provide guides that help every employee create accessible communications, events, and meetings. And our NeuroCats Community of Belonging and AccessCats Affinity Group give employees with disabilities a strong voice within the company.



Over the past year, our People team updated the categories that Hubbers use to self-identify, which allows us to better represent the changing demographics within GitHub. While the AccessCats team completed a survey of disability at GitHub that will enable us to better serve Hubbers with disabilities.



Join us



Accessibility is never done. Publishing our strategy is not the finish line&mdash;it is the starting line for the next chapter. We are building in public, sharing our tools, and inviting the global developer community to join us.



Here is how you can get started:




Explore our strategy at accessibility.github.com.



Try our tools: the Annotation Toolkit and the accessibility scanner.



Join the open source accessibility movement at github.com/open-source-accessibility.



Share your feedback at accessibility.github.com/feedback.


The post Building GitHub&#8217;s next chapter in accessibility appeared first on The GitHub Blog.

---
*원문: [https://github.blog/open-source/building-githubs-next-chapter-in-accessibility/](https://github.blog/open-source/building-githubs-next-chapter-in-accessibility/)*
