---
categories:
- MS
- Azure
date: '2026-06-02T18:15:00+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tHow\
  \ Microsoft Discovery supports R&amp;D workflows at scaleExpanding access with the\
  \ Microsoft Discovery app previewAppl"
draft: false
original_url: https://azure.microsoft.com/en-us/blog/announcing-microsoft-discovery-general-availability-and-microsoft-discovery-app-preview/
source: Azure Blog
tags:
- AI + machine learning
- Internet of things
- AI
- Copilot
- Generative AI
title: Announcing Microsoft Discovery general availability and Microsoft Discovery
  app preview
---

In this article
		

		
			
		
	
	
		
			How Microsoft Discovery supports R&amp;D workflows at scaleExpanding access with the Microsoft Discovery app previewApplying Microsoft Discovery across R&amp;D		
	
	




Breakthroughs in science and engineering rarely come from a single insight. They emerge through cycles of hypothesis, experimentation, refinement, and review across teams, tools, and data.



Today at Microsoft Build, we are announcing that Microsoft Discovery is now generally available for all organizations, providing a comprehensive platform for building and governing agentic AI workflows across scientific and engineering disciplines. We are also introducing the Microsoft Discovery app in preview, a local desktop experience that helps researchers, students, and scientific teams begin working with Microsoft Discovery today.




Explore Microsoft Discovery features




Since introducing Microsoft Discovery in private preview at Microsoft Build last year, we have worked closely with organizations applying AI to complex research and development (R&amp;D) workflows. Their feedback helped reinforce where agentic AI needs to go beyond individual assistance, like supporting the iterative loops, evidence preservation, and tool coordination that define scientific work.



The most challenging problems in R&amp;D require more than just a prompt interface or a single model response. Scientific workflows require:




Integration with institutional knowledge and domain expertise.



Access to specialized modeling, simulation, and analysis tools.



Connection to experimental evidence and validation data.



Support for review processes that shape research decisions.




A materials scientist may need to evaluate performance, safety, and cost alongside manufacturability and regulatory constraints. A semiconductor team may need to explore a larger design space without losing physical fidelity or traceability. A life sciences researcher may need to connect literature and experimental data with models and cohort-level evidence before deciding what to validate next.



Microsoft Discovery is designed to work within these existing R&amp;D environments, not replace them. The platform helps experts understand the reasoning path behind outputs and keeps human judgment at the center of scientific and engineering decisions. The general availability of Microsoft Discovery marks a significant milestone in turning these requirements into a production-ready platform for R&amp;D environments with governance and transparency built in.



			
				
			
		Figure 1: The Microsoft Discovery workspace welcome experience.



How Microsoft Discovery supports R&amp;D workflows at scale



Microsoft Discovery enables organizations to define agentic workflows around their own R&amp;D programs. Teams can create and coordinate specialized agents, connect those agents to institutional knowledge and external scientific information, and orchestrate work across modeling, simulation, analysis, and validation tools.



At the center of the platform is the Microsoft Discovery Engine, which supports the core loop of scientific work by helping teams move from evidence to hypotheses, through execution and analysis, and into the next iteration. This loop allows teams to move beyond isolated analysis toward repeatable, evidence-driven exploration, where they can compare tradeoffs, question assumptions, and narrow a search space in a way that can be reviewed and repeated.



			
				
			
		Figure 2: The Microsoft Discovery Engine task creation and status overview.



As we continued product development, we focused on what it takes to bring agentic AI into production R&amp;D environments:




Workflows need to remain reproducible.



Outputs must be reviewable.



Proprietary knowledge must be connected and governed appropriately.



Agentic systems need to fit into the operating model of R&amp;D organizations.




Those considerations, along with continued customer feedback, helped shape the general availability release and the platform capabilities behind it.



			
				
			
		Figure 3: The Microsoft Discovery Engine output with confidence scoring and cited research findings.



Expanding access with the Microsoft Discovery app preview



An important goal for Microsoft is to make advanced AI and computing capabilities more accessible to the people working on some of today’s most difficult scientific and engineering challenges. Alongside the general availability of Microsoft Discovery, we are introducing the Microsoft Discovery app available in preview today.



The Microsoft Discovery app is a localized experience that gives researchers, students, academic labs, and scientific teams a simpler way to begin using Microsoft Discovery capabilities without starting with a full enterprise deployment. It is available for download on the Microsoft Discovery GitHub and users can get started with a GitHub Copilot account.



This preview extends Microsoft Discovery to earlier stages of exploration, where research ideas begin as small-team projects, academic work, or individual investigation. The Microsoft Discovery app is designed to lower the barrier to hands-on exploration, with a practical entry point for literature exploration, hypothesis generation, scientific reasoning, and iterative experimentation.



The app lets researchers explore Microsoft Discovery capabilities using their own working environment. As projects mature and complexity increases, researchers and teams can bring work developed locally into Microsoft Discovery platform to support more advanced R&amp;D programs.




			
				
			
		Figure 4: The Microsoft Discovery app welcome screen.



Applying Microsoft Discovery across R&amp;D



During preview, organizations helped shape the path to general availability by sharing feedback on how they were using Microsoft Discovery to explore advanced R&amp;D workflows grounded in domain-specific data, established research methods, and expert review.



Partners are contributing domain expertise and solution depth that can help organizations adapt Microsoft Discovery to the tools, data, and processes already central to their R&amp;D work. Together, this work offers an early view into how Microsoft Discovery is being used across domains and how a growing ecosystem can help make complex R&amp;D workflows more systematic, transparent, and repeatable.



Yale Engineering



A collaboration across Professor David Kwabi’s group at Yale Engineering and researchers from Microsoft used the Discovery Engine to advance the frontier of agentic small molecule design for grid-scale aqueous organic redox flow batteries (ORFBs).



ORFBs are promising, leading candidates for sustainable, environmentally friendly, long-duration energy storage, but challenging to optimize. Electrolytes must balance complex molecular properties like redox potential, aqueous solubility, synthetic tractability, and electrochemical reversibility. The Discovery Engine, building on our cognitive loop via in-situ optimization research, enables long-horizon scientific reasoning while ensuring trust in the entire process.



With these capabilities, the team used the agentic loop to drive in-silico exploration and convergence of candidates, interpret experimental results, and propose diagnostic experiments. Experts at Yale Engineering led all experimental characterization, verified results interpretation, and evaluated the practical applicability of the designs. The research is available here.




This work introduces a powerful new framework for advancing battery science with AI. By endowing an agent with the ability to reason from and adapt to experiments, we combine the strengths of human-led experimentation with AI&#8217;s capacity to explore vast chemical design spaces &#8211; and we&#8217;re only beginning to see what it can do.
—David Kwabi, Associate Professor, Yale




Read more about the Yale and Microsoft collaboration




Georgia Institute of Technology



Georgia Tech is exploring how an agentic AI system can re-evaluate the prebiotic plausibility of histidine, a biochemically important amino acid whose emergence under plausible prebiotic conditions remains unclear despite its ubiquity in biology. Classical machine learning and AI approaches have struggled in this domain due to the lack of standardized datasets and the inherently multimodal nature of the data.



The proposed scenario requires a multi-agent AI system composed of specialized AI ‘scientists’ for distinct data modalities, including mass spectrometry analysis, literature extraction, planetary mission data retrieval, and chemical reaction pathway modeling.



These agents will collaborate through a central reasoning coordinator to integrate diverse and heterogeneous datasets, aiming to move from “absence-of-evidence” to a robust, evidence-based assessment of histidine’s prebiotic viability. The framework developed can also be repurposed to investigate other contested biosignatures, building a scalable pipeline for origins-of-life inquiry.




Our collaboration with the Microsoft Discovery team through the Georgia Tech AI for Research program has been highly valuable, both scientifically and operationally. Working together on agentic AI systems to probe questions about the origins of life has given us early exposure to the state of the art embodied in the Discovery platform, while also enabling genuinely close technical collaboration. This hands-on partnership has enabled meaningful bidirectional learning.
—Dr. Amirali Aghazadeh, Assistant Professor, School of Electrical and Computer Engineering, Georgia Tech



Pacific Northwest National Laboratory



Microsoft and Pacific Northwest National Laboratory (PNNL) are rewriting the rules of scientific discovery, unleashing AI that doesn&#8217;t just assist researchers but orchestrates the entire discovery journey from new hypotheses to real-world experiments.



Powered by Microsoft Discovery, cutting-edge robotics and AI agents work like a virtual research team: imagining experiments, reasoning across mountains of scientific data, designing brand-new molecules, and learning on the fly from live laboratory results at PNNL.



In energy storage, this collaboration is fast-tracking the hunt for next-generation organic redox flow battery materials—breakthroughs that could slash our reliance on critical minerals like vanadium while providing cheaper, more scalable energy storage technologies that make our power grid tougher than ever.



In biosystems engineering, Microsoft Discovery is plugging directly into PNNL&#8217;s laboratory automation infrastructure to launch self-driving scientific workflows that autonomously design, run, and fine-tune biological experiments in real time.




Together, Microsoft and PNNL are pioneering a new model for science, where robotics and autonomous laboratories fuse with AI and cloud infrastructure into one intelligent, closed-loop discovery engine that dramatically reduces the timeline from ideas to breakthroughs and opens the door to a new era of innovation in energy, biology, and material synthesis.
—Robert Runkle, Physicist and Lead for Autonomous Discovery Strategy, Pacific Northwest National Laboratory



Ginkgo Bioworks



Ginkgo Bioworks and Microsoft are collaborating to bring agentic AI into biological discovery. Specialized agents can analyze biological datasets, generate hypotheses, and design experiments to execute on an autonomous lab. Soon, researchers will be able to scope and plan experiments in Microsoft Discovery and run them directly on Ginkgo Cloud Lab—no in-house automation required.




Together, agentic AI and autonomous labs will change every part of the scientific process. Iteration cycles will get faster, experiments will require less manual hands-on time, and computational analyses will become more systematic and exhaustive. By making both easier to use, Microsoft and Ginkgo aim to bring greater speed, scale and reproducibility to pre-clinical research.
—Jason Kelly, CEO, Ginkgo Bioworks, Inc.



Causaly



Causaly provides agentic solutions that compound the world&#8217;s biomedical evidence with an organization&#8217;s proprietary knowledge to deliver confident, traceable, cited decisions at every stage, from discovery through launch.




Drug discovery does not suffer from a lack of data. It suffers from a lack of trustworthy interpretation. Microsoft Discovery brings scientific computation over enterprise data, and Causaly brings the prior knowledge, mechanistic reasoning, and provenance needed to turn those signals into decisions. Together, we can help researchers move from raw data to evidence-backed judgment much faster and with greater confidence.
—Yiannis Kiachopoulos, Co-Founder and CEO, Causaly



Cambridge Consultants



With Microsoft Discovery, Cambridge Consultants is helping demonstrate how AI agents, simulation, and physical lab systems can work together in a closed-loop discovery process.



These autonomous, AI-powered cycles can turn months of experimental work into days or hours. The result is a more connected model for R&amp;D, one designed to accelerate candidate generation, experimental planning, and real-world validation.




Microsoft Discovery has the potential to help researchers move faster from promising ideas to real-world results. We see this as an important step toward more scalable, integrated, and intelligent R&amp;D.
—Joe Corrigan, Chief Technology Officer, Cambridge Consultants



Wiley



At every stage of the research and development process, life sciences and pharmaceutical teams need fast access to the most current, credible evidence available. Wiley Research Agent: Life Sciences delivers a continuously updated index of more than one million authoritative, high-quality, and trusted articles with hybrid search capabilities to support advanced scientific reasoning.



The agent searches, retrieves, and synthesizes relevant findings into a coherent, evidence-based response to queries. It can operate as a stand-alone research service, or in orchestration with other Microsoft Discovery agents, fitting naturally into the broader scientific reasoning workflows that Discovery enables. The Wiley Life Sciences Research Agent will be the first of several Wiley agents offered commercially on the Microsoft Discovery platform over time.




Scientific discovery depends on connecting trusted evidence with increasingly powerful AI systems. By bringing Wiley’s authoritative life sciences research into Microsoft Discovery, we can help life sciences and pharmaceutical teams accelerate hypothesis generation, experimentation, and results interpretation across a continuous scientific reasoning loop.
—Josh Jarrett, Senior Vice President and General Manager of Applied Research Intelligence at Wiley



BHP



BHP, the largest mining company in the world, is using Microsoft Discovery to accelerate discovery of advanced copper leaching solutions—in a matter of months instead of years.




As copper demand grows and new deposits become harder to find and more expensive to develop, improving recovery from existing ores is a critical lever to help meet future supply needs. This partnership has given our technical experts the tools they need to narrow an almost infinite field of possibilities down to a small number of options that could one day be deployed in our global copper operations. We are testing against the realities of our ore bodies and operating constraints, so we are solving for what can actually work in practice. This shows how technology and human expertise can be applied together to solve complex, real-world challenges.
—Jessica Farrell, Vice President Innovation, BHP



Syensqo



Syensqo is a global science company developing groundbreaking solutions that enhance the way we live, work, travel, and play. The company is currently leveraging Microsoft Discovery to scale agentic AI that accelerates discovery, improves decision-making, and unlocks measurable business impact, particularly in the development of next-generation heat transfer fluids for semiconductor manufacturing.




We are now entering a new phase of our partnership with Microsoft, focused on scaling AI agents across research, sales and marketing to drive near-term growth. By connecting customer demand to scientific development and back-to-market execution, agentic AI is enabling faster cycles, sharper prioritization, and tangible impact on revenue growth and business performance.
—Mike Radossich, CEO of Syensqo



GSK



GSK, the global biopharma, is working to accelerate the discovery, development, and delivery of medicines and vaccines to patients.




Working with partners like Microsoft Discovery, we see the opportunity to rapidly iterate on candidate molecules, potentially accelerating decision-making via rapid data generation and analysis.
—Christopher Austin, Senior Vice President, R&amp;D Technologies, GSK




			
				
			
		Figure 5: Microsoft Discovery has an expanding ecosystem of partners offering integrated tools and specialized expertise.




	
					
							
		
		
			Get started today with Microsoft Discovery
			Microsoft Discovery is now generally available for organizations ready to bring agentic AI into R&amp;D workflows.
							
					
						Review documentation					
				
					
	




Microsoft Discovery is generally available. The Microsoft Discovery app is available in preview. Preview features and capabilities are subject to change.
The post Announcing Microsoft Discovery general availability and Microsoft Discovery app preview appeared first on Microsoft Azure Blog.

---
*원문: [https://azure.microsoft.com/en-us/blog/announcing-microsoft-discovery-general-availability-and-microsoft-discovery-app-preview/](https://azure.microsoft.com/en-us/blog/announcing-microsoft-discovery-general-availability-and-microsoft-discovery-app-preview/)*
