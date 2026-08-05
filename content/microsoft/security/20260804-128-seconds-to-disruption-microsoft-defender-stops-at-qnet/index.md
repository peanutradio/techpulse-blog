---
categories:
- MS
- 보안
date: '2026-08-04T17:54:04+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tWhat\
  \ is device isolation?Case study: QNETAttack chain overviewMITRE ATT&amp;CK techniques\
  \ observedReferencesLearn more\t\t"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/08/04/129-seconds-disruption-microsoft-defender-stops-ransomware-qnet/
source: Microsoft Security Blog
tags: []
title: '128 Seconds to disruption: Microsoft Defender stops ransomware at QNET'
---

In this article
		

		
			
		
	
	
		
			What is device isolation?Case study: QNETAttack chain overviewMITRE ATT&amp;CK techniques observedReferencesLearn more		
	
	




Microsoft Defender&#8217;s attack disruption now includes device isolation, a new response action that extends autonomous protection directly to compromised endpoints.



At QNET, an attacker initiated a multi-stage attack using a legitimate Windows tool on a compromised endpoint to retrieve a malicious remote payload&#8211;a classic living-off-the-land (LOL) technique that often evades traditional containment. By automatically enforcing the new device isolation action on the compromised endpoint, Defender attack disruption stopped the attack dead in its tracks. From the first high-severity alert to completed isolation, after only 128 seconds, Defender cut off the attack chain before the second-stage payload could establish persistence or move beyond the host.



The growing threat: when the endpoint is the blast radius



Attack disruption has proven highly effective at stopping multistage, cross-domain attacks by disrupting the attacker’s ability to move across the environment. In many identity-driven attack scenarios, containing the compromised user is enough to shut down the attack chain, preventing lateral movement and limiting the attacker’s ability to access additional systems, identities, and resources.



However, we are increasingly seeing a different class of high-severity incidents that begin with initial access directly on the device. Once adversaries establish a foothold on an endpoint, they can plant multiple persistence mechanisms and continue operating locally on the machine. This means that acting against the user&#8217;s identity alone is no longer enough to dismantle the threat.



In these scenarios, the attacker has multiple ways to communicate and operate on the device beyond the user entity; the malicious code is already executing locally on the machine. The attacker doesn’t have to move laterally immediately; they can establish persistence, steal credentials, inject into processes, and prepare follow-on stages directly from the compromised endpoint itself.



Previously, stopping these attacks required manual triage and response, giving attackers time to advance. Device isolation closes this gap by automatically correlating signals, assessing the threat, and isolating the compromised device within seconds.



Traditional response approaches often depend on static playbooks triggered by individual alerts and maintained through manual tuning. Attack disruption instead uses AI-driven correlation and real-time analysis to identify multi-stage attacks by connecting signals across the environment before taking action. Device isolation is enforced only when the disruption pipeline reaches a high-confidence verdict—a threshold maintained at 99% precision.



What is device isolation?



When Microsoft Defender determines with high confidence that an endpoint is compromised, it isolates the device to immediately stop attacker activity and reduce the risk of further impact, such as data exfiltration and lateral movement.



What happens during device Isolation



When a device is isolated, all external network connectivity is blocked while maintaining access to required security services like Microsoft Defender for Endpoint. Selective isolation is supported, allowing customer-defined services or exclusions to continue functioning.



Automatic device isolation is scoped to the affected device (supported today on onboarded MDE workstations), time-limited, and operator-controlled. Security teams can review context, take follow-up actions, and manually release isolation when it’s safe to do so.



Why it matters



Device isolation is a powerful containment control because it disrupts the attack regardless of how the device was compromised or what the attacker planned to do next. A single action cuts off network access, breaking lateral movement, command and control, credential theft, and rapid encryption&#8211;effectively stopping hands-on activity and preventing spread to other systems. It is designed to work hand in hand with user containment. Isolating only the device or only the user leaves gaps; together, each one makes up for the weaknesses of the other, thereby mitigating these gaps to more effectively contain the attack.



Case study: QNET



QNET is a global direct-selling company with a distributed workforce and a lean security operations center (SOC). Like most teams of its size, QNET runs Defender with attack disruption enabled and relies on it to handle the first five minutes of a high-severity incident so analysts can focus on finding the root cause.



In the incident detailed here, attack disruption proved decisive: it stopped a multi-stage attack on a single endpoint within 128 seconds by automatically enforcing device isolation, its newest disruption action. Without this autonomous disruption, the human-in-the-loop delay could have been the difference between a contained initial living-off-the-land binary (LOLBin) execution and a fully detonated second-stage payload that had achieved credential theft and persistence.



In the customer&#8217;s words



&#8220;At QNET, we&#8217;ve seen a real impact from Microsoft&#8217;s attack disruption capability. During a recent incident, the device isolation was triggered almost immediately, which gave us confidence that the threat was contained early before it had any chance to spread.



What stood out for us is how this changes the way the team operates. Instead of racing against time to investigate and contain an active threat, my team can step in knowing the situation is already under control. That shift allows us to focus more on root cause analysis and remediation, rather than spending critical time trying to piece together what&#8217;s happening while the risk is still ongoing.



From a day-to-day SOC perspective, it makes our response more efficient and far less reactive. The alerts are clear, the actions are meaningful, and the disruption happens early enough to actually make a difference, not after the damage is done.



Overall, it&#8217;s helped us streamline our incident response and reduce exposure, while giving the team more breathing room to focus on what really matters.&#8221;



—  Ben Bredenkamp, Group CIO, QI Group



Attack chain overview






08:30 &#8211; 09:22BaselineA user opened a malicious file, likely delivered through email or browser download. The file executed mshta.exe, a legitimate Windows utility commonly abused by attackers. The mshta.exe process contacted an attacker-controlled URL and retrieved a second-stage payload. Persistence artifacts were then prepared (RunMRU activity was observed shortly afterward).09:23:20Initial Access / ExecutionThe malicious second stage executed through mshta.exe, establishing code execution on the device. Observed activity included suspicious command execution and user-level persistence behavior (RunMRU registry interaction). &nbsp;09:23:20DetectionTwo independent Defender detection engines triggered within the same second:&#8211; Behavioral/execution-based detection flagged suspicious command activity (RunMRU abuse).&#8211; The correlation engine identified the activity pattern as malicious and consistent with real attack behavior (not benign tooling usage).  09:25:02Disruption decisionThe disruption pipeline correlated the alerts, evaluated the threat model (single endpoint, no lateral movement signs, malicious code already executing under user context), and selected device isolation as the action most likely to immediately contain the attack. &nbsp;09:25:16Playbook startDefender autonomously initiated the IsolateDevice response playbook – the same containment action a SOC analyst would trigger manually – with full audit logging and a built-in auto-release mechanism to prevent prolonged business impact. &nbsp;09:25:28Device isolatedThe IsolateDevice action completed successfully. The endpoint was cut off from all external and internal network communication, allowing only Defender management traffic. Communication with attacker-controlled infrastructure was immediately terminated. &nbsp;09:25 &#8211; onwardPost-isolationNo additional malicious activity was observed. The mshta-launched payload was unable to continue execution, retrieve additional stages, or establish persistence. With no lateral movement or follow-on activity, the incident remained fully contained to a single endpoint. The SOC inherits a contained incident. &nbsp;



Total time from first detection to enforced isolation: 128 seconds.



The results



To summarize the results of the new device isolation response action:




From first detection, Defender isolated the device in just 128 seconds.



No second-stage payloads were observed after isolation. The mshta process was orphaned at the network layer; there was no outbound C2, and no follow-on download.



No lateral movement attempts were observed before or after isolation.



No SOC actions were required during the disruption window. The QNET SOC analyst who picked up the incident inherited an already-contained host and a complete action timeline.




MITRE ATT&amp;CK techniques observed



TacticTechnique IDTechnique nameObserved detailsInitial Access / ExecutionT1204.002User Execution: Malicious FileUser opened a malicious file delivered via browser download or email, resulting in execution of mshta.exe at approximately 09:23:20 UTC on device a3198469&#8230;b13.Defense EvasionT1218.005System Binary Proxy Execution: MshtaSigned Microsoft binary mshta.exe was abused to proxy execution of attacker-controlled HTA/script content and evade application trust controls.Command and ControlT1071.001Application Layer Protocol: Web Protocolsmshta.exe initiated outbound HTTP/HTTPS communication to attacker-controlled infrastructure to retrieve a second-stage payload.ExecutionT1059Command and Scripting InterpreterHTA-delivered script content executed through the mshta.exe host process, enabling attacker-controlled command execution in user context.PersistenceT1112Modify RegistrySuspicious RunMRU-related registry interaction indicated attempted user-level persistence preparation.Discovery / ExecutionT1057Process DiscoveryDefender behavioral detections observed suspicious command activity consistent with attacker reconnaissance and execution staging immediately after payload launch.Impact Mitigation (Defender response)&#8211;Device Isolation (Defender Automatic Attack Disruption)Defender correlated multiple high-confidence detections and autonomously executed the IsolateDevice response action at 09:25:16 UTC, completing isolation by 09:25:28 UTC.Command and Control (Prevented)T1105Ingress Tool TransferIsolation interrupted outbound connectivity before additional payload stages or tooling could be retrieved from attacker infrastructure.Lateral Movement (Prevented)TA0008Lateral MovementNo evidence of lateral movement activity was observed before containment; device isolation prevented any subsequent propagation opportunities.Persistence (Prevented)TA0003PersistenceAfter isolation, no additional persistence artifacts or follow-on malicious processes were observed on the endpoint.



References




Automatic attack disruption in Microsoft Defender XDR



Automatic attack disruption &#8212; supported triggers and enforced actions



Device Isolation in Attack Disruption



MITRE ATT&amp;CK T1218.005 &#8212; Mshta



MITRE ATT&amp;CK T1204 &#8212; User Execution




Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the&nbsp;Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on&nbsp;LinkedIn,&nbsp;X (formerly Twitter), and&nbsp;Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the&nbsp;Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;




Learn more about securing Copilot Studio agents with Microsoft Defender  



Evaluate your AI readiness with our latest Zero Trust for AI workshop.



Microsoft 365 Copilot AI security documentation 



How Microsoft discovers and mitigates evolving attacks against AI guardrails 

The post 128 Seconds to disruption: Microsoft Defender stops ransomware at QNET  appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/08/04/129-seconds-disruption-microsoft-defender-stops-ransomware-qnet/](https://www.microsoft.com/en-us/security/blog/2026/08/04/129-seconds-disruption-microsoft-defender-stops-ransomware-qnet/)*
