---
categories:
- MS
- Fabric
date: '2026-09-02T13:00:00+00:00'
description: Organizations can now bring governed business data from Microsoft Fabric
  into Copilot Studio agents, enabling agents to answer questions, generate insights,
  and
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Data-Agents-in-Microsoft-Copilot-Studio-Generally/ba-p/5362882
source: Microsoft Fabric Blog
tags: []
title: Fabric Data Agents in Microsoft Copilot Studio (Generally Available)
---

Organizations can now bring governed business data from Microsoft Fabric into Copilot Studio agents, enabling agents to answer questions, generate insights, and support business processes using trusted enterprise data. Fabric data agents make it easier to ground experiences in the data your organization already relies on.
Since we announced this integration in preview, both Fabric and Copilot Studio have evolved significantly. General availability provides a more integrated experience, broader publishing options, and a stronger foundation for deploying data-powered agents across the Microsoft ecosystem.
Fabric Data Agents are now integrated as tools
Previously, you added a Fabric data agent from the Agents category as a connected agent.
Now, Fabric data agents integrate through the new tool-based experience in Copilot Studio. Simply select Add a tool, search for Fabric, and select Fabric IQ Data MCP. Your Copilot Studio agent can then invoke the Fabric data agent the same way it calls any other tool. The orchestrator decides when to reach for enterprise data and when to rely on its other knowledge sources and tools.
The Fabric data agent continues to execute in Fabric, respect permissions on underlying data sources, and return answers grounded in governed enterprise data.
The benefit is a more composable architecture. A Fabric data agent becomes one capability among the connectors, workflows, and other tools available to your Copilot Studio agent. You can combine multiple Fabric data agents with other business systems to create richer, more capable solutions.
Built on the new Copilot Studio experience and runtime
Copilot Studio introduced a redesigned authoring surface and runtime powered by the GitHub Copilot harness. You can build, test, evaluate, publish, and monitor agents from a unified environment while using natural language to describe agent behavior and capabilities.
For Fabric customers, the biggest benefit is improved orchestration. The harness uses an improved reasoning model that is better at deciding which tool to call and how to combine what comes back, which matters when a Fabric data agent sits next to SharePoint knowledge, connectors, and other tools in the same agent.
You choose the harness when you create the agent. Agents built with the standard harness remain fully supported. Instead of building isolated data experiences, organizations can create
agents that reason across multiple sources of enterprise knowledge while using Fabric as their trusted data foundation.
Publish to Microsoft Teams and Microsoft 365 Copilot
Another improvement since preview is broader reach. You can now publish your Copilot Studio agent to Microsoft Teams and Microsoft 365 Copilot, bringing governed, data-grounded experiences directly into the tools people use every day.
For example, a business user can ask about last quarter's regional performance in Microsoft 365 Copilot and get an answer from governed Fabric data without leaving the conversation they are already in.
As always, permissions continue to apply. Users must have access to the Fabric data agent and its underlying data sources.
Figure: Animated GIF - Add Fabric data agent as a tool to your Copilot Studio Agent.
Why this matters
The rise of AI agents is creating a new challenge for organizations: how to ensure agents are grounded in trusted business data rather than disconnected information sources. Fabric and Copilot Studio solve different parts of that challenge.
Fabric provides the intelligence layer. It is where organizations bring together, govern, and publish data expertise through data agents.
Copilot Studio provides the orchestration layer. It is where organizations build agents, workflows, and business processes that put that expertise to work inside a broader business process.
Adding a Fabric data agent means everything you compose there can reason over governed enterprise data.
Now, Fabric data agents and Copilot Studio (Generally Available) makes it easier than ever to bring governed data, business context, and intelligent action together in a single agent experience. Together, they enable organizations to:

Build multi-agent solutions. Fabric data agent supplies the analysis while other agents and tools carry the rest of the process, such as drafting the document, updating the system of record, or routing an approval.
Bring data together with everything else your agent knows. A Copilot Studio agent can pull a number from Fabric and a policy from SharePoint in the same answer.
Meet users where they work. Publish to Teams and Microsoft 365 Copilot so business users get data-grounded answers in the flow of their work.
Keep one definition of your data. Fabric data agent stays the single source of truth for every Copilot Studio agent that consumes it, so answers do not drift between teams. Build it once in Fabric and reuse it across departments.

Getting started
Getting started with Fabric data agent integration with Copilot Studio (Generally Available) is easy:

Build and publish a Fabric data agent with a clear, detailed description.
Create or open an agent in Copilot Studio.
Under Tools, select Add a tool, search for Fabric, and add Fabric IQ Data MCP.
Test your agent in Preview.
Publish to Microsoft Teams or Microsoft 365 Copilot.
To learn more, refer to the&nbsp;Add a Fabric data agent as a tool in Microsoft Copilot Studio documentation for the full setup, and join the community discussion to share feedback and report issues.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Data-Agents-in-Microsoft-Copilot-Studio-Generally/ba-p/5362882](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-Data-Agents-in-Microsoft-Copilot-Studio-Generally/ba-p/5362882)*
