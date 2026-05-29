---
categories:
- MS
- 보안
date: '2026-05-28T15:00:00+00:00'
description: "In this article\n\t\t\n\n\t\t\n\t\t\t\n\t\t\n\t\n\t\n\t\t\n\t\t\tPre-encryptionFile\
  \ encryptionPost-encryptionDefending against The Gentlemen ransomwareMicrosoft Defender\
  \ detections and "
draft: false
original_url: https://www.microsoft.com/en-us/security/blog/2026/05/28/the-gentlemen-ransomware-dissecting-a-self-propagating-go-encryptor/
source: Microsoft Security Blog
tags:
- Elevation of privilege
- Extortion
- Human-operated ransomware
- Lateral movement
- Ransomware
- Ransomware as a service
- Storm
- Windows
title: 'The Gentlemen ransomware: Dissecting a self-propagating Go encryptor'
---

In this article
		

		
			
		
	
	
		
			Pre-encryptionFile encryptionPost-encryptionDefending against The Gentlemen ransomwareMicrosoft Defender detections and hunting guidanceIndicators of compromise		
	
	




Ransomware that combines robust encryption with rapid lateral movement significantly increases the risk and impact of an attack. The Gentlemen ransomware is a ransomware-as-a-service (RaaS) threat that is distinguished by its ability to pair its strong per-file encryption with an aggressive self-propagation capability designed to enable broad network compromise. In addition to using per-file ephemeral Curve25519 keys with XChaCha20 stream cipher, The Gentlemen ransomware attempts to spread across an environment using series of simultaneous, distinct lateral movement methods, increasing the likelihood of widespread impact once initial access is achieved.




	
		
			Understand the threat		
		
							
						Protect against ransomware and extortion activity							›
					
	




Microsoft Threat Intelligence tracks the operators behind the ransomware as Storm-2697, a financially motivated threat actor that manages the RaaS platform known as “The Gentlemen” while affiliates carry out attacks. Emerging around mid-2025, The Gentlemen initially started as a closed ransomware group then began offering its RaaS to affiliates in September 2025. More recently, The Gentlemen operators established an official partnership with BreachForums, a popular cybercriminal marketplace, to recruit affiliates including penetration testers and initial access brokers. Given that The Gentlemen is already a widely adopted RaaS platform, this partnership may lead to increased activity as the program becomes accessible to a broader pool of threat actors.



The operators behind the ransomware use double extortion tactics, encrypting data while also exfiltrating sensitive information to pressure victims through the threat of public release if the ransom is not paid. The ransomware is written in Go and obfuscated with Garble to target the Windows environment. Microsoft has observed The Gentlemen ransomware impacting organizations across education, transportation, healthcare, and financial industries in North America, South America, Europe, Africa, and Asia.



In this blog, we present a detailed analysis of the Gentlemen ransomware encryptor, including its execution flow, defense evasion behaviors, encryption design, and lateral movement techniques. This research is intended to provide defenders, incident responders, and the broader security community with a better understanding of how the threat operates, from initial argument parsing and defense evasion, through its file encryption internals, to the full lateral movement that enables it to propagate across the network. We also provide mitigation guidance, Microsoft Defender detections, hunting queries, and indicators of compromise (IOCs) to help organizations defend against this threat and similar ransomware activity.



Pre-encryption



Command-line argument processing



The ransomware operator can control The Gentlemen encryptor through command-line arguments. A password is required for execution, and optional arguments allow the operator to specify encryption scope, speed, lateral movement, and post-encryption behaviors.



The binary accepts the following arguments:



Command-line argumentDescription--password &lt;password&gt;Required access password (build-specific)--path &lt;list of paths&gt;Comma-separated list of target directories or file paths--T &lt;minutes&gt;Delay in minutes before file encryption begins--silentSilent mode. Disable renaming files, changing timestamps after encryption, and setting the desktop wallpaper--systemEncrypt files as SYSTEM, targeting only local drives--sharesEncrypt only mapped network drives and available Universal Naming Convention (UNC) shares--fullTwo-phase encryption by relaunching itself as two separate processes, one with --system for local drives and one with --shares for network shares--spread &lt;domain/user:password&gt;Enable self-propagation. Accept credentials for lateral movement. If no credential is provided, the current session token is used for lateral movement.--ultrafastEncrypt 0.3% per chunk (~0.9% total for large files)--superfastEncrypt 1% per chunk (~3% total for large files)--fast Encrypt 3% per chunk (~9% total for large files)--keepDisable self-delete after file encryption completes--wipeWipe free disk space after encryption



The --full command-line argument appears to be the intended mode of operation for comprehensive file encryption on the infected device. When this argument is provided, the malware spawns two child processes of itself: one appended with the argument --system to encrypt local volumes under a SYSTEM-privileged scheduled task, and one appended with the argument --shares to encrypt network shares. This separation ensures that the malware can reach both local drives (which might require SYSTEM privileges) and mapped network shares (which are only visible in the user&#8217;s session).


Figure 1. Encryption mode command-line arguments



The speed arguments (--fast, --superfast, --ultrafast) are mutually exclusive and control how much of each large file is encrypted. When no speed flag is specified, the default per-chunk percentage is 9%. These flags only affect files that are larger than 1 MB, and small files are fully encrypted regardless of the speed setting.



Usage prompt



When the encryptor is executed with no command-line argument, the malware prints a branded usage banner to the console.



It first executes the following PowerShell commands to render a console header:







This is followed by a detailed usage prompt provided by the malware author that documents all available flags with descriptions and examples:


Figure 2. The Gentlemen ransomware&rsquo;s usage prompt



It is worth noting that the file size percentages listed in the usage prompt refer to the total file encryption amount. Internally, the malware encrypts three separate chunks, and the per-chunk percentage used in the code is: fast=3%, superfast=1%, ultrafast=0.3%, default=9%.



Password check



Before executing its primary functionality, the malware validates the --password argument against a hardcoded value embedded within the binary. For the sample analyzed in this blog, the expected password is “9VoAvR7G”. If the provided password does not match, the malware outputs bad args and terminates execution.



This password check is a simple operator authentication mechanism, with each build containing a unique embedded password. Its purpose is to restrict execution to authorized operators and reduce the risk of accidental or unauthorized detonation if the binary is recovered or intercepted. However, because this validation relies on a static comparison, it can be easily identified and bypassed through static analysis techniques.



System encryption: Privilege escalation



When the --system argument is provided (either directly or via the --full argument), the malware creates a scheduled task to re-execute itself as SYSTEM. If a delay value is also specified through the --T argument, the scheduled execution time is adjusted accordingly.



To relaunch itself as SYSTEM, it issues the following sequence of commands:






The malware can only perform this task if it’s executed from an account with administrator privilege. It first deletes any existing task named gentlemen_system to avoid conflicts, creates a new one-time task that runs its binary under the SYSTEM account, and finally triggers that task.



This sequence ensures a clean state by first removing any existing task with the same name (gentlemen_system), creating a new scheduled task that executes the ransomware binary with SYSTEM-level privileges before finally triggering its immediate execution.



When running within this scheduled task context, the malware sets the environment variable LOCKER_BACKGROUND=1. This variable functions as an internal execution flag, indicating that the process is operating as a background encryption worker with elevated privileges, rather than as the original operator-invoked instance.



Defense evasion



Before starting file encryption, the malware executes a sequence of commands to disable defensive controls and remove potential forensic artifacts.



Disable Microsoft Defender






The PowerShell commands disable Microsoft Defender real-time monitoring to remove active protection on the infected device. The malware then adds its own executable to the Defender exclusion list to avoid detection. Finally, it excludes the entire C:\ volume from scanning, reducing the likelihood of subsequent detection during file encryption.



Delete shadow copies and event logs






To further impede recovery efforts, the malware deletes all Volume Shadow Copies using both vssadmin and wmic (Windows Management Instrumentation command-line utility). It then clears the System, Application, and Security event logs using wevtutil to remove key audit trails.



Delete forensics artifacts






These commands remove a variety of forensic artifacts, including prefetch files that track program execution, Defender diagnostic and support logs, and Remote Desktop Protocol (RDP) logs.



Additionally, the malware manually deletes PowerShell command history across all user profiles by removing the following file:






This action eliminates evidence of previously executed PowerShell commands, further reducing the visibility of execution history and threat actor activity.



Process and service termination



Process termination



The malware stops a list of running processes using the command:






The table below summarizes the different categories and processes being targeted:



CategoryTargeted processesVirtualizationvmms, vmwp, vmcompute, Docker DesktopDatabasessqlservr, sqlbrowser, SQLAGENT, sqlwriter, dbeng50, dbsnmp, mysqld, postgres, postmaster, psql, oracle, sqlceip, DBeaver, Ssms, pgAdmin3, pgAdmin4Backup and recovery softwareVeeamNFSSvc, VeeamTransportSvc, VeeamDeploymentSvc, Veeam.EndPoint.Service, Iperius, IperiusService, vsnapvss, cbVSCService11, CagService, CVMountd, cvd, cvfwd, CVODS, xfssvccon, bedbhEndpoint detection and response (EDR)vxmon, benetns, bengien, beserver, pvlsvr, avagent, avscc, EnterpriseClient, cbService, cbInterface, raw_agent_svcSAPSAP, saphostexec, saposco, sapstartsrvOffice applicationsexcel, winword, wordpad, powerpnt, visio, infopath, msaccess, mspub, onenoteEmail clientsoutlook, thunderbird, tbirdconfig, thebatWeb and application serversw3wp, isqlplussvcBrowser applicationsfirefox, steam, notepadRemote access managementTeamViewer_Service, TeamViewer, tv_w32, tv_x64, mydesktopservice, mydesktopqos, mvdesktopserviceAccounting applicationsQBIDPService, QBDBMgrN, QBCFMonitorServiceOther utilitiesencsvc, agntsvc, synctime, ocautoupds, ocomm, ocssd, DellSystemDetect



Service termination



In addition to terminating processes, the malware disables and stops a list of Windows services using the commands:






The table below summarizes the different categories and services being targeted:



CategoryTargeted servicesVirtualizationvmms, dockerDatabasesMSSQLSERVER, MSSQL*, MSSQL$SQLEXPRESS, SQLSERVERAGENT, SQLAgent$SQLEXPRESS, sql, (.)sql(.), MySQL, MariaDB, postgresql, OracleServiceORCLBackup, storage, and recovery softwareveeam, backup, vss, VeeamNFSSvc, VeeamTransportSvc, VeeamDeploymentService, BackupExecVSSProvider, BackupExecAgentAccelerator, BackupExecAgentBrowser, BackupExecJobEngine, BackupExecManagementService, BackupExecRPCService, BackupExecDiveciMediaService, AcronisAgent, YooBackup, AcrSch2Svc, VSNAPVSS, GxBlr, GxVss, GxClMgrS, GxCVD, GxClMgr, GXMMM, GxVsshWProv, GxFWD, PDVFSServiceEDRSophos, DefWatch, SavRoam, RTVscan, ccSetMgr, ccEvtMgr, CAARCUpdateSvc, stc_raw_agent, MVarmor, MVarmor64, mepocs, memtas, zhudongfangyuSAPSAP, SAPService, SAP$, SAPD$, SAPHostControl, SAPHostExecMicrosoft Exchangemsexchange, MSExchange, MSExchange$, WSBExchangeAccounting applicationsQBIDPService, QBDBMgrN, QBCFMonitorServiceOther utilitiessvc$, YooIT



Terminating these processes and services serves two primary objectives:




File access and encryption reliability: Many targeted processes/services, such as databases, Office applications, and backup agents, maintain active file locks. By forcibly terminating these processes, the ransomware ensures that locked files become accessible for encryption.



Defense and recovery disruption: By stopping backup services, endpoint protection agents, and remote access tools, the malware reduces the likelihood of real-time detection and data restoration from backups.




Collectively, these behaviors maximize encryption coverage while hindering the environment’s ability to detect, respond to, or recover from the attack.



Persistence



The encryptor can establish persistence for itself through two mechanisms: scheduled tasks and registry keys.


Figure 3. The Gentlemen ransomware&rsquo;s persistence mechanism



Scheduled tasks persistence



For establishing persistence with scheduled tasks, the malware executes the following sequence of commands:






These commands first remove any pre-existing tasks with the same names, then create two persistence mechanisms that execute automatically at system startup. The UpdateSystem task launches the payload in the SYSTEM security context, while the UpdateUser task launches it in the currently signed-in user’s context. This design increases the likelihood that the ransomware will run after reboot regardless of privilege level or sign-in state.



Registry keys persistence



For establishing persistence with the registry, the malware executes the following sequence of commands:






The GupdateS value under HKEY_LOCAL_MACHINE (HKLM) provides device-wide persistence that allows the malware to run at startup for all users, while the GupdateU value under HKEY_CURRENT_USER (HKCU) provides user-scoped persistence within the current profile. By writing to both registry hives, the malware establishes redundant autorun paths across both system-level and user-level execution contexts.



Together, the scheduled tasks and Run key modifications create layered persistence, ensuring that the encryptor is re-executed after a reboot in both privileged and user-context scenarios.



Network share traversal



When the command-line argument --shares is provided, the malware initiates network share discovery and enumeration. It begins by probing all drive letters A through Z to identify mapped network drives using the following commands:






This sequence discovers any drives that are already mapped in the current user&#8217;s session, which are then added to the encryption target list.



To further enhance visibility into the network environment, the malware enables multiple Windows network discovery services and their associated firewall rules using the following commands:






The services enabled as part of this process include:




Function Discovery Resource Publication (fdrespub): Publishes the host’s resources to the network, allowing other systems to detect it.



Function Discovery Provider Host (fdPHost): Hosts provider components responsible for discovering network resources.



Simple Service Discovery Protocol (SSDP) Discovery (SSDPSRV): Enables discovery of Universal Plug and Play (UPnP) devices.



UPnP Device Host (upnphost): Supports the hosting and management of UPnP devices.




Finally, the malware reinforces this configuration by enabling the Network Discovery firewall rule group. This redundancy ensures that firewall restrictions do not limit its network visibility, further maximizing the number of reachable targets for encryption and propagation.



Volume and directory traversal



To enumerate all available volumes on the system, the malware executes the following PowerShell command sequence:






This command queries Windows Management Instrumentation (WMI) for all mounted volumes with drive letter paths and attempts to enumerate Cluster Shared Volumes (CSVs).



Additionally, the malware performs a secondary enumeration routine by iterating through drive letters A through Z while verifying their existence on disk. This brute-force method ensures broader coverage by identifying volumes that might not be retrieved through WMI queries to maximize visibility into all potential encryption targets.



Directory exclusion list



To maintain system stability and avoid disrupting critical operating system components, the malware excludes a predefined set of directories from traversal and encryption. These directories include core Windows system paths, application directories, and locations commonly associated with security and system management:






Extension exclusion list



The ransomware also excludes a set of file extensions associated with system-critical binaries, configuration files, and executable content:






By avoiding executable files, libraries, scripts, and other system-relevant formats, the malware preserves the integrity of the operating environment. This selective encryption model is a common ransomware design pattern, ensuring that the system remains operational enough for the victim to receive instructions and facilitate ransom payment.



File name exclusion list



The specific file names below are also excluded:






The inclusion of README-GENTLEMEN.txt, the ransomware’s ransom note, prevents it from being encrypted during execution. This ensures that the ransom instructions remain accessible to the victim, which is critical for the operator’s monetization workflow.



Ransom note



During directory traversal, the malware drops a ransom note named README-GENTLEMEN.txt in each scanned directory to provide victim-facing instructions.



The note contains identifiers assigned to the victim, communication channels, and guidance on how to initiate contact with the operators.


Figure 4. Ransom note content



File encryption



File ownership



Before encrypting a file, the ransomware modifies the file ownership and access control settings to ensure it has unrestricted write access to the target. This is achieved through the following sequence of commands:






The takeown command recursively transfers ownership of the specified file or directory to the executing user, overriding existing ownership constraints. The icacls command then grants full control permissions to the Everyone security identifier (SID S-1-1-0), applying inheritance flags to propagate these permissions to all child objects. Finally, the attrib command removes the read-only attributes.



Cryptographic scheme



The Gentlemen ransomware implements a hybrid cryptographic design that combines Curve25519 elliptic-curve cryptography with the XChaCha20 stream cipher to achieve efficient and secure per-file encryption.



For each file, the malware performs the following sequence of operations:




Generates a unique ephemeral Curve25519 key pair, consisting of a randomly generated private key and its corresponding public key



Computes the Elliptic-curve Diffie–Hellman (ECDH) shared secret between the ephemeral private key and the operator’s embedded public key



Uses the resulting shared secret as the XChaCha20 key, and derives the nonce from the first 24 bytes of the ephemeral public key



Encrypts the file contents using XChaCha20 with this key and nonce combination



Appends the Base64-encoded ephemeral public key to the file footer to enable subsequent key reconstruction during decryption



Figure 5. The Gentlemen ransomware&rsquo;s file encryption mechanism



In this sample, the operator’s public key is hard-coded within the binary as a Base64-encoded value:






This design ensures that each file is encrypted with a distinct key and nonce derived from a per-file ephemeral key exchange, eliminating any possibility of key or nonce reuse across files.



During decryption, the decryptor can use the operator’s Curve25519 private key together with the stored ephemeral public key to reconstruct the ECDH shared secret and recover the XChaCha20 key. The nonce is deterministically reconstructed by extracting the first 24 bytes of the recovered ephemeral public key, making separate nonce storage unnecessary.



Overall, this approach provides strong cryptographic isolation between encrypted files while maintaining operational simplicity and efficiency for the threat actor during both encryption and decryption.



Size-based encryption



The malware uses different encryption strategies based on file size:



File sizeEncryption behavior≤ 1 MB (0x100000 bytes)The entire file content is encrypted&gt; 1 MB (0x100000 bytes)Three chunks are encrypted at distributed offsets



Small files that are less than 1MB in size are fully encrypted. This ensures that documents, configuration files, and other small but critical data are completely corrupted. For larger files such as databases, virtual disk images, archives, full encryption would be time-consuming. Instead, the malware encrypts three data chunks distributed across the file, which is sufficient to corrupt the file structure while dramatically reducing encryption time.



After encryption, each affected file is renamed with the appended extension .umc16h. This extension serves as a quick indicator of files already encrypted by the ransomware.



Large file chunking logic



For files larger than 1 MB, the malware performs partial encryption by dividing the file into three non-contiguous chunks distributed across its contents:






The first chunk begins at the start of the file, the second is positioned near the midpoint, and the third is located toward the end. This distribution ensures that even limited encryption is sufficient to corrupt the file structure while minimizing processing time.



Each chunk is encrypted in 64 KB (0x10000) blocks using XChaCha20. To maintain cryptographic separation between chunks, the malware modifies the nonce on a per-chunk basis. Specifically, the last byte of the 24-byte XChaCha20 nonce is XOR-ed with the chunk index (0, 1, or 2), and a new cipher instance is initialized for each chunk using the modified nonce. As a result, chunk 0 uses the original nonce, while subsequent chunks use deterministically altered variants.



Although all chunks for a given file share the same derived encryption key, this nonce mutation ensures that each chunk is processed under a unique keystream, preventing keystream reuse across different regions of the file.



The encryption percentage for each file is determined by the provided speed command-line arguments:



ArgumentPer-chunk percentTotal encrypted percent (3 chunks)(default)9%~27%--fast3%~9%--superfast1%~3%--ultrafast0.3%~0.9%



File footer



After encrypting each file, the malware appends a structured footer containing metadata required for identification and decryption. The footer format differs slightly depending on whether the file was fully or partially encrypted.



Small file encryption (files ≤ 1 MB):





Figure 6. Small file footer example



Large file encryption (files &gt; 1 MB):





Figure 7. Large file footer example



The footer serves three primary functions:




Key and nonce reconstruction: The Base64-encoded ephemeral public key, located after --eph--, allows the decryptor to recompute both the XChaCha20 key (using ECDH shared secret) and the nonce (first 24 bytes of the ephemeral public key).



Identification: The GENTLEMEN marker, located after --marker--, serves as a unique identifier, allowing encryptors/decryptors to quickly determine that the file has been encrypted by The Gentlemen ransomware.



Decryption mode: The optional speed flag marker (only present on large files) tells the decryptor which chunking percentage was used.




Notably, the speed marker is only present for large-file encryption. Files that are ≤ 1 MB do not include a speed marker, and its absence signals that the file was fully encrypted. This implicit encoding in the footer allows the decryptor to distinguish between full and partial encryption modes without requiring additional metadata fields.



Post-encryption



Wallpaper setup



If the --silent argument is not provided, the malware drops the following bitmap image file to %TEMP%\gentlemen.bmp and sets it as the system’s desktop wallpaper.



Figure 8. The Gentlemen ransomware’s wallpaper



This behavior serves as an immediate visual indicator of compromise, signaling to the victim that encryption has completed.



Self-propagation



The self-propagation module is the more distinctive component of The Gentlemen ransomware. When enabled with the --spread argument, it turns the malware from a single-host encryptor into a self-propagating worm that attempts to deploy its encryptor to every reachable system on the network.



The --spread argument accepts either explicit credentials in domain/user:password format for authenticated lateral movement, or an empty string to reuse the current session’s authentication token.



Placeholder legend



The executed commands in this section use the following placeholders:



PlaceholderMeaning&lt;self&gt;Host name of the infected device running the malware&lt;target&gt;Remote host discovered during network enumeration&lt;malware_path&gt;Full local path to the malware executable&lt;payload_name&gt;The malware file name&lt;ps_blob&gt;PowerShell defense evasion command executed on the remote target&lt;user&gt;Username parsed from the provided credentials&lt;pass&gt;Password parsed from the provided credentials&lt;time&gt;Current time plus two minutes, formatted as HH:MM



Phase 1: Local staging setup



The malware prepares the infected host to act as a distribution point for its binary by executing the following command sequence:






The commands copy the malware executable into C:\Temp, creates a hidden Server Message Block (SMB) share named share$ pointing to that directory, and modifies registry settings to allow anonymous access. With this setup, other systems on the network can retrieve the payload from \\&lt;self&gt;\share$, even when valid credentials are not available.



Phase 2: PsExec drop



The malware binary carries an embedded copy of PsExec and drops it to C:\Temp\psexec.exe on the infected device.



If the embedded PsExec payload cannot be extracted successfully, the malware falls back to downloading PsExec directly from Microsoft’s Sysinternals Live service using the following PowerShell command:






Phase 3: Network enumeration



After dropping PsExec, the malware attempts to enumerate and discover remote systems on the network, including workstations, servers, and domain controllers. Each discovered host becomes a candidate target for propagation.



Phase 4: PowerShell defense evasion blob



Before attempting to run the payload on a remote system, the malware executes the following PowerShell command on the remote target to weaken local defenses and make payload execution more reliable:






This command disables Microsoft Defender real-time monitoring, adds broad Defender exclusions, turns off Windows Firewall across all profiles, shares local drives, grants permissive New Technology File System (NTFS) access, enables SMB1, and loosens anonymous-access restrictions through Local Security Authority (LSA) registry settings. Together, these changes make the remote system significantly more exposed and ready for the payload deployment step.



Phase 5: Payload deployment



For each discovered remote host, the malware attempts a series of independent lateral movement techniques to execute its payload. Notably, these techniques are executed without dependency on prior success, and each method is attempted regardless of whether earlier attempts fail. This execution model of The Gentlemen’s propagation logic can significantly increase the likelihood that at least one execution path succeeds even in secured environments.



5.1: Remote file copy



The malware first stages its payload on the remote system by copying the encryptor binary over the administrative C$ share:






This operation ensures a local copy of the payload is available on the target host, allowing subsequent execution methods to reference a path that does not depend on network shares.



5.2: PsExec-based execution



If PsExec is successfully dropped or downloaded, the malware leverages it to perform a multi-stage execution sequence on the remote host.



First, the malware executes the PowerShell defense evasion payload to weaken host protections:






After a delay to allow defenses to be disabled, the malware executes the payload from the locally staged path C:\Temp under SYSTEM privileges:






After another sleep period, the malware executes the final command to run the payload with the &#8211;h flag for elevated token and &#8211;c -f to copy and force execution:






5.3: WMIC process creation



The malware uses WMI via wmic.exe to create remote processes:






The first command executes the defense evasion blob, the second runs the payload from the infected host&#8217;s SMB share, and the third runs the pre-staged copy from the target&#8217;s local C:\Temp directory.



5.4: Scheduled tasks (user)



The malware creates three scheduled tasks under the target user’s context, each running two minutes after the time when they are created:






The scheduled task DefU is set to run the defense evasion blob, UpdateGU executes the payload from the infected host’s SMB share, and UpdateGU2runs the pre-staged copy from the target&#8217;s local C:\Temp directory.



5.5: Scheduled tasks (system)



The same three tasks are repeated, running under the SYSTEM account:






By attempting both user-context and SYSTEM-context task creation, the ransomware can improve its chance of propagation across environments with different permission boundaries.



5.6: Service-based execution



The malware executes the following command sequence to create three Windows services on the target host:






Similar to the scheduled tasks, the service DefSvc is set to run the defense evasion blob, UpdateSvc executes the payload from the infected host’s SMB share, and UpdateSvc2 runs the pre-staged copy from the target&#8217;s local C:\Temp directory. These services run as SYSTEM by default, which provides another high-privilege execution path for the ransomware payload on the remote system.



5.7: Payload deployment: PowerShell remoting



Using PowerShell remoting, the malware executes commands directly on the target using Invoke-Command:






This method leverages Windows Remote Management (WinRM), providing an alternative execution channel when PsExec or WMIC are unavailable or blocked.



5.8: PowerShell WMI execution



Finally, the malware uses the PowerShell WMI class interface directly to create remote processes with the following command sequence.






This provides functionality equivalent to wmic.exe, but through a different execution path. As a result, it might succeed in environments where the WMIC binary is restricted but WMI access remains available.



Self-propagation summary



Across all techniques, the malware attempts 21 remote execution operations per target host, spanning multiple APIs, privilege levels, and execution contexts. Each method attempts to launch the payload from:




The infected host’s SMB share: \\&lt;self&gt;\share$\&lt;payload_name&gt;



The target host’s locally staged path: C:\Temp\&lt;payload_name&gt;




This redundancy is central to The Gentlemen’s propagation strategy. In secured environments where most lateral movement techniques are mitigated, a single successful execution on a single additional host is sufficient to continue the propagation.



Free space wipe



If the --wipe argument is provided, The Gentlemen ransomware performs an additional post-encryption routine to eliminate recoverable artifacts from disk.



The malware first enumerates all available volume paths on the system. For each volume, it creates a temporary file named wipefile.tmp at the root directory and determines the amount of available free space. It then writes random data to this file in 64 MB blocks until the volume is completely filled. Once the disk space has been exhausted, the temporary file is deleted.



This process effectively overwrites all unallocated disk space with random data, preventing forensic tools from recovering remnants of previously deleted files. This includes cached or temporary versions of original unencrypted data that might still reside on disk. When combined with earlier actions such as Volume Shadow Copy deletion, this behavior reduces the likelihood of data recovery without access to the threat actor’s decryption key.



Self-delete



If the --keep flag is not provided, the malware attempts to remove its executable from disk after completing encryption.



Since a running process cannot directly delete its own binary, the ransomware generates and executes a temporary batch script at &lt;malware_path&gt;.batwith the following contents:






The batch script introduces a short delay by sending three Internet Control Message Protocol (ICMP) echo requests to the local host, pausing execution long enough for the main malware process to terminate. After this delay, the script deletes the original ransomware executable before removing itself. This mechanism helps reduce on-disk artifacts and hinders post-incident forensic analysis by eliminating the ransomware binary from the compromised system.



Defending against The Gentlemen ransomware



Microsoft recommends the following mitigations to reduce the impact of this threat.




Read&nbsp;the&nbsp;human-operated ransomware threat overview&nbsp;for advice on developing a holistic security posture to prevent ransomware, including credential hygiene and hardening recommendations.&nbsp;



Turn on&nbsp;cloud-delivered protection&nbsp;in Microsoft Defender Antivirus or the equivalent for your antivirus product to cover rapidly evolving threat actor tools and techniques.&nbsp;Cloud-based machine learning protections block a huge majority of new and unknown variants.&nbsp;



Turn on&nbsp;tamper protection&nbsp;features to prevent threat actors from stopping security services. In addition to tamper protection, you can also enable and configure Microsoft Defender Antivirus always-on protection in Group Policy.&nbsp;



Enable&nbsp;controlled folder access. Controlled folder access helps protect your valuable data from malicious apps and threats, such as ransomware. Controlled folder access works by only allowing trusted apps to access protected folders. Protected folders are specified when controlled folder access is configured. Apps that&nbsp;aren&#8217;t&nbsp;included in the trusted apps list are prevented from making any changes to files inside protected folders.&nbsp;



Run&nbsp;endpoint detection and response (EDR) in block mode&nbsp;so that Microsoft Defender for Endpoint can block malicious artifacts, even when your non-Microsoft antivirus does not detect the threat or when Microsoft Defender Antivirus is running in passive mode. EDR in block mode works behind the scenes to remediate malicious artifacts that are detected post-breach.&nbsp;



Configure&nbsp;investigation and remediation&nbsp;in full automated mode to let Microsoft&nbsp;Defender for&nbsp;Endpoint take immediate action on alerts to resolve breaches, significantly reducing alert volume.&nbsp;



Configure&nbsp;automatic attack disruption&nbsp;in Microsoft Defender XDR. Automatic attack disruption is designed to&nbsp;contain&nbsp;attacks in progress, limit the impact on an organization&#8217;s assets, and&nbsp;provide more time for security teams&nbsp;to remediate the attack fully.&nbsp;



Microsoft Defender XDR customers can turn on&nbsp;attack surface reduction rules&nbsp;to prevent several of the infection vectors of this threat. These rules, which can be configured by any user, offer significant hardening against targeted attacks. In observed attacks, Microsoft customers who had the following rules turned on could mitigate the attack in the&nbsp;initial&nbsp;stages and prevent hands-on-keyboard activity:&nbsp;&nbsp;Block executable files from running unless they meet a prevalence, age, or trusted list criterion&nbsp;Block process creations originating from PSExec and WMI commands&nbsp;if&nbsp;you&#8217;re&nbsp;managing your devices with Intune or another MDM solution. This rule is incompatible with management through Microsoft Endpoint Configuration Manager.&nbsp;

Use advanced protection against ransomware






Microsoft Defender detections and hunting guidance



Microsoft Defender customers can refer to the list of applicable detections below. Microsoft Defender coordinates detection, prevention, investigation, and response across endpoints, identities, email, apps to provide integrated protection against attacks like the threat discussed in this blog.



Microsoft Defender Antivirus



Microsoft Defender Antivirus detects threat components as the following malware:




Ransom:Win64/Gentlemen




Microsoft Defender for Endpoint



The following alerts might indicate threat activity associated with this threat. These alerts, however, can be triggered by unrelated threat activity and are not monitored in the status cards provided with this report.




Ransomware-linked threat actor detected



Ransomware behavior detected in the file system



Possible ransomware activity



File backups were deleted



Potential human-operated malicious activity



Possible data exfiltration



Suspicious wallpaper change




The following alerts might indicate threat activity associated with The Gentlemen ransomware if Defender for Endpoint is set to block mode.




&#8216;Gentlemen&#8217; ransomware was detected



&#8216;Gentlemen&#8217; ransomware was prevented




Microsoft Defender for Cloud Apps



The following alert might indicate threat activity associated with this threat. This alert, however, can be triggered by unrelated threat activity and are not monitored in the status cards provided with this report.




Ransomware activity




Microsoft Security Copilot



Microsoft Security Copilot is embedded in Microsoft Defender and provides security teams with AI-powered capabilities to summarize incidents, analyze files and scripts, summarize identities, use guided responses, and generate device summaries, hunting queries, and incident reports.



Customers can also deploy AI agents, including the following Microsoft Security Copilot agents, to perform security tasks efficiently:




Threat Intelligence Briefing agent



Phishing Triage agent



Threat Hunting agent



Dynamic Threat Detection agent




Security Copilot is also available as a standalone experience where customers can perform specific security-related tasks, such as incident investigation, user analysis, and vulnerability impact assessment. In addition, Security Copilot offers developer scenarios that allow customers to build, test, publish, and integrate AI agents and plugins to meet unique security needs.



Threat intelligence reports



Microsoft Defender XDR customers can use the following threat analytics reports in the Defender portal (requires license for at least one Defender XDR product) to get the most up-to-date information about the threat actor, malicious activity, and techniques discussed in this blog. These reports provide the intelligence, protection information, and recommended actions to prevent, mitigate, or respond to associated threats found in customer environments.



Microsoft Defender XDR threat analytics




Tool profile: Gentlemen ransomware



Threat overview profile: Human-operated ransomware




Microsoft Security Copilot customers can also use the Microsoft Security Copilot integration in Microsoft Defender Threat Intelligence, either in the Security Copilot standalone portal or in the embedded experience in the Microsoft Defender portal to get more information about this threat actor.



Hunting queries



Microsoft Defender XDR



Microsoft Defender XDR customers can run the following advanced hunting queries to find related activity in their networks:



Known The Gentlemen ransomware files



Search for the file hashes associated with The Gentlemen ransomware activity identified in this report.&nbsp;



let fileHashes = dynamic(["22b38dad7da097ea03aa28d0614164cd25fafeb1383dbc15047e34c8050f6f67"]);
union
(
   DeviceFileEvents
   | where SHA256 in (fileHashes)
   | project Timestamp, DeviceId, DeviceName, FileName, InitiatingProcessFileName, FileHash = SHA256, SourceTable = "DeviceFileEvents"
),
(
   DeviceEvents
   | where SHA256 in (fileHashes)
   | project Timestamp, DeviceId, DeviceName, FileName, InitiatingProcessFileName, FileHash = 
SHA256, SourceTable = "DeviceEvents"
),
(
   DeviceImageLoadEvents
   | where SHA256 in (fileHashes)
   | project Timestamp, DeviceId, DeviceName, FileName, InitiatingProcessFileName, FileHash = SHA256, SourceTable = "DeviceImageLoadEvents"
),
(
   DeviceProcessEvents
   | where SHA256 in (fileHashes)
   | project Timestamp, DeviceId, DeviceName, FileName, InitiatingProcessFileName, FileHash = SHA256, SourceTable = "DeviceProcessEvents"
)
| order by Timestamp desc



Microsoft Sentinel



Microsoft Sentinel customers can use the TI Mapping analytics (a series of analytics all prefixed with ‘TI map’) to automatically match the malicious domain indicators mentioned in this blog post with data in their workspace. If the TI Map analytics are not currently deployed, customers can install the Threat Intelligence solution from the Microsoft Sentinel Content Hub to have the analytics rule deployed in their Sentinel workspace.



Detect web sessions IP and file hash indicators of compromise using Advanced Security Information Model (ASIM)



The following query checks IP addresses, domains, and file hash IOCs across data sources supported by ASIM web session parser:



//IP list - _Im_WebSession
let lookback = 30d;
let ioc_ip_addr = dynamic([]);
let ioc_sha_hashes =dynamic(["22b38dad7da097ea03aa28d0614164cd25fafeb1383dbc15047e34c8050f6f67"]);
_Im_WebSession(starttime=todatetime(ago(lookback)), endtime=now())
| where DstIpAddr in (ioc_ip_addr) or FileSHA256 in (ioc_sha_hashes)
| summarize imWS_mintime=min(TimeGenerated), imWS_maxtime=max(TimeGenerated),
  EventCount=count() by SrcIpAddr, DstIpAddr, Url, Dvc, EventProduct, EventVendor



Detect files hashes indicators of compromise using ASIM



The following query checks IP addresses and file hash IOCs across data sources supported by ASIM file event parser:



// file hash list - imFileEvent
let ioc_sha_hashes = dynamic(["22b38dad7da097ea03aa28d0614164cd25fafeb1383dbc15047e34c8050f6f67"]);
imFileEvent
| where SrcFileSHA256 in (ioc_sha_hashes) or
TargetFileSHA256 in (ioc_sha_hashes)
| extend AccountName = tostring(split(User, @'')[1]), 
  AccountNTDomain = tostring(split(User, @'')[0])
| extend AlgorithmType = "SHA256"



Indicators of compromise



IndicatorTypeDescription22b38dad7da097ea03aa28d0614164cd25fafeb1383dbc15047e34c8050f6f67SHA-256Gentlemen ransomware encryptor078163d5c16f64caa5a14784323fd51451b8c831c73396b967b4e35e6879937bSHA-256PsExec binaryfe1033335a045c696c900d435119d210361966e2fb5cd1ba3382608cfa2c8e68SHA-256Gentlemen wallpaper Bitmap file



Acknowledgements




License to Encrypt: The &#8220;Gentlemen&#8221; Make Their Move. Cybereason (accessed 2026-02-06)



Unmasking The Gentlemen Ransomware: Tactics, Techniques, and Procedures Revealed. TrendMicro (accessed 2026-02-06)




Learn more



For the latest security research from the Microsoft Threat Intelligence community, check out the Microsoft Threat Intelligence Blog.



To get notified about new publications and to join discussions on social media, follow us on LinkedIn, X (formerly Twitter), and Bluesky.



To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the Microsoft Threat Intelligence podcast.




The post The Gentlemen ransomware: Dissecting a self-propagating Go encryptor appeared first on Microsoft Security Blog.

---
*원문: [https://www.microsoft.com/en-us/security/blog/2026/05/28/the-gentlemen-ransomware-dissecting-a-self-propagating-go-encryptor/](https://www.microsoft.com/en-us/security/blog/2026/05/28/the-gentlemen-ransomware-dissecting-a-self-propagating-go-encryptor/)*
