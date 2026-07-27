---
categories:
- MS
- Azure
date: '2026-07-09T17:00:00+00:00'
description: What&#8217;s new todayGPT‑5.6 is generally available in Microsoft Foundry,
  alongside the Asia-Pacific Data Zone, and hosted agents in Foundry Agent Service.
  See
draft: false
original_url: https://azure.microsoft.com/en-us/blog/gpt-5-6-now-available-in-microsoft-foundry/
source: Azure Blog
tags:
- AI + machine learning
title: GPT-5.6 now available in Microsoft Foundry
---

What&#8217;s new todayGPT‑5.6 is generally available in Microsoft Foundry, alongside the Asia-Pacific Data Zone, and hosted agents in Foundry Agent Service. See GPT-5.6 pricing.	





	


		
		
		
		

	








AI only creates value when it shows up in real systems—systems that are reliable, observable, and aligned to business outcomes. More than 100,000 organizations are already building on Microsoft Foundry, and companies like Adobe, Telefónica, and Tata Consultancy Services are running agents in production today.



At Microsoft Build, we laid out a simple promise for the agentic era: developers should be able to build an agent where they already work, run it on infrastructure they can trust, and put it in front of the people who need it— without stitching together disconnected platforms. Today, that vision moves from roadmap to reality with three sets of updates now generally available in Microsoft Foundry:




OpenAI’s latest frontier model series: GPT-5.6 Sol, GPT-5.6 Terra, and GPT-5.6—each tuned to a different workload, available in Standard Global and Standard Data Zones.



Asia-Pacific Data Zone, giving APAC customers a regional option to run frontier OpenAI models while keeping data processing within the region.



Production agents in Foundry Agent Service, with hosted agents, toolboxes, and publishing to Microsoft 365 Copilot and Microsoft Teams.




Together, these capabilities bring frontier models, production agent runtime, enterprise-grade identity, security, and compliance controls, and distribution across Microsoft 365 into a single platform—helping organizations move from experimentation to production without assembling disconnected tools and services.



Why Foundry is the best agent platform



Microsoft Foundry is Microsoft&#8217;s end-to-end platform for building, running, governing, and distributing AI agents. Foundry brings together the capabilities organizations need to move agents into production across three pillars:




Build: Open and flexible across models and frameworks.



Generate: Connected to enterprise data, tools, and users.



Govern: Secured, managed, and optimized for long-term value.




These pillars come together in the latest Foundry updates, helping organizations build, run, and scale production agents on a single platform.




	
		
		
	
	



Build with any framework and model on the industry’s end-to-end AI platform



Agent development starts where developers already work—in GitHub Copilot and Microsoft Visual Studio (VS) Code—with the Foundry Toolkit for VS Code and the Foundry skill handling deployment to Foundry. Whether teams build with Microsoft Agent Framework, GitHub Copilot SDK (generally available), or Claude Agent SDK, Foundry is the production destination—and it all starts with the right model.



Start with the right model for the right job



An agent is only as capable as the model reasoning behind it. Microsoft Foundry gives organizations access to industry-leading frontier, open-source, and task-specific models through a single platform, allowing teams to choose the right model for every workload.



Today, we&#8217;re making OpenAI&#8217;s GPT-5.6 series generally available in Microsoft Foundry Models and Microsoft Foundry Agent Service:




GPT-5.6 Sol delivers the most advanced reasoning capabilities yet, supporting extended reasoning, agentic workflows, and code-focused scenarios, for the most demanding enterprise workloads.



GPT-5.6 Terra is a balanced model for everyday work, delivering performance competitive with GPT-5.5 at a lower cost, making it ideal for scaling intelligent applications across the enterprise.



GPT-5.6 Luna is the fastest and most affordable model in the family, making it well suited to high-volume, latency-sensitive workloads.




Together, the GPT-5.6 series gives organizations the flexibility to match model capability, cost, and performance requirements to specific business scenarios, rather than forcing every workload onto a single model.



Customers consistently tell us that access to new models matters as much as model quality. That’s why we are making GPT-5.6—the latest model—available through Global Standard and Global Priority Processing for all the existing 28 global regions, Data Zones Standard, and Global Provisioned from day one. This enables customers to adopt the latest frontier AI innovations where they have already built, deployed, and scaled their applications.



GPT-5.6 pricing for Sol, Terra, and Luna



The following table outlines GPT-5.6 pricing for Sol, Terra, and Luna in Microsoft Foundry. Use this pricing information to compare model options and plan deployment costs:



Model Deployment Pricing (USD $/million tokens) Input Output GPT-5.6 Sol Standard Global 5.00 30.00 GPT-5.6 Terra Standard Global 2.50 15.00 GPT-5.6 Luna Standard Global 1.00 6.00 



Run frontier AI where your business operates



More models, in more regions, are only half of what platform expansion means. The other half is more places to run them compliantly. This is exactly what the Asia-Pacific Data Zone delivers. Today we&#8217;re also announcing the general availability of the Asia-Pacific (APAC) Data Zone for Microsoft Foundry, enabling APAC customers to run frontier OpenAI models while keeping data processing within the Asia-Pacific regions, with no separate environment to stitch together and no waiting for capability to catch up.



With Global, Data Zone, and Regional deployment options available in Foundry, organizations can align AI adoption with their sovereignty, compliance, performance, and scale requirements while maintaining a consistent development and operations experience across environments.




As financial institutions adopt AI, responsible data handling becomes foundational to trust. Microsoft Foundry’s APAC Data Zone allows us to keep data processing regionally anchored while accessing advanced AI models at scale. This gives us the confidence to accelerate AI innovation responsibly and reinforces our ambition to be a leading AI-powered financial platform in Asia.
—Hongsoo Kim, Chief Data and AI Officer (CDAO), Viva Republica (Toss)



Generate impact with action-oriented, context aware agents



A&nbsp;capable model is only the starting point.&nbsp;To put one to work&nbsp;in production,&nbsp;an agent&nbsp;also&nbsp;needs&nbsp;somewhere to run, knowledge of your business, governed access to its tools, memory that carries across interactions&nbsp;and acts on real-world events, and a path to the people who use it.&nbsp;Foundry provides each of&nbsp;these&nbsp;as a&nbsp;built-in&nbsp;capability,&nbsp;designed to&nbsp;work&nbsp;together.




Where it lives: Hosted agents&nbsp;in Foundry Agent Service is now generally available,&nbsp;giving developers&nbsp;one production runtime for agents built with any framework&nbsp;and harness—Microsoft Agent Framework, GitHub Copilot SDK, LangGraph,&nbsp;OpenClaw,&nbsp;Hermes,&nbsp;and others. It&#8217;s enterprise-ready on day one:&nbsp;Network&nbsp;isolation&nbsp;with&nbsp;Microsoft Azure Virtual Network (VNet) integration&nbsp;keeps&nbsp;agent traffic inside your security boundary. For long-running workloads, the new&nbsp;resilient&nbsp;task support in hosted agents&nbsp;(private&nbsp;preview)&nbsp;makes&nbsp;it&nbsp;easier to build agents that survive failures. The platform provides primitives to keep the sandbox running,&nbsp;your harness provides checkpointing, and together they allow an&nbsp;agent to resume when it restarts. The result: multi-turn conversations, reasoning loops, and human-in-the-loop approvals can pick up where they left off, without developers having to build their own recovery, retry, and state-management.



How it talks: Hosted agents with Voice Live is now generally available. Developers can add&nbsp;real-time voice experiences to the agents they built with the frameworks they prefer, using the Azure VoiceLive SDK.&nbsp;



What it knows: Foundry&nbsp;gives agents access to enterprise knowledge without&nbsp;requiring&nbsp;developers to&nbsp;build a&nbsp;complex&nbsp;retrieval pipeline from scratch. Microsoft IQ brings together Work IQ for real-time awareness of your Microsoft 365 environment, Fabric IQ for your structured data, and Web IQ for low-latency live web grounding—all unified behind Foundry IQ, now generally available as the SLA-backed knowledge layer behind every Foundry agent.&nbsp;



How it reaches its tools: Toolboxes in Foundry is generally available. Instead of shipping every tool definition on every request, a toolbox dynamically selects the right tool for the job—giving agents governed, curated access while dramatically reducing the token overhead of large tool sets.&nbsp;



How it&nbsp;remembers&nbsp;and&nbsp;responds to the world: Memory&nbsp;and&nbsp;routines&nbsp;in Foundry Agent Service are in public preview.&nbsp;Memory (procedural, user, and session) lets agents carry context across interactions. Routines run any agent on a recurring schedule or timer so it acts without a user prompt;&nbsp;new event-based triggers, powered by the connector gateway, now let the same agent wake up the moment an upstream system signals a change—a ticket filed, a file landing in storage, a workflow completing.&nbsp;



How it reaches users: Publishing to Microsoft Teams and Microsoft 365 Copilot is generally available next week.&nbsp;The agents your developers build land in the applications where&nbsp;hundreds&nbsp;of millions of people already do their work with identity, permissions, and policy flowing through automatically.&nbsp;And&nbsp;it works even for network-isolated agents: when a project runs behind a private endpoint, you publish through a documented flow rather than the one-click button. The agent stays on your private network while Microsoft&#8217;s channel adapters reach it through your own firewall and reverse proxy.




Govern&nbsp;and&nbsp;optimize&nbsp;the full AI lifecycle with observability and controls



An agent you&nbsp;can&#8217;t&nbsp;see,&nbsp;improve, or secure is an agent you&nbsp;can&#8217;t&nbsp;put&nbsp;in production—so Foundry treats trust as a platform priority, not a developer responsibility. What&#8217;s&nbsp;new in this release closes the loop around everything that happens after you&nbsp;build: seeing what an agent did, making it better, and proving it&#8217;s worth running.




How you see what it did—tracing&nbsp;and&nbsp;evaluation&nbsp;for hosted agents, generally available. See exactly what an agent did, why, and where it went wrong, and evaluate behavior systematically before and after you ship.



How it gets better and cheaper—agent optimizer in Foundry Agent Service, in public preview. It tests your prompts, skills, models, and tools together and automatically&nbsp;identifies&nbsp;better configurations—often letting you hold quality while moving to a smaller, cheaper model.



How you prove its value—ROI for agents in Microsoft Foundry, in private preview. It connects an agent&#8217;s traces, business-value evaluations, and operating cost into a single view—surfacing KPIs like net value, total cost, and current ROI in the dashboard, so&nbsp;customers&nbsp;can see whether a production agent is creating more value than it costs to run, and drill into traces when it isn&#8217;t.&nbsp;




As agents scale from pilots to thousands of runs a day, Foundry gives teams the levers to keep spend predictable without leaving the platform.



That starts with choice.




Choose how you deploy. Foundry offers Global, Data Zone, and Regional deployments,&nbsp;so you can align AI to your sovereignty, compliance, and performance requirements, running frontier models while keeping data processing in-region.



Choose how you pay for&nbsp;model&nbsp;inference. A full spectrum of offers—Standard, Priority Processing, Provisioned Throughput, and Batch—lets you optimize for agility, latency, throughput, and cost on one platform.




On top of that foundation,&nbsp;model&nbsp;router matches each request to the right model, prompt&nbsp;caching cuts redundant computation,&nbsp;and PTU&nbsp;spillover and&nbsp;quota optimization preserve service continuity through usage spikes. For agents,&nbsp;toolboxes in Foundry send only the tools each request needs,&nbsp;and&nbsp;agent optimizer&nbsp;tunes prompts, skills, tools, and model choice against your own evaluators.



And spend is only half the equation.&nbsp;ROI for agents in Foundry&nbsp;connects business value, usage, and cost in one view—so teams can see whether a production agent is creating more value than it costs to run, and where cost is outpacing value.



For a hands-on walkthrough, watch our new&nbsp;Microsoft Mechanics episode&nbsp;on token economics for agents.



In production: What teams are&nbsp;building on Foundry&nbsp;&nbsp;



The organizations&nbsp;building on&nbsp;Foundry&nbsp;aren&#8217;t&nbsp;experimenting; they&#8217;re&nbsp;shipping, from digital natives to the world&#8217;s largest enterprises.




Adobe&nbsp;is building with GitHub, Foundry Agent Service, and Azure Functions, deploying agents&nbsp;for their applications and saving time and work getting to production.



Telefónica&nbsp;has adopted Microsoft Foundry as the core of their corporate agentic platform, with the first wave of agents tackling network operations—a telco’s most complex, strategic domain—across Microsoft Agent Framework, hosted agents, AI Gateway, and&nbsp;Azure&nbsp;Logic Apps.



Tata Consultancy Services&nbsp;is using agent optimizer in Foundry Agent Service to improve agent performance with a more structured approach to prompt tuning, helping teams reduce manual effort while measuring gains in task adherence and execution efficiency.




The pattern is consistent: teams that once spent weeks integrating, securing, and deploying agents are now doing it in days, on infrastructure that meets their compliance bar, reaching users through tools they already trust.



Get started



Everything&nbsp;in this post is live in&nbsp;Microsoft Foundry.



Follow the documentation and Microsoft Learn courses. Developers can get started in minutes by following the Quickstart, which walks through setting up, testing, and deploying a production-ready hosted&nbsp;agent&nbsp;end to end.



Check out AI Agents for Beginners for a 12-lesson curriculum, then go deeper with guided labs: Develop AI Agents in Azure, Hosted Agents Workshop (.NET),&nbsp;the&nbsp;Foundry Toolkit for VS Code and hosted agents workshop,&nbsp;and the ZavaShop Supply Chain Workshop.&nbsp;To put your agents on a solid quality footing, read Evaluating AI Agents: A Practical Guide with Microsoft Foundry.&nbsp;



Watch: Foundry Agent Service + Microsoft Agent Framework Explained—Jeff Hollan walks through how to operationalize AI agents from deployment to real-world impact.




	

	
		
			
				

Start building on Foundry



The end-to-end platform for building, running, governing, and distributing AI agents.




Visit Foundry


			
		
					
				
																				
			
			


The post GPT-5.6 now available in Microsoft Foundry  appeared first on Microsoft Azure Blog.

---
*원문: [https://azure.microsoft.com/en-us/blog/gpt-5-6-now-available-in-microsoft-foundry/](https://azure.microsoft.com/en-us/blog/gpt-5-6-now-available-in-microsoft-foundry/)*
