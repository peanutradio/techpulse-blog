---
categories:
- MS
- 보안
date: '2026-07-16T23:12:02+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tCampaign\
  \ 1: WebDAV-based ClickFix with Python loaders and blockchain C2Campaign 2: MSHTA-initiated\
  \ PowerShell chain with"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/07/16/acr-stealer-two-observed-intrusion-chains-amid-increased-threat-activity/
source: Microsoft Security Blog
tags:
- ClickFix
- Malware
- Social engineering
title: 'ACR Stealer: Two observed intrusion chains amid increased threat activity'
---

In this article
		

		
			
		
	
	
		
			Campaign 1: WebDAV-based ClickFix with Python loaders and blockchain C2Campaign 2: MSHTA-initiated PowerShell chain with steganographic payload deliveryMitigation and protection guidanceReferencesLearn more		
	
	




From late April 2026 to mid-June 2026, Microsoft Defender Experts observed increased ACR Stealer activity across customer environments. These campaigns are successfully using ClickFix lures to steal browser credentials, authentication tokens, and sensitive documents from enterprise environments. Successful compromise can expose browser credentials, session tokens, authentication artifacts, and sensitive enterprise data, potentially enabling account compromise, unauthorized access to cloud resources, and follow-on intrusion activity. Security teams should prioritize monitoring for ClickFix lures, suspicious WebDAV activity, obfuscated PowerShell execution, and attempts to access browser credential stores.



ACR Stealer is an information-stealing malware family reportedly offered through a malware-as-a-service (MaaS) model and associated with the rebranding of Amatera Stealer. During this period, two campaigns stand out, together appearing frequently in reviewed recent intrusions. Both begin the same way, with a ClickFix social engineering technique that tricks targets into running the threat actor’s command, but the intrusion chains that follow diverge in how they deliver payloads, establish execution, and evade detection.



The first campaign relies on WebDAV-delivered payloads, staged PowerShell, Python-based loaders and persistence, and, in some intrusions, blockchain-backed dead-drop command-and-control (C2) resolution. The second campaign takes a more fileless route, using MSHTA, obfuscated PowerShell, and steganography-assisted in-memory execution. Despite these differences, both campaigns ultimately pursue the same goal: stealing browser-stored credentials and other sensitive data for exfiltration.



These two campaigns represent some of the most prevalent ACR Stealer delivery campaigns observed by Defender Experts; however, they do not represent the full range of delivery methods used by this malware family. Attribution to ACR Stealer is based on the observed behavior and post-exploitation tradecraft, corroborated by open-source intelligence on the infrastructure associated with this malware family. Additional campaigns, infrastructure patterns, and execution chains are likely active, and organizations should treat the indicators and techniques described here as representative.



Microsoft Defender for Endpoint can help surface both campaigns through behavioral coverage for living-off-the-land execution, suspicious WebDAV and MSHTA activity, obfuscated PowerShell, scheduled-task persistence, in-memory payload execution, and browser credential theft. In this blog, we analyze both campaigns in detail, including their delivery mechanisms, post-exploitation tradecraft, indicators of compromise, hunting opportunities, and guidance to help defenders detect and disrupt related activity in their environments.






Campaign 1: WebDAV-based ClickFix with Python loaders and blockchain C2



Initial access



In this campaign, a ClickFix prompt, likely delivered through malvertising or SEO-manipulated search results, instructs the target user to run a command that launches cmd.exe. The command subsequently invokes rundll32.exe to load a DLL from a remote WebDAV share accessed over HTTPS. The WebDAV path commonly uses a GUID-based directory structure and filenames designed to resemble legitimate resources (for example, google.ct), enabling the activity to blend with expected network traffic and evade casual inspection.



We observed three variants of the initial execution command:



Variant 1: Direct rundll32 invocation






Variant 2: pushd-Mounted WebDAV Share






Variant 3: Headless and obfuscated pushd execution






Variants 2 and 3 are notable for their use of pushd, which transparently maps the remote WebDAV share to a temporary local drive prior to execution. This technique allows threat actors to execute remotely hosted content through what appears to be a local path, simplifying payload execution while reducing user awareness. In the more advanced variant, threat actors further enhance stealth by launching commands through conhost.exe &#8211;headless, suppressing visible console windows, and employing environment variable obfuscation with delayed variable expansion to conceal critical execution components such as pushd, rundll32, and the remote host name. Combined with minimized or headless execution, these techniques reduce user visibility, complicate static analysis and detection, and enable the infection chain to execute with minimal indication to the victim.



Execution, persistence, and evasion through process masquerading



Once rundll32.exe loads the DLL retrieved from the remote server, the malware establishes communication with threat actor-controlled infrastructure and executes a heavily obfuscated PowerShell script. The script employs excessive arithmetic no-ops, dead loops, fake control flow, and randomized variable names to hinder static analysis and evade signature-based detection.



The PowerShell script subsequently deploys another stage that functions as both a malware installer and a persistence mechanism. It:




Downloads a ZIP-packaged payload from a remote server and extracts it into a deceptive directory under %LocalAppData%\Temp (for example, LogiOptionsPlus).



Launches a Python script using a bundled pythonw.exe instance to avoid displaying a console window.



Removes previous deployments and terminates running instances before installation, effectively operating as an updater.



Establishes persistence through a hidden scheduled task disguised as a legitimate software update, ensuring execution at user sign-in.



Copies timestamps from a trusted Windows binary (notepad.exe) to the deployed files and clears PowerShell command history to reduce forensic visibility.



PowerShell loader downloads and executes a payload through a masqueraded scheduled task.



Python loader launching the stealer



The Python component serves as a heavily obfuscated loader designed to conceal its true functionality until runtime. It employs multiple layers of defense against static analysis, including dynamic API resolution, encoded string reconstruction, junk-data removal, character shifting, string reversal, Base64 decoding, and zlib decompression. These techniques ensure that the embedded payload remains unreadable in its static form and is reconstructed only during execution, significantly hindering signature-based detection and automated analysis.



Once decoded, the final-stage payload functions as an in-memory shellcode loader. It extracts an archive file masquerading as a legitimate application installer, reads a file from the archive, and injects the payload into a system process. The loader allocates executable memory using VirtualAlloc, copies the payload into the allocated memory region, and transfers execution through the Windows Fiber API (ConvertThreadToFiber, CreateFiber, and SwitchToFiber). This technique facilitates stealthy in-memory execution while minimizing artifacts written to disk.


Decoded Python shellcode loader using VirtualAlloc and Fiber-based execution.



Credential theft and data staging for exfiltration



The malware (injected code) aggressively harvests information from browser credential stores. It invokes Windows Data Protection API (DPAPI) routines to decrypt locally stored browser passwords, cookies, and authentication tokens. It also enumerates files across the system, targeting PDFs, Microsoft 365 documents, and data stored in enterprise-synchronized directories such as OneDrive and SharePoint. The collected data is subsequently archived, indicating preparation for exfiltration.



Blockchain dead-drop C2 resolution



A notable variation in this campaign is the use of blockchain services for C2 resolution, utilizing a technique known as EtherHiding. While most intrusions rely on more conventional C2 mechanisms, a subset deploys an additional secondary Python loader that leverages blockchain services as dead-drop resolvers. When this loader executes, it has been observed communicating with public blockchain RPC endpoints and third-party Web3 node infrastructure, likely querying data stored on a decentralized public ledger to retrieve follow-up payloads or a C2 address.



By externalizing C2 information to the blockchain, operators could dynamically update infrastructure without modifying or redeploying the malware, significantly complicating detection and takedown efforts. This behavior was observed across both variants of the campaign.



Campaign 2: MSHTA-initiated PowerShell chain with steganographic payload delivery



The second campaign takes a distinctly different approach to both delivery and execution. Where Campaign 1 relies on disk-based artifacts (Python runtime, scheduled tasks, and masquerading binaries), this campaign achieves its objectives almost entirely through fileless, in-memory execution, making it harder to detect through file-based scanning and forensic analysis.



Initial access through MSHTA and ClickFix



The execution chain begins when the victim, directed through malvertising or SEO-manipulated search results, encounters a ClickFix prompt that triggers a command spawning MSHTA to fetch and execute remote HTA content from an threat actor-controlled domain. The embedded VBScript loader abuses COM objects to decode and execute encoded PowerShell content.


VBScript loader using COM objects to decode and launch a PowerShell payload.



PowerShell downloader and obfuscation



The decoded PowerShell stage employs obfuscation techniques similar to those seen in Campaign 1: randomized variable names, arithmetic no-op operations, dead loops, misleading control flow, and custom encryption routines. Prior to contacting its next-stage infrastructure, the malware generates a victim-specific identifier and disables certificate validation. The retrieved content is executed directly in memory.



Steganography-based payload delivery



A notable technique in this campaign is the use of steganography to conceal malicious content inside a publicly hosted image. Instead of downloading a secondary script (as in Campaign 1), the malware retrieves a JPEG image from an image-hosting service.


Steganographic payload extraction from a downloaded image prior to decryption and execution.



Analysis of the script revealed custom routines that extract an embedded payload from image pixels, decrypt and decompress it, and execute it entirely in memory. The payload dynamically resolves APIs such as LoadLibrary, GetProcAddress, VirtualAlloc, CreateThread, and WaitForSingleObject at runtime to perform reflective shellcode execution. By combining steganography with in-memory execution, the malware minimizes on-disk artifacts and complicates both detection and analysis.



Credential theft, data collection, and exfiltration



Following execution, the malware accesses credential stores belonging to Chromium-based browsers, including Google Chrome and Microsoft Edge, specifically the Login Data and Web Data databases, alongside Windows DPAPI decryption activity. This behavior indicates attempts to recover stored browser credentials, session cookies, authentication tokens, and other sensitive user information.



The malware also enumerates and accesses multiple high-value PDF documents across Desktop and Downloads locations, suggesting targeted collection of potentially sensitive files. The combination of browser credential harvesting and systematic document access points to an information-stealing objective focused on staging credentials and valuable user data for exfiltration.



Mitigation and protection guidance



Microsoft recommends the following mitigations to reduce the impact of ClickFix lures, script-based payload delivery, credential theft, and post-compromise activity.




Educate users to recognize ClickFix-style prompts, fake verification checks, and paste-and-run instructions as malicious, especially when they invoke command interpreters or script hosts such as cmd.exe, PowerShell, rundll32.exe, or mshta.exe.



Reduce exposure to malvertising, SEO poisoning, and other web-based delivery chains by enforcing web filtering, blocking low-reputation or newly observed domains, and limiting access to remote content sources that are not required for business operations.



Use application control and attack surface reduction rules to restrict PowerShell, Python, mshta.exe, rundll32.exe, and similar tools from launching untrusted or internet-delivered content, particularly from user-writable directories such as Downloads, Temp, and %LocalAppData%.



Monitor for suspicious persistence and defense-evasion behavior, including scheduled tasks masquerading as software updates, timestomping, PowerShell history clearing, and execution chains that progress from remote content retrieval into PowerShell, Python, or shellcode-loading behavior.



Investigate abnormal access to Chromium-based browser databases, DPAPI-related decryption activity, staged collection of Microsoft 365 documents or PDFs, and compression activity that may indicate credential theft or data staging for exfiltration.



If compromise is suspected, isolate affected devices, rotate exposed credentials, revoke potentially compromised tokens, review persistence mechanisms, and investigate outbound connections to remote shares, image-hosting services, or other infrastructure used to resolve or retrieve follow-on payloads.



Harden endpoints against credential theft by reducing reliance on browser-stored credentials, enforcing multifactor authentication and conditional access, and reviewing how privileged accounts access sensitive applications and synchronized enterprise data.



Turn on cloud-delivered protection and behavior-based detections to help identify rapidly evolving threats, suspicious script execution, in-memory payload delivery, abuse of browser credential stores, and unusual child-process activity.



Run endpoint detection and response (EDR) in block mode and enable automated investigation and remediation so post-breach detections are contained, and malicious artifacts can be removed with minimal delay.



Harden PowerShell by enforcing appropriate execution policies, turning on script block logging, module logging, and transcription, and monitoring this telemetry for signs of malicious script activity.



Turn on tamper protection and prevent local administrators from weakening antivirus protection through local policy or exclusion changes.




Microsoft Defender XDR detections



Microsoft Defender XDR customers can refer to the list of applicable detections below. Microsoft Defender XDR coordinates detection, prevention, investigation, and response across endpoints, identities, email, and apps to provide integrated protection against attacks like the threat discussed in this blog.&nbsp;



TacticObserved ActivityMicrosoft Defender CoverageExecution&#8211; Suspicious MSHTA launch through ClickFix execution&#8211; Rundll32 loads remote WebDAV DLL&#8211; COM objects launch in-memory PowerShellMicrosoft Defender for Endpoint&#8211; Use of living-off-the-land binary to run malicious code&#8211; Obfuscated command line was launched&#8211; Suspicious process executed PowerShell command&#8211; Suspicious process launch by Rundll32.exeMicrosoft Defender for AntivirusBehavior:Win32/Interhta.IntPersistencePowerShell creates Scheduled task, masquerading as a software updateMicrosoft Defender for Endpoint &#8211; Suspicious Scheduled Task Process Launched   &#8211; Suspicious scheduled taskStealth/Defense Evasion&#8211; Fiber-API in-memory shellcode execution&#8211; Reflective shellcode via CreateThreadMicrosoft Defender for Endpoint Possible process hollowingCredential AccessCollects browser credentials, cookies, and tokens while enumerating files for exfiltrationMicrosoft Defender for Endpoint&#8211; Information stealing malware activity   &#8211; Suspicious DPAPI activity&#8211; Possible theft of passwords and other sensitive web browser information



Microsoft Security Copilot



Microsoft Security Copilot&nbsp;is&nbsp;embedded in Microsoft Defender&nbsp;and provides security teams with AI-powered capabilities to summarize incidents, analyze files and scripts, summarize identities, use guided responses, and generate device summaries, hunting queries, and incident reports.



Customers can also&nbsp;deploy AI agents, including the following&nbsp;Microsoft Security Copilot agents, to perform security tasks efficiently:




Threat Intelligence Briefing agent



Phishing Triage agent



Threat Hunting agent



Dynamic Threat Detection agent




Security Copilot is also available as a&nbsp;standalone experience&nbsp;where customers can perform specific security-related tasks, such as incident investigation, user analysis, and vulnerability impact assessment. In addition, Security Copilot offers&nbsp;developer scenarios&nbsp;that allow customers to build, test, publish, and integrate AI agents and plugins to meet unique security needs.



Threat intelligence reports



Microsoft Defender XDR customers can use the following threat analytics reports in the Defender portal (requires license for at least one Defender XDR product) to get current information available in the Defender portal about the threat actor, malicious activity, and techniques discussed in this blog. These reports provide the intelligence, protection information, and recommended actions to help prevent, mitigate, or respond to associated threats found in customer environments:




Amatera Stealer: Rebranded ACR Stealer With Improved Evasion, Sophistication



Double Trouble: Latrodectus and ACR Stealer observed spreading via Google Authenticator Phishing Site




Microsoft Security Copilot customers can also use the&nbsp;Microsoft Security Copilot integration&nbsp;in Microsoft Defender Threat Intelligence, either in the Security Copilot standalone portal or in the&nbsp;embedded experience&nbsp;in the Microsoft Defender portal to get more information about this threat actor.



Advanced hunting queries



Microsoft Defender XDR customers can run the following advance hunting queries to find related activity in their networks:



Run the query below to identify suspicious commands executed through ClickFix-based activity observed while delivering this stealer



DeviceRegistryEvents
| where RegistryKey has "RunMRU"
| where (RegistryValueData has_all ("rundll32", "@ssl", " /c ", " start ") and (RegistryValueData matches regex @"\\\\[^\\]+@ssl\\[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}\\\w+\.\w+,#1" or
RegistryValueData matches regex @"(?i)pushd \\\\[^\\]+@ssl\\[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12} ")) 
or RegistryValueData has_all ("@ssl", " /c ", "conhost --headless ") and RegistryValueData contains "rundll32"



Run the query below to identify scheduled task creation used for persistence by a malicious PowerShell script



DeviceProcessEvents
| where InitiatingProcessFileName =~ "powershell.exe"
| where InitiatingProcessCommandLine has_all ("-Command", "powershell")
| where ProcessCommandLine has_all ("schtasks", " /run /tn ", " Autoupdate ") and ProcessCommandLine matches regex "[0-9]{8}"



Run the query below to identify suspicious MSHTA launch through PowerShell



DeviceProcessEvents
| where InitiatingProcessParentFileName has "explorer.exe"
| where InitiatingProcessFileName =~ "powershell.exe" and InitiatingProcessCommandLine in~ ('"PowerShell.exe" ', '"PowerShell.exe"')
| where ProcessCommandLine has_all ('"mshta.exe" https://') and ProcessCommandLine matches regex "/[0-9]{7}"



MITRE ATT&amp;CK techniques observed



The following mapping summarizes the primary tactics and techniques observed across the two ACR Stealer intrusion chains. The mapping is intended to help defenders align observed behaviors with existing detection coverage, response playbooks, and hunting priorities.



TacticTechniqueObserved behaviorInitial AccessDrive-by Compromise; User ExecutionClickFix lure prompts command execution.ExecutionCommand and Scripting Interpreter: Windows Command Shell; PowerShell; Pythoncmd.exe, PowerShell, and pythonw.exe launch staged payloads.ExecutionSystem Binary Proxy Execution: Rundll32; MshtaRundll32 loads WebDAV DLLs; mshta.exe runs remote HTA content.PersistenceScheduled Task/Job: Scheduled TaskHidden scheduled task maintains user-logon execution.Defense EvasionObfuscated Files or Information; Masquerading; Indicator Removal: Clear Command HistoryObfuscation, timestomping, history clearing, and masquerading.Defense EvasionObfuscated Files or Information: SteganographyJPEG pixel data hides the encrypted payload.Defense Evasion / ExecutionReflective Code Loading; Process InjectionIn-memory shellcode execution via runtime API resolution.Credential AccessCredentials from Web BrowsersBrowser stores and DPAPI activity used to recover credentials and tokens.CollectionData from Local System; Data StagedPDFs, Office files, and synced enterprise data are staged.Command and ControlWeb Service; Dead Drop ResolverInfrastructure and blockchain RPC endpoints resolve payload or C2 data.



Indicators of compromise (IOC)



Campaign 1IndicatorDescriptionlooksta[.]icuC2 domaincontrite.quirksturdy[.]icuC2 domainux.strainedeasily[.]icuC2 domaincpppemwjewjoiwejow[.]saleC2 domainbreaksd.wifihot[.]icuC2 domainwalter.filloco[.]icuC2 domainfast.raidher[.]icuC2 domainapigrokcloud[.]icuC2 domainCampaign 2enhanceblabber[.]ccC2 domaindeep-harborio[.]com1st Stage payload hosting siteauramatrixa[.]com1st Stage payload hosting sitezealpraxis[.]com1st Stage payload hosting siteprism-vertex[.]com1st Stage payload hosting siteprism-matrixs[.]com1st Stage payload hosting siteproton-network[.]com1st Stage payload hosting sitecreativecommunityinfo[.]artPayload hosting site



References




Intelligence Insights: May 2026 | Red Canary



Possible ACR Stealer From Page Impersonating Claude




Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the&nbsp;Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on&nbsp;LinkedIn,&nbsp;X (formerly Twitter), and&nbsp;Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the&nbsp;Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;




Evaluate your AI readiness with our latest Zero Trust for AI workshop.



Microsoft 365 Copilot AI security documentation 



How Microsoft discovers and mitigates evolving attacks against AI guardrails 



Learn more about securing Copilot Studio agents with Microsoft Defender  

The post ACR Stealer: Two observed intrusion chains amid increased threat activity appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/07/16/acr-stealer-two-observed-intrusion-chains-amid-increased-threat-activity/](https://www.microsoft.com/en-us/security/blog/2026/07/16/acr-stealer-two-observed-intrusion-chains-amid-increased-threat-activity/)*
