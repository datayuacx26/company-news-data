---
schema_version: "1.0.0"
document_id: "81623b33fdc7bddd2aa68b0d5c35899f822d6117be56468b8efd3cc3da43c24e"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-news-import-29d04d5fbb0d"
canonical_url: "https://www.varonis.com/blog/varonis-prevents-ransomware-disaster"
published_at: "2025-10-09T14:00:26+00:00"
first_seen_at: "2026-07-26T04:13:19.356665+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:0e0a53467a70b40126631968fc092cb30453d5ad87a372acc7ca2621f2d9e207"
---

# From CPU Spikes to Defense: How Varonis Prevented a Ransomware Disaster

Varonis recently helped a customer who observed a spike in CPU activity on a server in their environment, where a shallow review of the device revealed an in-progress compromise by an advanced threat actor we later attributed to[RansomHub](https://www.varonis.com/blog/ransomhub?hsLang=en) affiliates.


Over the next 48 hours, our team worked closely with the customer to investigate, hunt, contain, and remediate the threat before it could become ransomware. Due to our team's advanced intervention capabilities, we secured the customer’s network with zero business downtime. Continue reading to see how the incident started.


## **Initial Access**


The incident started when a user downloaded and subsequently executed a file that they were led to believe was a legitimate browser update. In this case, it was a malicious JavaScript payload.


Initial execution of the malicious payload from a user click


Initial execution of the malicious payload from a user click


The download kicked off a chain of automated reconnaissance and initial command and control activity, including enumerating[Active Directory](https://www.varonis.com/coverage/active-directory?hsLang=en) users and computers, querying key local system information, hunting for credentials in memory, and various other discovery techniques.


Within minutes, second-stage malware was deployed as a recurring Scheduled Task for persistence. Following this, a legitimate Python distribution was downloaded to % LOCALAPPDATA%ConnectedDevicesPlatform, along with an encrypted Python script that served as a SOCKS proxy with attacker infrastructure, exposing the corporate network directly over the Internet.


Downloading a legitimate Python distribution to %LOCALAPPDATA%\\ConnectedDevicesPlatform


Installing additional Python packages from the Python distribution located at %LOCALAPPDATA%\\ConnectedDevicesPlatform


Installation of a Scheduled Task as a Persistence mechanism on a compromised endpoint


The encrypted script was protected by an unpacking routine consisting of 10 layers of multi-stage encryption, with each stage unpacking the next along with other components.


Each level of the decryption process had randomized variable names designed to thwart unpacking attempts. Additionally, each stage attempted to implement basic anti-analysis techniques, including VM detection, Debug detection, and Process Tracing detection.


Snippet of packed Python malware designed to run silently and unpack itself multiple times


[Our team wrote an unpacking routine for this specific variant](https://github.com/joeavanzato/socgholish_c2_unpacker) to assist us in retrieving the final payload in plaintext. If you are a Varonis customer and need help with this task, please[contact our IR team](https://www.varonis.com/company/contact?hsLang=en) .


The final payload was a SOCKS proxy designed to facilitate communication between attacker endpoints and internal network infrastructure using the compromised host as a transport pivot.


Additionally, our forensics team noted that this TA was manipulating all email signatures stored at $env:APPDATA\\Microsoft\\Signatures by embedding a malicious image reference at the end. This type of change would go unnoticed by end users. Still, it could potentially be used on vulnerable clients to coerce[an NTLM authentication attempt and result in additional credential harvesting.](https://www.morphisec.com/blog/5-ntlm-vulnerabilities-unpatched-privilege-escalation-threats-in-microsoft/#:~:text=Imagine%20receiving%20an%20email%20from%20an%20untrusted)


Manipulation of Email Signatures


Manipulation of Email Signatures


## **Discovery**


After initially compromising the endpoint, the adversary immediately began hunting for credentials and privilege escalation opportunities in the customer’s network. This included scanning network shares for credential-containing material such as RDP/. As shown below, OVPN files, KeePass Vaults, and other extensions or file names commonly contain authentication data.


PowerShell launched by the TA hunting for files commonly containing credentials


PowerShell launched by the TA hunting for files commonly containing credentials


The above command was launched against all network shares attached to the device as part of an automatic recon stage by the malware. In addition, the threat attempted to identify credentials stored in browsers by analyzing local state databases for Chrome and Edge. The snippet below demonstrates their attempts to use Data Protection API (DPAPI) to decrypt passwords stored in the browsers from files, including:


- $env:LOCALAPPDATA\\(Google|Microsoft)\\(Chrome|Edge)\\User Data\\Default\\Login Data
- $env:LOCALAPPDATA\\(Google|Microsoft)\\(Chrome|Edge)\\\\User Data\\Local State


Threat Actor abusing DPAPI to extract stored browser credentials


Threat Actor abusing DPAPI to extract stored browser credentials


## **Privilege Escalation**


At this point, the threat had direct network access to the environment through a network tunnel and direct command of the initially compromised device. Our analysis determined that the attacker began controlling a Domain Admin account approximately four hours after the initial breach.


In addition to hunting across the network to identify compromised devices, we investigated exactly how this privilege escalation occurred to lock down any misconfigurations or weakened posture areas.


Approximately two hours after the initial compromise, this customer’s ADFS account was observed authenticating from the compromised workstation into a read-only Domain Controller, where it subsequently had administrative privileges, indicated by the Elevated Token parameter inside the related 4624 authentication event.


A sample of an authentication event demonstrating how to detect if a session was established using Administrative privileges on the target machine


This logon session also possessed the SeTcbPrivilege privilege assignment, a hazardous role that allows the grantee to impersonate any other user in the environment, including Domain Admins or SYSTEM.


Suspicious login activity with high-privilege assignment originating from a compromised device


Shortly thereafter, we observed the threat beginning to abuse multiple Domain Admin accounts. Due to limited telemetry, we could not pinpoint the exact escalation method.


We did perform an audit on the customers’ Active Directory environment and identified a few key issues that would have provided an attack surface for escalation.


Key among our findings were misconfigured certificates in Certificate Services (AD CS) that would have allowed privilege escalation via[ESC1](https://www.semperis.com/blog/esc1-attack-explained/) . Misconfigured AD CS is extremely dangerous as it often allows regular users to escalate privileges to the Domain Administrator with little resistance.


We believe the threat actor recognized this and exploited the configuration error to obtain access to multiple high-privileged accounts, including Domain Admin users. This rapid escalation from regular user to administrator occurred less than four hours after the initial click of a malicious file, demonstrating the need to act as soon as possible when threats are detected.


## **Additional Discovery**


Once the attacker had privileges, they embarked on an exploration journey throughout the network. One of the first things they did was target laptops belonging to domain admins. To do this, they ensured that RDP was enabled and allowed on the relevant machines through remote service and registry interaction. Then, they interactively accessed the device after ensuring no one else was logged on, as demonstrated below.


A configuring and starting remote access via sc.exe


A configuring and starting remote access via sc.exe


The threat actor also abused reg.exe and netsh.exe to configure appropriate registry settings to allow remote connections and ensure that port 3389 was open on targeted devices. Following this, quser was used to check for logged-in users.


Once it was determined that a device was not in use, the TA deployed additional information and credential gathering scripts in a batch file on endpoints. These scripts were immediately deleted following execution, as shown below.


Checking logged-in users, deploying a remote scheduled task, then connecting to targeted devices


Example showing TA creating, executing, and then deleting Scheduled Tasks designed to collect data from targeted devices


In addition to standard discovery techniques such as the abuse of ping, nltest, net, qwinsta, etc, the threat also did something we found surprising — they used installed copies of Word, Visio, and Excel to open specific files of interest about the internal architecture, networking, and server environments of the client.


We found this unique because this type of data is typically taken offline for analysis. Still, this threat actor opened the files from compromised servers with Microsoft Office utilities installed, giving us high visibility into their motivations and processes.


Examples of files opened included:


- ESXi Host Cheat Sheets
- Azure VM Networking Solutions
- Server Readmes
- General information about client architecture/databases


The activity was deliberate and targeted and is just one example of the lengths threat actors will go. Monitoring data access is critical for any company’s security posture and risk appetite.


## **Data Exfiltration**


Approximately 24 hours after the initial compromise, the attacker had successfully obtained Domain Admin, deployed multiple persistence mechanisms throughout the environment, enumerated almost the entire Active Directory, and performed extensive network and file discovery.


Satisfied with their progress, the threat deployed[AzCopy](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10?tabs=dnf%22) , a Microsoft Azure Storage Account interaction utility. As shown below, the threat used this to achieve mass data exfiltration across a few targeted directories..


TA Abuse of AzCopy for data exfiltration – excluding specific files by extension and date to find recent data of interest


Increase in File Access events correlated with the exfiltration activity - on average, this user read 1k files per day, and on the day of exfiltration, nearly 270k, generating corresponding alerts in Varonis


The exfiltration activity caused the initial CPU spike and drew the customer's attention. Shortly afterwards, our team identified the threat’s persistence mechanism and associated IOCs and organized a joint cut-off with the customer to ensure all malicious access was simultaneously severed across the network. This gave the customer breathing room for remediation and ensured the threat did not evolve into ransomware.


Our analysis of the tactics, techniques, and procedures (TTPs) and the relevant Indicators of Compromise (IOCs) tied this intrusion to affiliates of the[RansomHub](https://www.varonis.com/blog/ransomhub?hsLang=en) group utilizing[SocGhoulish](https://www.trendmicro.com/en_us/research/25/c/socgholishs-intrusion-techniques-facilitate-distribution-of-rans.html) malware for initial access activities.


Fortunately, our intervention services were able to help this customer completely eradicate the threat with zero business downtime. Had this gone unnoticed for even a few hours longer, ransomware would likely have been deployed across the customer’s environment.


## Don't wait for a breach to occur


Don’t wait for ransomware to strike. Our advanced threat response stops attacks before they escalate. Protect your business continuity and data integrity —[start your security journey with Varonis today](https://www.varonis.com/company/contact?hsLang=en) .
