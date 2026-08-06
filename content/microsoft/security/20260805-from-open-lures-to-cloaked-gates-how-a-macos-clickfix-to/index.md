---
categories:
- MS
- 보안
date: '2026-08-05T15:48:39+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tActivity\
  \ overviewHow ClickFix works Campaign overviewClickFix moved from open pages to\
  \ fingerprinting gatesThe fingerpri"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/08/05/macos-clickfix-campaign-learned-hide/
source: Microsoft Security Blog
tags:
- ClickFix
title: 'From open lures to cloaked gates: How a macOS ClickFix campaign learned to
  hide'
---

In this article
		

		
			
		
	
	
		
			Activity overviewHow ClickFix works Campaign overviewClickFix moved from open pages to fingerprinting gatesThe fingerprinting gateMitigation and protection guidanceIndicators of compromise (IOC)ReferencesLearn more		
	
	




Microsoft Threat Intelligence observed a macOS ClickFix campaign distributing infostealers, including MacSync and Atomic Stealer (AMOS), through a large cluster of look-alike domains. The campaign evolved from broadly serving ClickFix lures to using a server-side browser-fingerprinting gate that shows the lure primarily to visitors whose environment appears consistent with a genuine macOS browser. This cloaking limits visibility for crawlers, sandboxes, and some automated analysis workflows. The blog details the domain pattern, fingerprinting checks, infection chain, detection coverage, and hunting pivots that defenders can use to identify related activity.



Activity overview



Microsoft Threat Intelligence has been tracking a macOS ClickFix operation that distributes information-stealing malware through a large family of algorithmically named domains. Over several weeks of monitoring, Microsoft observed a notable shift in tradecraft: the same infrastructure moved from openly serving the malicious command in the served page’s HTML source to concealing the lure behind a server-side fingerprinting gate that reveals the payload only to visitors the server assesses as a genuine macOS target. The chain ultimately delivers information stealers such as MacSync or Atomic Stealer (AMOS).



This activity is consistent with the broader shift in macOS ClickFix tradecraft that Microsoft Threat Intelligence previously documented, in which threat actors instruct users to run Terminal commands that retrieve remotely hosted content rather than the traditional approach of delivering a disk image for manual installation. The cluster described here is notable for two reasons: its domains are mass-produced by a recognizable name generator, and it adopted server-side cloaking on existing infrastructure, giving defenders a clear before-and-after view of the same operation.



In this blog, we describe the campaign’s domain-generation pattern, the two delivery phases we observed, the fingerprinting gate that now fronts the infrastructure, and the end-to-end infection chain. We also provide hunting guidance, mitigation recommendations, and defanged indicators of compromise.



How ClickFix works&nbsp;



ClickFix is a social-engineering technique where attackers persuade users to copy and run a command in Terminal instead of downloading a traditional macOS application. The lure usually appears as a fake verification step, software update, download error, or CAPTCHA, with the command disguised as something required to complete the action. Because execution starts from a user-run Terminal command rather than a downloaded app bundle, the flow can avoid parts of the normal macOS application trust path, including quarantine handling, code-signing evaluation, and notarization checks typically applied to downloaded applications. 



In this campaign, ClickFix remains the delivery mechanism, but the important change is that the lure is no longer shown to every visitor. The page first profiles the visitor through a browser-fingerprinting gate and primarily requests consistent with a genuine macOS browser environment receive the fake “Download for macOS” page and copied Terminal command.


Figure 1a &#8211; The counterfeit &ldquo;Download for macOS&rdquo; page served to a qualifying visitor by a cloaked gate (apricotfilepoint[.]com). The page displays a forged &ldquo;Verified Publisher&rdquo; badge and offers a one-click Copy of an obfuscated curl one-liner.



Delivery is conditional. During analysis, the same URLs returned different content to different requests. In some case the macOS ClickFix lure, and in others an apparently benign decoy page. 



In our testing, a request presenting a Windows browser received a decoy page such as a fake browser-extension or VPN landing page (Figure 1b) or a page impersonating an unrelated business such as a logistics and freight-forwarding company rather than the ClickFix lure. Because this decision is made server-side on a per-request basis, a given scan or visit may receive benign or decoy content and still be interacting with malicious infrastructure, so an apparently benign or look-alike response does not mean the domain is safe. We examine how the gate evaluates each request later in this post.


Figure 1b &#8211; A decoy page (a fake &#8220;Urban VPN Proxy&#8221; browser extension landing page) returned to non qualifying requests on the same domain (apricotfilepoint[.]com).



Campaign overview



The key change in this campaign is not the ClickFix lure itself, but the new layer placed in front of it. Microsoft Threat Intelligence confirmed more than 250 ClickFix front-end domains during the tracking window, and many followed a repeated naming pattern using the token “file” with dictionary-style words, such as filecopperbasket, filevelvettractor, fileoceanhammer, and filemarblegarden.



Some related domains place “file” token in the middle or at the end, such as applefilevault, bananafastfile, and orangesmartfile, while others omit it completely, such as cloudsendhub and syncdatavault. Defenders should treat the naming pattern as a hunting pivot, not a complete signature.&nbsp;The stronger signal is the combination of dictionary-style domains, shared infrastructure behaviour, and the fingerprinting gate that controls who sees the ClickFix lure. This naming pattern is useful for clustering and hunting, but it is not the main story. The more important behaviour is that these domains now serve a browser-fingerprinting gate before showing any malicious content.



ClickFix moved from open pages to fingerprinting gates



In its earlier phase, the campaign’s domains served the lure directly. Retrieving one returned a “complete your download in Terminal” page with the malicious command present in the HTML. A scanner that does not execute JavaScript could recover the entire attack from the page source, including: the macOS paste-to-Terminal instructions, clipboard-write logic, obfuscated shell command, and encoded staging URL. Because the command was embedded in the served page, the domains were readily identifiable from passive data and static content matching.



The same infrastructure that previously exposed its&nbsp;ClickFix&nbsp;lure directly to visitors has evolved to employ a server-side fingerprinting gate. Rather than&nbsp;immediately&nbsp;presenting the malicious content, affected domains now return a minimal page&nbsp;containing&nbsp;only a lightweight JavaScript profiling routine(~2.5 KB size). To both casual visitors and automated scanners, the site may appear blank, inactive, or apparently benign.&nbsp; In reality, the&nbsp;page serves as an evaluation layer that&nbsp;determines&nbsp;whether a visitor should be&nbsp;shown&nbsp;the&nbsp;ClickFix&nbsp;lure.



Across Microsoft Threat Intelligence&#8217;s investigation of this domain cluster, the outcomes were consistent. Simple crawlers received an empty, parked-looking page. JS-capable crawlers and sandbox environments that failed fingerprinting checks were served apparently benign decoy page, and requests presenting a genuine macOS browser fingerprint were shown the ClickFix lure.


Figure 2 &#8211; Earlier open-lure delivery compared with the current fingerprinting-gated delivery flow.



The fingerprinting gate



The gate profiles each visitor using a combination of browser, hardware, and runtime attributes, which are submitted to the server for evaluation. The following sections break down the categories of signals collected.



Browser profiling and environment collection



The first stage builds a browser fingerprint by collecting browser and page details from six objects exposed to the page: navigator, screen, window, document, location, and console. From navigator, it captures values such as platform, for example, &#8220;MacIntel&#8221;, user agent, language, vendor, and plugins, which establish the visitor’s claimed device and browser identity. 



Display values from screen and window, including screen size, color depth, window dimensions, and pixel ratio, provide consistency signals for whether that identity is consistent with a real, non‑virtualized Mac environment. Page context from document and location, including title, referrer, character set, URL, and host, helps tie the fingerprint to the delivery context. The console object is also enumerated as part of the runtime surface and later helps identify developer tools or automated log-capturing environments. These values are merged into a single fingerprint object tagged with mode: &#8220;php&#8221; and later submitted back to the server for evaluation.


Figure 3a &#8211; The gate collects browser, system, and environment characteristics from multiple browser objects to build a visitor fingerprint.



Hardware validation



The gate then performs additional validation to determine whether the visitor resembles a genuine macOS user. One notable check uses WebGL, a browser graphics API normally used to render 2D and 3D content, to retrieve graphics-processing details from the visitor’s device. In this campaign, those WebGL-derived GPU signals help distinguish real Apple hardware from virtualized, emulated, software-rendered, or sandboxed environments before the server decides whether to return the ClickFix lure.


Figure 3b &#8211; WebGL-derived GPU signals can help distinguish likely Apple hardware from virtualized, emulated, software-rendered, or sandboxed environments.



Environment and&nbsp;behavioral&nbsp;checks



Additional&nbsp;probes evaluate characteristics such as&nbsp;timezone&nbsp;configuration, touch-input support, and whether the page is running inside an embedded frame. These signals help&nbsp;identify&nbsp;uncommon execution contexts that may&nbsp;indicate&nbsp;automated analysis or monitoring infrastructure.



The script records three signals:




timezoneOffset reads the system&#8217;s local timezone offset. Unusual or inconsistent values can contribute to identifying hosted infrastructure, sandbox environments, or otherwise atypical execution context.



frame checks whether the page is running inside an iframe. While common in legitimate scenarios, embedded execution contexts can also be associated with crawlers, analysis tools, and other automated environments, making this a useful qualification signal.



touchEvent checks for touch-input support. On desktop macOS systems, touch support is generally uncommon; unexpected touch capabilities can contribute to identifying an emulated, spoofed, or otherwise atypical environment.




Together, these checks help the gate distinguish a normal macOS desktop browser session from framed, headless, mobile, sandboxed, or automated environments before the server decides what content to return.


Figure 3c &#8211; Additional checks evaluate environmental attributes that can help differentiate legitimate users from automated systems.



Anti-analysis techniques



The gate also incorporates checks designed to detect browser instrumentation, automation frameworks, and modified browser&nbsp;behavior. Rather than simply&nbsp;determining&nbsp;whether a visitor is a bot, these probes appear intended to&nbsp;identify&nbsp;environments commonly used by researchers, crawlers, and security-analysis platforms. The implementation details described here are intended to help defenders recognize and detect gate behavior in malicious traffic-distribution infrastructure.


Figure 3d &#8211; The gate performs checks intended to identify browser instrumentation and automated analysis environments.



Two checks stand out. The first is a toString() counter. The script creates a temporary function whose toString() method increases a counter, then writes that function to the console. In a normal browser, this counter usually remains unchanged. However, if the developer console is open, or if a headless or log-capturing tool serializes console output, the function may be converted to a string, causing the counter to increase.



The second is a prototype-tamper probe built around a normal browser capability check. The gate calls canPlayType(&#8220;video/mp4&#8221;), which normally checks whether the browser supports MP4 playback. Here, that check is repurposed as a tripwire. A genuine browser handles the codec check natively and silently, but some automated or stealth browsers fake codec support in JavaScript. If that JavaScript path calls the hooked Array.prototype.includes, the gate sets the proto:true signal and flags the environment as potentially instrumented or automated.



Fingerprint submission



Once profiling is complete, the collected attributes are packaged and silently&nbsp;submitted&nbsp;back to the same server for evaluation. This process occurs without any user interaction or visible page content.


Figure 3e &#8211; Collected fingerprint data is submitted to the server, which determines whether the visitor qualifies to receive the ClickFix lure.



The following is the sample fingerprint the client sends to the server (values are representative and defanged):






Server-side victim selection



With the fingerprinting logic in place, the malicious content is no longer present in the initial page shown to the visitor. Instead, the server withholds the ClickFix lure until it receives and evaluates the submitted fingerprint, then returns one of two responses:




A bot, crawler, sandbox, virtual machine, unexpected geography, or unexpected browser receives a blank page, a benign decoy, or no content.



A genuine Mac and browser in an expected context receive the ClickFix lure: the counterfeit “Verified Publisher / Download for macOS” page and its poisoned one-liner. The targeting is primarily environment-based: genuine macOS users in an expected browser and request context receive the ClickFix lure.




This is a Traffic Distribution System (TDS) gate. We call it a TDS because the payload is delivered by server-side, on demand, only to visitors the operator selects security crawlers, researchers, and sandboxes are served no malicious content. This gating can make automated detection and analysis more difficult because those tools may see only an apparently benign response even though the infrastructure can deliver the ClickFix lure to selected macOS visitors.


Figure 4 &#8211; Server-side fingerprint evaluation and possible responses for selected and non-selected visitors.



Inside the infection chain: from gated lure to AMOS



The individual techniques used by the gate are not inherently malicious or novel. Browser fingerprinting, hardware validation checks, and Traffic Distribution System (TDS)-style visitor filtering are common in anti-abuse systems and have previously appeared in exploit-kit and malvertising ecosystems. What distinguishes this activity is how these techniques are integrated into a ClickFix campaign. Rather than immediately presenting a malicious command, the actor performs server-side victim qualification before revealing the lure, reducing visibility to researchers and automated security systems while maintaining access to intended macOS targets.



Using a qualified macOS target, we analyzed the complete infection chain. The activity began on a file&lt;word&gt;&lt;word&gt;[.]com domain hosting the fingerprinting gate, which returned the counterfeit Download for macOS page (Figure 1a). A non-qualifying request received little or no visible content. The page uses GitHub-themed branding to mimic a legitimate software download experience; the branding is spoofed and does not indicate any compromise of GitHub.



When the victim runs the Terminal command, the campaign retrieves and executes a remote script from a /curl/&lt;id&gt; URL. The chain then progresses through multiple script stages before ultimately downloading and launching Atomic Stealer (AMOS), an information stealer that harvests credentials, browser and cryptocurrency wallet data, authentication stores, and other sensitive files before exfiltrating them. We detailed AMOS delivery across multiple macOS ClickFix lures in earlier research.



Because delivery is restricted to qualified visitors, the fingerprinting gate is often a more reliable hunting target than the downstream malware. Systems that inspect page content without executing client-side JavaScript can observe the gate logic directly, while environments that fail qualification are redirected to apparently benign or no content. Because these characteristics also appear in legitimate anti-bot implementations, evaluate combinations rather than single indicators. Useful signals include self-submitting fingerprinting forms, hidden fingerprint data fields, artifacts such as the mode:&#8221;php&#8221; parameter, and domains following the observed file naming convention; correlating several of these improves confidence and reduces false positives.



Mitigation and protection guidance



Organizations can apply the following recommendations to reduce exposure to this and similar macOS ClickFix campaigns:




Educate users. Reinforce that no legitimate download, CAPTCHA, or verification step requires pasting a command into Terminal.



Monitor Terminal usage. Alert on Terminal or shell sessions that spawn curl, base64, gunzip, or osascript, particularly when initiated shortly after web browsing.



Detect native-tool abuse. Flag unusual sequences of macOS utilities such as curl piped to zsh, base64 -d, and xattr -c immediately preceding chmod +x.



Inspect outbound downloads. Monitor curl activity that retrieves encoded or compressed payloads from newly registered or low-reputation domains, including /curl/&lt;hex-id&gt; request paths.



Protect credential stores. Detect unauthorized access to keychain items, browser credential databases, SSH keys, and cryptocurrency wallet data.



Monitor data staging. Alert on the creation of archives of sensitive artifacts followed by HTTP POST exfiltration.



Block on infrastructure, not just front-end domains. Where validated, prioritize blocking known shared back end and staging hosts (for example, malware-c2 and the /curl/&lt;id&gt; staging hosts) over individual disposable front-end domains.



Hunt the generation pattern. Where feasible, alert the file&lt;word&gt;&lt;word&gt; domain pattern rather than maintaining a list of individual domains.




On macOS 26.4 and later, Apple introduced a mitigation that displays a warning when a user attempts to paste a potentially malicious command into Terminal, directly addressing the ClickFix delivery mechanism.



When a user attempts to paste a potentially malicious command into Terminal, they will now see the following prompt:



Possible malware, Paste blocked



Your Mac has not been harmed. Scammers often encourage pasting text into Terminal to try and harm your Mac or compromise your privacy. These instructions are commonly offered via websites, chat agents, apps, files, or a phone call.



Microsoft Defender XDR detections



Tactic&nbsp;Observed activity&nbsp;Microsoft Defender coverage&nbsp;&nbsp;Initial Access&nbsp;Malicious webpage&nbsp;Microsoft Defender for SmartScreenSmartScreen Detection Blocks webpage (Figure 5)&nbsp;Execution&nbsp; &nbsp;User copies, pastes, and runs encoded instructions. The instructions are decoded, executable files are created from remote attacker infrastructure, and the malware implant is executed.Microsoft Defender for Endpoint&#8211; Behavior:MacOS/SuspAmosExecution&#8211; Malicious file execution &nbsp; &#8211; Behavior:MacOS/SuspOsascriptExec&#8211; Malicious osascript execution&#8211; Behavior:MacOS/SuspDownloadFileExec &#8211; Behavior:MacOS/SuspInfoExfil&#8211; Behavior:MacOS/SuspiciousActiviyGen.AE&#8211; Suspicious file download and executionCredential access Keychain extraction&nbsp;Behavior:MacOS/SuspKeyChainCopy.ABCollection &amp; Exfiltration &nbsp;Browser data, crypto wallets, keys etc. &nbsp;&#8211; Behavior:MacOS/SuspInfostealExec&#8211; Behavior:MacOS/SuspCredCopy&#8211; Behavior:MacOS/SuspPassSteal



Microsoft Defender SmartScreen displays a warning message to Microsoft Edge users when they visit a ClickFix landing page:


Figure 5. Microsoft Defender SmartScreen flagging a ClickFix webpage.



Microsoft Security Copilot&nbsp;&nbsp;



Security Copilot customers can use the standalone experience to&nbsp;create their own prompts&nbsp;or run the following&nbsp;prebuilt promptbooks&nbsp;to automate incident response or investigation tasks related to this threat:&nbsp;




Incident investigation



Microsoft User analysis&nbsp;&nbsp;



Threat actor profile&nbsp;&nbsp;



Threat Intelligence 360 report based on MDTI article&nbsp;&nbsp;



Vulnerability impact assessment




Note that some promptbooks require access to plugins for Microsoft products such as Microsoft Defender XDR or Microsoft Sentinel.



Advanced hunting



The following query is an illustrative starting point. Validate table/column names and adjust the time range and indicators for your environment before running.



Known-IOC network sweep (mirrors a standard IOC hunt; populate from the IOC table and refresh as domains rotate)



let lookback = 30d;
let SuspiciousDomains = 
dynamic(["lemonfilewave.com","limefilescope.com","mangocloudfile.com"]);
DeviceNetworkEvents   
| where Timestamp >ago(lookback) 
| where RemoteUrl has_any (SuspiciousDomains)



Indicators of compromise (IOC)



Indicator&nbsp;Type&nbsp;Description&nbsp;applefilevault[.]comDomainClickFix Webpageapricotfilepoint[.]comDomainClickFix Webpage&nbsp;bananafastfile[.]comDomainClickFix Webpagecloudfilebridge[.]comDomainClickFix Webpagefilecedarwallet[.]online.DomainClickFix Webpagefilecopperbasket[.]sbsDomainClickFix Webpagefilecrimsonsignal[.]onlineDomainClickFix Webpagefilemarblegarden[.]sbsDomainClickFix Webpagefileoceanhammer[.]sbsDomainClickFix Webpagefilerubyfolder[.]sbsDomainClickFix Webpagefilevelvettractor[.]sbsDomainClickFix Webpagelemonfilewave[.]comDomainClickFix Webpagelimefilescope[.]comDomainClickFix Webpagemangocloudfile[.]comDomainClickFix Webpageorangesmartfile[.]comDomainClickFix Webpagesyncdatavault[.]comDomainClickFix Webpagecloudsendhub[.]comDomainClickFix Webpage



References




ClickFix campaign uses fake macOS utilities lures to deliver infostealers | Microsoft Security Blog



Think before you Click(Fix): Analyzing the ClickFix social engineering technique | Microsoft Security Blog



macOS ClickFix Lures Deploy AppleScript Stealer &amp; Persistent RAT | The Lens blog &#8211; netskope &#8211; June 17, 2026



Clickfix Github Themed macOS Infostealer Deliver Campaign IOCs | GitHub gist &#8211; brkalbyrk &#8211; May 16, 2026



Atomic macOS Stealer (AMOS): Reading the C2 Protocol from a PCAP | Pcap AI blog &#8211; June 23, 2026



Evil evolution: ClickFix and macOS infostealers | Sophos Blog &#8211; March 11, 2026




Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the&nbsp;Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on&nbsp;LinkedIn,&nbsp;X (formerly Twitter), and&nbsp;Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the&nbsp;Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;




Microsoft 365 Copilot AI security documentation&nbsp;



How Microsoft discovers and mitigates evolving attacks against AI guardrails&nbsp;



Learn more about securing Copilot Studio agents with Microsoft Defender &nbsp;



Evaluate your AI readiness with our latest&nbsp;Zero Trust for AI workshop.

The post From open lures to cloaked gates: How a macOS ClickFix campaign learned to hide appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/08/05/macos-clickfix-campaign-learned-hide/](https://www.microsoft.com/en-us/security/blog/2026/08/05/macos-clickfix-campaign-learned-hide/)*
