---
categories:
- MS
- 보안
date: '2026-07-10T16:00:00+00:00'
description: Security is never finished. That conviction is where the Secure Future
  Initiative (SFI) started two years ago and continues to guide us today. AI is reshaping
  c
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/07/10/securing-our-future-july-2026-progress-report-on-microsofts-secure-future-initiative/
source: Microsoft Security Blog
tags: []
title: 'Securing our future: July 2026 progress report on Microsoft’s Secure Future
  Initiative'
---

Security is never finished. That conviction is where the Secure Future Initiative (SFI) started two years ago and continues to guide us today. AI is reshaping cybersecurity. Cyberattackers can discover vulnerabilities, chain attack paths, and scale exploitation faster than manual approaches allow. Defenders can use the same advances to identify risk, strengthen protections, and accelerate response. As the threat landscape evolves, security must evolve with it.



This latest SFI progress report shows how Microsoft is adapting to that reality: strengthening security foundations for an AI-accelerated cyberthreat landscape, applying AI to improve security outcomes at scale, and preparing for future challenges such as scalable quantum computing.




Read the full July 2026 SFI progress report




This report organizes our progress into three outcome-driven themes—secure foundations, proactive defense, and future-ready security—and shares lessons learned, practical guidance, and deeper insights across the culture, governance, principles, and engineering pillars that underpin security at Microsoft.



Secure foundations



The most consequential security failures rarely come from a single missing control. They come from environments where identity gaps, unmanaged assets, and inconsistent configurations sit side by side, creating composite attack paths that determined threat actors can chain together. SFI addresses this systemically, strengthening security across our environment. The results show the progress:




Phishing-resistant multifactor authentication now protects 99.97% of user/device pairs at Microsoft.



More than 732,000 resources have had public access revoked, with network isolation scaling across 1 million resources.



1.4 million unused apps were decommissioned and cross-boundary credential isolation reached 98.7%.



Engineering defaults now prevent 83% of pipelines from accessing unapproved package endpoints.




These controls form reinforcing layers: identity feeds access governance, access governance feeds segmentation, segmentation contains blast radius, and engineering defaults reduce what enters production in the first place. One of the lessons we have learned is that foundations are durable only when they&#8217;re continuously validated, not periodically audited.



Proactive defense



Secure foundations reduce the attack surface. Proactive defense builds on that foundation to find and fix weaknesses quickly. Traditional practices like code review and penetration testing remain essential. The difference now is that frontier AI can discover vulnerabilities and chain exploit paths faster than manual review can keep up. That&#8217;s a threat and, when used well, an advantage. We&#8217;ve leaned into that advantage to find real risk earlier and close it before a cyberattacker can act.




We built a multi-agent AI system that delivers proactive assessment of a cloud service’s source code, identity configurations, network topology, and runtime state to surface composite vulnerabilities that a single-layer review could not catch. More than 90% of findings confirmed by our security engineers, enabling proactive actions to improve security posture.



This system builds on other tools in our security portfolio—such as the Microsoft Security multi-model agentic scanning system (codename MDASH), which scans source code to identify, validate, and prioritize vulnerabilities at scale—and adds configuration, identity, network, and runtime context to comprehensively assess the service.



More than 100 new detections were added this year (more than 350 total), shifting from signature-based to behavior- and baseline-driven detection.



More than 550,000 critical and high-risk open-source vulnerabilities were remediated, with about 3 million container vulnerabilities patched per month through automation.




Future-ready security



Some risks have not fully arrived yet, but waiting for them is not an option. The most urgent example is the transition to post-quantum cryptography. The threat is already here in the form of “harvest now, decrypt later”: data encrypted today could be captured and decrypted once quantum capability matures.




We are accelerating the Microsoft Quantum Safe Program (QSP) timeline, with the goal of transitioning to post-quantum cryptography (PQC) in critical products and services by 2029.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;



PQC is now an SFI-measured engineering requirement, with workstreams advancing across network traffic, data-at-rest protection, and trust chain modernization.



Quantum-safe algorithms (ML-KEM, ML-DSA) are available today across major platforms.



Read more in the recent blog: Accelerating quantum-safe readiness.




Governance, culture, and principles



Foundational progress like this is only possible because of the people committed to making it possible. Security is a core responsibility for every employee at Microsoft: mandatory Trust Code training was completed by more than 99% of full-time employees. Governance is what makes it scale, with accountability driven through our Deputy Chief Information Security Officer (CISO) structure and a centralized risk register. And our principles—secure by design, secure by default, secure in operations—are what turn intent into product, like Microsoft 365 Baseline Security Mode. Tools alone don&#8217;t create durable security; culture, accountability, and secure defaults do.



What you can do today



Throughout the report, we share actionable guidance for organizations at any stage of their security journey. A few starting points:




Enforce phishing-resistant multifactor authentication and eliminate legacy authentication protocols.



Inventory every tenant and classify it. Apply secure-by-default provisioning with drift detection.



Evaluate how identity, code, configuration, and network relationships interact in production. Prioritize composite attack paths over isolated findings.



Inventory your cryptographic dependencies now and establish transition plans for post-quantum readiness.



Enable Baseline Security Mode in Microsoft 365 for secure-by-default configuration at no additional cost.




Read the full SFI report, including detailed pillar-level progress and additional customer guidance.



Each hardening action changes the cyberattacker&#8217;s approach. The compounding effect of SFI is that attackers face a shrinking set of viable paths, while defenders gain better telemetry, stronger defaults, and sharper prioritization for the paths that remain.



Security is a team sport. We are grateful for the partnership of our customers, security researchers, and the broader industry as we work together to make the world a safer place for all.




Read the July 2026 SFI progress report




Learn more




Defending the cloud at AI speed.



Beyond the benchmark: Advancing security at AI speed.




To learn more about Microsoft Security solutions, visit our&nbsp;website.&nbsp;Bookmark the&nbsp;Security blog&nbsp;to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity)&nbsp;for the latest news and updates on cybersecurity.
The post Securing our future: July 2026 progress report on Microsoft&#8217;s Secure Future Initiative appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/07/10/securing-our-future-july-2026-progress-report-on-microsofts-secure-future-initiative/](https://www.microsoft.com/en-us/security/blog/2026/07/10/securing-our-future-july-2026-progress-report-on-microsofts-secure-future-initiative/)*
