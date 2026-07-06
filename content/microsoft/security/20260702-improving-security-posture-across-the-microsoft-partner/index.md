---
categories:
- MS
- 보안
date: '2026-07-02T16:00:00+00:00'
description: The Deputy CISO blog series is where&nbsp;Microsoft&nbsp;&nbsp;Deputy
  Chief Information Security Officers&nbsp;(CISOs) share their thoughts on what is
  most impo
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/07/02/improving-security-posture-across-the-microsoft-partner-ecosystem/
source: Microsoft Security Blog
tags: []
title: Improving security posture across the Microsoft partner ecosystem
---

The Deputy CISO blog series is where&nbsp;Microsoft&nbsp;&nbsp;Deputy Chief Information Security Officers&nbsp;(CISOs) share their thoughts on what is most important in their respective domains. In this series, you will get practical advice, tactics to start (and stop) deploying, forward-looking commentary on where the industry is going, and more. In this article, Raji Dani, Vice President and Deputy CISO for Microsoft business functions, finance, and marketing dives into the importance of securing customer service solutions.



Following up on our previous post about managing risk in customer support operations, I wanted to share insight into how we manage the potential risk associated with another critical element of our ecosystem: Microsoft partners that we work with to help our customers deploy and manage some of our products.



While organizations often rely on a wide range of partners, including hardware suppliers and application developers, this post focuses on a specific category of trusted partners that many enterprises use to manage and maximize the value of their technology investments. For Microsoft, these partners are Microsoft Cloud Solution Providers (CSPs), and they help customers buy, manage, and optimize cloud services like Microsoft 365&nbsp;and Microsoft Azure.



Like many organizations, Microsoft has a strong partner network that is a core part of the success of its services. Partners play a critical role in reaching and enabling broad customer segments and are core to our commercial business and go-to-market strategy. It’s therefore critical that we understand and manage risk in this space. This helps us ensure that the Microsoft partner ecosystem remains healthy, compliant, and effective, and ultimately helps drive the best outcomes for our customers. Keep reading to learn about the approach we have taken at Microsoft to secure this ecosystem, along with our roadmap for upcoming work in this space.



The risks facing partner ecosystems



As with the other business areas we have written about, the risks here are not theoretical. Threat actors, including nation-states, look to exploit partners as a vector to attack customers. Microsoft relies on its partners to engage deeply with customers across multiple scenarios. Cyberattackers in turn see this as a potential opportunity to exploit those customers through the infrastructure and platforms used by Microsoft partners.



CSPs often manage a large set of downstream customers, which means compromise of a CSP can have a large impact.



If not securely configured, a cyberattacker with access to a CSP’s tenant could potentially gain access to a broad set of customers managed by that CSP. As a result, CSPs can become targets of cyberattackers looking to steal large quantities of customer data or compromise customer resources in Azure. Again, these risks are not theoretical. We have seen nation-state attackers target our CSPs with this exact goal in mind.



This is a particularly challenging problem because securing this ecosystem depends on work taken on by both Microsoft and its partners. Microsoft provides the platforms that CSPs use to operate, while each partner manages their own tenants used for CSP operations. We need to ensure every element of this space is secure, since threat actors can exploit weaknesses in any part of the ecosystem.



How Microsoft secures its partner ecosystem



As with other key business areas, it is the goal of Microsoft to enable business success while managing risk. In the CSP scenario, this means building strong protections into the platforms that our CSPs depend on, enabling robust visibility into potential misuse of those platforms, and working with our CSPs to continually raise security standards within their own environments.



We continue to invest in strengthening security in this space at Microsoft. Our approach is guided by a set of core principles that can be applied broadly across partner ecosystems, helping organizations reduce risk and improve resilience. The following sections outline these principles and how Microsoft is implementing them in practice.



1. Partner vetting



Before an organization can begin operating as a CSP, it goes through a vetting process ensuring its validity. This process verifies the identity of the organization and ensures that it legitimately intends to operate as a CSP. This complements the work we are doing to improve CSP security posture. Partner vetting helps ensure that only legitimate organizations can enter the ecosystem, while CSP security posture improvements help enhance the operating standards of organizations already in the ecosystem. We continue to enhance these vetting capabilities based on an understanding of threat intelligence and cyberattacker trends.



2. Enhancing security posture of CSP tenants



Security in the CSP ecosystem is a shared responsibility, with Microsoft enforcing controls at the platform and control plane layer through mechanisms like granular delegated administrative privileges (GDAP), while CSPs are responsible for maintaining the security posture of their tenants. To reduce the risk of tenant compromise and limit negative downstream effects on customers, we have evolved CSP authorization to incorporate mandatory security requirements as a condition for obtaining and retaining authorization. This establishes a clear expectation that maintaining a strong security posture is not optional, but a prerequisite for operating as an authorized CSP.



As the threat landscape continues to evolve, we will periodically reassess the expectations associated with CSP authorization to ensure they remain aligned with the risks facing the ecosystem. This may, over time, result in refinements to the security baseline we define for our partners. We will continue to collaborate closely with our partners to maintain clarity and alignment as these expectations evolve.



3. Least privilege for access to downstream customers



CSPs require access to customer environments to perform their management operations. But this does not mean that a CSP needs unfettered access to those customer environments. Instead, access from a CSP to a customer tenant should follow the principles of least privilege and have strong role-based access control (RBAC). Access should only be granted with customer consent and should be constrained both in terms of scope and duration. The GDAP protocol enables CSPs to manage downstream customers based on these principles.



As part of this access control principle, we have built capabilities that allow internal Microsoft security teams to rapidly revoke a CSP’s GDAP access to customers when required. This capability can be used in a range of scenarios, including incident response, changes in partner status, or termination of a partner relationship. It helps ensure that access can be quickly withdrawn and contained when risks are identified, limiting potential impact to downstream customers.



4. Strong monitoring and response capabilities throughout the stack



Microsoft is responsible for providing strongly secured common platforms and key to that promise is robust telemetry, monitoring, and incident response capabilities across those platforms. We collect a high volume of diverse telemetry signals from across our platforms and analyze them to detect suspicious activity. This enables our security response teams to quickly identify and respond to CSP-targeting threats that arise from our platforms. Containing risk in this way is an important reason that Microsoft reserves the right to revoke a CSP’s GDAP access to downstream customers when required.



In short, we have made a set of improvements to the security posture across the CSP ecosystem, both at the Microsoft platform layer and at the partner tenant layer. Like all other areas of security, our work here is never completely done. We plan to continually enhance security across all of these areas as we learn more about cyberattacker trends and risks to the ecosystem.



Protecting partners and customers means protecting the ecosystem



The key lesson here remains that the platforms we provide to partners cannot be an afterthought when it comes to security. Even though these partner platforms are not directly part of the product or service infrastructure we maintain, Microsoft must treat them just like it does its “core” infrastructure. Cyberattackers do not care whether a given system is considered internal or marked for external use. If it gives them a way to achieve their goals (in this case the compromise of customers) they will look to exploit it.



This applies broadly to any organization working with partners. As the provider of a partner platform, there is a responsibility to protect both partners and customers by ensuring these platforms meet the highest security bar, and that is what we at Microsoft are working diligently to do.




	

	
		
			
				

MicrosoftDeputy CISOs



To hear more from Microsoft Deputy CISOs, check out the OCISO blog series:



To stay on top of important security industry updates, explore resources specifically designed for CISOs, and learn best practices for improving your organization’s security posture, join the Microsoft CISO Digest distribution list.

			
		
					
				
																				
			
			





To learn more about Microsoft Security solutions, visit our&nbsp;website.&nbsp;Bookmark the&nbsp;Security blog&nbsp;to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity)&nbsp;for the latest news and updates on cybersecurity.
The post Improving security posture across the Microsoft partner ecosystem appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/07/02/improving-security-posture-across-the-microsoft-partner-ecosystem/](https://www.microsoft.com/en-us/security/blog/2026/07/02/improving-security-posture-across-the-microsoft-partner-ecosystem/)*
