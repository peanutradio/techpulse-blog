---
categories:
- MS
- 보안
date: '2026-07-13T17:00:00+00:00'
description: As identity attacks grow more sophisticated in the AI era, organizations
  need stronger authentication methods that protect users from phishing, credential
  theft
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/07/13/microsoft-entra-id-security-updates-passkeys-are-the-default-authentication-method-in-entra-id/
source: Microsoft Security Blog
tags: []
title: 'Microsoft Entra ID security updates: Passkeys are the default authentication
  method in Entra ID'
---

As identity attacks grow more sophisticated in the AI era, organizations need stronger authentication methods that protect users from phishing, credential theft, and social engineering. To address these evolving threats, Microsoft Entra ID is updating its authentication experience by making passkeys the default phishing-resistant authentication method, helping customers reduce reliance on phishable methods such as SMS and voice.



Beginning September 1, 2026, Microsoft will begin rolling out passkeys as the default authentication experience in Microsoft Entra ID. As the rollout reaches each organization, users enabled for SMS or voice authentication will automatically be enabled for passkeys, and the next time they perform multifactor authentication, they’ll be prompted to register a passkey.



Following this transition, on February 1, 2027, Microsoft will retire Microsoft-provided telecom delivery for SMS and voice authentication and will no longer offer SMS and voice as a native Microsoft Entra capability. Organizations that still require SMS or voice authentication methods will have the option to choose one of our telecom partners through the Microsoft Security Store. Customers will be responsible for any associated telecom-related costs charged by the telecom partners.



We strongly recommend moving users to passkeys or another phishing-resistant authentication method as soon as possible.




Explore Microsoft Entra solutions




Why stronger authentication matters in the AI era



Authentication methods that use SMS or voice rely on shared secrets or channels that attackers increasingly intercept, phish, or manipulate. Passkeys use public-key cryptography rather than shared secrets, making them phishing-resistant by design. They also provide a faster, simpler sign-in experience for users.



The case for moving beyond SMS and voice is no longer just that attackers intercept or socially engineer these methods. The threat environment has changed in speed, scale, and sophistication. Microsoft Threat Intelligence has observed AI-enabled phishing campaigns reaching click-through rates as high as 54%, compared with roughly 12% for more traditional campaigns, making stolen passwords and phishable second factors an urgent risk.1 At the same time, tactics such as SIM swapping and multifactor authentication bypass have become more accessible and repeatable.



An AI-powered cyberattack can use a compromised identity to automate discovery, privilege escalation, and lateral movement much faster than a human attacker working manually. This is why phishing-resistant authentication methods are so important.



By making passkeys the default authentication experience, organizations reduce reliance on phishable authentication methods and strengthen protection against credential theft and phishing.



Still need SMS or voice? Select a telecom provider in Microsoft Security Store 



Today, Microsoft provides the telecom delivery behind SMS and voice authentication natively within Entra ID. As part of this transition, we&#8217;ll step back from providing that native telecom delivery to encourage phishing-resistant methods as the standard for everyone.



For most organizations, the recommended path is simple: move users to passkeys at no additional cost.  



If you have a regulatory, technical, or business requirement to keep SMS or voice, you&#8217;ll be able to select, configure, and manage a third-party telecom provider through the Microsoft Security Store—a partner marketplace where you can contract directly with supported carriers. 



On September 18, 2026, we’ll share information on supported providers, deployment guidance, and technical documentation with pricing and commercial terms available through the Microsoft Security Store.



How to prepare



Start planning your transition now so you can select the deployment approach that best fits your organization and ensure your users are prepared for upcoming changes to their sign-in experience.




Identify users who still use SMS or voice. Review your authentication method policy and identify which users or groups are enabled for SMS or voice authentication.



Plan your passkey rollout. Enable passkeys and select the types that best fit your users’ devices and workflows. Microsoft Entra ID supports:

Synced passkeys, such as passkeys stored in platform credential managers like iCloud Keychain and Google Password Manager.



Device-bound passkeys, such as Microsoft Authenticator passkeys, Entra passkey on Windows, and FIDO2 security keys.





Use registration campaign to drive adoption. Microsoft Entra ID can help organizations move users at scale by prompting them to register a passkey during multifactor authentication sign-in.



Prepare user communications. Tell affected users what’s changing, when they’ll see a passkey registration prompt, and how to complete registration on their device.




For step-by-step guidance on planning, deploying, and managing passkeys, see our Microsoft Learn documentation and passkey deployment guide. 




aka.ms/passkeybydefault



Get started with a phishing-resistant passwordless authentication deployment in Microsoft Entra ID




If regulated, technical, or operational scenarios still require SMS or voice:




Identify and document affected user segments.



Starting October 30, 2026, select and configure a supported telecom provider through the Microsoft Security Store.



Test your configuration with a pilot group before any broad rollout.




Timeline



DateMilestoneSeptember 1, 2026 All users enabled for SMS or voice are auto-enabled and nudged for passkey registration upon multifactor authentication sign-in.Use the passkey deployment guide to prepare your environment for passkey use. Notify affected users about the upcoming change. Ensure every user has a phishing-resistant authentication method, such as a passkey, Entra passkeys on Windows, or a FIDO2 security key.September 18, 2026 Pricing, commercial terms, and a list of supported telecom providers will be shared. If you plan to continue using SMS or voice authentication, review the available provider options and identify affected users. October 30, 2026 Admins may select and configure a supported telecom provider through the Microsoft Security Store. February 1, 2027 Microsoft-provided SMS and voice authentication ends.  If SMS or voice remains necessary for specific users, configure a supported telecom provider before this date. After February 1, 2027 Users who use SMS or voice for multifactor authentication will be required to register a passkey before they can sign in. Automatic prompts to register a passkey will be enforced for all users in all tenants. There will be no opt-out option.



Note: The dates outlined in this post apply to Microsoft Entra ID in the public cloud only. Support for other cloud environments will follow on a separate timeline, with additional guidance and dates to be announced in advance.



SMS and voice have served their purpose well, bringing multifactor authentication to billions of users who otherwise would have had none. But the threat environment has evolved beyond their capabilities, and we need to evolve with it.



We’re making passkeys the default in Entra ID because they work better for users and worse for cyberattackers. We’re trying to make this transition as predictable as possible with clear dates, fallback options during migration, and recovery that doesn’t depend on phishable credentials anymore.



Learn more at aka.ms/passkeybydefault 




Learn more about Microsoft Entra identity and access solutions




To learn more about Microsoft Security solutions, visit our website. Bookmark the Security blog to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity) for the latest news and updates on cybersecurity.







1Microsoft Digital Defense Report 2025.
The post Microsoft Entra ID security updates: Passkeys are the default authentication method in Entra ID appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/07/13/microsoft-entra-id-security-updates-passkeys-are-the-default-authentication-method-in-entra-id/](https://www.microsoft.com/en-us/security/blog/2026/07/13/microsoft-entra-id-security-updates-passkeys-are-the-default-authentication-method-in-entra-id/)*
