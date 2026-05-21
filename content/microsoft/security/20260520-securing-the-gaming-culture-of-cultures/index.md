---
categories:
- MS
- 보안
date: '2026-05-20T16:00:00+00:00'
description: 'The Deputy CISO blog series is where&nbsp;Microsoft&nbsp;Deputy Chief
  Information Security Officers&nbsp;(CISOs) share their thoughts on what is most
  important '
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/05/20/securing-the-gaming-culture-of-cultures/
source: Microsoft Security Blog
tags: []
title: Securing the gaming culture of cultures
---

The Deputy CISO blog series is where&nbsp;Microsoft&nbsp;Deputy Chief Information Security Officers&nbsp;(CISOs) share their thoughts on what is most important in their respective domains. In this series, you will get practical advice, tactics to start (and stop) deploying, forward-looking commentary on where the industry is going, and more. In this article, Aaron Zollman, Vice President and Deputy CISO for Gaming at Microsoft discusses the unique challenges and rewards of securing gaming.



There are more than 500 million monthly active players¹ across Xbox consoles, PC, handheld, and more through Xbox cloud gaming. They’re the folks who come to mind when people refer to “gaming culture.” But they’re not really the whole story. Globally, more than 3 billion people engage with gaming.² The majority of these people are gamers, but the number also includes developers working for independent gaming studios, engineers supporting the Xbox platform, and the security and operations professionals that support them all.



In my role as Deputy CISO for Gaming at Microsoft, it’s this much larger, much more complex community that I have to take into account. My team and I aren’t tasked solely with protecting consoles or player accounts. We’re safeguarding intellectual property (IP), live operations, and the trust of billions of interactions. We’re also partnering on risks that range from cheating and monetization exploits to supply chain vulnerabilities and regulatory compliance for child safety and privacy.



Gaming isn’t really a single culture, but rather a culture of cultures—each with their own risk factors to account for. At the heart of gaming is the player experience—their need for seamless access, low latency, and frictionless, immersive experiences. This goes hand-in-hand with privacy and safety in a world where cyberattackers could target well-known players. But aside from those basic needs, players form their own tribes, and a diverse, global player base requires a different approach—which makes securing gaming unique. You don’t approach it like you might traditional enterprise. Studios operate with creative autonomy, platforms demand global scale and low latency, and players expect frictionless experiences. That diversity makes gaming vibrant while also creating unique security challenges.



Each culture comes with its own security risks



Let’s first take a look at the risks that most often appear with each of the overlapping cultures that make up the world of gaming:



Platforms, underpinning services like Xbox Game Pass and Xbox Cloud Gaming, require centralized infrastructure with high availability. Here, security must integrate seamlessly with identity systems and Microsoft-wide standards without slowing down gameplay. But platforms face a number of distinct risks.



The complexity of platforms makes them a rich target for financially-motivated cyberattackers seeking to take over top accounts—or send targeted messages to individuals in an environment where they aren’t expecting phishing, which can threaten both ecosystem trust and commercial strategy. And because platforms serve as the connective tissue between devices, we have to pay special attention to weaknesses in integration points.



We also contend with fraud and abuse in commerce systems, where bad actors attempt to manipulate in-game economies or exploit payment flows. These persistent cyberthreats require layered defenses, real-time monitoring, and rapid responses.




Learn more about Microsoft identity and access solutions




Game development studios, whether they are AAA giants, indie teams, or sole developers, thrive on flexibility. Their environments are highly individualized and frequently blend proprietary tools with third-party assets and co-development with partners. My job is to make sure they can innovate securely—balancing their creative freedom with governance and compliance timelines. But this flexibility introduces risks that look very different from experienced by centralized platforms.



On the plus side, studios’ independence creates smaller failure domains, leaving them free to make their own choices and experiment with new tools, partners and engineering practices, without putting the broader platform and peer studios at risk. But reputation, regulatory liability, and cyberattacker interest can’t be firewalled off so easily. So, we need to establish a baseline of controls and detect anomalies early, closing down blind spots—despite fragmented development environments and third-party risk from studios that rely on external contractors, middleware providers, and asset marketplaces.



And some of the cyberattacks are the same: Without tight identity governance, credential sprawl can create highly-privileged accounts that become prime targets for threat actors. Studios operate under tight deadlines and with small margins, so we need empathy for their desire to make things easier—and to avoid security checks when under milestone pressure—despite the risk those actions could cause to production.



It&#8217;s also important to note that the driving factor for many threat actors targeting studios is the incredibly high value of unreleased IP. For the same reason, social engineering and insider threats are a constant risk for studios.




Explore Microsoft data governance solutions




Studio Central Teams provide shared IT and infrastructure support. They’re the bridge between creative teams and operational security, ensuring that artists, producers, and marketers work in environments that are both productive and resilient. But that role comes with its own set of risks, which are often hidden in the complexity of shared services.



When central teams support diverse projects, maintaining consistent security baselines across cloud resources, build servers, and collaboration tools becomes difficult. Failing to maintain security consistency can lead to configuration drift—where a single misconfigured storage bucket or firewall rule can expose critical assets. But because central teams manage shared infrastructure, they are risk-averse to changes, including some critical security patches, that could cause cascading production failures.



These central teams can be security’s best partners for implementing strong monitoring and segmentation—but also need to be governed to avoid insider risk and toxic combinations of overlapping permissions.



Collaboration over control



Security in gaming isn’t about imposing rules. It’s more about partnership. I work closely with Temi Adabambo, General Manager for Gaming Security, Microsoft, and Eric Mourinho, Chief Architect, Microsoft, to co-develop secure environments and shared tooling. Governance is a dialogue. We collaborate between platform teams, studio IT, security architects, and technical directors in game studios. That’s how we manage exception handling, cross-team dependencies, and the tension between creative speed and security rigor.



One of the advantages of the Microsoft environment is the access it grants us to a security ecosystem that scales globally. In gaming, we build upon that foundation, adapting it for the unique needs of developers, platforms, and players:




Identity and access management: We use Microsoft Entra ID to secure identities across Xbox Live, Game Pass, and studio environments. Shared identity systems allow frictionless sign-in for players while enforcing strong authentication for developers and partners.



Compliance and governance: We rely on a combination of tools and processes to manage sensitive data and meet regulatory obligations across environments like public cloud infrastructure and bespoke studio setups. This includes Microsoft Purview for data classification and compliance monitoring, Microsoft Defender for Cloud for policy enforcement and resource hardening, Entra ID for identity governance, and Microsoft Sentinel for audit and reporting. Together, these capabilities help us maintain visibility, enforce standards, and respond quickly to compliance exceptions without slowing down development.



Threat&nbsp;intelligence and&nbsp;detection: With Microsoft Defender for Cloud, Microsoft Sentinel, and proprietary Microsoft tooling, we gain visibility into cyberthreats across platforms and supply chains. These tools allow us to detect anomalies, respond quickly, and share intelligence across teams without slowing down creative workflows.



Secure&nbsp;development&nbsp;lifecycles: We embed security into game development through automated code scanning, vulnerability management, and secure build pipelines, helping studios ship faster without sacrificing safety.




These&nbsp;are&nbsp;enterprise-grade&nbsp;capabilities, adapted to the needs of&nbsp;the global gaming culture of cultures. They allow us to protect billions of interactions while enabling the creativity that defines this industry.&nbsp;



Looking&nbsp;ahead&nbsp;



Gaming will only grow more complex. But I see that as an opportunity. Security presents challenges, but in facing those challenges head-on, we are constantly refining our practices, products, and player experiences. When we design for resilience, we protect not just games but the communities that help them thrive.



For Microsoft, that means treating gaming security as an ever-evolving system—one that changes with each new iteration of technology, player expectations, and the creative heartbeat of the industry.



Security teams and their families are gamers too. Visit the Xbox Wire and our recent blog post for Safer Internet Day to learn more about how we keep players and communities safe and secure at Xbox.




	

	
		
			
				

MicrosoftDeputy CISOs



To hear more from Microsoft Deputy CISOs, check out the&nbsp;OCISO blog series:







To stay on top of important security industry updates, explore resources specifically designed for CISOs, and learn best practices for improving your organization’s security posture, join the&nbsp;Microsoft CISO Digest distribution list.

			
		
					
				
																				
			
			





To learn more about Microsoft Security solutions, visit our website. Bookmark the Security blog to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and&nbsp;X&nbsp;(@MSFTSecurity) for the latest news and updates on cybersecurity.







¹Microsoft FY25 Fourth Quarter Earnings Conference Call&nbsp;&nbsp;



²Microsoft to acquire Activision Blizzard to bring the joy and community of gaming to everyone, across every device&nbsp;
The post Securing the gaming culture of cultures appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/05/20/securing-the-gaming-culture-of-cultures/](https://www.microsoft.com/en-us/security/blog/2026/05/20/securing-the-gaming-culture-of-cultures/)*
