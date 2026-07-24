---
categories:
- MS
- 보안
date: '2026-07-23T15:00:00+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tTycoon2FA\
  \ Q2 disruption impactQR code phishing attacksCAPTCHA-gated phishing tacticsMalicious\
  \ payloadsBusiness email com"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/07/23/email-threat-landscape-q2-2026-trends-and-insights/
source: Microsoft Security Blog
tags:
- Adversary-in-the-middle (AiTM)
- Credential theft
- Phishing
- Social engineering
title: 'Email threat landscape: Q2 2026 trends and insights'
---

In this article
		

		
			
		
	
	
		
			Tycoon2FA Q2 disruption impactQR code phishing attacksCAPTCHA-gated phishing tacticsMalicious payloadsBusiness email compromiseMicrosoft Teams threatsNotable phishing campaignsMitigation and protection guidanceIndicators of compromise (IOCs)		
	
	




The second quarter of 2026 (April–June) was largely defined by the continuing downstream effects following Microsoft’s Digital Crimes Unit-led disruption efforts against the Tycoon2FA phishing-as-a-service (PhaaS) platform in March. Phishing volume linked to the platform fell 92% from pre-disruption averages, including QR code phishing and CAPTCHA-gated phishing both declining from their March highs. Despite ongoing efforts to rebuild operations, Tycoon2FA did not recover its previous scale or influence during Q2, and no single service emerged to replace the platform at comparable scale.




	
		
			Inside tycoon2fa		
		
							
						Infrastructure, tradecraft, and detections							›
					
	




These trends reflect both the measurable impact that disruption operations can have on phishing ecosystems and the adaptability of threat actors as they diversify delivery channels. At the same time, Microsoft Threat Intelligence observed continued growth in Teams-based social engineering, particularly voice phishing (vishing), with weekly malicious call attempts reaching nearly ten times the mid-2025 baseline by the end of the quarter. This activity illustrates how threat actors continue to expand beyond email into trusted workplace communication platforms where communications may appear more trustworthy to users.



Microsoft detected approximately 7.6 billion email-based phishing threats throughout the quarter, with monthly volumes declining modestly from 2.7 billion in April to 2.4 billion in June. Credential phishing remained the dominant objective behind malicious payloads, while business email compromise (BEC) activity largely returned to historical norms after a brief, anomalous surge in April. Notable campaigns observed during the quarter also demonstrated how threat actors combine automation, trusted services, and multi-stage delivery chains to scale operations. These campaigns ranged from an automated BEC campaign that reached more than 67,000 users across 42,000 organizations in under three hours, to a multi-stage phishing campaign that used nested EML files, calendar invitations, and a Microsoft authentication redirect to deliver malware.




	
		
			Q2 AiTM token compromise		
		
							
						April phishing campaign tactics, detections, and mitigations							›
					
	




This blog provides a view of email threat activity across the second quarter of 2026, highlighting key trends in phishing techniques, payload delivery, and threat actor behavior observed by Microsoft Threat Intelligence. We examine shifts in QR code and CAPTCHA-gated phishing activity, malicious payload trends, BEC activity, the growth of Teams-based threats, and notable campaigns observed during the quarter. We also provide recommendations and Microsoft Defender detections to help organizations identify and mitigate evolving threats while prioritizing defensive measures.



Tycoon2FA Q2 disruption impact



The disruption operation that Microsoft&#8217;s Digital Crimes Unit launched against Tycoon2FA infrastructure in early March continued to produce measurable results throughout Q2 2026. After falling 15% in March and another 22% in April, Tycoon2FA-linked phishing volume dropped 74% in May to just 1.5 million messages, then fell another 20% in June to 1.2 million, by far the lowest monthly volumes observed in at least a year. For reference, the average monthly volume of phishing messages linked to Tycoon2FA during the second half of 2025 was 15.1 million. By the end of Q2, volumes were running at roughly 8% of that baseline, representing a 92% total decline since the disruption operation began.




	
		
			email threat landscape		
		
							
						Q1 trends that shaped Q2 activity							›
					
	



Figure 1. Tycoon2FA monthly malicious messages volume (July 2025&ndash;June 2026)



Tycoon2FA&#8217;s influence across two primary phishing tactics, QR code lures and CAPTCHA-gated landing pages, also continued to decline throughout the quarter:




CAPTCHA-gated phishing: Tycoon2FA&#8217;s share of CAPTCHA-gated phishing sites fell from 41% in March to 16% in April and 12% by June, down from a peak of 76% in December 2025.



QR code phishing: The share of QR code campaigns redirecting to Tycoon2FA domains decreased from 20% in March to 17% in April and 14% by June, down from a peak of 33% in November 2025.




These declines indicate that the platform&#8217;s customer base has not migrated to replacement infrastructure at anything close to the scale they previously operated.



After being forced off Cloudflare, which had provided anti-analysis protection that made Tycoon2FA pages harder to scan and take down, the service continued to rely on infrastructure hosted on the .RU top-level domain (TLD), a shift that began in late March. More than 40% of newly observed Tycoon2FA domains used .RU registrations throughout Q2. While this reflects an ongoing effort to find replacement hosting, Tycoon2FA&#8217;s role in the phishing ecosystem has nonetheless been significantly diminished and the pace of recovery has been slow.



QR code phishing attacks



After peaking at 18.7 million attacks in March, the highest monthly volume in at least a year, QR code phishing declined for three consecutive months in Q2. Volume fell 7% in April to 17.4 million, then dropped more sharply in May (-38%) and June (-22%), closing the quarter at 8.3 million attacks. By June, QR code phishing had returned to levels last seen in mid-2025.


Figure 2. Trend of QR code phishing attacks by weekly volume (January 2026&ndash;June 2026)



The delivery methods used in QR code attacks shifted notably during Q2. PDF attachments remained the dominant vehicle throughout, but their dominance weakened after April:




PDF attachments peaked at 79% of QR code attacks in April before falling to 59% in May and 58% in June. By raw volume, malicious PDFs containing QR codes dropped more than 60% between April and June.



DOC/DOCX attachments moved in the opposite direction, increasing 30% in May to account for 38% of QR code payloads, the highest share since December 2025. By June, DOC/DOCX payloads reached 40% of QR code attacks. This swap between PDF and DOC/DOCX dominance is a pattern that has recurred throughout the past year, as operators appear to rotate between delivery formats.



Email-embedded QR codes, which had surged 336% in March and accounted for 5% of QR code attacks, effectively disappeared in Q2. This delivery method dropped to near-zero across all three months, leaving QR code phishing almost entirely an attachment-based tactic.



Figure 3. QR code phishing delivery method share by month (January-June 2026)



CAPTCHA-gated phishing tactics



After accumulating to nearly 12 million attacks in March, the highest monthly volume observed over the past year, CAPTCHA-gated phishing declined sharply throughout Q2. Volume fell 32% in April to 8.2 million, then dropped another 65% in May and 24% in June, closing the quarter at just 2.2 million attacks. Since the March peak, CAPTCHA-gated phishing has fallen more than 81%, reaching its lowest monthly volume in more than a year.


Figure 4. CAPTCHA-gated phishing volume (January 2026&ndash;June 2026)



The rapid rotation of delivery methods that characterized Q1 continued into Q2, with no single payload type maintaining the top position for more than one or two months:




PDF attachments surged to 63% of CAPTCHA-gated attacks in April, the highest single-payload share observed in the past year, after more than quadrupling in March. This dominance was short-lived, however. PDF volumes dropped 69% in May and another 70% in June, falling to just 22% of attacks by the end of the quarter.



HTML attachments, which had been a major delivery vector through January (37% of attacks), declined sharply during Q2. After declining to 8% in April, HTML payloads fell to just 3% in May before recovering slightly to 5% in June, their lowest sustained share in at least a year.



SVG files reached their lowest observed volume in April (5% of attacks) before rebounding to 12% in May and 26% in June. While still well below the levels seen when Tycoon2FA actively used SVG files, this gradual recovery bears monitoring.



Email-embedded URLs reclaimed the top position in June for the first time since December 2025, accounting for 30% of CAPTCHA-gated attacks. This was more a function of every other delivery method declining in raw volume than a resurgence in URL-based delivery. The actual volume of URL-delivered CAPTCHA-gated phish in June was still far lower than most months over the past year.



DOC/DOCX files declined from their March spike, falling steadily from 15% to 10% of attacks over the quarter.



Figure 5. CAPTCHA-gated phishing distribution method share by month (January-June 2026)



Tycoon2FA&#8217;s continued decline was a significant factor in the overall volume reduction. The platform&#8217;s share of CAPTCHA-gated phishing fell from 41% in March to 16% in April, 18% in May, and 12% by June, down from a peak of 76% in December 2025. No single service has emerged to fill the gap at comparable scale, contributing to the sustained decline in CAPTCHA-gated phishing activity overall.



Malicious payloads



Credential phishing continued to dominate the malicious payload landscape throughout Q2, accounting for 94–96% of all payload-based attacks each month. These credential phishing payloads either linked users to phishing pages or locally loaded spoofed sign-in screens on a user&#8217;s device. Traditional malware delivery represented just 4–6% of payloads, consistent with its long-term decline.



HTML and PDF attachments remained the two most common malicious payload types across the quarter, together accounting for roughly 60–70% of all payload-based attacks each month:




HTML attachments held the top position across all three months at 35–41% of attacks. After peaking in April, HTML payload volume declined 33% in May and another 17% in June.



PDF attachments consistently ranked second at 24–31% of attacks. PDF volume was relatively stable in April before declining 41% in May and 4% in June.



SVG files continued the decline that has tracked closely with Tycoon2FA&#8217;s diminishing activity. After peaking at 23% of malicious payloads in July 2025, SVG&#8217;s share fell to around 7% by Q2, consistent with SVG&#8217;s historical role as a preferred Tycoon2FA payload format.



DOC/DOCX and ZIP/GZIP files oscillated without a clear directional trend. DOC/DOCX increased 26% in May before falling 17% in June, while ZIP/GZIP attachments declined 48% in April, rebounded 27% in May, then dropped 40% in June.



ICS files (calendar invitations), while still a small share of overall payload volume (roughly 4%), nearly quadrupled in June (+277%). These attacks take advantage of the fact that calendar invitations are processed differently than standard email attachments and can inject malicious links into a user&#8217;s calendar without requiring an explicit open-and-click interaction.



EXE files continued to decline, falling to their lowest monthly volume in June, reflecting the broader shift away from traditional malware delivery via email attachments.



Figure 6. Malicious payload file type (Q2 2026)



Business email compromise



April 2026 produced the most anomalous BEC data point in more than a year: nearly 9 million attacks, a 121% increase from March and more than double any previous month. The spike was short-lived as volume fell 62% in May to 3.4 million and settled at 3.9 million in June, both figures consistent with the monthly baseline that had held throughout the prior year. The April surge appeared to be driven by a small number of high-volume campaigns rather than a fundamental escalation in BEC activity.


Figure 7. Monthly BEC attack volume (January 2026&ndash;June 2026)



The composition of BEC attacks remained consistent throughout Q2. Generic outreach messages (like &#8220;Are you at your desk?&#8221;) accounted for 87–92% of initial contact emails each month, while explicit requests for specific financial transactions or documents represented just 3–8%. This pattern underscores that BEC operators overwhelmingly favor establishing conversational rapport with targets before making fraudulent requests, rather than leading with direct financial asks.


Figure 8. Initial BEC email content by type (Q2 2026)



Within the smaller subset of explicit financial requests, the most notable trend was the near-disappearance of fake invoice payment requests:




Invoice payment requests fell 67% in May and another 77% in June, reaching their lowest volume in more than a year. By June, invoice-themed BEC accounted for less than 0.4% of all attacks, down from around 3.6% in March.



Payroll update requests declined moderately across the quarter, from roughly 4% of attacks in March to 2.3% by June.



Gift card requests remained at roughly 1–4% of attacks, with no clear directional trend.




Microsoft Teams threats



While email remains the dominant initial access vector, threat actors increasingly abused Microsoft Teams during Q2 to deliver social engineering, phishing, and malware payloads. Unlike email, Teams traffic typically bypasses secure email gateways and benefits from the perceived legitimacy of a colleague-initiated chat, which can make lures particularly effective in this environment.



Teams-based phishing volume climbed steadily throughout Q2, with the average number of detected attacks rising 19% from March to April, holding roughly flat into May (+1%), then increasing another 10% into June. Financial and executive impersonation has remained largely absent from Teams-based attacks over the past several months.


Figure 9. Weekly observed malicious Microsoft Teams calls (January-June 2026)



The dominant lure theme remained technical support impersonation, with attackers posing as an employee&#8217;s information technology (IT) help desk, typically warning of an impending account lockout. However, the way attackers presented themselves continued to evolve:




Display names shifted away from IT- or help desk-branded identities. For the second consecutive month, more than half (52%) of Teams-based phishing attacks in June used generic display names rather than obvious IT support impersonation.



Attacker email addresses associated with these chats moved away from support-themed domains toward software-as-a-service (SaaS) terminology, scan/update language, and infrastructure keywords. This shift may align with the broader rise of ClickFix-style attacks adopting update-fix and similar themes.



Figure 10. Malicious Teams call impersonation percentage (Q2 2026)



Vishing through Teams showed the steepest growth of any threat category tracked in this report during Q2. Average weekly malicious call attempts rose 31% from April to May and another 27% into June, with the final two weeks of June recording the two highest weekly volumes on record. Since the beginning of 2026, weekly vishing attempts have increased roughly 80% and now run at nearly ten times the mid-2025 baseline. Attackers time these calls deliberately when targets are most likely to be online and active, with the heaviest activity falling between 14:00 and 20:00 UTC, Monday through Friday, with near-zero weekend activity. Notably, a growing share of these calls go unanswered, end quickly, or are rejected outright, partly reflecting Microsoft&#8217;s ongoing efforts to harden the Teams attack surface and improve protections against social engineering abuse.



Notable phishing campaigns



The following campaigns were observed during the quarter and highlight notable credential phishing, BEC, and malware delivery activity. For analysis of a separate code of conduct-themed credential phishing campaign observed in April of Q2, see Breaking the code: Multi-stage ‘code of conduct’ phishing campaign leads to AiTM token compromise.



Automated BEC campaign scales aging report and payroll diversion lures



On June 1, 2026, Microsoft Defender Research observed a high-volume BEC campaign that used automation to operate at scale. Over a send window of under three hours (14:08–16:52 UTC), the actor reached more than 67,000 users across more than 42,000 organizations, almost exclusively in the United States. Targeting spanned a broad range of industries rather than a single vertical, most notably retail and consumer goods (17%), technology and software (15%), and financial services (14%). The campaign ran two lures in succession from shared infrastructure: arequest impersonating sales executives to obtain aging report data and customer contact details, and a payroll diversion pretext impersonating the CEO or President to redirect salary payments to attacker-controlled bank accounts.


Figure 11. Timeline of campaign messages sent by minute, separated by lure theme



Delivery was fully scripted. The messages were generated programmatically using Python&#8217;s email.mime library, identifiable from its default MIME boundary format (===============[integer]==), and dispatched through the Amazon Simple Email Service (SES) API rather than a manual webmail interface, as indicated by the SES Feedback-ID and Message-ID formats. This allowed the actor to iterate through a recipient list and inject per-message variables (like spoofed executive display names, recipient addresses, and unique tracking identifiers) at volume. Messages were sent from a DomainKeys Identified Mail (DKIM)-configured Slovak domain (ecajovna[.]sk) through SES, so they passed Sender Policy Framework (SPF) and achieved DKIM alignment. Neither lure contained a malicious link or attachment; both relied on eliciting a reply to attacker-controlled mailboxes that mimicked legitimate providers (ilyff[.]com, j-gmails[.]com, x2mails[.]com).



Automation also extended to targeting and follow-up. The actor addressed generic role-based mailboxes (like &#8220;ar&#8221;, &#8220;accountsreceivable&#8221;, &#8220;hr&#8221;, &#8220;payroll&#8221;) rather than named individuals, reducing per-target effort. Each message embedded a 1&#215;1 open-tracking pixel served from an Amazon SES engagement subdomain, with per-message identifiers that let the actor confirm which recipients opened the email and prioritize follow-up against those targets. The combination of scripted message generation, API-based bulk delivery, role-based targeting, and automated engagement tracking allowed a single actor to run a personalized, financially motivated BEC operation at a scale not practical to execute manually.


Figure 12. Rendered example of aging report email used in this campaign


Figure 13. Rendered example of payroll diversion email used in this campaign



Staff update campaign with nested EML file and calendar invitation leads to BAT file dropper



Between June 14–15, 2026, Microsoft Defender Research observed a phishing campaign targeting more than 107,000 users across nearly 19,000 organizations, almost exclusively in the United States. The campaign targeted a broad range of industries rather than a single vertical, most notably financial services (17%), technology and software (14%), and retail and consumer goods (14%). Emails impersonated an internal &#8220;Internal Affairs – Financials &amp; Staff Updates&#8221; function at the recipient&#8217;s own organization, with the display name and subject line both opening with the recipient&#8217;s organization name and closing with constant trailing text. The messages were sent from a Postfix host on 9i6pokerdepot[.]com routed through Barracuda&#8217;s outbound mail service, and DKIM passed cleanly for the sending domain.


Figure 14. Rendered sample of initial campaign email



The visible email body contained minimal content. One line told the reader to download the attached file for the meeting summary, followed by a confidentiality notice. Each message carried two attachments: a nested EML posing as a Teams archive recording, and an ICS calendar invite addressed to placeholder administrative accounts at the recipient’s domain. The nested EML&#8217;s file name retained an unfilled template token ( {{DATE2}} ), indicating a per-recipient templating tool.



When opened, the EML displayed a voicemail notification with a single action button. That button pointed to Microsoft’s OAuth sign-in endpoint at login.microsoftonline[.]com, with parameters that asked for a silent sign-in attempt against an Entra application that the attacker had registered as multi-tenant.


Figure 15. Rendered sample of voicemail notification from the nested EML



Because no active sign-in session could satisfy the silent request, Microsoft’s authentication service redirected the recipient to the destination the attacker had pre-registered on the application. That destination was a path on clickup-attachments[.]com, ClickUp’s public attachment host, and served a Windows batch file named Financial_report.bat. Because the link routed through Microsoft authentication infrastructure, both recipients and URL scanners saw a login.microsoftonline[.]com link.



The batch file ran a hidden PowerShell command that pulled installer.exe from pixeldrain[.]com, saved it under the user’s Temp directory, ran it with a silent flag, and deleted the dropper on exit. Rather than stealing credentials, the campaign ultimately resulted in silent malware execution on the user’s Windows device.


Figure 16. Source code of Financial_report.bat



Mitigation and protection guidance



Microsoft recommends the following mitigations to reduce the impact of this threat. Check the recommendations card for the deployment status of monitored mitigations.




Review the recommended settings&nbsp;for Exchange Online Protection and Microsoft Defender for Office 365 to ensure your organization has established essential defenses and knows how to monitor and respond to threat activity.



Invest in user awareness training and phishing simulations.&nbsp;Attack simulation training&nbsp;in Microsoft Defender for Office 365, which also includes simulating phishing messages in Microsoft Teams, is one approach to running realistic attack scenarios in your organization.



Enable Zero-hour auto purge (ZAP)&nbsp;in Defender for Office 365 to quarantine sent mail in response to newly acquired threat intelligence and retroactively neutralize malicious phishing, spam, or malware messages that have already been delivered to mailboxes.



Responders could also manually check for and purge unwanted emails containing URLs and/or Subject fields that are similar, but not identical, to those of known bad messages.&nbsp;Investigate malicious email that was delivered in Microsoft 365&nbsp;and use&nbsp;Threat Explorer&nbsp;to find and delete phishing emails.



Turn on&nbsp;Safe Links&nbsp;and&nbsp;Safe Attachments&nbsp;in Microsoft Defender for Office 365.



Enable&nbsp;network protection&nbsp;in Microsoft Defender for Endpoint.



Encourage users to use Microsoft Edge and other web browsers that support&nbsp;Microsoft Defender SmartScreen, which identifies and blocks malicious websites, including phishing sites, scam sites, and sites that host malware.



Enable password-less authentication methods (for example, Windows Hello, FIDO keys, or Microsoft&nbsp;Authenticator) for accounts that support password-less. For accounts that still require passwords, use authenticator apps like Microsoft Authenticator for MFA.&nbsp;Refer to this article&nbsp;for the different authentication methods and features.

Conditional access policies can also be scoped to&nbsp;strengthen privileged accounts with phishing resistant MFA.





Configure&nbsp;automatic attack disruption&nbsp;in Microsoft Defender XDR. Automatic attack disruption is designed to contain attacks in progress, limit the impact on an organization&#8217;s assets, and provide more time for security teams to remediate the attack fully.




Microsoft Defender detections



Microsoft Defender customers can refer to the list of applicable detections below. Microsoft Defender coordinates detection, prevention, investigation, and response across endpoints, identities, email, apps to provide integrated protection against attacks like the threat discussed in this blog.



Microsoft Defender for Endpoint



The following alert might indicate threat activity associated with this threat. The alert, however, can be triggered by unrelated threat activity.




Suspicious activity likely indicative of a connection to an adversary-in-the-middle (AiTM) phishing site




Microsoft Defender for Office 365



The following alerts might indicate threat activity associated with this threat. These alerts, however, can be triggered by unrelated threat activity.




A potentially malicious URL click was detected



A user clicked through to a potentially malicious URL



Suspicious email sending patterns detected



Email messages containing malicious URL removed after delivery



Email messages removed after delivery



Email reported by user as malware or phish




Microsoft Security Copilot



Microsoft Security Copilot is embedded in Microsoft Defender and provides security teams with AI-powered capabilities to summarize incidents, analyze files and scripts, summarize identities, use guided responses, and generate device summaries, hunting queries, and incident reports.



Customers can also deploy AI agents, including the following Microsoft Security Copilot agents, to perform security tasks efficiently:




Threat Intelligence Briefing agent



Phishing Triage agent



Threat Hunting agent



Dynamic Threat Detection agent




Security Copilot is also available as a standalone experience where customers can perform specific security-related tasks, such as incident investigation, user analysis, and vulnerability impact assessment. In addition, Security Copilot offers developer scenarios that allow customers to build, test, publish, and integrate AI agents and plugins to meet unique security needs.



Threat intelligence reports



Microsoft Defender XDR customers can use the following Threat Analytics reports in the Defender portal (requires license for at least one Defender XDR product) to get the most up-to-date information about the threat actor, malicious activity, and techniques discussed in this blog. These reports provide intelligence, protection information, and recommended actions to prevent, mitigate, or respond to associated threats found in customer environments.



Microsoft Defender XDR threat analytics




Activity Profile: Email threat landscape, June 2026



Activity Profile: Email threat landscape, May 2026



Activity Profile: Email threat landscape, April 2026



Tool Profile: Tycoon2FA



Technique Profile: QR code phishing



Technique Profile: ClickFix technique leverages clipboard to run malicious commands



Threat Overview Profile: Business Email Compromise



Threat Overview Profile: Adversary-in-the-middle credential phishing




Microsoft Security Copilot customers can also use the Microsoft Security Copilot integration in Microsoft Defender Threat Intelligence, either in the Security Copilot standalone portal or in the embedded experience in the Microsoft Defender portal to get more information about this threat actor.



Indicators of compromise (IOCs)



IndicatorTypeDescriptionFirst seenLast seen9i6pokerdepot[.]comDomainSending domain; DKIM-signed by the operator2026-06-152026-06-15Customer.Service[@]9i6pokerdepot[.]comEmail addressCampaign sender address2026-06-152026-06-15t90141296286.p.clickup-attachments[.]comDomainClickUp attachment subdomain hosting the stage 2 BAT dropper2026-06-152026-06-15hxxps://t90141296286.p.clickup-attachments[.]com/t90141296286/fb39c3a9-3161-40ad-847b-0683e0409d6f/Financial_report.batURLStage 2 BAT dropper download URL2026-06-152026-06-15hxxps://pixeldrain[.]com/api/file/3v92oJiLURLFinal installer payload download URL2026-06-152026-06-15Re: Teams Archive Recording for {{DATE2}}.emlFile nameNested EML attachment template name; the literal {{DATE2}} indicates an unfilled per-recipient template token2026-06-152026-06-15Financial_report.batFile nameStage 2 dropper batch file delivered from the OAuth error redirect2026-06-152026-06-15ecajovna[.]skDomainDomain used to send campaign emails2026-06-012026-06-01ilyff[.]comDomainReply-to domain used to receive victim responses2026-06-012026-06-01j-gmails[.]comDomainReply-to domain used to receive victim responses2026-06-012026-06-01x2mails[.]comDomainReply-to domain used to receive victim responses2026-06-012026-06-01contact[@]ecajovna[.]skEmail addressAddress used to send campaign emails2026-06-012026-06-01mail[@]ilyff[.]comEmail addressReply-to address2026-06-012026-06-01me[@]j-gmails[.]comEmail addressReply-to address2026-06-012026-06-01me[@]x2mails[.]comEmail addressReply-to address2026-06-012026-06-01compliance-protectionoutlook[.]deDomainDomain hosting malicious campaign content2026-04-142026-04-16acceptable-use-policy-calendly[.]deDomainDomain hosting malicious campaign content2026-04-142026-04-16cocinternal[.]comDomain &nbsp;Domain hosting sender email address2026-04-142026-04-16gadellinet[.]comDomain &nbsp;Domain hosting sender email address2026-04-142026-04-16harteprn[.]comDomainDomain hosting sender email address2026-04-142026-04-16cocpostmaster[@]cocinternal[.]cmEmail addressEmail address used to send campaign emails2026-04-142026-04-16nationaladmin[@]gadellinet[.]comEmail addressEmail address used to send campaign emails2026-04-142026-04-16nationalintegrity[@]harteprn[.]comEmail addressEmail address used to send campaign emails2026-04-142026-04-16m365premiumcommunications[@]cocinternal[.]comEmail addressEmail address used to send campaign emails2026-04-142026-04-16documentviewer[@]na[.]businesshellosign[.]deEmail addressEmail address used to send campaign emails2026-04-142026-04-165DB1ECBBB2C90C51D81BDA138D4300B90EA5EB2885CCE1BD921D692214AECBC6SHA-256File hash of campaign PDF attachment2026-04-142026-04-16B5A3346082AC566B4494E6175F1CD9873B64ABE6C902DB49BD4E8088876C9EADSHA-256 &nbsp;File hash of campaign PDF attachment2026-04-142026-04-1611420D6D693BF8B19195E6B98FEDD03B9BCBC770B6988BC64CB788BFABE1A49DSHA-256 &nbsp;File hash of campaign PDF attachment2026-04-142026-04-16



Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on LinkedIn, X (formerly Twitter), and Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the Microsoft Threat Intelligence podcast.
The post Email threat landscape: Q2 2026 trends and insights appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/07/23/email-threat-landscape-q2-2026-trends-and-insights/](https://www.microsoft.com/en-us/security/blog/2026/07/23/email-threat-landscape-q2-2026-trends-and-insights/)*
