---
categories:
- MS
- Azure
date: '2026-05-22T17:00:00+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tThe\
  \ challenge of multi-cluster networkingOur vision: Multi-cluster management with\
  \ seamless networkingStrategic resilien"
draft: false
original_url: https://azure.microsoft.com/en-us/blog/powering-multi-cluster-workloads-with-seamless-cross-cluster-networking-for-azure-kubernetes-fleet-manager/
source: Azure Blog
tags:
- Compute
- Containers
- Networking
title: Powering multi-cluster workloads with seamless cross‑cluster networking for
  Azure Kubernetes Fleet Manager
---

In this article
		

		
			
		
	
	
		
			The challenge of multi-cluster networkingOur vision: Multi-cluster management with seamless networkingStrategic resilience with cross-cluster networkingGetting started with cross-cluster networkingDocumentation and resources		
	
	




As organizations modernize their application portfolios, we are witnessing a fundamental shift in how cloud-native infrastructure is architected. No longer is the question &#8220;How do we scale a cluster?&#8221; but rather &#8220;How do we scale across clusters, regions, and clouds while maintaining operational simplicity?&#8221;



Today, we are thrilled to announce the public preview of cross-cluster networking for Azure Kubernetes Fleet Manager. This capability represents the next evolution in multi-cluster management by introducing transparent E-W multi-cluster networking powered by Advanced Container Networking Services.




Learn more about cross-cluster networking for Azure Kubernetes Fleet Manager




The challenge of multi-cluster networking



Whether driven by regulatory requirements, regional disaster recovery, or the need to isolate blast domains, organizations of every size often run multiple Azure Kubernetes Service (AKS) clusters. However, managing these clusters has historically introduced a “networking tax.” Traditional approaches rely on complex VPNs, gateways, and manual service discovery, adding latency and operational complexity.



Even when operating just a few clusters and especially when operating large scale fleets of clusters, teams need consistent, reliable cross‑cluster connectivity to support scenarios like failover, shared services architectures, and seamlessly shifting workloads across regions for capacity or latency. At the same time, platform teams want to abstract infrastructure details from developers, enabling seamless cluster-level changes without disrupting applications.



Our vision: Multi-cluster management with seamless networking



In response to similar challenges, we built Azure Kubernetes Fleet Manager. Fleet Manager is designed to simplify multi-cluster Kubernetes for everyone. While Fleet Manager has already simplified workload propagation (deploying to many clusters) and update orchestration (safe, staged upgrades), the network remained a challenge.



With the introduction of Cilium-based cross-cluster networking in Azure Kubernetes Fleet Manager, we are delivering a managed, high-performance network that can span your entire fleet.



This capability extends the Kubernetes networking model across clusters, enabling services and workloads to communicate across cluster boundaries as if they were local, while preserving cluster-level isolation and governance.



Built on an open-source foundation, this capability uses Cilium for dataplane and Kubefleet for fleet-level orchestration, both active Cloud Native Computing Foundation (CNCF) projects. This ensures transparency, portability, and alignment with the broader Kubernetes ecosystem, while benefiting from continuous innovation from the open-source community.



The following diagram shows how clusters in a fleet are connected through a unified, managed network, enabling seamless communication, service discovery, and policy enforcement.



			
				
			
		



Key capabilities include:




Seamless east-west connectivity: Using eBPF-based routing with power of Azure CNI powered by Cilium and Advanced Container Networking Services, pods can communicate across clusters with native performance, no proxies or gateways required.



Global service discovery: With a simple annotation (service.cilium.io/global=true), a standard Kubernetes Service becomes &#8220;global.&#8221; Cross-cluster networking automatically discovers endpoints across joined member clusters, providing transparent load balancing and failover.



Multi‑cluster observability: Gain a unified view of network health across clusters with aggregated metrics, logs, and flow visibility. Advanced Container Networking Services integrates Cilium telemetry to provide consistent insights, faster troubleshooting, and end‑to‑end visibility across the fleet.



Unified security and governance: Security policies are no longer confined by cluster boundaries. Through Advanced Container Networking Services, you can now enforce enterprise-grade network policies and gain deep observability across your entire global footprint, ensuring identity-based security follows your workloads wherever they run.



Zero-touch management: Fleet Manager handles the complex lifecycle, managing certificates, and network configurations, so you don&#8217;t have to.




These capabilities are using eBPF to enable efficient routing, policy enforcement, and observability for high-performance networking



Strategic resilience with cross-cluster networking



In a digital-first economy, resilience is a competitive advantage. Cross-cluster networking enables customers to build architectures that are inherently resilient to single-cluster or single-region failures.



Whether you are running &#8220;Shared Services&#8221; clusters to support hundreds of tenants or building &#8220;Global Services&#8221; that route traffic to the healthiest available endpoints, cross-cluster networking for Azure Kubernetes Fleet Manager ensures your infrastructure is as agile as your business needs.



We are committed to providing the most robust, secure, and performant platform for multi-cluster environments. Cross-cluster networking is a big step towards a future where the physical boundaries of a cluster no longer limit the innovation within it.



Getting started with cross-cluster networking



Cross-cluster networking for Azure Kubernetes Fleet Manager is designed to minimize operational complexity:



Prerequisites for your clusters:




Azure CNI powered by Cilium as the networking dataplane.



Advanced Container Networking Services enabled.




Set up cross-cluster networking:




Join clusters to a Fleet.



Associate the members with a cross-cluster network profile.



Deploy services with global annotations to enable cross-cluster communication.




Once configured, Fleet Manager automatically deploys and manages the required components, enabling direct pod-to-pod communication across clusters without additional gateways or overlays.



This managed approach removes the burden of setting up and maintaining Cilium multi-cluster components manually, allowing teams to focus on application delivery instead of infrastructure management.



See it in action: Watch the Cross-Cluster Networking for Azure Kubernetes Fleet Manager Video Guide to learn more and see a quick demo.



Documentation and resources




Cross-cluster networking for Azure Kubernetes Fleet Manager



How to enable cross-cluster networking for Azure Kubernetes Fleet Manager



Azure CNI powered by Cilium



Advanced Container Networking Services




If you have feedback or would like to learn more, reach out to your Microsoft account team or share feedback through the Azure Kubernetes Service community channels, we would love to hear from you!




Get started with cross-cluster networking

The post Powering multi-cluster workloads with seamless cross‑cluster networking for Azure Kubernetes Fleet Manager appeared first on Microsoft Azure Blog.

---
*원문: [https://azure.microsoft.com/en-us/blog/powering-multi-cluster-workloads-with-seamless-cross-cluster-networking-for-azure-kubernetes-fleet-manager/](https://azure.microsoft.com/en-us/blog/powering-multi-cluster-workloads-with-seamless-cross-cluster-networking-for-azure-kubernetes-fleet-manager/)*
