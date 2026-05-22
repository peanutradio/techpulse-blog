---
categories:
- MS
- 보안
date: '2026-05-20T15:00:00+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tWhy\
  \ we are investing in thisRAMPART: Continuous safety testing for agentic AIClarity:\
  \ Helping check software engineering"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/05/20/introducing-rampart-and-clarity-open-source-tools-to-bring-safety-into-agent-development-workflow/
source: Microsoft Security Blog
tags: []
title: 'Introducing RAMPART and Clarity: Open source tools to bring safety into Agent
  development workflow'
---

In this article
		

		
			
		
	
	
		
			Why we are investing in thisRAMPART: Continuous safety testing for agentic AIClarity: Helping check software engineering assumptionsRAMPART and Clarity available now		
	
	




The AI systems shipping inside enterprises today are fundamentally different from the ones we were building even two years ago, because they have moved well past answering questions and into accessing your email, retrieving records from your CRM, writing and executing code, and taking actions on your behalf across dozens of connected systems. That shift from &#8220;generate text&#8221; to &#8220;do things in the world&#8221; changes the safety equation entirely, because an agent that can act can also potentially act in ways nobody intended.



Today Microsoft is open-sourcing two tools designed to help engineers: Microsoft RAMPART, an agent test framework for encoding adversarial and benign scenarios as repeatable tests that can run in CI, making it easy to turn red-team findings and AI incidents into lasting regression coverage; and Clarity, a structured sounding board that helps teams figure out whether they are building the right thing before they write a single line of code.



We built these tools because we believe that AI safety has to become a continuous engineering discipline rather than a periodic checkpoint, and we think the best way to make that happen is to put practical, open tools in the hands of the people doing the building.



Why we are investing in this




Helping teams think through the “why,” before the “how” of software building: In the vibe coding era, execution is easy and the harder question is the “why.” The most expensive safety failures we see almost always trace back to design mistakes that nobody questioned early enough, long before any adversary got involved &#8212; say, when a product team decided their agent should have access to a tool, or handle a particular user flow, without fully working through what could go wrong. By the time a red team engagement surfaces the issue, the system is largely built, and addressing it means going back to the drawing board. We wanted to give product managers and engineers a way to pressure-test their assumptions at the start of a project, when changing course is cheap and the right conversation can save months of rework.



Scaling the lessons of red teaming across the industry. The techniques that uncover vulnerabilities in one agentic product almost always shed light on another. A cross-prompt injection attack that works against one system will often work, with minor variations, against a customer service agent or a coding assistant. But those lessons tend to stay locked inside individual engagement reports. Our goal was to build a system where the lessons of red teaming exercises can be turned into runnable engineering assets. &nbsp;



Making incidents reproducible and mitigations verifiable. If something goes wrong in production AI systems, the team responding needs to do two things quickly: replicate the incident so they understand exactly what happened, and verify that whatever fix they ship actually holds up against variants of the original attack. Both of those tasks are harder than they sound with probabilistic LLMpowered systems, and most teams end up doing them manually in an ad hoc way. We wanted tooling that is purpose-built for exactly this workflow, so that incident response becomes a repeatable engineering process rather than a scramble.




RAMPART: Continuous safety testing for agentic AI






RAMPART is an open-source testing framework that brings red teaming techniques directly into the development workflow. It is built on top of PyRIT, Microsoft&#8217;s open automation framework for red teaming generative AI systems so that RAMPART leverages the best in class, out of the box adversarial tests. Where PyRIT is optimized for black-box discovery by security researchers after the system is built, RAMPART is built for engineers as the system is being built.



The developer experience will feel familiar to anyone who has written integration tests. Teams write standard pytest tests that describe scenarios drawn from their threat model. Each test connects to the agent through a thin adapter, orchestrates an interaction, and evaluates observable outcomes. Tests return a clear pass or fail signal and can be gated in CI just like any other integration test. When a new tool or data source is added to the agent, the corresponding safety test can be added in the same pull request.








	
		
		
	
	







RAMPART is different from conventional testing in the following ways:




Built for prompt injection attacks: RAMPART&#8217;s most mature coverage today focuses on cross-prompt injection attacks, scenarios in which an agent retrieves or processes potentially poisoned content from documents, emails, tickets, or other data sources that manipulate its behavior indirectly.&nbsp; New threat categories can be added incrementally as attack patterns evolve, and the framework&#8217;s extension points are all defined as Python protocols, so integration stays lightweight even for complex agent architectures.&lt;



Built for probabilistic behavior: Because LLM behavior is probabilistic, RAMPART supports statistical trials. The same test can run multiple times with policies like &#8220;this action must be safe in at least 80 percent of runs.&#8221; This reflects how agents actually behave in production far more accurately than single-shot validation ever could.



Built to reproduce your AI red team findings and AI incidents: RAMPART is designed to work alongside dedicated red teaming, and the two reinforce each other. Findings from a red team engagement can be encoded as RAMPART tests, which means the issue is permanently covered, runs on every change, and never silently regresses. The ownership model is intentionally flipped from the traditional approach: engineers write the tests, engineers run them, and engineers treat failures like any other bug. The framework supplies the attack strategies, adversarial payload generation, and evaluation logic. The test author focuses on expressing expectations about what their agent should and should not do.




Agent safety ultimately comes down to what the agent does, which means evaluators need to look at which tools it invokes, what side effects occur, and whether those actions stay within expected boundaries. RAMPART&#8217;s evaluators are designed to inspect all of that. They are composable, so teams can combine them with boolean logic to express nuanced safety conditions rather than relying on a single binary signal.



Clarity: Helping check software engineering assumptions






Where most AI tools are designed to help teams execute faster, Clarity was designed by Microsoft to help them figure out whether they are executing on the right thing in the first place. It asks the kinds of questions that experienced architects, product managers, and safety engineers would ask, the ones that are easy to skip when a team is excited about building something new.



Consider a team that wants to add real-time collaboration to a document editor. Instead of jumping straight to implementation options, Clarity will ask what happens when two people edit the same paragraph at the same time, and whether the team actually needs true real-time collaboration with cursors and presence indicators, or whether &#8220;nobody loses their work&#8221; is the real requirement. Those two answers can lead to very different architectures with very different failure modes, and getting clarity on that distinction early can save months of rework.



Clarity runs as a desktop app, a web UI, or embedded directly in a coding agent. It guides engineers through structured conversations covering problem clarification, solution exploration, failure analysis, and decision tracking. As the conversation progresses, the results are written to a .clarity-protocol/ directory in the repo as plain, human-readable markdown files that get committed, reviewed in pull requests, and diffed just like source code. They capture the problem statement, the solution rationale, the failure analysis, and the key decisions made along the way.



The failure analysis deserves a closer look, because it goes well beyond what a single reviewer would typically catch. Multiple AI &#8220;thinkers&#8221; independently examine the system from different angles, including security, human factors, adversarial scenarios, and operational concerns. The team then works through the results together with Clarity, grouping related failures, tracing causal chains, and building management plans. &nbsp;



Clarity also tracks staleness across these documents, because they form a dependency graph. When a problem statement changes, Clarity knows that the solution description and failure analysis might need revisiting and nudges the team to do so. Important decisions are captured with their criteria, the options considered, and the rationale behind each choice, so that six months later anyone on the team can revisit the full reasoning, including which alternatives were ruled out and why.



The .clarity-protocol/ directory becomes a shared artifact that everyone on the team can see and contribute to, and for stakeholders who need a summary before a review, Clarity can generate a review packet that tells a coherent narrative.



RAMPART and Clarity are part of a broader movement toward spec-driven, engineering-native AI safety. They complement Microsoft’s work on policy-to-measurement systems: Clarity helps teams clarify design intent and capture assumptions; RAMPART gives teams the building blocks to write concrete agent safety testsand keep them running as agents evolve.. Together, these approaches move AI safety from a one-time review to a set of living artifacts that developers can use throughout the lifecycle.



RAMPART and Clarity available now



Both RAMPART and Clarity are available today as open source projects from Microsoft.



We look forward to working with the community. For feedback, and partnership in deploying this in the enterprise setting, please contact aisafetytools@microsoft.com.



Contributions



Microsoft RAMPART is led by Bashir Partovi with contributions from Elliot H Omiya, Richard Lundeen, Nina Chikanov, Spencer Schoenberg, and Toby Kohlenberg. Clarity is joint project from Yonatan Zunger, Dharmin Shah, Elliot H Omiya, Eve Kazarian, Sarah Cooley, and Neil Coles. We would like to thank Minsoo Thigpen, Abby Palia, Mehrnoosh Sameki, Hilary Solan, Elliot Volkman, Pete Bryan, Roman Lutz, and Shiven Chawla for their helpful comments.
The post Introducing RAMPART and Clarity: Open source tools to bring safety into Agent development workflow appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/05/20/introducing-rampart-and-clarity-open-source-tools-to-bring-safety-into-agent-development-workflow/](https://www.microsoft.com/en-us/security/blog/2026/05/20/introducing-rampart-and-clarity-open-source-tools-to-bring-safety-into-agent-development-workflow/)*
