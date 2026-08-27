---
categories:
- MS
- 보안
date: '2026-08-26T16:43:53+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tAI\
  \ workloads are becoming high-value control pointsCase study 1: LiteLLM gateway\
  \ compromiseCase study 2: RAGFlow comprom"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/08/26/when-ai-infrastructure-becomes-target-securing-gateways-control-points/
source: Microsoft Security Blog
tags:
- Malware
title: 'When AI infrastructure becomes the target: Securing gateways and control points'
---

In this article
		

		
			
		
	
	
		
			AI workloads are becoming high-value control pointsCase study 1: LiteLLM gateway compromiseCase study 2: RAGFlow compromiseCase study 3: Kestra compromiseMitigation and protection guidanceMITRE ATT&amp;CK techniques observedReferencesLearn more		
	
	




AI is creating a new layer of enterprise infrastructure. Gateways, retrieval platforms, orchestration services, and containerized runtimes now sit between users, applications, data, and models. These systems concentrate credentials, data access, model connectivity, and execution privileges, making them some of the most powerful components in the AI stack.



That concentration of trust is also creating new opportunities for attackers. In recent investigations, Microsoft observed activity targeting three distinct AI workloads: a LiteLLM gateway, a RAGFlow deployment, and a Kestra workflow environment. The intrusion paths varied, but the objectives were strikingly similar. Attackers sought to steal credentials, establish persistence, and monetize compromised compute resources.



The individual techniques matter, but the broader pattern matters more. Across these cases, attackers treated AI infrastructure as a control plane where credential theft, host compromise, and downstream data access can converge. As organizations continue to deploy AI systems, these platforms are becoming high value targets that deserve the same security scrutiny as other critical enterprise infrastructure.



AI workloads are becoming high-value control points



The campaign-level signal extends beyond one product. The targeted workloads served different functions, but each exposed assets that could support follow-on abuse, including model-provider keys, proxy-issued virtual keys, database connection strings, tenant configuration, workflow execution, or host compute. Post-compromise behavior varied by workload role. Defenders should inventory exposed AI management surfaces, restrict administrative access, and monitor for gateway-originated execution and secret access.



Three observed compromises across AI workloads



AI workloadObserved activityAttacker objectiveLiteLLM  Observed attacker activity: Python droppers, runtime secret harvesting, PostgreSQL collection, miner deployment, and persistence activity from the LiteLLM gateway context.Microsoft assessment: Initial access likely occurred through exploitation of the exposed LiteLLM gateway surface, consistent with the vulnerability chain involving CVE-2026-42271 and CVE-2026-48710.Credential theft, backend database access, durable host access, and compute monetization.RAGFlow Observed attacker activity: Possible SSRF-style reconnaissance followed several days later by code execution, application-path modification, and placement of a Python hook in the TenantLLM credential-configuration flow.Public research: Describes multiple RAGFlow execution paths; Microsoft does not attribute this intrusion to a specific vulnerability.Intercept newly configured LLM provider credentials and model metadata.Kestra Observed attacker activity: Workflow-origin shell execution, Docker and container-environment discovery, XMRig deployment, and follow-on data collection.Microsoft assessment: Initial access likely involved exploitation of the exposed Kestra orchestration surface, with CVE-2026-49869 providing relevant public vulnerability context.Secret discovery, container-level access, data collection, and rapid compute monetization.



Case study 1: LiteLLM gateway compromise



Framework role and affected runtime context



LiteLLM is commonly deployed as a proxy or gateway between applications and model providers. In that position, the service may hold or retrieve model-provider keys, LiteLLM master keys, virtual-key records, database connection strings, routing configuration, and tenant policy data. Command execution in the gateway runtime therefore exposed a process context close to AI routing and credential material.


Figure 1. LiteLLM gateway compromise &ndash; attack chain.



Initial access



Microsoft assesses with high confidence that initial access likely occurred through exploitation of the exposed LiteLLM gateway surface. Relevant public vulnerability paths include CVE-2026-42271, an authenticated command-execution issue in LiteLLM MCP stdio test endpoints, and the route described in public research that chains this flaw with CVE-2026-48710, a Starlette host-header validation bypass, to achieve unauthenticated remote code execution in vulnerable exposed deployments.



In this chain, CVE-2026-42271 provides the command execution capability through the MCP stdio test path, while CVE-2026-48710 can weaken the authentication boundary in affected configurations, potentially making that capability reachable without valid credentials.



In this case, initial access occurred in the context of the LiteLLM gateway process. The gateway service, rather than an unrelated system process, became the execution origin. Subsequent activity from that point is described in the observed attack chain below.


Figure 2. Process tree observed from the compromised LiteLLM gateway, showing shell and Python execution originating from the gateway service process.



Observed attack chain



Stage 1: Credential harvesting from the gateway runtime



The first observed stage was credential harvesting from the LiteLLM gateway runtime. The payload read the gateway process environment and filtered for credential-related values, including model-provider API keys, the LiteLLM master key, database connection strings, UI credentials, tokens, passwords, and other secret-like fields.


Figure 3. Credential harvesting from the gateway process environment, filtered for provider keys and connection strings.



In containerized LiteLLM deployments where the gateway runs as PID 1, /proc/1/environ exposes the environment block for the gateway process. Telemetry showed the payload reading /proc/1/environ, filtering for keywords such as master, API key, token, password, and UI-related fields, then sending collected values to attacker-controlled infrastructure.



The exfiltration logic used multiple transports in sequence, including Python urllib, curl, and wget. This provided fallback paths if one tool was unavailable or if egress controls affected one outbound method.



Stage 2: Payload delivery and masqueraded execution



The second stage moved from gateway-level command execution to payload delivery. The first delivery path launched from the compromised LiteLLM gateway process as an inline Python command. The code retrieved a masqueraded ELF binary from attacker-controlled infrastructure, staged it under a temporary path, marked it executable, and launched it with command-line arguments resembling a Linux service process.



The downloaded ELF used service-style naming and arguments to masquerade as a benign Linux daemon.



A second delivery path used a shell-stage downloader. A gateway-spawned Python command invoked a shell that used multiple download methods with short timeouts and fallback behavior, staged the retrieved content under randomized temporary paths, marked it executable, and launched it with supplied parameters. Together, these paths show redundant payload retrieval and execution from the gateway process context.


Figure 4. ELF binary retrieved and staged under the interpreter&#8217;s name python3, then launched with service-manager argumentsStage 3: Host discovery and competing-miner checks.



The third stage performed host discovery from the second-stage payload. Observed commands fingerprinted the host, checked privilege boundaries, inspected listening ports, and searched for other miner or remote-access activity on the system.


Figure 5. Host reconnaissance and competing-miner sweeps.



Relevant artifacts included a silent passwordless-sudo check, listening-port inspection, and multiple process sweeps for miner or remote-access tooling. The searches included terms associated with miners, remote shells, competing infrastructure, and previously dropped scripts.



Stage 4: Cryptomining preparation and competing-miner removal



The fourth stage prepared the host for cryptocurrency mining and removed competing persistence. Telemetry showed XMRig or XMRig-like mining components, RandomX-related tuning, and cleanup commands targeting other miners.


Figure 6. MSR module loaded for CPU tuning, followed by removal of competing miner cron entries.



Two artifacts were central. A command loaded the Linux Model-Specific Register module with write access enabled, a behavior commonly associated with RandomX/XMRig CPU tuning. Separately, a crontab rewrite removed entries associated with other miner names before writing the modified cron data back.



Stage 5: LiteLLM database access through Azure PostgreSQL



The fifth stage used the previously collected database connection string to access the LiteLLM-backed PostgreSQL data tier. This moved the activity from host-level secret exposure into application-layer data collection from the gateway backend database.


Figure 7. Discovery of PostgreSQL configuration files and native-extension paths.


Figure 8. Database access and credential collection from LiteLLM model and virtual-key tables.



Telemetry captured a self-contained python3 one-liner that read the gateway PID 1   environment, parsed DATABASE_URL, installed or imported PostgreSQL support, connected to the database, and dumped LiteLLM model and virtual-key records. The connection string referenced &lt;name&gt;.postgres.database.azure.com, indicating Azure Database for PostgreSQL.



The payload targeted LiteLLM tables including LiteLLM_ProxyModelTable and LiteLLM_VerificationToken. The collected records could include model configuration, upstream provider key material, provider endpoints, and proxy-issued virtual keys.



Output was base64-encoded and exfiltrated in small chunks to an out-of-band callback endpoint. A sibling variant posted data to a separate web endpoint that was also observed during the earlier credential-harvesting stage.



Stage 6: Persistence, command-and-control, and defence evasion



The sixth stage added persistence, command-and-control, and defence-evasion mechanisms. Observed artifacts included service-account SSH authorized-key modification, hidden-file relay execution, masqueraded service names, self-relaunch loops, and immutable-file attributes.


Figure 9. Persistence and evasion artifacts: authorized-key writes, hidden-file relay execution, immutable attributes.



The durable access artifact was an authorized_keys write under a service account. Additional artifacts included hidden-file relay execution, command-and-control relay components, masqueraded systemd service names, and relaunch paths under hidden temporary files.



Names used in relaunch paths overlapped with common Linux daemon naming patterns. Periodic out-of-band callbacks were also observed, providing network telemetry that the payload continued to execute and retained outbound connectivity.



Impact



The LiteLLM compromise produced multiple impact paths: provider credential exposure, proxy-issued key exposure, database-backed configuration access, host resource abuse, and durable service-account access. The gateway role made these impacts broader than a standard single-process application compromise.



Case study 2: RAGFlow compromise



Framework role and affected runtime context



RAGFlow supports document-processing and retrieval-augmented generation workflows and stores tenant LLM configuration. The observed execution occurred inside the RAGFlow container under the application runtime lineage. That context is important because the affected code paths process provider credentials when users add or modify LLM settings.



Initial access and compromise pattern


Figure 10. RAGflow compromise &ndash; attack chain.



Microsoft assesses with high confidence that initial access likely occurred through exploitation of the exposed RAGFlow application surface. Telemetry showed the RAGFlow server process retrieving an attacker-supplied URL through the application’s own HTTP client, resulting in an outbound Burp Collaborator callback without corresponding child-process execution. Remote code execution in the same service context followed later in the observed sequence.



Microsoft assesses with low confidence which specific vulnerability, if any, enabled that code execution. Because the relevant application code paths execute within the RAGFlow Flask service process, endpoint telemetry could not distinguish the precise execution sink. Publicly documented vulnerabilities affecting relevant RAGFlow versions include CVE-2026-45312 and CVE-2026-28797, authenticated Jinja2 server-side template injection issues in the prompt generator and Agent workflow components; CVE-2026-24770, a MinerU parser path-traversal issue that can permit arbitrary file overwrite and subsequent code execution; and CVE-2025-68700, a Canvas CodeExec sandbox-bypass issue tracked as GHSA-8xw3-v6c2-j84j.



These vulnerabilities provide plausible technical context but are not attributed as the confirmed cause of this intrusion. Depending on the affected version and deployment configuration, access to authenticated functionality could also be influenced by separate account-access weaknesses, including CVE-2025-69286. For defenders, the possible SSRF activity through the OASTify relay network is a useful precursor signal because remote code execution in the same service context followed several days later.



Observed attack chain



Stage 1: Application discovery and hook creation



The first payload stage located the RAGFlow installation from inside the container and identified the tenant LLM model-configuration path. Telemetry showed discovery logic for common application locations, followed by creation of a hidden runtime hook under the application tree.


Figure 11. First stage Python credential theft hook.



Stage 2: Persistence through application startup modification



The second stage modified the application startup or import path so the hidden hook would load with the RAGFlow service. This tied the credential-interception behavior to the application runtime rather than to a separate long-running process.


Figure 12. Exec hook created in the startup path of RAGFLow.



Stage 3: Credential interception during LLM configuration



The hook wrapped the tenant LLM configuration flow and captured newly supplied provider metadata during credential setup. Captured fields included provider type, model name, API key material, and related endpoint metadata. The collection routine used outbound HTTP from within the container and suppressed errors so the application flow could continue if collection failed.


Figure 13. Credential Stealer extracting configured API keys.



Stage 4: Finalization and installation verification



The final stage wrote or refreshed the hook and created a local marker indicating that installation had completed. Command-line telemetry was partially truncated, but the repeated execution sequence, process lineage, and application-file modifications were sufficient to reconstruct the functional behavior.


Figure 14. Exfiltration of collected data to C2.



Impact



The RAGFlow compromise was primarily focused on LLM credential collection rather than host monetization. Telemetry did not show miner deployment or an interactive reverse shell in this case. The affected runtime path could capture provider credentials configured after the hook was installed, and the startup-path modification could persist across service restarts if the modified filesystem state remained present. SSH-key material was also written inside the container, but its durability depends on container privileges, filesystem persistence, and host-container boundary configuration.



Case study 3: Kestra compromise



Framework role and affected runtime context



Kestra is a workflow orchestration environment. Because workflows are designed to execute tasks and interact with external systems, abuse of workflow-creation and execution capabilities can provide direct code execution in the worker runtime. 



Initial access and compromise pattern


Figure 15. Kestra Compromise &ndash; attack chain.



Microsoft assesses with high confidence that initial access likely occurred through exploitation of CVE-2026-49869, a critical authentication-bypass vulnerability in Kestra. Exploitation could allow an unauthenticated remote attacker with network access to bypass the login mechanism, define a malicious workflow using the Process runner, and trigger worker-side shell-script execution.



Following the assessed initial-access sequence, telemetry showed two closely timed workflow-origin shell sessions. The first produced shell initialization activity, while the second performed the main follow-on actions, including Docker socket access, container-environment enumeration, miner deployment, and defence-evasion file operations. A later workflow-origin event used a curl-pipe-shell delivery pattern to retrieve remote script content directly into a shell and store collected output through the application’s own key-value interface.



Observed attack chain



Stage 1: Workflow-origin shell execution



Telemetry showed the Kestra worker lineage spawning shell activity from the orchestration layer. Two closely timed workflow-origin shell sessions were observed; the first produced shell initialization activity, while the second performed the main follow-on actions.



Stage 2: Docker container environment discovery



After workflow-origin execution, commands accessed the mounted Docker socket from inside the compromised orchestration environment. The activity queried container metadata and inspected container environment arrays, exposing environment-backed values from other containers reachable through the mounted runtime socket.



This behavior is significant because workflow engines often run near automation secrets. Environment arrays, mounted configuration, service credentials, and container metadata may expose cloud keys, database passwords, API tokens, or internal service endpoints when the container runtime socket is accessible.


Figure 16. Container discovery performed through malicious workflow.



Stage 3: Cryptominer deployment



The monetization phase followed the workflow-origin execution chain. Telemetry showed miner retrieval from a public release source, archive extraction, binary renaming, background execution, and mining-pool communication. CPU-tuning behavior commonly associated with RandomX/XMRig mining was also observed.



Additional defence-evasion file operations were observed around a temporary path, including restrictive permissions and immutable-file attributes. These artifacts provide file-system telemetry alongside the workflow-origin process lineage and network activity.


Figure 17. Credential harvesting performed through malicious workflow.



Stage 4: Data harvesting through workflow task execution



A later workflow-origin event used a curl-pipe-shell pattern for follow-on collection. Remote script content was retrieved and executed directly by the shell without being written as a standalone script file first. The resulting output was encoded and stored through Kestra’s own key-value interface.


Figure 18. Deployment of cryptominer through malicious workflow.



Impact



The Kestra compromise exposed four impact paths: shell execution through the workflow engine, container-environment exposure through Docker socket access, host resource hijacking through miner deployment, and follow-on collection through workflow task execution. The later curl-pipe-shell event encoded collected output and stored it through Kestra’s own key-value interface, reducing reliance on standalone file artifacts.



Possible AI-assisted payload development



Several payloads exhibited characteristics often associated with assisted or generated code, including organized imports, explicit timeout handling, dependency fallbacks, formatted output, defensive exception handling, and explanatory comments. Compared with minimal, one-off shell payloads, these samples showed a more structured and robust implementation style.


Figure 19. Dropper source with structured imports, timeout handling, and non-English comments.


Figure 20. Collection routine with dependency fallback on import failure.



These characteristics are observations about the tooling, not evidence of attribution. From a security perspective, their significance is that they can improve payload portability and resilience across Linux and container environments. No conclusion about the code’s authorship or development method is required.



Key patterns observed across AI workloads



Initial access differed by workload. LiteLLM involved command execution from the gateway runtime. RAGFlow progressed from SSRF-style probing to runtime modification. Kestra used workflow execution as the shell-access path.



The observed objectives were consistent. Across the cases, telemetry showed credential collection, durable access mechanisms, and resource monetization, even though the execution path differed by product.



Payload behavior was specific to each workload. LiteLLM payloads targeted gateway environment variables and database-backed proxy records. RAGFlow activity targeted LLM credential configuration. Kestra activity focused on workflow execution, container discovery, and cryptomining.



What this means for defenders: Defenders should monitor AI workloads according to their control-plane role, not only as isolated applications. Gateway, retrieval, and orchestration services can concentrate credentials, database access, workflow execution, and container privileges in one runtime. High-value detections should therefore correlate unexpected application-origin shells or interpreters with secret access, application-file modification, Docker socket use, outbound callbacks, and resource-hijacking activity. Treating these signals as a connected compromise path can expose attacks earlier than product-specific indicators alone.



Mitigation and protection guidance



Microsoft recommends the following mitigations to help reduce the risk and impact of AI workload compromise.




Treat AI gateways as Tier-0 secrets stores. Keep LiteLLM and similar proxies patched, require authentication across API and UI surfaces, restrict administrative and management ports, and do not expose management interfaces directly to the internet.



Scope and protect provider credentials. Issue per-team virtual keys with spend limits instead of sharing master keys, store upstream API keys in a managed secret store rather than process environment variables, and rotate credentials associated with an exposed or compromised gateway.



Apply least privilege to gateway and database access. Run the proxy under a dedicated service account, limit its PostgreSQL permissions to required objects, place the database behind a private endpoint with restrictive firewall rules, and enable Microsoft Defender for Cloud monitoring for the database and surrounding cloud resources.



Constrain outbound traffic. Use deny-by-default egress rules and allowlist only required model-provider and service endpoints. Block direct connections to raw-IP hosts and non-standard ports, and route permitted traffic through an FQDN-filtering firewall or inspecting proxy.



Monitor outbound callbacks and campaign infrastructure. Filter and log DNS traffic to identify out-of-band callbacks and subdomain-encoded beacons, and monitor connections to campaign-associated C2 and OAST domains.



Harden the host runtime. Mount temporary directories as non-executable where operationally feasible, alert on execution from world-writable paths, and monitor changes to cron entries, SSH authorized_keys files, and immutable-file attributes.




Enable Microsoft Defender for Endpoint protections on Linux. Keep real-time and cloud-delivered protection enabled to detect files written to disk, newly observed droppers, miners, and second-stage payloads. Enable behavior monitoring for anomalous child processes, credential access, data staging, exfiltration, and persistence activity.



Microsoft Defender detections



Microsoft Defender coordinates detection, prevention, investigation, and response across endpoints, identities, cloud workloads, and apps to provide integrated protection against attacks on AI infrastructure like the one discussed in this blog. Given the criticality of this new attack layer, defender is providing differentiated visibility, detection and protection from attacks against AI resources. Customers with provisioned access can also use Microsoft Security Copilot in Microsoft Defender to investigate and respond to incidents, hunt for threats, and protect their organization with relevant threat intelligence.



TacticObserved activityMicrosoft Defender coverageInitial AccessExploitation of internet-exposed AI workload surfaces, including model gateway, retrieval, and workflow orchestration services reachable without network restriction.Microsoft Defender for Endpoint&#8211; Suspicious shell execution from an AI workload process&#8211; Suspicious shell execution from a scripting application runtimeCredential AccessLiteLLM: reads of /proc/1/environ and the model-config table to harvest provider API keys and the database connection string. RAGFlow: TenantLLM.insert() monkey-patched to intercept provider API keys (OpenAI, Azure, Anthropic, Gemini) on every LLM configuration event, exfiltrated to a secondary C2 endpoint.Kestra: Docker socket used to enumerate container Config.Env arrays across all running containers, collecting embedded cloud, database, and API secrets.Microsoft Defender for Endpoint&#8211; Suspicious process collected data from local system&#8211; Suspicious file copy operations Enumeration of files with sensitive dataExecution &amp; Defense EvasionLiteLLM: second-stage binary dropped to /tmp and executed under names impersonating system services and daemons. RAGFlow: base64-encoded Python payloads decoded and written to /tmp, executed sequentially to discover the RAGFlow install, inject a persistence hook, and verify implant success — fully automated with no interactive shell. Kestra: malicious workflow submitted via the pipeline API caused the Java worker to spawn a bash reverse shell; XMRig was downloaded, unpacked, and renamed to evade name-based detection.Microsoft Defender for Endpoint&#8211; Hidden file executed&#8211; Suspicious process launched from a world-writable directory&#8211; Suspicious path deletion&#8211; Suspicious file dropped and launched&#8211; Suspicious shell command execution&#8211; Suspicious piped command launched&#8211; Executable permission added to file or directory Possible reverse shell&#8211; Suspicious Python command-line execution\&#8211; Suspicious script launched&#8211; Process launched in the background&#8211; Suspicious file or information obfuscation detected&#8211; Suspicious deletion of launched process binary&#8211; Suspicious shell execution from a scripting application runtimeImpact (Resource Hijacking)LiteLLM: trojanized runtime binary maintained persistent access enabling ongoing credential and compute abuse.RAGFlow: every LLM API key configured after infection silently exfiltrated, enabling unauthorized use of provider accounts at the attacker&#8217;s direction. Kestra: XMRig v6.26.0 launched with RandomX MSR tuning toward a Monero mining pool, consuming host CPU for attacker profit.Microsoft Defender for Endpoint&#8211; Possible coin mining activity&#8211; Trojan:Linux/CoinMiner!rfnMicrosoft Defender for Cloud&#8211; Digital currency mining activityPersistenceLiteLLM: SSH key written to a service account, cron entries created, and payload directories made immutable with chattr +i to resist cleanup.RAGFlow: api/__init__.py backdoored to load a hidden hook file on every service start, surviving container restarts. SSH key planted in the container. Kestra: miner launched with nohup to survive shell exit; follow-on harvest.sh collected and stored host data through the Kestra KV API.Microsoft Defender for Endpoint&#8211; Suspicious addition of an SSH key;&#8211; Suspicious cron job creation;&#8211; Suspicious kernel module loadedCommand and ControlLiteLLM: outbound beacons to raw-IP infrastructure on port 81, sslip.io DNS rebinding to bypass reputation checks, and OAST callbacks to yosemite[.]jp, gobygo[.]net, and oast[.]me/pro/fun. RAGFlow: SSRF probing to shared scanning infrastructure in phase 1; API key exfiltration to a separate C2 endpoint in phase 2. Kestra: interactive reverse shell to a Linode VPS; sustained mining pool connections to auto.c3pool[.]org.Microsoft Defender for Endpoint&#8211; Suspicious communication with a remote target;&#8211; Suspicious file or content ingress.&#8211; Suspicious connection to cryptocurrency mining pool



Microsoft Security Copilot



Security Copilot customers can use the standalone experience to create their own prompts or run prebuilt promptbooks to automate incident response or investigation tasks related to this threat:




Incident investigation: correlate gateway process, credential-access, mining, and persistence signals into a single timeline and surface the provider keys that may have been exposed.



Microsoft user analysis: assess accounts and service principals whose credentials the gateway could have exposed.




Advanced hunting queries



Microsoft Defender XDR customers can use these Advanced hunting queries to identify behaviors associated with this intrusion across Linux workloads and AI gateway environments. Each query focuses on a specific detection objective and is designed to help analysts validate suspicious activity, pivot across related process and network telemetry, and prioritize results that combine gateway-originated execution, secret access, payload staging, persistence, or outbound communication. Tune the queries for known administrative activity and approved gateway maintenance in your environment.



When reviewing results, prioritize events where a gateway process launches a shell, downloader, interpreter, or system utility; where command lines reference /proc/1/environ, LiteLLM database tables, provider keys, or PostgreSQL libraries; and where outbound traffic reaches raw-IP infrastructure or out-of-band callback domains. Matches that combine gateway ancestry, secret-access terms, and outbound communication should be treated as higher confidence.



AI gateway process spawning shells, downloaders, or interpreters



This query looks for a LiteLLM gateway process launching execution utilities that are not expected for normal model-routing activity. In this intrusion, that relationship was the earliest high-value pivot: the gateway runtime became the parent process for shell commands, Python one-liners, downloaders, secret discovery, and follow-on payload execution.



// Low-FP pivot: AI gateway parent process spawning execution utilities.
DeviceProcessEvents
| where isnotempty(ProcessCommandLine) and isnotempty(InitiatingProcessCommandLine)
| extend ParentCmd = tolower(InitiatingProcessCommandLine), Cmd = tolower(ProcessCommandLine)
| where ParentCmd has_any ("litellm", "litellm-proxy", "litellm_proxy", "ragflow", "kestra")
| where FileName in~ ("bash", "sh", "dash", "curl", "wget", "python", "python3")
| where Cmd has_any ("/proc/1/environ", "database_url", "psycopg2", "urllib.request", "urlretrieve", "base64")
| project Timestamp, DeviceName, AccountName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId
| sort by Timestamp asc



Direct access to container environment variables



This query detects command-line access to /proc/1/environ, a high-signal behavior in containerized services where the main process often runs as PID 1. For an AI gateway, this environment can contain model-provider API keys, the gateway master key, database connection strings, UI passwords, and other secrets.



// High-signal secret access in containerized services.
DeviceProcessEvents
| where isnotempty(ProcessCommandLine)
| extend Cmd = tolower(ProcessCommandLine)
| where Cmd contains "/proc/1/environ"
| where FileName in~ ("cat", "bash", "sh", "python", "python3", "grep")
| project Timestamp, DeviceName, AccountName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId
| sort by Timestamp asc



LiteLLM-specific secret and configuration discovery



This query narrows secret-discovery hunting to LiteLLM-specific context before matching sensitive terms. That structure reduces noise from generic words such as key, token, and password, while still surfacing command lines that reference LiteLLM proxy tables, virtual keys, provider configuration, or database material.



// Hunt for command lines that combine LiteLLM context with secret-related terms.
// This helps reduce false positives from generic credential keywords.
DeviceProcessEvents
| where isnotempty(ProcessCommandLine)
| extend Cmd = tolower(ProcessCommandLine)
| where Cmd has_any (
    "litellm",
    "litellm_proxymodeltable",
    "litellm_verificationtoken",
    "proxymodeltable",
    "verificationtoken"
)
| where Cmd has_any (
    "secret",
    "token",
    "key",
    "password",
    "master",
    "database_url",
    "postgres",
    "psycopg2",
    "psycopg2-binary"
)
| project Timestamp, DeviceName, AccountName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId, FolderPath
| sort by Timestamp asc



Python-based database credential discovery



This query hunts for Python execution that references database connection material or PostgreSQL client libraries. In the observed attack chain, Python was used to parse DATABASE_URL, install or import PostgreSQL support, and access LiteLLM-backed database tables containing model configuration and virtual-key material.



// Hunt for Python activity associated with database credential discovery or use.
// Pivot from matches to parent process, network connections, and any package-install activity.
DeviceProcessEvents
| where isnotempty(ProcessCommandLine)
| extend Cmd = tolower(ProcessCommandLine)
| where Cmd has_any ("python", "python2", "python3")
| where Cmd has_any (
    "database_url",
    "postgres",
    "postgresql",
    "psycopg2",
    "psycopg2-binary"
)
| project Timestamp, DeviceName, AccountName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId, FolderPath
| sort by Timestamp asc



Shell-based secret discovery with text-processing tools



This query looks for common Linux text-processing utilities used to search environment files, application configuration, or LiteLLM-related material for secrets. It requires three signals: a discovery utility, a relevant target, and a sensitive keyword, making it more precise than broad keyword searches alone.



// Hunt for shell utilities searching for secrets in environment or configuration data.
// Higher confidence results combine a discovery tool, a relevant target, and a secret keyword.
DeviceProcessEvents
| where isnotempty(ProcessCommandLine)
| extend Cmd = tolower(ProcessCommandLine)
| where Cmd has_any ("grep", "egrep", "fgrep", "awk", "sed", "cat", "strings")
| where Cmd has_any ("litellm", "database_url", "environ")
    or Cmd contains "/proc/1/environ"
    or Cmd contains ".env"
| where Cmd has_any ("secret", "token", "key", "password", "master", "postgres")
| project Timestamp, DeviceName, AccountName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId, FolderPath
| sort by Timestamp asc



Combined high-signal secret-discovery triage



This combined query is useful for triage dashboards or incident review because it labels each result with a detection reason. Analysts can use the DetectionReason field to quickly separate direct environment access, LiteLLM-specific secret discovery, Python database credential access, and shell-based searching.



// Combined triage query using only high-confidence secret-discovery signals.
DeviceProcessEvents
| where isnotempty(ProcessCommandLine)
| extend Cmd = tolower(ProcessCommandLine)
| extend DetectionReason = case(
    Cmd contains "/proc/1/environ", "Direct access to PID 1 environment variables",
    Cmd has_any ("litellm", "litellm_proxymodeltable", "litellm_verificationtoken", "proxymodeltable", "verificationtoken") and Cmd has_any ("database_url", "postgres", "psycopg2", "master", "secret", "token", "password"), "LiteLLM-related secret or database discovery",
    FileName in~ ("python", "python2", "python3") and Cmd has_any ("database_url", "postgres", "postgresql", "psycopg2", "psycopg2-binary"), "Python-based database credential discovery",
    "")
| where DetectionReason != ""
| project Timestamp, DeviceName, AccountName, FileName, DetectionReason, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId
| sort by Timestamp asc



Second-stage payload retrieval and masqueraded execution



This query identifies the payload-delivery pattern observed after gateway execution: raw-IP retrieval, staging under /tmp, and execution with supervisord-style arguments or bridge-related environment values. Review matches for masquerading, unexpected executable files in world-writable paths, and parentage from the gateway process.



// Hunt for staged payload execution and supervisord-style masquerading.
// Focus on /tmp execution, bridge variables, and known payload path fragments.
DeviceProcessEvents
| where isnotempty(ProcessCommandLine)
| where ProcessCommandLine has_any ("/private/python3", "/anonymus/bins_s", "BRIDGE_STANDALONE", "PORT")
    or (FolderPath == "/tmp/python3" and ProcessCommandLine has "supervisord")
| project Timestamp, DeviceName, AccountName, FileName, FolderPath, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId
| sort by Timestamp asc



Crypto mining preparation through MSR write access



This query hunts for attempts to load the Linux msr kernel module with write access enabled. That behavior is strongly associated with performance tuning for RandomX/XMRig mining and is unusual on most production servers unless explicitly approved for low-level performance testing.



// Hunt for MSR write access often used to optimize RandomX/XMRig mining.
// Validate whether the host has any legitimate reason to load msr with allow_writes.
DeviceProcessEvents
| where isnotempty(ProcessCommandLine)
| where ProcessCommandLine has_all ("modprobe", "msr", "allow_writes")
| project Timestamp, DeviceName, AccountName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId
| sort by Timestamp asc



Persistence, hidden relay execution, and defense evasion



This query groups the persistence and defense-evasion behaviors observed in the intrusion: hidden-file relaunch from /tmp, cron manipulation, SSH authorized-key modification, and immutable-flag changes. These signals should be reviewed with process ancestry and file-write events to identify the account and payload responsible for durable access.



// Hunt for persistence and defense-evasion activity used to keep the payload running. // Review matches for service-account abuse, hidden /tmp execution, and cleanup resistance. DeviceProcessEvents | where isnotempty(ProcessCommandLine) | where (ProcessCommandLine contains "exec /tmp/." and ProcessCommandLine contains "-c /tmp/.")     or (ProcessCommandLine contains "crontab" and ProcessCommandLine contains "grep -v")     or ProcessCommandLine has "chattr"     or (ProcessCommandLine has "authorized_keys" and ProcessCommandLine contains ">>") | project Timestamp, DeviceName, AccountName, FileName, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessCommandLine, ProcessId, InitiatingProcessId, FolderPath | sort by Timestamp asc



Outbound communication to known campaign infrastructure



This query hunts for connections to infrastructure directly tied to the observed campaign. To reduce false positives, it focuses on known campaign domains/IPs and execution tools commonly used in the attack chain.



// Known campaign infrastructure only (low-FP network pivot).
DeviceNetworkEvents
| extend RU = tolower(RemoteUrl), RIP = tostring(RemoteIP)
| where RU has_any ("yosemite.jp", "gobygo.net", "auto.c3pool.org", "45.150.109.151.sslip.io")
    or RIP in ("45.150.109.151", "135.125.10.56", "172.232.38.92", "47.86.197.116", "2001:41d0:701:1100::adfd")
| where InitiatingProcessFileName in~ ("bash", "sh", "dash", "python", "python3", "curl", "wget", "nohup")
| project Timestamp, DeviceName, InitiatingProcessAccountName, InitiatingProcessFileName, InitiatingProcessCommandLine, RemoteUrl, RemoteIP, RemotePort
| sort by Timestamp asc



For higher-confidence triage, correlate these results across time and telemetry types. A single match may represent administrative activity, but the combination of gateway-originated execution, secret access, database-focused Python, payload staging in /tmp, MSR tuning, persistence attempts, and outbound callbacks should be investigated as a potential end-to-end compromise path.



MITRE ATT&amp;CK techniques observed



TacticTechniqueObserved activityInitial AccessT1190 Exploit Public-Facing ApplicationAbuse of the internet-exposed LiteLLM gateway runtimeExecutionT1059 Command and Scripting Interpreterpython3 -c one-liners and shell scripts launched from the gateway processCredential AccessT1552.001 Unsecured Credentials: Credentials in FilesHarvest of provider API keys from /proc/1/environ and the LiteLLM model-config tableDiscoveryT1057 Process Discovery / T1518 Software Discoverypgrep sweeps for rival miners and enumeration of PostgreSQL config filesDefense EvasionT1036.005 Masquerading / T1564.001 Hidden Files and DirectoriesPayloads named after system daemons, executed from hidden /tmp filesImpactT1496 Resource HijackingCryptomining with MSR tuning and competing-miner evictionPersistenceT1098.004 SSH Authorized Keys / T1053.003 CronService-account SSH key and cron entries for durable accessDefense EvasionT1222.002 Linux File and Directory Permissions Modificationchattr +i immutable flags on payload directories to resist cleanupCommand and ControlT1071.001 Application Layer Protocol / T1095 Non-Application Layer ProtocolHTTP beacons to raw-IP infrastructure, exfil to yosemite[.]jp, and OAST callbacks



Indicators of compromise



Network indicators



IOCTypeRole45.150.109[.]151IPv4Scanning/recon infrastructure &#8211; multiple targeted AI workloads135.125.10[.]56:19888IPv4:portRAGFlow exploitation C2 — LLM API key exfiltration endpoint172.232.38[.]92:32991IPv4:portKestra reverse shell C2 (Linode VPS)45.150.109.151.sslip[.]ioDomainDNS rebinding used in LiteLLM attacks to evade domain reputation checksauto.c3pool[.]org:443Domain:portXMRig Monero mining pool (Kestra)2001:41d0:701:1100::adfdIPv6c3pool mining endpoint (Kestra)47.86.197[.]116IPv4c3pool mining endpoint (Kestra)yosemite[.]jpDomainC2/exfiltration endpoint — LiteLLM credential harvesting (OAST + recv.php)gobygo[.]netDomainC2 beacon infrastructure — subdomain-encoded LiteLLM beaconsoast[.]me / oast[.]pro / oast[.]funDomainsOut-of-band callback domains — execution confirmation and credential exfiltration (LiteLLM)194.213.18[.]133IPv4Attacker-controlled mail MX / mail infrastructure



File indicators



File / PathSHA256Notes/tmp/d (ELF binary)f64b88e9318bdf23f2dd119a0ce1dd1bdb3c8cd2e0e1e23ba3ef2e19072b79ccLiteLLM #2 — unknown ELF; not on VirusTotalXMRig cryptominer49fdcf32bfe837899a84e8938f0d07ae96ddd218a280a09eb60df8d64597bd8fLiteLLM — XMRig binaryXMRig cryptominer3af9f25a4d45bb4f1ec5627cdbc6703cf3b4be75a892162d299d80ddfb266f42LiteLLM — XMRig binary (variant)Installer / bridge script3d24ac736635e0fa0c5c459c9e18ca09d1ec9a1751a4503130934395609bd7e0LiteLLM — drops /tmp/python3 and launches supervisord bridge



References




CVE-2026-42271: LiteLLM MCP command-execution vulnerability, NVD entry





LiteLLM: Authenticated command execution via MCP stdio test endpoints, GitHub advisory



LiteLLM: Proxy security, virtual keys, and key management documentation



Horizon3.ai: CVE-2026-42271 chained with CVE-2026-48710 unauthenticated RCE





CISA: Adds Two Known Exploited Vulnerabilities to Catalog





Microsoft: Taking legal action to protect the public from abusive AI-generated content



Microsoft: Disrupting a global cybercrime network abusing generative AI



CVE-2026-48710: Starlette host-header validation bypass, CVE record



CVE-2026-45312: RAGFlow prompt-generator server-side template injection, CVE record



CVE-2026-28797: RAGFlow Agent workflow server-side template injection, CVE record



CVE-2026-24770: RAGFlow MinerU parser path-traversal vulnerability, CVE record



CVE-2025-68700: RAGFlow Canvas CodeExec sandbox-bypass vulnerability, CVE record



GHSA-8xw3-v6c2-j84j: RAGFlow Canvas CodeExec sandbox-bypass advisory



CVE-2025-69286: RAGFlow account-access weakness, CVE record



CVE-2026-49869: Kestra authentication-bypass vulnerability, CVE record



Microsoft Learn: Create prompts in Microsoft Security Copilot




Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on LinkedIn, X (formerly Twitter), and Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.   




How Microsoft discovers and mitigates evolving attacks against AI guardrails 



Learn more about securing Copilot Studio agents with Microsoft Defender  



Evaluate your AI readiness with our latest Zero Trust for AI workshop.



Microsoft 365 Copilot AI security documentation 





The post When AI infrastructure becomes the target: Securing gateways and control points appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/08/26/when-ai-infrastructure-becomes-target-securing-gateways-control-points/](https://www.microsoft.com/en-us/security/blog/2026/08/26/when-ai-infrastructure-becomes-target-securing-gateways-control-points/)*
