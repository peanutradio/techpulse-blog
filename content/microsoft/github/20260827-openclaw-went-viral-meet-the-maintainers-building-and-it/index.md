---
categories:
- MS
- GitHub
date: '2026-08-27T16:00:00+00:00'
description: 'What began as a personal experiment quickly became a global open source
  project with extraordinary momentum.




  OpenClaw is a personal AI assistant that runs on'
draft: false
original_url: https://github.blog/open-source/maintainers/openclaw-went-viral-meet-the-maintainers-building-and-securing-it/
source: GitHub Blog
tags:
- Maintainers
- Open Source
- Security
- maintainers
- open source
- OpenClaw
title: OpenClaw went viral. Meet the maintainers building and securing it.
---

What began as a personal experiment quickly became a global open source project with extraordinary momentum.



OpenClaw is a personal AI assistant that runs on users&rsquo; devices and connects with the messaging channels they already use. Started by Peter Steinberger as a weekend project in November 2025, its GitHub repository has grown to approximately 388,000 stars, 81,000 forks, and more than 80,000 commits by August 26, 2026.



In this video interview, filmed just six months into the project, creator Peter Steinberger and several OpenClaw maintainers discuss managing a surge of pull requests, rethinking contributor trust and code review, addressing software supply chain risks, and balancing powerful agent capabilities with security. They also share security lessons from the GitHub Secure Open Source Fund and the value of connecting with maintainers facing similar challenges. Watch the full video above, then explore the key lessons below.



People in this video



The following maintainers shared their experiences maintaining and securing OpenClaw.




Peter Steinberger, Creator of OpenClaw



Brad Groux, CEO, Digital Meld



Josh Avant, Member of technical staff, OpenClaw Foundation



Josh Lehman, Martian Engineering



Sally O&rsquo;Malley, Principal software engineer at Red Hat



Val Alexandar, OpenCoven



Vincent Koc, Chief architect, OpenClaw Foundation




Here are the top 10 lessons that we took away from the conversation.



Lessons 1&ndash;3: How AI changed contributions and community



1. Pull requests became prompt requests



OpenClaw&rsquo;s maintainers found themselves managing thousands of pull requests and issues, with some contributors opening hundreds of pull requests at once.




I&nbsp;don&rsquo;t&nbsp;even call them pull requests. I call them prompt requests.
Peter Steinberger




There were some contributors that had multiple hundreds of pull requests running these sort of automated software factories that were just mining everything for issues.
Josh Lehman



The challenge shifted from attracting participation to finding valuable contributions amid a flood of activity that could overwhelm human review.



2. Keep the door open for new contributors



OpenClaw&rsquo;s maintainers wanted the project to be welcoming to new participants, whether they were first-time open source contributors, non-developers solving a specific problem, or people using AI agents to help. Rather than dismissing imperfect contributions, they looked for promising ideas and worked with contributors to refine, rewrite, or complete the final changes themselves.




I know how it felt when, many years ago, my first pull request was accepted on a project.
Peter Steinberger



Some of the first-time contributions that were merged came from people without a development background. They used an agent to create a pull request, and worked with maintainers to finish the change.




A good proportion of those first-time pull requests that got merged are from non-developers. They&rsquo;re just people that have a specific problem and a need.
Vincent Koc



3. Agents save time but make it harder to sign off



The maintainers described two very different outcomes from the same technology: agents can help people reclaim time, but they can also make it harder to stop working.




I&rsquo;ve seen the other side of it, where people are just so in love with it and they realize, wow, if I don&rsquo;t sleep tonight, I can do what used to take a week for me to do.
Val Alexandar




I have three kids. They&rsquo;re very small. OpenClaw lets me manage agents that go and work for me so I can get back to playing with my kids.
Josh Lehman




Sometimes the maintainers will go on the channel and say, &lsquo;I&rsquo;m&nbsp;going to touch grass now.&nbsp;I&rsquo;m&nbsp;taking a few hours off.&rsquo;
Sally O&rsquo;Malley



Agents are neither good nor bad for work-life balance. But they amplify both the opportunity to do more and the importance of knowing when to step away.



Lessons 4-6: How maintainers adapted



4. Earn trust by finding where you can add value



There was no single path to becoming an OpenClaw maintainer. Some contributors arrived through security work, others through integrations or community participation, but the common thread was finding a way to add value and taking ownership.




Peter ignored me, so I was like, how else can I get his attention? Security.
Vincent Koc




I&rsquo;m a Microsoft guy, so I thought, is there a plugin for Microsoft Teams?
Brad Groux




I looked into the community and I was in voice chat, and people were asking a lot of questions, and I was like, well, how can I add value in these conversations?
Val Alexandar



5. The new trust signal is showing your work



As contribution counts became less informative, the team identified evidence that could help a pull request stand out: agent transcripts, screenshots, testing, and an explanation of the contributor&rsquo;s thinking.




If you provide us with the transcripts, we&nbsp;actually see&nbsp;how you came to the pull request and your discussion with the agent. Incredibly valuable. If you add screenshots, you can prove that you tested this.
Peter Steinberger



The important question was not simply whether a human or an agent wrote the code. It was whether the contributor understood the feature and had considered how it interacted with the rest of the project.




Nobody cares if you wrote the code or not, but we care if you actually thought about this feature.
Peter Steinberger



6. Maintainers are reviewing agent code with agents



Maintainers increasingly relied on AI tools to help review AI-generated contributions, while also taking a more hands-on approach to improving submitted code.




Whenever I get a pull request from an AI, one thing I love to do now is use GitHub Copilot for all the reviews. I just press a button right there. It does a review and generates clarity on all the files that are attached, what the files mean, and how they changed.
Val Alexandar




This is the first project where I saw it become normalized that when someone submits a pull request, as a maintainer, you just edit it. You just make it right.
Josh Lehman



Lessons 7&ndash;9: Security challenges



7. Reputation became an attack surface



Contribution history itself could be manipulated. OpenClaw&rsquo;s maintainers saw people duplicate existing pull requests, and Vincent Koc explains why.




People would basically duplicate other people&rsquo;s pull requests. What they were attempting to do here was to build credibility, because we had these badges, like how many you&rsquo;ve merged. So the more merges you had, it was like a trust signal to us maintainers.
Vincent Koc



Peter described a company using an automated pull request to promote their product. The team had to identify duplicate work and determine which pull request was the original.



The code was not the only thing the project needed to evaluate. Maintainers also had to reconsider the social signals they used to decide what, and whom, to trust.



8. &ldquo;Safe by default&rdquo; depends on who you ask



What feels safe to one user may feel unnecessarily restrictive to another.



The tradeoff was clear in practice. Tighter workspace restrictions generated user complaints, while fewer restrictions could expose the project to security incidents.




It&rsquo;s really often a hard game to find the right balance between making it really convenient for users and also building something that is safe enough as a default.
Peter Steinberger



Secure defaults must account for an agent&rsquo;s capabilities, what users understand, and what a particular environment is prepared to allow.



9. Know who maintains your dependencies



Recent supply chain attacks pushed the maintainers to think more carefully about both the dependencies they relied on and their relationship with the projects behind them.




We went through our dependencies with a fine-tooth comb. What it&rsquo;s pushed us to do is actually reduce the core dependencies, but also create a relationship with the maintainers that we have a dependency on.
Vincent Koc




It&rsquo;s not the default that companies actually try to contribute back instead of just maintaining a fork and not caring.
Peter Steinberger



Lesson 10: How the GitHub Secure Open Source Fund helped



Participants described the GitHub Secure Open Source Fund as both a security learning experience and a way to connect with maintainers confronting similar, often overwhelming, problems.




The presenter was like, first, go get a cup of coffee. Step one, take a breath. It connected us to the human element of being a maintainer.
Josh Avant



The program provided greater awareness of security practices and helped participating maintainers understand how to prompt agents.




We have agents now, and they can do just about anything that you ask them to do, but you still have to know what to ask them to do. Now I have the ability to know what to ask for.
Josh Lehman



Vincent emphasized the value of meeting other maintainers who were experiencing the same challenges of securing open source projects. The program gave participants a community they could tap into and learn from as those challenges continued.



Continue the conversation



Watch the full conversation to hear how OpenClaw&rsquo;s maintainers are adapting when contributions scale faster than the human systems used to review, secure, and sustain them.



OpenClaw participated in Session 4 of the GitHub Secure Open Source Fund. To learn more, read What 50 open source projects taught us about security in the AI era.



Applications for the GitHub Secure Open Source Fund are now open. If you&rsquo;re maintaining an open source project, apply to learn from security experts, connect with fellow maintainers, and strengthen the security of your project.



Head over to the GitHub Community and ask the maintainers what it&rsquo;s really like building the fastest-growing open source project in GitHub history!



Thank you to all GitHub Secure Open Source Fund Partners



Together, we are helping secure the open source ecosystem for everyone!



Funding Partners: Alfred P. Sloan Foundation, American Express, Chainguard, Datadog, Herodevs, Kraken, Mayfield, Microsoft, Shopify, Stripe, Superbloom, Vercel, Zerodha, 1Password







Ecosystem Partners: Atlantic Council, Ecosyste.ms, CURIOSS, Digital Data Design Institute Lab for Innovation Science, Digital Infrastructure Insights Fund, Microsoft for Startups, Mozilla, OpenForum Europe, Open Source Collective, OpenUK, Open Technology Fund, OpenSSF, Open Source Initiative, OpenJS Foundation, University of California, OWASP, Santa Cruz OSPO, Sovereign Tech Agency, SustainOSS








Looking for practical security steps you can take for your project? Set up the security baseline for your project in one minute &gt;


The post OpenClaw went viral. Meet the maintainers building and securing it. appeared first on The GitHub Blog.

---
*원문: [https://github.blog/open-source/maintainers/openclaw-went-viral-meet-the-maintainers-building-and-securing-it/](https://github.blog/open-source/maintainers/openclaw-went-viral-meet-the-maintainers-building-and-securing-it/)*
