---
categories:
- MS
- 보안
date: '2026-08-19T17:30:00+00:00'
description: 'Security teams are overwhelmed with findings but still struggle to answer
  a simple question: which risks matter right now? A vulnerability alone is rarely
  the p'
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/08/19/microsoft-named-a-leader-in-the-frost-radar-cloud-workload-protection-platforms-2026/
source: Microsoft Security Blog
tags: []
title: 'Microsoft named a Leader in the Frost Radar™: Cloud Workload Protection Platforms,
  2026'
---

Security teams are overwhelmed with findings but still struggle to answer a simple question: which risks matter right now? A vulnerability alone is rarely the problem. The same vulnerability running in production, exposed through a misconfiguration or over-permissioned identity, is a real path to compromise. Organizations do not need longer lists of alerts. They need context that connects code, cloud resources, identities, and runtime activity so they can prioritize the issues that pose the greatest risk and stop cyberthreats before they reach production.



As organizations adopt cloud-native architectures at scale, protecting workloads requires more than scanning. Today, 82% of container users run Kubernetes in production, making runtime visibility and protection critical for modern applications.1 



That change, from scanning workloads to protecting them where they run, is exactly what Frost &amp; Sullivan describes in its Frost Radar™: Cloud Workload Protection Platforms, 2026. Out of more than 45 qualified vendors, it benchmarked 20, and it found the category moving to a single runtime security model, one that ties together code, cloud, runtime, identity, and the security operations center (SOC).




Read the full Frost Radar report




Within that market, Frost &amp; Sullivan names Microsoft a visionary leader, its category for vendors that balance innovation with growth and help set the direction of the market. Microsoft is also the largest cloud workload protection platform (CWPP) provider by revenue, with an estimated share of more than 22% of the global CWPP market. In the analyst&#8217;s words:



&#8220;Microsoft is positioned as a visionary leader in this analysis for its scale and breadth of [Microsoft] Defender for Cloud within a unified framework. The platform stands out for its breadth of coverage across infrastructure, workloads, identities, entitlements, data, and applications, and for its deep integration with Microsoft’s broader security ecosystem, allowing organizations to secure modern and AI-native application lifecycles, while reducing operational complexity.&#8221;



Scale and breadth, in one framework. That is what customers are asking for, and it is where this category is heading.&nbsp;






Why cloud workload protection is being redefined



For a long time, protecting a workload meant scanning its image, fixing known vulnerabilities, and hardening configurations before deployment. That still matters. But it is no longer enough, because what looks safe before deployment can become exploitable once the workload is running.



Most teams are also dealing with real sprawl. A modern estate spans several clouds and mixes containers, Kubernetes, serverless functions, microservices, and AI workloads. Every layer throws off its own signals, and those signals rarely connect on their own. One misconfiguration looks harmless until it sits next to an over-permissioned identity and a container that is already live. Then it is a path into production.



The tools were not built for this. Posture sits in one console, workload scanning in another, detection in a third, and teams are left connecting them by hand, usually in the middle of an incident. What they need instead is one platform that can:




Bring posture, runtime, identity, and control-plane signals into one place.



Rank risk by what is truly exploitable, not by a severity score alone.



Stop risky workloads close to deployment, before they reach production.



Get what it finds at runtime to the developers and the SOC who can act on it.




The market is moving the same way. Frost &amp; Sullivan expects CWPP spending to grow from $6.43 billion in 2025 to about $7.95 billion in 2026, and 19.1% a year through 2030. That is teams voting with their budgets to modernize cloud security, meet regulation, and protect the workloads behind their apps, data, and AI services.



What distinguishes leading platforms



Frost &amp; Sullivan scores vendors on two things: how fast they innovate and how fast they grow. But the report is blunt about something more telling: the bar for leadership has moved. It is now, in the analyst’s words:



“Increasingly defined by runtime telemetry depth, container, and K8s security, workload behavior analysis, cloud-native threat detection, remediation and response automation, SOC integration, AI workload protection, and global go-to-market execution.”



Put plainly, discovery, scanning, and compliance checklists no longer separate the leaders. Depth at runtime does. The platforms pulling ahead tend to share a few traits:




They cover real ground, from infrastructure and workloads to identities, data, and applications, without asking you to bolt five products together.



They go deep at runtime, not just posture and log review.



They carry cloud detection and response (CDR) straight into the SOC.



They connect code, cloud, and the SOC instead of treating each as its own island.



They span clouds with both agent and agentless coverage, and they are moving quickly on AI and data security.




None of that is about longer findings lists. It is about context: seeing how the pieces connect and acting on the few that matter.



How Microsoft helps organizations protect cloud workloads



Microsoft’s capabilities address the problems customers raise most, and Frost &amp; Sullivan points to the same strengths:&nbsp;



&#8220;The strength in scaled runtime protection depth, strong CDR expansion, and ability to operationalize cloud runtime security across [Microsoft] Defender XDR, [Microsoft] Sentinel, GitHub, [Microsoft] Security Copilot, and the broader Microsoft security stack give Microsoft clearest advantages, particularly for large enterprises that already operate across Microsoft security, Azure infrastructure, GitHub, and Sentinel environments.&#8221;



Here is what that looks like in practice, starting from the problem in each case.&nbsp;



1. Protect workloads while they are running



Microsoft Defender for Cloud watches workloads while they run. A lightweight sensor (eBPF-based) picks up Kubernetes events, process activity, and network traffic, and detections map to MITRE ATT&amp;CK, so alerts line up with real cyberattacker behavior. Most of the recent effort has gone into the container layer: DNS detection for Kubernetes on Azure AKS, Amazon EKS, and Google GKE; anti-malware that blocks rather than just alerts; runtime protection for EKS Bottlerocket; and drift blocking when a binary changes mid-run.



Defender for Cloud can also act before a workload starts. Kubernetes&#8217; gating applies policy at the cluster and namespace level, so a risky or non-compliant image is blocked before it ever starts. Frost &amp; Sullivan calls this out as especially relevant to CWPP, because it puts preventive controls right next to production. That is the whole idea: catch a bad image before it becomes an incident, not after.




Get started with Microsoft Defender for Cloud




2. Get runtime signal to the SOC



Runtime signal only helps if it reaches the people who respond. With expanded CDR, Defender for Cloud ties runtime telemetry, Kubernetes audit data, process and network activity, control-plane events, and identity signals to specific workload incidents, then hands them to Microsoft Defender XDR and Microsoft Sentinel. A suspicious process in a running cluster does not land as a lonely alert. It arrives already connected to the identity that launched it and the activity around it.



For the SOC, that means faster answers and far less stitching signals together by hand.



3. Send runtime findings back to the developers who can fix them



Finding a problem at runtime is only half the work. Someone still has to fix it. Defender for Cloud links runtime context, exploitability, and attack-path detail to developer workflows through GitHub Advanced Security and Copilot Autofix, syncing both ways between security and development. A risk caught in production can go straight to the engineer who owns the code, get fixed at the source, and be checked afterward.



The right issue reaches the right owner, and security and DevOps finally work from the same list.



4. Extend protection to AI and across clouds



More and more, the workloads worth protecting are AI. Defender for Cloud supports model scanning and threat protection, including prompt injection and suspicious access, for Azure AI Foundry and Azure OpenAI, and AI security posture management for Google Vertex AI and Amazon Bedrock. It spans Microsoft Azure, Amazon Web Services (AWS), Google Cloud Platform (GCP), and hybrid environments with both agent and agentless coverage, and Microsoft Security Copilot adds guided investigation across the workflow.



Protection follows the workload, whether that is a new AI service or a third cloud.



What this signals for security leaders



For anyone choosing a workload protection platform this year, the shift in this report changes the questions worth asking. The ones to put at the top:




Is workload protection part of one cloud security platform, or a separate tool wired onto the SOC after the fact?



Can it stop a risky workload before production, or only flag it afterward?



Does it connect runtime activity to identity, data, and control-plane context, and rank what is genuinely exploitable?



Do its findings reach both the SOC and the developers who can act on them?



Does it hold up across several clouds and AI workloads?




The vendors that can answer &#8220;yes&#8221; are the ones shaping what comes next, and the Frost Radar places Microsoft among them.



Bottom line



Frost &amp; Sullivan&#8217;s Frost Radar™: Cloud Workload Protection Platforms, 2026 reinforces a clear shift. Cloud workload protection is leaving isolated scanning behind for runtime security that connects posture, identity, code, and the SOC. Frost &amp; Sullivan positions Microsoft as a visionary leader, and the largest CWPP provider by revenue, because Defender for Cloud brings that range together in one framework, goes deep at runtime and in CDR, and plugs into the wider Microsoft security stack.




Get the full report




Learn more




Read Frost &amp; Sullivan’s Frost Radar™: Cloud Workload Protection Platforms, 2026 to see how leading vendors are evaluated and how the category is shifting toward unified cloud runtime security. 



Explore&nbsp;Microsoft cloud security solutions&nbsp;to see how unified posture management, risk prioritization, and protection across cloud and AI can help reduce risk.




To learn more about Microsoft Security solutions, visit our&nbsp;website.&nbsp;Bookmark the&nbsp;Security blog&nbsp;to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity)&nbsp;for the latest news and updates on cybersecurity.







1Kubernetes Established as the De Facto &#8216;Operating System&#8217; for AI as Production Use Hits 82% in 2025 CNCF Annual Cloud Native Survey. PR Newswire, January 20, 2026.
The post Microsoft named a Leader in the Frost Radar™: Cloud Workload Protection Platforms, 2026 appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/08/19/microsoft-named-a-leader-in-the-frost-radar-cloud-workload-protection-platforms-2026/](https://www.microsoft.com/en-us/security/blog/2026/08/19/microsoft-named-a-leader-in-the-frost-radar-cloud-workload-protection-platforms-2026/)*
