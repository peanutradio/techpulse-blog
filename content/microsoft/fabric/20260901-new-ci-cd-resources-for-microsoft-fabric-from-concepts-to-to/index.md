---
categories:
- MS
- Fabric
date: '2026-09-01T17:00:00+00:00'
description: Introducing a new set of resources that make it easier to build continuous
  integration and continuous deployment (CI/CD) into your Microsoft Fabric data project
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/New-CI-CD-resources-for-Microsoft-Fabric-from-concepts-to-end-to/ba-p/5358502
source: Microsoft Fabric Blog
tags: []
title: 'New CI/CD resources for Microsoft Fabric: from concepts to end-to-end automation'
---

Introducing a new set of resources that make it easier to build continuous integration and continuous deployment (CI/CD) into your Microsoft Fabric data projects. A Fabric solution is rarely a single artifact. It is a composition of workspace items, and with more than 70 item types available today (and the number continuing to grow), moving a solution from development to production takes a deliberate plan. These new resources connect the platform capabilities into one coherent workflow and form a path you can follow end to end: understand the platform, plan your approach, get hands-on with guided tutorials, and reach for advanced options when your scenario calls for them.
&nbsp;
Figure: The Fabric CI/CD platform, layered on the Fabric REST APIs.
Get started: Introduction to CI/CD in Microsoft Fabric
The new Introduction to CI/CD in Microsoft Fabric is the main landing page for these resources. It explains the platform layer by layer, from the Fabric REST API foundation through Git integration, deployment pipelines, the Variable library, the Fabric CLI, infrastructure as code with Terraform, and the fabric-cicd library. It also includes an enterprise reference architecture that shows how source control, CI automation, capacities, and workspaces fit together. If you are new to Fabric CI/CD, or want to understand how the pieces relate, start here.
Figure: An enterprise reference architecture for Fabric CI/CD across Dev, Test, and Prod.
Plan with confidence: Concepts and best practices guide
The Fabric CI/CD concepts and best practices guide moves from foundational concepts to platform capabilities, automation tooling, and a practical checklist you can apply to your projects. It explains what constitutes a Fabric solution, how a CI/CD project moves that solution across isolated dev, test, and prod environments, and which design decisions make promotion safe and repeatable. Use the best practices checklist when planning your next project.
Get hands-on: New step-by-step tutorials
For teams that learn by doing, new guided tutorials cover the full journey, from an automated end-to-end pipeline to focused walkthroughs of each approach:
Automate an end-to-end CI/CD workflow
A full walkthrough that uses Terraform to provision the dev, test, and prod workspaces and the fabric-cicd library to deploy content between them, tying the pieces into one automated flow from source control to production.
To learn more, refer to the Automate an end-to-end CI/CD workflow tutorial.
Deploy locally with the fabric-cicd library or the Fabric CLI
Set up a basic development environment and deploy a lakehouse and notebook from your local workstation to a Fabric workspace by using either the fabric-cicd Python SDK or the fab deploy command. This is the fastest way to see the deployment process in action.
To learn more, refer to the Deploy locally with the fabric-cicd library or the Fabric CLI tutorial.
Build a CI/CD pipeline with Azure DevOps and the fabric-cicd library
Promote changed items from dev to test to prod with approval gates, automatic replacement of environment-specific GUIDs through parameter files, and secure credentials in Azure Key Vault and a service principal.
To learn more, refer to the Build a CI/CD pipeline with Azure DevOps and the fabric-cicd library&nbsp;tutorial.
Deploy from Git with the Bulk Import and Export API
A build-environment pattern in which an Azure DevOps pipeline reads Fabric item definitions from a Git folder and deploys them to a target workspace that is not connected to Git. The Bulk Import API creates new items and updates existing ones in place, relying on Fabric's built-in dependency handling to deploy items in the correct order. This approach is a strong fit when you want to treat item definitions as code and promote them through a structured release flow.
To learn more, refer to the Deploy from Git with the Bulk Import and Export API&nbsp;tutorial.
Go deeper: Dependency binding across workspaces
When you promote a solution, the references between items can break. The new Understand dependency binding in cross-workspace deployment article explains why: some items reference their dependencies with portable logical IDs that bind automatically to the matching item in the target workspace, while others use workspace-specific object IDs that break unless you parameterize them. It maps which item types bind automatically and which need manual handling. For now, this guidance covers deployment through Git integration and the Bulk Import API.
Bring your own Git provider: open-source generic Git sample
Some teams promote content through a Git provider that Fabric integration does not natively support or run their environments in different Microsoft Entra tenants.
The open-source fabric-cicd-generic-git sample addresses exactly these cases. It uses the Bulk Export and Import APIs, with portable Bash scripts that work the same across providers such as GitLab, Bitbucket, GitHub Enterprise Server, and Azure DevOps Server, and includes first-class support for service principals and multi-tenant deployments. If your setup falls outside native Git integration, start with this sample.
Next steps
Start with the Introduction to CI/CD in Microsoft Fabric to build a mental model. Use the concepts and best practices guide to plan your approach, then choose the tutorial that best matches your environment and build your first CI/CD process. Share your feedback through the Fabric Community.

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/New-CI-CD-resources-for-Microsoft-Fabric-from-concepts-to-end-to/ba-p/5358502](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/New-CI-CD-resources-for-Microsoft-Fabric-from-concepts-to-end-to/ba-p/5358502)*
