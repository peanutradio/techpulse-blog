---
categories:
- MS
- Azure
date: '2026-08-26T16:00:00+00:00'
description: This blog post is the second of a four-part series called The Economics
  of Agent Optimization which shares the strategies, capabilities, and proof points
  to hel
draft: false
original_url: https://azure.microsoft.com/en-us/blog/the-economics-of-agent-optimization-four-ways-to-lower-the-cost/
source: Azure Blog
tags:
- AI + machine learning
- The Economics of Agent Optimization
title: 'The Economics of Agent Optimization: Four ways to lower the cost'
---

This blog post is the second of a four-part series called The Economics of Agent Optimization which shares the strategies, capabilities, and proof points to help you optimize agent costs and run AI as a managed investment system on Microsoft Foundry. The first post set out the three decisions that system rests on: optimize each request at runtime, optimize each workflow over time, and govern spend continuously. This post takes the first, the one that touches every dollar you will ever spend on AI.







An agent is a loop around a model. It plans, calls a tool, reads the result, and reasons again, so a single completed outcome can take a dozen model requests. That is why the number the business cares about is the cost of a successful outcome, not the price of a token. 



Every turn in that loop is still one model request, and each request carries decisions about the model, the offer it runs on, what gets reused, and what the model is told. When those decisions are right, the saving repeats on every turn. That is why agent optimization starts here. 




Learn how Microsoft Foundry can help you save time and money




The most expensive habit in production AI



Most AI applications are built the same way. In the prototype, you pick the strongest model available, put everything the model might need into the prompt, and confirm the idea works. That is the correct instinct for a prototype; the problem is what happens next. The prototype&#8217;s defaults quietly become the production architecture, and a pattern designed to answer &#8220;can this work?&#8221; becomes responsible for answering &#8220;can this scale economically?&#8221;



Two things break at that point. First, AI workloads are not uniform. A single application mixes intent classification, extraction, formatting, summarization, and genuine multi-step reasoning—AI workloads vary enormously in complexity. Routing all of them to one frontier model means overpaying on the majority of requests that never needed that capability.



Second, one outcome is many requests. A prototype pays for a single call. An agent pays for the whole loop, so anything wasteful gets multiplied. That is true of tokens, and it is more true of mistakes. An agent that takes a wrong turn calls the wrong tool and loops to recover, burning tokens on turns that should never have happened and still landing on a weaker answer. Cost per outcome is set as much by the turns you avoid as by the tokens in each one.



In production, the goal is not to minimize tokens. It&#8217;s to reduce the cost of a successful outcome while maintaining quality, safety, and latency. Every runtime decision must balance those factors together, which is why the economics of a request come down to four decisions.



Four levers you control at runtime



Microsoft Foundry gives you four levers for making those tradeoffs deliberately, rather than accepting the ones your prototype happened to choose. Each can be adopted on its own, measured against your quality bar, and reversed if the tradeoff does not hold.  



LeverFoundry capabilityModels and offersModel router, deployment types, provisioned throughput, batch, fine-tuning.CachingPrompt caching, semantic caching through the AI Gateway in Azure API Management.Prompt and agent optimizationPrompt optimizer, agent optimizer across instructions, skills, tool descriptions, and model selection.Observability and evaluationFoundry observability and evaluation, agent traces, Azure budgets, alerts, and cost tagging.



1. Send each request to the right model



The principle is simple: optimize the outcome based on the task complexity. Routine requests should not pay frontier-model economics, while complex requests should not sacrifice quality simply to save tokens.



Model router in Foundry Models removes that tradeoff. It assesses each incoming request and dispatches it to the most suitable underlying model in real time, behind a single endpoint and a single deployment. Routing modes let you prioritize cost, quality, or a balance of the two. Model subsets, which now align with Azure Policy, constrain routing to an approved allow-list where a compliance boundary applies. Built-in failover moves a request to the next best model when one is unavailable, so routing also buys resilience.




	
		
		
	
	



The same request can carry very different economics depending on how it is deployed, and this is the lever teams most often leave untouched. Organizations must decide:  




Where data is processed (Global, Data Zone, or Regional).



How throughput is purchased (pay-per-token, provisioned capacity).



Which workloads truly require interactive responses.  




Foundry provides multiple deployment options that allow these choices to align with business requirements. Most workloads can start with standard deployments, which provide the greatest flexibility and cost-efficient pay-as-you-go pricing. Interactive applications that require faster and more consistent response times can benefit from priority processing, while high-volume workloads with predictable demand can achieve better economics through Provisioned Throughput Units (PTUs), with overflow traffic handled through pay-as-you-go capacity. Large asynchronous workloads such as document processing, classification, and evaluation runs are often best suited for Batch deployments, which provide up to 50% lower costs for work that doesn&#8217;t require immediate responses.



Even within a single application, different experiences often benefit from different deployment strategies. Developer-facing tools that can tolerate some latency variability may run efficiently on Standard deployments. Interactive chat experiences may warrant priority processing, while agentic applications with sustained throughput demands can maximize value with PTUs. Background tasks such as document analysis, knowledge extraction, and large-scale classification can move to Batch without affecting the end-user experience, reducing cost simply by selecting the deployment model that matches the workload. 



Fine-tuning is the advanced version of this lever. Where routing picks among existing models, fine-tuning changes what a smaller model can do, teaching it your task, tone, or format well enough to match a larger model on that job. The payoff is a lower rate and shorter prompts. Reach for it when behavior is stable and volume is high enough to earn back the effort. 



2. Stop paying for the same tokens twice



Agents are highly cache effective.  The same system instructions, tool schemas, and policy text are re-sent on every turn, so an agent that takes 10 turns pays for that prefix 10 times. Prompt caching lets a previously processed prefix be reused rather than reprocessed. Cache reads are billed at a discount to normal input pricing on standard deployments, and can be discounted up to 100% on provisioned deployments. Latency improves alongside cost.



Getting value from it is mostly a matter of prompt architecture, and the rule is straightforward: stable content first, volatile content last. Put system instructions, tool definitions, and few-shot examples at the top, and user input, retrieved chunks, and turn history at the bottom. Caching depends on an exact match at the start of the prompt, so anything that changes per request, such as a timestamp or a user&#8217;s name, has to sit below that block. Put it at the top and the cache never matches. 



Caching works above the prompt too. When you deploy a gateway in front of the Foundry inference APIs, it’s important to choose a semantic-cache-aware gateway such as the AI Gateway in Azure API Management. It can maintain session affinity to the same endpoints, helping maximize cache effectiveness while matching near-duplicate requests across sessions and users. Deterministic tool results can be cached in your own store with a time-to-live tuned to how often the data changes.



3. Optimize the prompt, then optimize the agent



If model choice sets the rate, the instruction sets the volume. It is also the cheapest thing to fix, because it ships without touching infrastructure. The practices that cut tokens are the same ones that improve answers: lead with the task rather than burying it after a wall of context, be specific about the output you want and how long it should be, and use a few well-chosen examples in place of paragraphs of explanation. Then keep what accumulates across turns under control: 




Summarize completed conversations instead of replaying full transcripts.



Scope tool definitions to only the tools relevant to the task.



Store working state in external memory and retrieve it only when needed. 




Foundry now automates the hand-tuning this used to take. Prompt optimizer rewrites an agent&#8217;s system instructions using prompt-engineering best practices and shows its reasoning for each change, so you can steer it, run it again, and apply the result in a click. 



Agent optimizer in Foundry Agent Service goes further and closes the loop. It runs your agent against a dataset of real tasks, generates candidate configurations, scores each one, and ranks them so you can promote the winner. It can change instructions, skills, tool descriptions, and model selection, and the dataset can come from your own agent traces. 



4. Make it visible with observability and evaluation



You cannot tune what you cannot see, and you cannot claim a saving you did not measure. Observability in Foundry supplies the per-request signals that make the other three levers safe to pull: input and output tokens, cache hit rate, latency, the model that actually served the request, and the evaluation scores that say whether quality held. 



Two numbers matter here. Cost per request tells you whether the cheaper path still cleared the bar. Cost per completed outcome tells you what the business actually paid, across every turn and retry it took to get there. An optimization that lowers the first while raising the number of turns has made things worse, and only the second will show it. 



Evaluation turns that visibility into permission to change things. Measure cost, latency, and task success together, and keep a standing evaluation set that every optimization has to clear before it ships. Those same traces and evaluation sets are what agent optimizer consumes, so the work pays twice. Pair them with budgets, alerts, and cost tagging in Azure so a regression arrives as a notification rather than a surprise at month end.



How this adds up to a hill climb



None of these levers is a one-time saving. Together they form a loop that gets cheaper and better every time you go around it, which is what Microsoft AI means by building a hill-climbing machine: improving continuously, cycle after cycle, through better data and sharper evaluation.  




Model and offer decides where each request runs, and fine-tuning turns a proven task into a permanently cheaper one. 



Caching lowers the cost of every cycle, which is what lets you run the loop often enough to matter.



Prompt and agent optimization generates the next candidate and proves it against your evaluation set.



Observability and evaluation tells you where you are and whether the last change held.




The climb is a cycle with no fixed start, though most teams enter it at measurement. Traces become evaluation datasets. Those datasets drive the optimizer. Optimizer results show which tasks are stable enough to fine-tune. Fine-tuned models change what the router should choose, and the new routing produces fresh traces. 



Get started




Read the first post in this series, on moving from AI pilots to measurable ROI: The Economics of Agent Optimization: from pilots to measurable returns .



Deploy model router and compare it against your current baseline.



Watch the token economics episode on Microsoft Mechanics for a hands-on look at these levers in action.





	

	
		
			
				

Microsoft Foundry



Learn more about runtime optimization




Start building with Foundry


			
		
					
				
																				
			
			





Did you miss these posts in The Economics of Agent Optimization series?




AI cost management: From AI pilots to measurable ROI

The post The Economics of Agent Optimization: Four ways to lower the cost appeared first on Microsoft Azure Blog.

---
*원문: [https://azure.microsoft.com/en-us/blog/the-economics-of-agent-optimization-four-ways-to-lower-the-cost/](https://azure.microsoft.com/en-us/blog/the-economics-of-agent-optimization-four-ways-to-lower-the-cost/)*
