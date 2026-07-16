---
categories:
- MS
- 보안
date: '2026-07-16T01:36:21+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tAttack\
  \ chain overviewHow the attack started: GitHub Actions pwn requestMitigation and\
  \ protection guidanceLearn more\t\t\n\t\n"
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/07/15/unpacking-asyncapi-npm-supply-chain-compromise-import-time-payload-delivery/
source: Microsoft Security Blog
tags:
- npm
- Supply chain attack
title: Unpacking the AsyncAPI npm supply chain compromise and import-time payload
  delivery
---

In this article
		

		
			
		
	
	
		
			Attack chain overviewHow the attack started: GitHub Actions pwn requestMitigation and protection guidanceLearn more		
	
	




On July 14, 2026, Microsoft Threat Intelligence identified a coordinated supply chain compromise of the @asyncapi npm organization, a widely used set of packages for the AsyncAPI specification and code generation. Five package versions across four package names were republished within roughly ninety minutes, each carrying the same maliciously injected loader: @asyncapi/specs (in both the 6.11.2-alpha.1 prerelease and 6.11.2 stable release), @asyncapi/generator@3.3.1, @asyncapi/generator-components@0.7.1, and @asyncapi/generator-helpers@1.1.1.



Because @asyncapi/specs is a transitive dependency of numerous AsyncAPI tooling packages, this attack affected developer workstations, CI/CD pipelines, container builds, or production services that resolved and imported the affected versions during the exposure window. Unlike the more common postinstall-hook supply-chain pattern, this campaign executes at module-load (import/require) time. When any consuming build or application imports a poisoned package, the injected block runs immediately. Because the trigger is an import rather than an install script, the common npm install &#8211;ignore-scripts mitigation does not neutralize it. The second stage decrypts and evaluates a Miasma modular runtime with active command and control (C2), persistence, and decentralized fallback channels. Although disabled in this instance, credential-harvesting, propagation, and additional high-risk modules could be enabled through persistence.



Microsoft Defender Antivirus detects and blocks malicious artifacts as Trojan:JS/MiasmStealer.SC &nbsp;and Trojan:Script/Supychain.A. Microsoft Defender for Endpoint provides behavioral coverage for the suspicious detached Node.js process spawn, IPFS retrieval, and persistence activity. Organizations should immediately remove all five affected versions, purge npm and Yarn caches, hunt for sync.js under the NodeJS masquerade directories, block outbound connections to 85.137.53[.]71 on ports 8080, 8081, and 8091, and rotate all credentials accessible from any environment that imported the compromised packages. Detailed hunting queries, indicators of compromise, and mitigation guidance are provided in the succeeding sections.



Attack chain overview


Figure 1. End-to-end attack chain from CI/CD pipeline compromise through import-time execution to IPFS second-stage fetch, with C2 infrastructure and affected packages.



The compromise originated from a pwn request against asyncapi/generator. A misconfigured GitHub Actions workflow (pull_request_target) executed attacker-controlled pull-request (PR) code, exposed the asyncapi-bot personal access token (PAT), and enabled unauthorized pushes to auto-publish branches. The legitimate GitHub Actions OpenID Connect (OIDC) release workflows then published the poisoned packages under the automated identity npm-oidc-no-reply@github[.]com, producing artifacts with valid provenance signatures built from unauthorized source commits.



The campaign progressed through six phases, shown in Figure 1:




Pipeline compromise. The attacker exploited a vulnerable GitHub Actions workflow to steal a privileged bot token.



Code injection. Heavily obfuscated loaders were inserted into one source file per package.



Staged release. An alpha prerelease was followed by a stable release 24 minutes later, with a byte-identical payload, expanding blast radius.



Delivery. Consumers pulled poisoned versions through normal npm and Yarn dependency resolution; &#8211;ignore-scripts was not effective.



Import-time execution. require() or import triggered the malicious main(), which spawned a hidden detached child process.



IPFS second-stage fetch. The child downloaded sync.js from IPFS and wrote it to an OS-specific &#8220;NodeJS&#8221; masquerade directory.




The Miasma runtime provided encrypted bootstrap, persistence, C2 communication, data return paths, and resilient discovery via Nostr, Ethereum, BitTorrent DHT, libp2p, and IPFS. Six additional capability modules (credential harvest, encrypted exfiltration, supply-chain propagation, metamorphic generation, AI-tool poisoning, and sandbox evasion) were implemented but disabled in this build.



Time (UTC)Observed event~07:10@asyncapi/generator@3.3.1, @asyncapi/generator-components@0.7.1, and @asyncapi/generator-helpers@1.1.1 republished with the injected loader.08:06:20@asyncapi/specs@6.11.2-alpha.1 published with the malicious importer prepended to index.js.08:30:09@asyncapi/specs@6.11.2 stable published with a byte-identical payload, widening downstream reach.08:49:22First observed downstream fetch of the stable 6.11.2 tarball into a Yarn cache during dependency installation.



How the attack started: GitHub Actions pwn request



The attack chain began with a malicious pull request targeting the asyncapi/generator repository&#8217;s docs-preview automation. Opened as PR #2155, it carried the attacker-controlled commit 47be388, timestamped 05:08:58 UTC on July 14. The associated Docs Preview (Netlify) workflow started at 05:11:05 UTC.. Although the PR and source fork were later removed, the workflow record remains available.



The pull request PR #2155 targeted manual-netlify-preview.yml, which combined two unsafe choices: it used pull_request_target, placing the job in the base repository&#8217;s security context, and it checked out the pull request&#8217;s untrusted head commit. The run had a broadly privileged GITHUB_TOKEN, checkout credentials persisted in the local Git configuration until post-job cleanup (the default behavior of actions/checkout), and steps that referenced repository secrets.



The submitted MDX contained code was designed to retrieve JavaScript from rentry[.]co/elzotebo999 and evaluate the response. The public log confirms that the malicious commit was processed by the privileged workflow, but it does not show whether the rentry[.]co web request succeeded or whether a credential was stolen. Later push records identify asyncapi-bot as the authenticated actor. Together, these records establish that the vulnerable workflow ran before the bot-authenticated pushes, but they do not establish how the credential was obtained.



The underlying workflow weakness had been identified before the compromise. On April 29, a proof-of-concept examined whether untrusted pull-request content could be executed in the privileged docs-preview workflow. A May 17 proposal then sought to separate untrusted build activity from steps that received repository secrets and was still under review when the incident occurred.



Trusted publishing became the delivery mechanism



Once the attacker could push commits as asyncapi-bot, there was no need to compromise npm or construct a separate publishing channel. The attacker could ride the project&#8217;s normal release path and let its trusted pipeline do the distribution. Commit 3eab3ec carries a timestamp of 06:58:42 UTC, while a surviving push-triggered workflow started at 07:05:42 UTC. Its message, “fix: test release workflow on next”, matched the release workflow&#8217;s commit-message condition. The legitimate release-with-changesets.yml workflow then published three poisoned packages at approximately 07:10 UTC.



A closely linked compromise subsequently affected asyncapi/spec-json-schemas. The malicious lineage first triggered workflows on alpha between 07:56 and 08:04 UTC. The same malicious commit was later pushed to master at approximately 08:14 UTC, followed by a child commit at 08:28 UTC. The legitimate if-nodejs-release.yml workflow published @asyncapi/specs@6.11.2-alpha.1 at 08:06 UTC and @asyncapi/specs@6.11.2 at 08:30 UTC.



All five malicious versions were published through npm trusted publishing using GitHub OIDC and carried valid provenance attestations. The attestations accurately identified the legitimate repositories, commits, and workflows that created the packages, even though the triggering commits were unauthorized.


Figure 2. Miasma runtime capabilities recovered from sync.js, including active modules and implemented-but-disabled modules.



The payload operates in multiple stages, each designed to increase evasion and ensure resilient execution. Stage 0 establishes stealth by declaring no npm lifecycle hooks. Stage 1 executes the loader at require-time and spawns a hidden child process. Stage 1b deobfuscates the IPFS fetch logic and downloads sync.js. Stage 2 decrypts the ~8.2 MB encrypted bundle through three cryptographic layers. Stage 3 initializes the full Miasma modular runtime with C2, persistence, and decentralized fallback channels.



Stage 0: No lifecycle hooks declared



The absence of lifecycle hooks is a deliberate evasion choice. Security tooling that focuses on preinstall/postinstall auditing will not flag these packages. All affected packages declared no preinstall, install, or postinstall hooks in package.json. This bypassed hook-focused scanners and left import-time execution as the real trigger path.



Stage 1: Import-time loader



The loader executes the moment any application imports the compromised module; no user action beyond dependency resolution is required. The attacker placed the same bootstrap pattern in each package&#8217;s exported entry path, so normal application startup would trigger execution automatically.




@asyncapi/specs → index.js



@asyncapi/generator → lib/templates/config/validator.js



@asyncapi/generator-helpers → src/utils.js



@asyncapi/generator-components → lib/utils/ErrorHandling.js




spawn('node', [payloadPath], {
   detached: true,
   stdio: 'ignore',
   windowsHide: true,
 }).unref();



Stage 1b: IPFS fetch



The inner payload reveals hard-coded IPFS content identifiers and OS-aware drop logic. This intermediate stage reconstructs the transport routine at runtime, so the larger second stage never appears in cleartext in the published package.



Package setIPFS CIDspecsQmet4fhsAaWMBUxNDfREHwgiyDeSWy4YSYs9wiKUW5jGyfgenerator-familyQmQobZSp1wRPrpSEQ56qnyq7ecZh5Bg5k1fnjt4SUwwHb9



const FILE_URL = 'hxxps://ipfs[.]io/ipfs/';
 const FILE_NAME = 'sync.js';
 function getTargetDirectory() {
   if (process.platform === 'win32') return '%LOCALAPPDATA%\NodeJS';
   if (process.platform === 'darwin') return '~/Library/Application Support/NodeJS';
   if (process.platform === 'linux') return '~/.local/share/NodeJS';
   return '~/.config/NodeJS';
 }



Stage 2: Encrypted payload (sync.js)



Despite appearing cryptographically sophisticated, the entire decryption chain uses static embedded key material, meaning the runtime can be recovered offline without execution. The layered design primarily increases analyst effort; every secret required to unwrap the bundle ships inside the loader.




sync.js is ~8.2 MB; all key material is static and embedded.



HKDF-SHA256 uses master string rt-vault-master-key-32b-aaaaaaaa and info string rt-file-key.



AES-256-GCM uses IV = first 12 bytes and auth tag = last 16 bytes of the blob.



The decrypted string is ROT-94de-rotated and then executed with eval().




Stage 3: Miasma runtime



The  runtime is a command framework identified as M-RED-TEAM v6.4 with campaign configuration miasma-train-p1. In this build’s configuration, persistence and C2 are active, but data collection and propagation modules remain dormant. The runtime supports traditional remote access trojan (RAT) commands, including directory listing, file retrieval, file upload, remote shell execution, proxying, and data exfiltration. Persistence is installed through platform-specific mechanisms: a Windows HKCU Run key (miasma-monitor), a Linux systemd user unit (miasma-monitor.service), and macOS shell RC injection (.zshrc, .bashrc, or .bash_profile).




Recovered identifiers: M-RED-TEAM v6.4, miasma-train-p1, and miasma-test-org.



Persistence: Win HKCU Run value miasma-monitor, Linux miasma-monitor.service, and macOS user-space shell/launch persistence.



Primary endpoints: 85.137.53[.]71:8080 (C2), 85.137.53[.]71:8081 (upload), 85.137.53[.]71:8091 (management).



Fallback channels include Nostr, Ethereum, BitTorrent DHT, libp2p, and IPFS.



Disabled in the analyzed build: recon, propagation, AI-poisoning, metamorphic generation, and evasion.




Credential harvesting (disabled in this build)



The framework contains broad credential-access code targeting secrets across major platforms that a developer or continuous integration and continuous delivery (CI/CD) system might access, including browser-saved passwords from multiple browsers.



The framework targets over 100 environment variable names across source control (GITHUB_TOKEN, GITLAB_TOKEN), npm (NPM_TOKEN, NODE_AUTH_TOKEN), AWS (AWS_ACCESS_KEY, AWS_SECRET_ACCESS_KEY), Azure (AZURE_CLIENT_SECRET), GCP (GCLOUD_SERVICE_KEY), container/Kubernetes (DOCKER_TOKEN, K8S_AUTH_TOKEN), secrets managers (DOPPLER_TOKEN, VAULT_TOKEN), and AI platforms (ANTHROPIC_API_KEY, OPENAI_API_KEY).



Credential files targeted from disk include .npmrc (npm tokens), .aws/credentials (AWS keys), kubeconfig (Kubernetes API), id_rsa/id_ed25519 (SSH keys), .vault-token (HashiCorp Vault), .netrc (Git/HTTPS auth), .docker/config.json (Docker registry), and google_credentials.json (GCP service accounts). When a GITHUB_TOKEN is available, the framework can enumerate accessible repositories and CI/CD context through GitHub APIs.



Mitigation and protection guidance



Review dependency trees, lockfiles, artifact repositories, and CI caches for the five compromised versions, including transitive references.



Pin known-good versions: @asyncapi/specs 6.11.1 or earlier, @asyncapi/generator 3.3.0, @asyncapi/generator-components 0.7.0, and @asyncapi/generator-helpers 1.1.0.



Do not rely on npm install &#8211;ignore-scripts as a mitigation; this campaign executes when the module is imported, not through a lifecycle hook.



Purge npm and yarn caches on affected developer endpoints and build hosts, especially if the compromised tarballs were written into shared CI caches.



Hunt for sync.js and the NodeJS masquerade directory on endpoints, and investigate any detached Node.js execution that references the IPFS CID or the sync.js file name.



Block or alert on retrieval of the specific IPFS CID and monitor for network connections to 85.137.53[.]71 on ports 8080, 8081, and 8091.



Rotate credentials and secrets from a clean host if a build system or workstation imported a compromised version, because second-stage execution can expose tokens and build integrity.



Ensure that Microsoft Defender Antivirus cloud-delivered protection, Microsoft Defender for Endpoint telemetry, and Microsoft Defender XDR investigation workflows are enabled across developer and CI assets.



Update to NPM CLI to npm CLI v11.10.0+ or Use the NPM CLI min-release-age feature.



Organizations that do not rely on IPFS for business operations can reduce their attack surface by blocking public IPFS gateways (ipfs.io, dweb.link, cloudflare-ipfs.com, and others) at the network perimeter. This proactive measure removes an increasingly common payload delivery channel used in supply chain campaigns without affecting standard development workflows.



Organizations that produce software artifacts should also review their own release hardening. Because this incident appears consistent with CI/CD pipeline abuse through GitHub Actions OIDC publishing, defenders should review token scopes, workflow approvals, protected environments, release provenance, and anomaly detection around automated package publication. Supply chain response cannot stop at host triage; it must also include verification that the release process itself has not been subverted.



After remediation, validate recovery deliberately. Rebuild affected projects from a known-good dependency baseline, confirm that compromised hashes are absent from package caches and artifact stores, and review endpoint telemetry for any lingering sync.js, NodeJS directory artifacts, or suspicious node child processes. For development organizations that share base images or golden build runners, rebuild those images as well so future jobs do not silently inherit poisoned caches or post-compromise persistence.



Indicators of compromise



PackageVersionInjected fileTarball SHA-256@asyncapi/specs6.11.2-alpha.1index.jsd425e4583cc6185d41e95c45eda00550045a5d1919b9a012236a4520d009dbd7@asyncapi/specs6.11.2index.js9b2e65db653ca8575c9b10eefb9a80c6006404812c2ec212bf5675e3c690233b@asyncapi/generator3.3.1lib/templates/config/validator.jsbfaeb987faa6de2b5a5eb63b1233d055215b09b0349a9394f2175fd7cdf385e4@asyncapi/generator-components0.7.1lib/utils/ErrorHandling.js082d733db0687dcd768104972b065d4b58cb1e6043688c6c20fa3702337f36ab@asyncapi/generator-helpers1.1.1src/utils.js34014776d3d3ff11bc4439b02fd7ac0f02a887eb3a052eeafff236e2f6db8ad1



TypeIndicatorPublisher identitynpm-oidc-no-reply@github[.]comIPFS URLhxxps://ipfs[.]io/ipfs/Qmet4fhsAaWMBUxNDfREHwgiyDeSWy4YSYs9wiKUW5jGyfIPFS CIDQmet4fhsAaWMBUxNDfREHwgiyDeSWy4YSYs9wiKUW5jGyf@asyncapi/generator lib/templates/config/validator.jsb9993a8ad0518849416798cf29668256ccb96598fc4423501ccab5312812653a@asyncapi/generator-components lib/utils/ErrorHandling.jsb270bdf8e2274ea1af0a6eed74d8f10e5fe61012d6cc226a43cc7cc7fd9f6292@asyncapi/specs index.js (alpha AND stable — identical)8351d251cf0b5a0bd82242deaa0a14e3e1394418d55c0f4259dac4303b79fc0c@asyncapi/generator-helpers src/utils.js6e78713b75bd34828d49896176627f7face7aa9036cd874f2e02d9f23a9a9c71Wrapper – sync.js (generator-family IPFS object)24b9ee242f21a73b55f7bb3297eafb33c60840907386b542ed79fc6b72365168Central C285.137.53[.]71:8080Upload service85.137.53[.]71:8081Management configuration85.137.53[.]71:8091Windows drop path%LOCALAPPDATA%\NodeJS\sync.jsLinux drop path~/.local/share/NodeJS/sync.jsmacOS drop path~/Library/Application Support/NodeJS/sync.jsFallback drop path~/.config/NodeJS/sync.jsRuntime lock file~/.config/.miasma/run/node.lockmDNS service_miasma._tcpHTTP path examples/api/v1/beacon, /api/v1/file-result, /api/v1/file-content/&lt;cid&gt;



Microsoft Defender XDR detections



Microsoft Defender XDR customers can refer to the list of applicable detections below. Microsoft Defender XDR coordinates detection, prevention, investigation, and response across endpoints, identities, email, and apps to provide integrated protection against attacks like the threat discussed in this blog.



TacticObserved activityMicrosoft Defender coverageInitial access / ExecutionCompromised packages published though GitHub Actions OIDC trusted publishingMicrosoft Defender Antivirus &#8211; Trojan:Script/Supychain.A &#8211; Trojan:JS/MiasmStealer.SC &#8211; Trojan:JS/SpawnLoader.MKV!MTB Microsoft Defender for Endpoint – Suspicious Node.js process behavior – Suspicious Node.js script executionExecution / Defense evasionModule import triggers obfuscated main(), which spawns a hidden detached nodeMicrosoft Defender Antivirus &#8211; Trojan:JS/VaultLoader.MJZ!MTB Microsoft Defender for Endpoint  – Suspicious Node.js process behavior – Suspicious Node.js script executionPersistenceOS-specific persistence installed Microsoft Defender for Endpoint  &#8211; Anomaly detected in ASEP registry &#8211; Suspicious modification of shell profile &#8211; Suspicious Linux service created



Advanced hunting queries



Microsoft Defender XDR customers can run the following advanced hunting queries to find related activity in their networks:



Persistence drop and detached spawn



// Query 1: sync.js dropped under a NodeJS directory or related detached execution
 union isfuzzy=true
 (
 DeviceProcessEvents
 | where Timestamp > ago(30d)
 | where (ProcessCommandLine has "sync.js" and ProcessCommandLine contains_cs "NodeJS")
     or ProcessCommandLine has "Qmet4fhsAaWMBUxNDfREHwgiyDeSWy4YSYs9wiKUW5jGyf"
 | project Timestamp, DeviceName, Evidence = ProcessCommandLine, Initiator = InitiatingProcessCommandLine, EventType = "Process"
 ),
 (
 DeviceFileEvents
 | where Timestamp > ago(30d)
 | where FileName == "sync.js" and FolderPath contains_cs "NodeJS"
 | project Timestamp, DeviceName, Evidence = strcat(FolderPath, "\\", FileName), Initiator = InitiatingProcessFileName, EventType = "File"
 )



IPFS CID retrieval



// Query 2: outbound retrieval of the IPFS second stage
DeviceNetworkEvents
| where Timestamp > ago(30d)
| where RemoteUrl has "ipfs.io"
| where RemoteUrl has "Qmet4fhsAaWMBUxNDfREHwgiyDeSWy4YSYs9wiKUW5jGyf"
| project Timestamp, DeviceName, RemoteUrl, RemoteIP, InitiatingProcessFileName



Poisoned package artifacts in caches



// Query 3: presence of a poisoned tarball in caches
DeviceFileEvents
| where Timestamp > ago(30d)
| where SHA256 in (
    "d425e4583cc6185d41e95c45eda00550045a5d1919b9a012236a4520d009dbd7",
    "9b2e65db653ca8575c9b10eefb9a80c6006404812c2ec212bf5675e3c690233b",
    "bfaeb987faa6de2b5a5eb63b1233d055215b09b0349a9394f2175fd7cdf385e4",
    "082d733db0687dcd768104972b065d4b58cb1e6043688c6c20fa3702337f36ab",
    "34014776d3d3ff11bc4439b02fd7ac0f02a887eb3a052eeafff236e2f6db8ad1")
| project Timestamp, DeviceName, FolderPath, FileName, InitiatingProcessFileName



Suspicious Node.js execution



DeviceProcessEvents
| where Timestamp > ago(3d) 
| where FileName in~ ("node", "node.exe")
| where ProcessCommandLine has "node.exe -e \"const _0x5af5e1" or ProcessCommandLine has "node -e \"const _0x5af5e1"
| project Timestamp, DeviceName, FileName, FolderPath, ProcessCommandLine, InitiatingProcessFileName, InitiatingProcessFolderPath



Microsoft Security Copilot



Security Copilot customers can use the standalone experience to create their own prompts or run prebuilt promptbooks to automate investigation and response tasks related to this threat. Useful promptbooks for this activity include Incident investigation, Microsoft User analysis, Threat actor profile, Threat Intelligence 360 report based on MDTI intelligence, and Vulnerability impact assessment. Some promptbooks require access to Microsoft Defender XDR, Microsoft Sentinel, or related Microsoft security plugins.



For this campaign, Security Copilot can help analysts summarize affected devices, pivot from the package hashes to endpoint evidence, identify hosts that communicated with the IPFS path or C2 infrastructure, and build remediation actions such as cache purge, credential rotation, and containment sequencing for impacted developer systems and build runners.



Threat intelligence reports



Microsoft customers can use Microsoft Defender XDR Threat analytics and related Microsoft threat intelligence reporting to stay current on the malicious activity, indicators, detection coverage, and recommended response actions associated with this compromise. These reports provide investigation context, protection guidance, and updated intelligence that security teams can use to prevent, mitigate, or respond to related activity in customer environments.



As with other active supply-chain investigations, defenders should monitor for updated intelligence on package status, additional affected versions, infrastructure changes, and newly surfaced post-compromise tradecraft. Microsoft will continue to incorporate validated indicators and detections into Microsoft security products as the investigation evolves.



Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the&nbsp;Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on&nbsp;LinkedIn,&nbsp;X (formerly Twitter), and&nbsp;Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the&nbsp;Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;




Microsoft 365 Copilot AI security documentation 



How Microsoft discovers and mitigates evolving attacks against AI guardrails 



Learn more about securing Copilot Studio agents with Microsoft Defender  



Evaluate your AI readiness with our latest Zero Trust for AI workshop.

The post Unpacking the AsyncAPI npm supply chain compromise and import-time payload delivery appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/07/15/unpacking-asyncapi-npm-supply-chain-compromise-import-time-payload-delivery/](https://www.microsoft.com/en-us/security/blog/2026/07/15/unpacking-asyncapi-npm-supply-chain-compromise-import-time-payload-delivery/)*
