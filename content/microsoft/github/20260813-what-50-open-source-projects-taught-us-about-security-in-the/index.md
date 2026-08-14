---
categories:
- MS
- GitHub
date: '2026-08-13T16:00:00+00:00'
description: AI is changing the pace of open source development and the security challenges
  that come with it. Maintainers are reviewing unfamiliar contributions, managing
  n
draft: false
original_url: https://github.blog/open-source/maintainers/what-50-open-source-projects-taught-us-about-security-in-the-ai-era/
source: GitHub Blog
tags:
- Maintainers
- Open Source
- Security
- Supply chain security
- AI security
- open source
- supply chain security
title: What 50 open source projects taught us about security in the AI era
---

AI is changing the pace of open source development and the security challenges that come with it. Maintainers are reviewing unfamiliar contributions, managing new attack surfaces, and responding to vulnerabilities with limited time and resources.



Session 4 of the GitHub Secure Open Source Fund tested a practical response. The Secure Fund invested more than $500,000 across 50 projects, pairing maintainers with GitHub Security Lab experts, GitHub security tools, AI-assisted workflows, and a peer community.



One lesson emerged consistently: AI can help maintainers investigate, prioritize, and respond faster. Maintainers still provide the context, judgement, and accountability required to decide what ships.



OpenClaw was invited to participate in Session 4 because it is GitHub&rsquo;s fastest-growing open source project, and its maintainers wanted to strengthen its security posture.



By the end of Session 4, OpenClaw developed an incident response plan, expanded its use of GitHub security tooling, audited its GitHub Actions workflows, and strengthened its processes for identifying and responding to security issues.



The maintainers shared:







OpenClaw&rsquo;s experience reflects the broader story of Session 4. While the specific risks varied across the cohort, maintainers shared a consistent need: the knowledge, tools, and expert support to secure software as AI changed how they built it.



Across the program, maintainers turned that support into concrete security improvements. Projects strengthened established practices, prepared for emerging AI-related risks, and explored how tools like GitHub Copilot could support vulnerability triage, threat modeling, code review, and remediation.







The benefits extend beyond individual projects. When maintainers strengthen the security of widely used open source software, they help build a more resilient ecosystem for everyone who depends on it.




&nbsp;Session 4, by the numbers




50 projects



71 maintainers



22 Countries



$500,000+ in non-dilutive funding powered by GitHub Sponsors



92% of projects completed the program with core GitHub security features enabled&ndash;secret scanning, code scanning, protected branches, private vulnerability reporting, Dependabot



Learn more or enable these security features for your own project.




Security results across all sessions:



Across all GitHub Secure Open Source Fund Sessions and follow-up periods through August 2026:




188 projects and 290 maintainers have participated across 42 countries



GitHub, Microsoft, and external funding partners have contributed $1.88 million, distributed through GitHub Sponsors.



Participating projects have identified and disclosed 533 new CVEs, performed more than 1,500 Dependabot security updates, and resolved more than 650 exposed secrets.



During the last six months ending in July 2026, participating and Alumni projects fixed 4,210 CodeQL alerts and blocked 119 secrets from being exposed.





How the GitHub Secure Open Source Fund works



The GitHub Secure Open Source Fund links funding directly to measurable security outcomes. The program combines hands-on security education, direct engagement with GitHub Security Lab experts, and a trusted community where maintainers can work through security challenges with their peers.



Each session is a three-week sprint and engagement for a total of 12 months. Funding and participation are tied directly to outcome&#8209;driven goals and verified security improvements.



The sprint is designed and curated by the GitHub Security Lab, and delivered by security experts from GitHub and our partners. The training is structured into different focus areas per week.



These include:




Foundations of open source security



Threat modeling and secure coding



AI security and vulnerability management




Throughout this program, each project receives $10,000 USD via GitHub Sponsors (which breaks down to $6,000 USD during the sprint and $2,000 USD at six- and 12-month security check-ins). Projects are invited to a new security-focused community and office hours with the GitHub Security Lab, which they can take advantage of during the full 12 months. They also receive security resources to immediately implement in their project and Azure credits for cloud infrastructure.




Learn more about the Secure Open Source Fund.



Apply for Session 5 of the GitHub Secure Open Source Fund before August 24.



Become a Funding or Ecosystem Partner of the GitHub Secure Open Source Fund.




Where security work happened in Session 4



Session 4 focused on improving security across the systems developers rely on every day. The projects below are grouped by the role they play in the software ecosystem.



AI, machine learning, and intelligent systems &#129302;



Caracal &bull; Deep Agents &bull; DocsGPT &bull; LadybugDB &bull; LangChain &bull; n8n-MCP &bull; Nasiko &bull; ONNX &bull; OpenClaw &bull; PageIndex &bull; Scenic &bull; Serena



These projects sit at the intersection of AI, automation, data infrastructure, and machine learning. They increasingly serve as foundational components for modern AI workflows and production deployments. As AI adoption accelerates, security improvements in these projects help establish stronger foundations for emerging AI ecosystems.















Build systems, supply chain, and release tooling &#129520;



browserslist &bull; CycloneDX Python Library &bull; Cucumber &bull; golangci-lint &bull; JReleaser &bull; postcss &bull; Task



These projects help developers test, validate, package, release, and maintain software across diverse environments. Tools in this group influence everything from software bills of materials and release pipelines to code quality and testing automation.















Core programming languages, runtimes, and foundational libraries &#128218;



Byte Buddy &bull; core-js &bull; FS2 &bull; Gleam &bull; htmx &bull; Pkl &bull; Pyodide &bull; termcolor



These projects help define how software is written, configured, executed, and extended. Improvements at this layer flow downstream to thousands of applications and developer ecosystems.



Security improvements in foundational runtimes and libraries can extend downstream to the many tools and applications that depend on them.















Developer tools and productivity platforms &#9874;&#65039;



cheerio &bull; Ciphey &bull; CodeRunner &bull; Hoppscotch &bull; MapStruct &bull; Python Pillow &bull; Proyecto Respira &bull; Readest &bull; ToolJet &bull; Vuetify &bull; Yjs



These projects shape the everyday experience of building, testing, collaborating on, and using software. Many serve as widely adopted utilities, applications, and platforms that appear throughout developer environments and application stacks.



Together, this group supports API development, low-code platforms, collaborative applications, content processing, and software delivery workflows. When infrastructure projects become more resilient, the benefits extend far beyond a single application and strengthen entire technology ecosystems.















Web, networking, APIs, and infrastructure services &#128202;



actix-web &bull; aiohttp &bull; Apache Solr &bull; Apache ZooKeeper &bull; etcd &bull; FastAPI &bull; Haraka &bull; Hummingbird &bull; mimetype &bull; Sniffnet &bull; Starlette &bull; UAParser.js



These projects form part of the internet&rsquo;s operational backbone. They handle APIs, networking, search, messaging, service coordination, and distributed systems infrastructure relied on by organizations around the world.



This group includes technologies that sit on the critical path of modern cloud applications and internet services.















AI security as a shared frontier



AI-related security questions appeared across projects in Session 4, from machine learning infrastructure and agent frameworks to developer tools and internet infrastructure.



At the same time, established security responsibilities did not go away. Maintainers still needed to manage vulnerabilities, secure dependencies, protect release workflows, and prepare for incidents. AI introduced new risks and increased the speed at which maintainers needed to understand and respond to them.



The lesson from Session 4 is clear: AI security is not evolving in isolation. It is becoming part of the broader practice of building secure software. As that shift continues, maintainers will need practical education, trusted communities, and expert support that can evolve with them.







Thank you to all of our partners



We couldn&rsquo;t do this without our incredible network of partners. Together, we are helping secure the open source ecosystem for everyone!



Funding Partners: Alfred P. Sloan Foundation, American Express, Chainguard, Datadog, Herodevs, Kraken, Mayfield, Microsoft, Shopify, Stripe, Superbloom, Vercel, Zerodha, 1Password







Ecosystem Partners: Atlantic Council, Ecosyste.ms, CURIOSS, Digital Data Design Institute Lab for Innovation Science, Digital Infrastructure Insights Fund, Microsoft for Startups, Mozilla, OpenForum Europe, Open Source Collective, OpenUK, Open Technology Fund, OpenSSF, Open Source Initiative, OpenJS Foundation, University of California, OWASP, Santa Cruz OSPO, Sovereign Tech Agency, SustainOSS





The post What 50 open source projects taught us about security in the AI era appeared first on The GitHub Blog.

---
*원문: [https://github.blog/open-source/maintainers/what-50-open-source-projects-taught-us-about-security-in-the-ai-era/](https://github.blog/open-source/maintainers/what-50-open-source-projects-taught-us-about-security-in-the-ai-era/)*
