---
categories:
- MS
- 보안
date: '2026-07-08T17:00:00+00:00'
description: AI models have reached a threshold where they exhibit expert-level capabilities
  in vulnerability discovery, exploit chaining, and proof-of-concept generation. A
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/07/08/protecting-microsoft-at-ai-speed-how-sfi-proactively-hardens-our-cloud/
source: Microsoft Security Blog
tags: []
title: Protecting Microsoft at AI speed: How SFI proactively hardens our cloud
---

AI models have reached a threshold where they exhibit expert-level capabilities in vulnerability discovery, exploit chaining, and proof-of-concept generation. As AI-powered vulnerability discovery matures, every organization that builds or runs software at scale needs continuous proactive evaluation to ensure security controls are correctly implemented, layered effectively, and working as intended in production.



At Microsoft we encompass these security requirements, along with threat knowledge and operational frameworks in&nbsp;our Secure Future&nbsp;Initiative (SFI),&nbsp;to guide what a well-defended cloud service looks like.&nbsp;But defining the requirements is only the start.&nbsp;Meeting them means continuously&nbsp;evaluating our live services against them,&nbsp;at AI speed.&nbsp;&nbsp;



That is why Microsoft built a multi-agent AI system that proactively evaluates and hardens our cloud infrastructure—matching the speed, scale, depth, and quality needed for our unique hyper-scale production environments. This system is purpose-built to evaluate Microsoft&#8217;s own cloud services against our stringent security requirements and make our infrastructure harder to compromise. While this is an internal capability and not available as a customer-facing product or service, the insights and patterns we develop through this work will inform how we improve our products over time. This system complements existing tools in Microsoft&#8217;s security ecosystem. For example, this system incorporates code-level vulnerabilities, including from systems like codename MDASH and adds configuration, identity, network, and runtime context, to assess overall service security posture. 



A&nbsp;modern&nbsp;AI&nbsp;architecture for proactive defense&nbsp;



Vulnerabilities don&#8217;t just live in code. They emerge from the interplay between how a service is built, configured, deployed, and connected. Consider a cloud service where the application code passes every security review, the identity configuration follows least-privilege policy, and the network rules restrict inbound traffic as designed. Individually, each component is compliant. The system evaluates the service as a whole and may find that a combination of a permissive service-to-service trust relationship, a token scope that grants broader access than the service requires, and a deployment configuration that exposes an internal API to an adjacent network tier creates a composite vulnerability that no single-component review would surface.



At its core,&nbsp;the system employs a multi-tier agent hierarchy: orchestration agents for workflow management, analysis agents that specialize in security reasoning and are grounded in Microsoft’s threat intelligence—including emerging patterns and threat actor activity—and evidence-gathering agents that investigate across code repositories, infrastructure definitions, identity configurations, runtime settings, network topologies, and live resource states.&nbsp;&nbsp;



The result of this multi-stage analysis is a comprehensive security understanding of each service that goes beyond what any single analysis method can&nbsp;provide on its own.&nbsp;Compared to traditional human-led security reviews that take weeks,&nbsp;the system&nbsp;compresses the same depth of analysis into hours.&nbsp;



How it works: The system follows a multi-stage analysis pipeline, where each stage builds on the one before it:




Profiles&nbsp;each&nbsp;service architecture&nbsp;to understand components, data flows, trust boundaries, risk exposure, and more.&nbsp;



Enumerates applicable security controls&nbsp;based on&nbsp;SFI requirements across identity, network, tenant isolation, engineering systems, and detection domains.&nbsp;



Verifies control implementations&nbsp;against real-world code, configurations, and cloud resources.&nbsp;



Evaluates defense-in-depth coverage to help ensure layered protections exist across all control domains. 



Identifies&nbsp;where controls are missing, misconfigured, or brittle, and maps the compensating controls that&nbsp;determine&nbsp;whether a gap is exploitable in practice.&nbsp;



Produces&nbsp;compensating controls and durable fix recommendations for immediate-risk reduction while driving lasting remediation.&nbsp;



Continuously learns and improves by incorporating feedback from security reviewers and service teams, and by tapping into Microsoft’s evolving threat intelligence to adapt to new patterns.&nbsp;




Core design principles&nbsp;&nbsp;



The analysis pipeline is shaped by four principles that&nbsp;determine&nbsp;how&nbsp;the system reasons about security:&nbsp;



1. Frontier-ready architecture



The system is built with modular model interfaces that can take advantage of new frontier capabilities as they emerge.&nbsp;New models, enhanced planning, and execution capabilities can be integrated behind stable agent interfaces—preserving existing tooling, orchestration, knowledge, pipelines, reporting, and governance.&nbsp;&nbsp;



2. Compositional risk reasoning



The system&nbsp;uses “what-if” agentic ideation to reason compositionally about risk. It explicitly explores how individual security gaps can chain together into multi-step attack paths. For example, a minor misconfiguration in identity,&nbsp;combined with&nbsp;a seemingly unrelated&nbsp;network exposure,&nbsp;and a missing data encryption control,&nbsp;might together enable a serious breach. Modern attacks are often complex sequences rather than single bugs, and&nbsp;the system&nbsp;is designed to help identify and analyze&nbsp;them. By running diverse models and large-scale reasoning trials in parallel,&nbsp;the system explores an expansive space of scenarios that traditional static analysis or single-scan tools would miss.&nbsp;



3. Service-specific adaptation



Cloud services aren’t one-size-fits-all, so security analysis shouldn’t be either. Rather than applying a fixed checklist, the system builds a service-specific understanding of each service it analyzes. It profiles the service in depth—identifying its components, mapping data flows, locating trust boundaries, and determining which security controls should apply given that service’s unique architecture and risk profile. If a service uses a novel pattern, a microservices architecture spanning multiple codebases, or an agent-to-agent communication model, the system adapts its analysis to account for those patterns. This adaptive approach, guided by current SFI requirements, means that the system can tackle emerging cloud paradigms that don’t fit traditional security checklists.



4. Defense-in-depth evaluation



A key focus area for SFI is layered defense. The system asks two questions: “What vulnerabilities exist?” and “Where does this service lack multiple lines of defense?”. It evaluates whether critical security domains have overlapping, robust controls, and it flags any missing or brittle layers—even if no immediate exploit is identified.



For example, the system will highlight a scenario where a service might have a weak network segmentation or an overly permissive admin role—even in the absence of a known attack—because those gaps mean a single failure could lead to a compromise.



This forward-looking, “assume breach” analysis embodies the Zero Trust and defense-in-depth principles reinforced by SFI. In an era when AI-assisted attackers can enumerate systems faster and chain together weaknesses more systematically, ensuring redundant safeguards is increasingly critical.&nbsp;&nbsp;



The assurance tree: SFI in action&nbsp;



At the core of the system are the SFI engineering and security principles: a structured body of security requirements shaped by years of hardening the Microsoft infrastructure. These requirements guide what the system evaluates, how it reasons about risk, and the recommendations generated. When security expectations evolve—whether to address a new class of threats or incorporate lessons from remediation—the system’s reasoning evolves with them. The assurance tree is how we express these requirements: a structured, hierarchical map of security controls that the system expects a service to have in place, tailored to that service&#8217;s usage and design.



As the system profiles a cloud service, it generates an assurance tree tailored to that service. At the top level of the tree are the fundamental security domains, that map to the SFI pillars.&nbsp;Each of these domains is recursively decomposed into more granular controls and sub-controls&nbsp;tailored to the service.&nbsp;For instance, Identity security decomposes into controls for password policies, OAuth token handling, and MFA enforcement—down to verifying that the service’s code correctly validates a JSON Web Token’s issuer and&nbsp;expiration. The assurance tree guides the system’s evidence-gathering agents to verify that thousands of expected controls&nbsp;are&nbsp;in place and effective—or to identify where something is missing.&nbsp;



This approach turns security from an open-ended hunt into a systematic verification of the SFI requirements: the system is essentially asking, “Have all the security measures that should protect this service been properly implemented?&#8221;. Crucially, it goes further—considering how individual gaps might combine, helping to ensure that even combinations of missing controls are identified and addressed. 



Proven results:&nbsp;From&nbsp;theory to practice&nbsp;



Within a few months, the system has enabled Microsoft security engineering teams to proactively harden our cloud services. It generates findings and recommendations which our security engineering teams then validate and implement. Because the system evaluates the whole service in context and reasons about the severity and exploitability of each issue before surfacing it, its findings have proven high quality and actionable: more than 90% have been confirmed as genuine security issues by our security engineers, enabling proactive action to improve security posture. Just as important as the volume and precision of findings is their nature. Many issues the system discovers are nuanced, cross-domain vulnerabilities that wouldn’t have been caught by traditional methods. For example, the system has uncovered security gaps that only become apparent when considering code, configuration, and cloud resources together—the kind of issue that isolated scans or compliance checklists could overlook.  



This capability allows us to enhance how we do security reviews. Traditionally, a deep security review of a complex service might span weeks of effort by multiple domain experts. The system can achieve a thorough review in a matter of hours—allowing teams to assess more services, more frequently.



The path forward: Applying these principles in your environment



If you are responsible for security at your organization, the key question is whether your defenses are keeping pace. AI models will continue to evolve. The organizations that are hardest to compromise will be the ones that have layered, verified controls already in place—not the ones that react fastest after something is found.



Based on what we have learned from building and operating this system, here are three principles any organization can apply now:




Go beyond code scanning to system-level discovery.&nbsp;The most consequential issues emerge not from a single bug, but from how factors including code, configuration, identity, and network interact in production. Collect rich signals across these domains and evaluate your services as composed systems, not isolated components.&nbsp;Prioritize composite attack paths over individual findings.&nbsp;



Move beyond known vulnerability patterns to proactive defensive controls.&nbsp;Traditional scanning asks,&nbsp;&#8220;Is there a known bug here?&#8221; Proactive hardening asks,&nbsp;&#8220;Does this service have comprehensive controls and&nbsp;layered defenses?&#8221; Reason about not just vulnerabilities, but controls, and how defense-in-depth coverage can improve protection before a specific exploit is discovered.&nbsp;



Integrate AI to drive proactive prevention at machine speed.&nbsp;The same AI capabilities that accelerate vulnerability discovery can be applied to continuously evaluate whether security controls are correctly implemented, layered effectively, and working as intended. Organizations that adopt AI-powered proactive evaluation will identify and close gaps faster than those relying solely on periodic manual review.&nbsp;




For deeper guidance on implementing AI-powered defense for an AI-accelerated threat landscape, customers can review Secure Now guidance for AI‑powered security and proactive defense. Any customer with a Microsoft Entra ID can access it. Microsoft Security customers will&nbsp;also&nbsp;have access to capabilities that enable them to assess their exposure and&nbsp;take action.&nbsp;



Moving forward, we will share more about how we are scaling our response operations to match machine speed and how SFI&#8217;s engineering practices are evolving for this new reality.



To learn more about Microsoft Security solutions, visit our website. Bookmark the Security blog to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity) for the latest news and updates on cybersecurity.&nbsp;
The post Protecting Microsoft at AI speed: How SFI proactively hardens our cloud   appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/07/08/protecting-microsoft-at-ai-speed-how-sfi-proactively-hardens-our-cloud/](https://www.microsoft.com/en-us/security/blog/2026/07/08/protecting-microsoft-at-ai-speed-how-sfi-proactively-hardens-our-cloud/)*
