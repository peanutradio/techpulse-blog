---
categories:
- MS
- 보안
date: '2026-06-30T19:00:00+00:00'
description: 'The quantum-safe timeline has changed




  For years, planning for post-quantum cryptography (PQC) was framed as a future problem:
  important, inevitable, but dist'
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/06/30/microsoft-advances-quantum-safe-security-as-the-risk-timeline-shifts/
source: Microsoft Security Blog
tags: []
title: Accelerating the quantum-safe timeline
---

The quantum-safe timeline has changed



For years, planning for post-quantum cryptography (PQC) was framed as a future problem: important, inevitable, but distant. That perspective is evolving as technology advances and organizations prepare for the scale and complexity of the transition ahead. At Microsoft, we are acting on this shift by bringing our quantum-safe timeline forward so organizations can begin the transition earlier and with greater confidence.



Advances in quantum research and development have shifted the risk horizon. We believe cryptographically relevant quantum computers could arrive sooner than previously expected—and the work required to prepare is significant so organizations need to start now.



Recent government actions, including United States1 and French2 guidance to adopt quantum-safe cryptography as early as 2030 in certain high-risk systems, reflect the same conclusion: preparing for this transition is already underway.



This is a recognition that the transition to quantum-safe cryptography is a multi-year engineering effort that benefits from early planning and action, and delaying that work increases both cost and risk. This reinforces our decision to bring the work forward.



The quantum capabilities are accelerating. The time to respond is now.




Learn more about post-quantum cryptography




Accelerating our timeline



In response to these shifts, we are accelerating the Microsoft Quantum Safe Program (QSP) timeline and the goal is to transition products and services to PQC by 2029.



We are also incorporating PQC requirements into our Secure Future Initiative (SFI). This brings quantum-safe readiness into the same disciplined engineering framework we use for other critical security outcomes: clear ownership, measurable milestones, and transparent progress. Embedding these capabilities into our platforms empowers customers to move sooner and more confidently.



What “accelerating” means in practice



Accelerating our timeline means pulling forward key engineering work so new standards can be adopted earlier and modernization can begin well ahead of broad quantum impact.



Our priorities fall into three areas:



1. Upgrade network cryptography (data in transit)



Modernizing network cryptography is a prerequisite for post-quantum adoption. As an example, adopting TLS 1.3 establishes a baseline that enables hybrid and post-quantum key exchange as standards mature.



What this looks like: Critical endpoints negotiate TLS 1.3 by default, with legacy protocol use reduced or eliminated wherever possible.



2. Build crypto-agility for stored data (data at rest)



Crypto-agility—the ability to change cryptography without redesigning systems—enables the safe, timely adoption of new cryptographic standards. This requires making cryptographic settings configurable outside of the application, standardizing key management and rotation, and eliminating hard-coded algorithms.



What this looks like: Cryptographic algorithms can be updated with minimal application changes and limited service disruption. You can learn more about crypto-agility here.&nbsp;



3. Modernize cryptographic trust chains (identity, signing, certificates)



The most complex work is securing the chains of trust that underpin software, devices, and services at scale. That includes code signing, certificate issuance, key protection, and update pipelines.



What this looks like: This includes hardware-backed key protection, updated certificate lifetimes and policies, and auditable signing and issuance processes for critical trust anchors, with a transition to PQC algorithms as they become available.



What this means for our customers



Accelerating the timeline doesn’t change the core challenge: for most organizations, the hardest part isn’t selecting post-quantum algorithms. It’s understanding and updating where cryptography already exists across apps, services, networks, identities, certificates, and hardware.



Bringing this work forward means Microsoft can help organizations begin that process sooner, starting with an inventory-first approach to identify, prioritize, and modernize cryptographic dependencies with greater confidence.



We will continue to share technical guidance and operational best practices to help organizations adopt quantum-safe cryptography with confidence as they move from planning into execution.



Microsoft moving earlier allows organizations to align to that same timeline, one that reduces risk while maintaining operational continuity.



What we are hearing from customers and partners



Across industries and regions, organizations are already taking steps, with several consistent themes emerging:




The future of security is agile and the transition is iterative.




Organizations are designing for change. Building crypto‑agility into systems delivers long-term resilience so new cryptography standards can be adopted over time without redesigning systems.




Long-lived, sensitive data requires earlier protection.




Organizations are prioritizing data with long confidentiality lifetimes, recognizing that encrypted data captured today could be exposed in the future (“harvest now, decrypt later”) as cryptographic capabilities evolve.




Preparation delivers immediate value.




Organizations that begin with cryptographic discovery and lifecycle management consistently uncover existing gaps that require attention today, independent of quantum risk.




The hardest problem isn’t quantum—it’s complexity.




Most organizations lack clear visibility into where cryptography exists across applications, infrastructure, and legacy systems, making discovery and prioritization the primary challenge.



These signals shape how we are approaching quantum safety at Microsoft and how we support and empower all organizations in their readiness.



What to do now



Organizations do not need to wait; there are steps you can take today to begin the transition:




Align on strategy: Define ownership, scope, and milestones for a multi-year cryptography transition.



Design for change: Build crypto-agility into new systems so future standards shifts are updates, not fire drills.



Begin with inventory: Create and maintain a living cryptographic inventory to identify, prioritize, and modernize dependencies.



Modernize protocols: Adopt modern standards such as TLS 1.3 as a baseline across client and server systems.




Looking ahead



The Microsoft Quantum Safe Program (QSP) goes beyond future cryptography. It is part of a broader effort to strengthen long-term resilience across identity, infrastructure, data, and supply-chain security—bringing this work into the systems and platforms organizations rely on daily.



Our goal is straightforward: ensure that Microsoft platforms and services can adopt new cryptographic standards quickly and safely as they mature, so organizations can move at the same pace without disrupting their operations.



Microsoft will continue to share progress and practical guidance to help organizations plan, prepare, and move into execution as standards and cyberthreats evolve. By starting now, organizations can reduce risk today and be better prepared for what comes next.



To learn more about Microsoft Security solutions, visit our&nbsp;website.&nbsp;Bookmark the&nbsp;Security blog&nbsp;to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity)&nbsp;for the latest news and updates on cybersecurity.







1Securing the nation against advanced cryptographic attacks, whitehouse.gov. June 22, 2026.



2France to stop certifying products without quantum-safe encryption, Reuters. June 16, 2026.
The post Accelerating the quantum-safe timeline appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/06/30/microsoft-advances-quantum-safe-security-as-the-risk-timeline-shifts/](https://www.microsoft.com/en-us/security/blog/2026/06/30/microsoft-advances-quantum-safe-security-as-the-risk-timeline-shifts/)*
