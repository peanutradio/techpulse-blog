---
categories:
- MS
- Azure
date: '2026-07-07T16:00:00+00:00'
description: 'Azure Key Vault Managed Hardware Security Module (HSM) provides strong
  sovereignty over your encryption keys. Keys are generated and stored in a single-tenant, '
draft: false
original_url: https://azure.microsoft.com/en-us/blog/external-key-management-for-azure-managed-hsm-is-now-in-public-preview/
source: Azure Blog
tags:
- Management and governance
- Azure
- Azure Key Vault API
- Azure Key Vault Managed HSM
- Azure RBAC
- Managed HSM
title: External key management for Azure Managed HSM is now in public preview
---

Azure Key Vault Managed Hardware Security Module (HSM) provides strong sovereignty over your encryption keys. Keys are generated and stored in a single-tenant, FIPS 140-3 Level 3 HSM that only you control: Microsoft has no access to your key material, and you govern who can use each key. For most organizations, including those with stringent regulatory requirements, this level of control is sufficient.




  Some organizations have a further requirement: the hardware that holds their key must reside physically outside Azure datacenters. External key management for Azure Key Vault Managed HSM is now in public preview to address that requirement, delivering on a commitment made a year ago.





Try External key management for Azure Key Vault Managed HSM




How Managed HSM delivers sovereignty today




  Before looking at external key management, it’s worth being precise about the sovereignty Managed HSM already provides. Managed HSM is a single-tenant service: each instance is a dedicated cluster of FIPS 140-3 Level 3 validated HSM partitions for each customer—built on Marvell LiquidSecurity adapters. Keys are generated inside that hardware and never leave it in plaintext, making the keys inaccessible to Microsoft operators.




Control rests with you, not Microsoft:




Customer-specific security domain. Each HSM cluster is cryptographically isolated by a security domain that you generate and own. Microsoft can’t decrypt your key material or recover your HSM cluster without it. You are in full control of the security domain as it’s protection and safeguarding is outside of Microsoft.



Multiperson control. The security domain is protected by a quorum of RSA key pairs that you hold offline. Recovery requires your quorum, so no single person—and no Microsoft operator—can act alone.



Local role-based access control (RBAC). A data-plane authorization model, independent of Azure RBAC, governs who can perform each cryptographic operation.



Key attestation. You can obtain cryptographic proof that a key was generated and is used within the FIPS 140-3 Level 3 hardware boundary.





  Managed HSM is built on FIPS 140-3 Level 3 HSMs and confidential computing technology based on Intel SGX, so request handling, access control, and key material are isolated in hardware enclaves and HSMs that no Microsoft operator—even one with administrative or physical access to the host—can read. Managed HSM provides redundancy, isolation, and protection—giving organizations the sovereignty assurances they need without compromising on key security, operational overhead or availability. 




What external key management adds




  Managed HSM already provides full customer control over your keys, with enterprise-grade availability, security, and operational simplicity. External key management adds one capability: the option to keep your key material on an HSM that you own and operate, either on-premises or with a trusted third party, completely outside Microsoft infrastructure.





  External key management is designed for scenarios where regulation or contractual obligations mandate the cryptographic keys must reside outside the cloud provider’s environment. These requirements are sometimes found in highly regulated sectors such as government, financial services, and critical infrastructure, and in jurisdictions with strict data-sovereignty rules. External key management ensures the root of trust and key material remain on hardware you own and operate, outside Microsoft infrastructure, and under your direct physical control. 





  However, this model should only be adopted deliberately and only when required. For most workloads, Managed HSM keys remain the recommended approach, delivering higher native availability, reduced operational complexity, and a security posture that meets or exceeds sovereignty requirements without introducing additional risk or overhead. External key management is about meeting specific regulatory constraints, not increasing baseline security. When those constraints do not apply, Managed HSM provides a stronger, more reliable, and more operationally efficient solution.




How it works




  External key management extends Managed HSM through a dedicated API endpoint that connects directly to the HSM you control. It allows cryptographic operations in Azure to invoke external key material without changing how applications interact with the service. The external key never resides in or passes through Microsoft infrastructure; only your hardware uses it. Because you control that hardware, you can disconnect it at any time to halt all cryptographic operations.





Integration is transparent to applications. Applications continue to use Managed HSM and the Azure Key Vault API with the customer-managed key envelope encryption pattern unchanged. When an data access requires your external key to decrypt local data encryption keys, Managed HSM forwards it to your hardware and returns the result.



You choose the hardware and partner. Because the external key management API is an open specification, you decide how to implement it. Your hardware, your partner, or your implementation.



All connections are mutually authenticated and encrypted. Traffic between Azure and your hardware is secured with mutual TLS, ensuring a secure and trusted connection between Azure and your HSM.




HSM ecosystem




  A growing ecosystem of HSM vendors support integration with the Managed HSM external key management API, as many providers are actively enabling compatibility for their platforms.




Microsoft doesn’t build or operate the connecting integration proxy itself. Instead, you benefit from an open model: you can use a vendor provided implementation, reply on a partner to operate it, or build your own.



Responsibilities and tradeoffs




  External key management deliberately shifts a portion of operational responsibility to you. This is the direct consequence of extending the trust boundary beyond Azure: you gain control over the root of trust, and with it, ownership of the systems that enforce it.





Availability of your hardware. The Managed HSM SLA applies up to the point Managed HSM calls your external HSM proxy. Availability of your proxy and HSM is your responsibility. Any disruption on your side directly impacts cryptographic operations and Azure service data accessibility.



Scope of operations. External key management focuses on the operations used to protect data at rest. It does not expose the full set of key operations available with Managed HSM keys, reflecting a deliberate trade-off between control and functionality.



Hardware operations. Provisioning, securing, scaling, monitoring, and recovery of your proxy and HSM become your responsibility, whether operated directly or through a partner.



Error transparency. Failures originating on your side of the connection are surfaced in Managed HSM logs, but remain your responsibility to diagnose and resolve.





  This is the core trade-off: more control means more responsibility.




Public preview scope




Availability: all Azure public regions at preview launch.



Access: gated. Your Microsoft account team enables external key management on your Managed HSM—contact them to request it.



Use case: protecting data at rest for Azure services that support customer-managed keys with Managed HSM.



Pricing: standard Managed HSM pricing, with no additional Microsoft surcharge. You cover the cost of your own hardware and any partner licensing.




Get started




Start with: What is Managed HSM external key management?



Review the SLA and shared-responsibility model



Try the Azure CLI quickstart





  External key management is the latest step in giving customers granular control over how and where their keys are protected. During public preview, your feedback will directly shape the feature on its path to general availability — including the operational guidance, vendor integrations, and scenarios we prioritize next.





	

	
		
			
				

Take control of your encryption keys



Explore external key management for Managed HSM and keep your key material outside Azure while maintaining secure, scalable operations.




Get started


			
		
					
				
																				
			
			


The post External key management for Azure Managed HSM is now in public preview appeared first on Microsoft Azure Blog.

---
*원문: [https://azure.microsoft.com/en-us/blog/external-key-management-for-azure-managed-hsm-is-now-in-public-preview/](https://azure.microsoft.com/en-us/blog/external-key-management-for-azure-managed-hsm-is-now-in-public-preview/)*
