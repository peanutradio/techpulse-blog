---
categories:
- MS
- 보안
date: '2026-07-13T22:02:41+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tAttack\
  \ chain overviewImproving visibility into Salesforce OAuth abuseMitigation and protection\
  \ guidanceLearn more\t\t\n\t\n\t\n"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/07/13/defending-saas-based-applications-against-shinyhunters-oauth-abuse/
source: Microsoft Security Blog
tags:
- Social engineering
- Supply chain attack
- Vishing
title: Defending SaaS-based applications against ShinyHunters OAuth abuse
---

In this article
		

		
			
		
	
	
		
			Attack chain overviewImproving visibility into Salesforce OAuth abuseMitigation and protection guidanceLearn more		
	
	




In a series of campaigns observed between mid-2025 and mid-2026, Microsoft identified threat actor activity with overlapping tradecraft commonly associated with ShinyHunters, including voice phishing (vishing), supply chain compromise, and misconfigured guest access to target customer SaaS-based applications such as Salesforce instances. The threat actors abused trusted OAuth relationships for unauthorized access, data exfiltration, and persistence.



Three primary intrusion paths were observed including vishing techniques targeting OAuth consent, supply chain compromise through trusted workflows and integrations such as Salesloft and Gainsight, and exploitation of misconfigured guest access. Abuse of these access paths led to inherited user and application privileges, allowing successful enumeration and querying of customer relationship management (CRM) records while evading conventional authentication detections. These intrusion paths often led to persistent access and exfiltration of data at scale. This tradecraft highlights how a single entry point can rapidly expand to greater enterprise impacts.



Microsoft observed activity associated with these techniques in many tenants from various industries such as retail, education and manufacturing. These findings reinforce the importance of monitoring OAuth-connected applications, validating third-party integrations, reviewing guest access configurations, and enabling Salesforce event monitoring. Leveraging this data, Microsoft consulted with Salesforce to improve granularity in telemetry for Defender for Cloud Apps with near-real-time detection, offering connected application attribution and expanded application permission insights. This activity was not the result of a vulnerability inherent to Salesforce. Rather, the threat actors abused trusted OAuth relationships for unauthorized access, data exfiltration, and persistence.



Attack chain overview



Threat actor campaigns targeting Salesforce customers and using tradecraft associated with ShinyHunters pose a high-impact risk to sensitive data and downstream SaaS ecosystems. These campaigns abuse OAuth trust relationships to operate within pre-existing, legitimate workflows.


Figure 1. Commonly observed attack paths for SaaS applications.



Observed activity can be grouped into three primary intrusion paths:



Voice‑phishing-driven OAuth consent abuse



In campaigns beginning in mid-2025, the threat actors conducted vishing attacks impersonating IT support personnel.&nbsp;Threat actors socially engineered employees into authorizing attacker-controlled connected apps within their Salesforce tenant. In several confirmed cases, threat actors guided users through the OAuth consent workflow to grant access to a malicious application disguised as a legitimate Salesforce Data Loader tool. After users granted consent, these highly privileged OAuth applications enabled threat actors to perform API calls on behalf of the victim user, facilitating:




Enumeration of Salesforce instances belonging to targeted organizations



Persistent access to Salesforce CRM data



Possible lateral movement into other SaaS platforms through discovered credentials




This intrusion path exploits the OAuth authorization flow of trusted SaaS services rather than relying on malware or credential replay. Threat actors exfiltrate data through sanctioned application access inherited from user privileges.



SaaS supply‑chain compromise targeting trusted integrations



Following initial access campaigns, threat actors&nbsp; escalated into supply‑chain-driven attacks targeting third‑party SaaS vendors offering popular solutions that integrate with Salesforce, often using OAuth tokens. In August 2025, compromised Salesloft Drift credentials enabled attackers to obtain connection secrets used by downstream SaaS applications, enabling the use of OAuth tokens in multiple customer Salesforce instances.



A subsequent campaign in November 2025 targeted Gainsight-published applications integrated with Salesforce, allowing attackers to leverage trusted external connections to maintain persistent API access in multiple Salesforce customer instances. These activities often appeared indistinguishable from legitimate integration behavior. Threat actors performed discovery, bulk data queries, and mass exfiltration of sensitive CRM records, including accounts, contacts, and service case data, without generating traditional sign-in anomalies.More recently, in June 2026, the market intelligence platform Klue experienced an incident where a threat actor, Storm-3138, gained access to its system.&nbsp; Credentials used to access Salesforce customer instances were used in the same fashion, to discover, query, and exfiltrate data.



Guest access used for exfiltration



Over recent months, Microsoft observed an increase in suspicious guest-user activity targeting Salesforce Aura endpoints across multiple organizations. In these incidents, threat actors leveraged unauthenticated access to Aura framework functionality and used GraphQL-based Aura requests to systematically query and retrieve data. While the activity did not exploit a software vulnerability, it took advantage of misconfigured guest-user permissions to gain unauthorized access to data. By chaining Aura requests and leveraging GraphQL queries, the actors were able to circumvent standard record-retrieval limitations and extract significantly larger volumes of data than would typically be accessible to guest users. All three intrusion paths relied on inheriting trusted application or user privileges, making malicious activity difficult to distinguish from normal operations. The resulting quiet persistence and large-scale data access highlight the need for stronger detection, visibility, and governance of OAuth-connected applications and guest user accounts.



Improving visibility into Salesforce OAuth abuse



For customers using Salesforce Shield: Event Monitoring, the upgraded Microsoft Defender for Cloud Apps Salesforce connector onboards the Real-Time Event Monitoring (RTEM) framework, enabling faster detection and investigation of Salesforce-based attacks.



Investigations into these campaigns exposed a recurring challenge for security teams: malicious activity often appeared indistinguishable from legitimate Salesforce usage because threat actors operated through trusted identities, approved OAuth applications, and authorized integrations. Traditional authentication-focused detections frequently provided limited visibility into the resulting application activity.



To improve investigation and detection of these scenarios, Microsoft expanded Salesforce visibility in Defender for Cloud Apps through additional event telemetry, connected application attribution, and enhanced application permissions insights. These capabilities help security teams identify suspicious OAuth activity, investigate potentially compromised integrations, and better understand how access was obtained and used within customer Salesforce instances.



Key capabilities include:




Near-real-time visibility into Salesforce security and activity events.



Connected application attribution, including application identity and granted OAuth scopes.



Expanded identity, session, and API activity context to support investigations.



Improved correlation within Microsoft Defender to help identify suspicious activity spanning identities, applications, and SaaS environments.




Together with Salesforce Shield: Event Monitoring, these capabilities help security teams investigate suspicious OAuth activity, validate the legitimacy of connected applications, and better understand the potential impact of a compromise.



New posture and governance capabilities for connected OAuth apps



While improved detection is critical, recent incidents have also highlighted the need for stronger preventive controls and ongoing governance of OAuth-connected applications. To address this, Microsoft Defender introduces new posture capabilities for connected and external client apps in Salesforce. Security teams can gain visibility into each OAuth app and its non-human identity, prioritize risk, and reduce the attack surface.



Deep visibility into app permissions and access



Microsoft Defender provides comprehensive visibility into all Salesforce-integrated connected and external client apps, including granted OAuth scopes and privileges.


Figure 2. Complete permission visibility for Salesforce connected apps and external client apps.



Highly privileged apps



Security teams often struggle to identify applications with powerful administrative or sensitive permissions. The highly privileged apps insight highlights applications that have been granted elevated scopes, enabling quick identification of apps that may pose significant risk.



Additionally, security teams can use permission-based filters to identify apps with specific high-risk scopes and validate whether such access is justified.


Figure 3. Identity inventory to identify highly privileged Salesforce apps.



Unused apps



Organizations often create applications for temporary or one-time use, but those applications are rarely removed afterward. These unused apps continue to retain permissions, creating unnecessary exposure. With the recent changes, Defender now allows security teams to identify applications that have been inactive for extended periods (for example, 90 days or more), making it easy to review and revoke access where appropriate to reduce the attack surface.


Figure 4. Identity inventory to discover unused Salesforce apps.



Risk-based prioritization of connected apps



To further streamline investigation and response, Defender introduces a comprehensive risk scoring model for connected applications. Each application is assigned a numerical risk score [0-100] based on multiple risk indicators, such as usage patterns, permission sensitivity, and behavioral signals. This allows security teams to prioritize efforts effectively and focus on applications that require immediate attention. Security teams can create custom policies based on risk thresholds to trigger alerts, actions, and notifications.


Figure 5. Use actionable insights to identify apps exceeding a defined risk threshold.



Risk score investigation



To further investigate the specific Non-Human identity risk details, the factors contributing to the risk score are available in Non-Human Identities Risk score tab.


Figure 6. Detailed risk insights explaining factors contributing to the risk score.



Mitigation and protection guidance



Microsoft&nbsp;recommends the following mitigations to reduce the impact of this threat. Check the recommendations card for the deployment status of&nbsp;monitored&nbsp;mitigations.&nbsp;&nbsp;




Microsoft Defender for Cloud Apps customers can connect their Salesforce instance to get improved visibility and threat detection capabilities.



Implement best practices as recommended by Salesforce.



Secure your Experience Cloud Guest User access



Familiarize yourself with and proactively monitor Salesforce event logs.




Microsoft Defender detections



Microsoft Defender customers can refer to the list of applicable detections including new detections powered by the upgraded Microsoft Defender for Cloud Apps Salesforce connector. Microsoft Defender coordinates detection, prevention, investigation, and response for endpoints, identities, email, and apps to provide integrated protection against attacks like the threat discussed in this blog.



Customers with provisioned access can also use Microsoft Security Copilot in Microsoft Defender to investigate and respond to incidents, hunt for threats, and protect their organization with relevant threat intelligence.



Tactic&nbsp;Observed activity&nbsp;Microsoft Defender coverage&nbsp;Initial AccessA user’s Salesforce session was hijacked and usedSalesforce detected a possibly hijacked user sessionCredential AccessA user was the target of credential stuffing activitySalesforce detected a successful credential stuffing attackLateral MovementA user with a very high risk score is signing into Salesforce via SSOSalesforce SSO sign-in by high-risk userCollection / ExfiltrationAPI-heavy access, report export, and scraping patterns; potential multi-SaaS expansion depending on victim footprint.&#8211; Possible Salesforce scraping activity &#8211; Salesforce detected a user performing anomalous API activity &#8211; Salesforce detected a user performing anomalous report activityCollection / ExfiltrationAnomalous behavior from Salesforce Connected Apps&#8211; Salesforce Connected App activity from a new IP address &#8211; Salesforce Connected App activity involving new &#8211; Salesforce entity Salesforce Connected App activity involving new endpoint(s)Collection / ExfiltrationGuest user activity associated with the AuraInspector frameworkSuspicious Salesforce Aura ActivityCollection / ExfiltrationAnomalous behavior from a guest userSalesforce detected a guest user performing anomalous activity



Threat intelligence reports&nbsp;



Microsoft customers can use the following reports in Microsoft products to get the most up-to-date information about the threat actor, malicious activity, and techniques discussed in this blog. These reports provide intelligence, protection information, and recommended actions to prevent, mitigate, or respond to associated threats found in customer Salesforce instances.




OSINT Profile: ShinyHunters Calling: Financially Motivated Data Extortion Group Targeting Enterprise Cloud Applications




Advanced hunting



NOTE:&nbsp;The sample queries let you search one week of events. To inspect events and hunt for threat actor-related indicators over a longer period, go to the&nbsp;Advanced Hunting&nbsp;page &gt;&nbsp;Query&nbsp;tab, and use the calendar dropdown to set the time range to&nbsp;Last 30 days (the maximum for raw data). 



Hunt for Salesforce connected-app activity from suspicious infrastructure



CloudAppEvents
| where Application == "Salesforce"
| where ActionType in ("ApiTotalUsage", "API Event")
| extend ConnectedAppId = tostring(
    coalesce(
        RawEventData.CONNECTED_APP_ID, // from ApiTotalUsage 
        RawEventData.ConnectedAppId // from API Event
    )
)
| where isnotempty(ConnectedAppId)
| where array_length(UncommonForUser) > 0 // at least 1 attribute is flagged as uncommon



Hunt for API activity associated with connected apps and relevant user ids



CloudAppEvents
| where Application == "Salesforce"
| where ActionType in ("ApiTotalUsage", "API Event")
| extend SalesforceUserId=coalesce(tostring(RawEventData.USER_ID), tostring(RawEventData.UserId))
| extend ConnectedAppName=tostring(RawEventData.CONNECTED_APP_NAME)  // Connected App Name is not available on the ApiEvent event
| summarize count() by AccountObjectId, AccountId, AccountDisplayName, SalesforceUserId, IPAddress, UserAgent, ConnectedAppName



Hunt for anomalous report export / large data access



CloudAppEvents
| where Application == "Salesforce"
| where ActionType  == "ReportExport"
| extend SalesforceUserId = tostring(RawEventData.USER_ID)
| summarize Events=count() by AccountObjectId, AccountId, AccountName, SalesforceUserId, IPAddress, UserAgent



Pivot from a suspicious connected app (name/id) to impacted users and actions



CloudAppEvents
| where Application == "Salesforce"
| where RawEventData has ""
| project Timestamp, AccountId, AccountDisplayName, ActionType, IPAddress, UserAgent, RawEventData
| order by Timestamp desc



Audit queries to verify what objects users are accessing



CloudAppEvents
| where Application == "Salesforce"
| where ActionType == "UniqueQuery"
| extend 
    QueryText = tostring(RawEventData.QUERY_IDENTIFIER), // Full query text
    QueryObject = extract(@"(?i)\bfrom\s+([^\s]+)", 1, tostring(RawEventData.QUERY_IDENTIFIER)), // Extract just the target object
    SalesforceUserId = tostring(RawEventData.USER_ID)
| where QueryText != "SOQL"
| project Timestamp, AccountDisplayName, SalesforceUserId, QueryObject, QueryText



Hunt for users with very high Defender risk score signing into Salesforce



let VeryRiskyUsers = IdentityInfo
| where DefenderRiskScoreNumber >= 90
| distinct AccountObjectId
CloudAppEvents
| where Application == "Salesforce"
| where ActionType has "sso" or ActionType has "saml"
| where AccountObjectId in (VeryRiskyUsers)
| project Timestamp, AccountObjectId, AccountDisplayName, ActionType, UserAgent
| order by Timestamp desc



Indicators of compromise (IOC)



Indicator&nbsp;&nbsp;Type&nbsp;&nbsp;Description&nbsp;&nbsp;138.226.246.94&nbsp;IP address&nbsp;Used by the Klue integration to call Salesforce API to perform CRM queries&nbsp;on June 11. Previously disclosed by Klue in their notification about the breach.212.86.125.24&nbsp;IP address&nbsp;213.111.148.90&nbsp;IP address&nbsp;94.154.32.160&nbsp;IP address&nbsp;103.75.11.78IP addressUsed to target the Aura framework with guest access from June 19 to 22. These IP addresses were not previously published and were discovered by Microsoft as part of a novel campaign.103.75.11.110IP address



MITRE ATT&amp;CK techniques observed



Initial Access




T1566.004 Phishing: Voice Phishing: Impersonating IT support to get victims to grant access.



T1528 Steal Application Access Token: Using stolen OAuth tokens from Salesloft and Gainsight.




Persistence




T1671 Cloud Application Integration: Leveraging Connected Apps for access to a customer Salesforce environment.




Collection




T1213.004 Data from Information Repositories: Customer Relationship Management Software: Stealing data from a customer Salesforce environment.




Exfiltration




T1567 Exfiltration Over Web Service: Usage of the fake Data Loader application to steal data.




This research is provided by Microsoft Defender Security Research, Shruti Ranjit,&nbsp;Doug Cranston, Anand Deshpande, Ronen Rafaeli, and&nbsp;with contributions from members of Microsoft Threat Intelligence.



Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the&nbsp;Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on&nbsp;LinkedIn,&nbsp;X (formerly Twitter), and&nbsp;Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the&nbsp;Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;




Microsoft 365 Copilot AI security documentation&nbsp;



How Microsoft discovers and mitigates evolving attacks against AI guardrails&nbsp;



Learn more about securing Copilot Studio agents with Microsoft Defender &nbsp;



Evaluate your AI readiness with our latest&nbsp;Zero Trust for AI workshop.

The post Defending SaaS-based applications against ShinyHunters OAuth abuse appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/07/13/defending-saas-based-applications-against-shinyhunters-oauth-abuse/](https://www.microsoft.com/en-us/security/blog/2026/07/13/defending-saas-based-applications-against-shinyhunters-oauth-abuse/)*
