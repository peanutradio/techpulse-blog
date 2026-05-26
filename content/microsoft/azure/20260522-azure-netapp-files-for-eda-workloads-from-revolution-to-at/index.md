---
categories:
- MS
- Azure
date: '2026-05-22T15:00:00+00:00'
description: Last year, we outlined how Azure NetApp Files helped reshape silicon
  design by delivering the low-latency, high-throughput storage required for Electronic
  Desig
draft: false
original_url: https://azure.microsoft.com/en-us/blog/azure-netapp-files-for-eda-workloads-from-revolution-to-breakthrough-at-scale/
source: Azure Blog
tags:
- Storage
title: 'Azure NetApp Files for EDA workloads: From revolution to breakthrough at scale'
---

Last year, we outlined how Azure NetApp Files helped reshape silicon design by delivering the low-latency, high-throughput storage required for Electronic Design Automation (EDA) workloads at cloud scale. Since then, we have continued to extend performance and scalability. Today, we are advancing that progress with another significant step forward.



Modern semiconductor design is defined by scale. Thousands of concurrent EDA jobs spanning simulation, synthesis, and verification run continuously against shared datasets, where even small variations in storage latency can ripple across entire design cycles. For many teams, this has historically limited how far EDA workflows could scale in the cloud. 



That constraint is now changing.



Azure NetApp Files (ANF) is redefining what is possible for EDA in the cloud by delivering predictable, high-performance shared storage at massive concurrency. With new independent benchmark results and growing adoption by leading semiconductor companies, Azure is establishing itself as a viable—and in many cases superior—platform for modern EDA environments. 




See how Azure NetApp Files delivers scalable, high-performance storage for EDA




Why EDA storage has been difficult to scale in the cloud



EDA workloads combine three characteristics that have traditionally challenged cloud storage architectures:




Extremely high concurrency, with thousands of jobs accessing shared file systems simultaneously.  



Strict latency sensitivity, where even minor delays reduce compute efficiency and extend runtimes.  



Intensive shared data access patterns, creating contention under load.  




While cloud compute scales easily, shared storage has often introduced variability that limits overall system efficiency. As concurrency increases, storage becomes the bottleneck, impacting regression cycles, increasing tool license costs, and slowing time to tape-out.



For EDA teams evaluating cloud transformation, the central question has remained consistent: can storage scale with compute while maintaining predictable performance?



A modern approach: Azure NetApp Files for EDA at scale



Azure NetApp Files is designed specifically to address this challenge. Its architecture aligns directly to the requirements of highly parallel, shared workloads like EDA. 



At its core, ANF enables independent scaling of compute and storage, so EDA clusters can grow without storage becoming the constraint, and additional compute nodes do not introduce hotspots or contention at the storage layer. It natively supports concurrent metadata operations at scale, handling the millions of small file interactions typical of EDA workflows without degradation. And its service-level performance model ensures that throughput and IOPS scale predictably with capacity, eliminating the need for complex tuning.



More recently, innovations such as large volumes and large volumes breakthrough mode have expanded the concurrency envelope even further. These capabilities allow thousands of parallel jobs to share a single storage environment while maintaining consistent latency under sustained load.



This delivers what cloud-based EDA systems have long struggled to provide: consistent, repeatable performance, not only at low utilization, but also under full production load.



Independent validation: SPECstorage® Solution 2020 benchmark results



To validate these capabilities in a real-world context, Azure NetApp Files was measured using the industry-standard SPECstorage® Solution 2020 EDA_BLENDED benchmark. This benchmark simulates realistic EDA workflows by combining metadata-intensive frontend operations with throughput-heavy backend processing, all under strict latency requirements. 



The Azure NetApp Files large volume breakthrough mode scale configuration reached 17,280 SPECstorage® Solution 2020 EDA_BLENDED JOBS with an overall response time of 0.60 milliseconds (ms).



These results demonstrate several important characteristics:




The ability to sustain very high levels of concurrent EDA workloads.  



Consistently low response times under load.  



Linear scaling behavior as concurrency increases.  



No requirement for overprovisioning.





  Historically, top benchmark outcomes in this category have been associated with tightly integrated on-premises systems. This validation underscores a broader shift in the industry: when architected correctly, cloud-based EDA infrastructure can not only match on-premises approaches, but in some scenarios surpass them in both scale and operational efficiency.




Proven in production: EDA workloads already running on ANF



This performance is not limited to benchmarks. Organizations such as AMD and ASML are already using Azure NetApp Files to run EDA and high-performance design workloads in production environments.



These companies operate at the leading edge of semiconductor innovation, where infrastructure must support both extreme scale and precise predictability. Their adoption of ANF reflects a broader industry trend: moving EDA workloads to the cloud is no longer experimental, it is becoming a strategic advantage.



These customers, along with others, consistently report the same operational benefits:




The ability to increase regression concurrency without performance degradation.  



Improved utilization of compute resources and reduced EDA tool license fees.  



Greater predictability in design cycles, enabling more confident scheduling of key milestones.  




In this context, storage is no longer the limiting factor—it becomes an enabler of scale.



How Azure helps EDA teams scale with confidence



Organizations have flexibility in how they deploy EDA environments with Azure NetApp Files, depending on workload characteristics and operational priorities.



Some teams choose a centralized model built around a single large volume to maximize throughput and tightly control latency. Others adopt a multi-volume approach to distribute workloads and scale concurrency across different job types. Many enterprises extend existing on-premises environments into Azure, using cloud capacity to absorb peak demand without permanent infrastructure expansion.



Across all of these patterns, one principle remains consistent: storage performance must scale predictably alongside compute. Azure NetApp Files provides that foundation.




Azure NetApp Files delivers the consistent, high‑throughput NFS performance that modern EDA workloads demand, shrinking runtimes, accelerating tape‑out schedules, and giving chip designers the confidence that storage will never be the bottleneck.
Srikanth Gubbala, Head of Global HPC Infrastructure, Applied Materials



Bringing it all together



The evolution of cloud storage for EDA marks an important inflection point for the semiconductor industry. What was once considered a tradeoff—scale versus predictability—is no longer a constraint.



With Azure NetApp Files, organizations can confidently run highly concurrent EDA workloads in the cloud, supported by architecture designed for their specific demands and validated by independent benchmarking.



For teams exploring how to modernize their EDA infrastructure, the path forward is increasingly clear. Cloud-based storage can now meet the requirements of even the most demanding design environments, while offering the flexibility to scale as workloads continue to grow.



For a deeper technical exploration of the benchmark configuration and design considerations, see the companion Azure Tech Community technical blog: “From scale to breakthrough: Azure NetApp Files sets a new cloud benchmark for EDA.”



For further information, explore the Azure NetApp Files documentation or email askanf@microsoft.com.




	
					
							
		
		
			Scale high-performance EDA workloads with Azure NetApp Files
			Discover how Azure NetApp Files delivers predictable, high-performance storage for EDA workloads, enabling massive concurrency, low latency, and consistent scaling in production.
							
					
						Get started with ANF					
				
					
	

The post Azure NetApp Files for EDA workloads: From revolution to breakthrough at scale appeared first on Microsoft Azure Blog.

---
*원문: [https://azure.microsoft.com/en-us/blog/azure-netapp-files-for-eda-workloads-from-revolution-to-breakthrough-at-scale/](https://azure.microsoft.com/en-us/blog/azure-netapp-files-for-eda-workloads-from-revolution-to-breakthrough-at-scale/)*
