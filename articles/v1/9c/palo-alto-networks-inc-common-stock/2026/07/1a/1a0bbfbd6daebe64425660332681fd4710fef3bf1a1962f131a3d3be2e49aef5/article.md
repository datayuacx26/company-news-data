---
schema_version: "1.0.0"
document_id: "1a0bbfbd6daebe64425660332681fd4710fef3bf1a1962f131a3d3be2e49aef5"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/siemens-rox-ii-zero-day-vulnerabilities/"
published_at: "2026-07-17T10:00:24+00:00"
first_seen_at: "2026-07-22T17:15:05.756127+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:bcdc00aad50656d01a78c6b4ab201c97d28385b20d983de14bd92b3cf9ddb129"
---

# Three Steps to the Terminal: A Siemens ROX II Zero-Day Trilogy

## Executive Summary


We conducted this research in close partnership with Siemens, reflecting our shared commitment to advancing the security and resilience of critical infrastructure.


This report details a critical, chained exploit comprising three zero-day vulnerabilities (CVE-2025-40948, CVE-2025-40947, and CVE-2025-40949) discovered in Siemens ROX II operational technology (OT) switches. Successful exploitation of this chain would allow an attacker to achieve full privilege escalation and persistent root-level access on these devices, which are critical components of industrial control networks. The vulnerabilities range from Medium to Critical severity, with CVSS 3.1 scores of 6.8 (CVE-2025-40948), 7.5 (CVE-2025-40947), and 9.1 (CVE-2025-40949).


The attack vector proceeds in three stages, escalating from reconnaissance to complete system compromise:


- **Arbitrary file disclosure (**[CVE-2025-40948](https://cve-2025-40948/) **):** An attacker leverages an insecure configuration of the xz utility, which executes with root privileges, to read any file on the switch’s file system. This vulnerability enables initial reconnaissance that could reveal critical information such as sensitive configuration files, password hashes and private cryptographic keys.
- **Privilege escalation via command injection (**[CVE-2025-40947](https://cve-2025-40947/) **):** This critical flaw resides in the feature key validation function. The function fails to sanitize an attacker-controlled payload before inserting it directly into a command executed with root privileges. Exploiting this allows for direct command injection and full root access.
- **Persistent root code execution (**[CVE-2025-40949](https://www.cve.org/CVERecord?id=CVE-2025-40949) **):** Following privilege escalation, the final vulnerability is exploited in the switch’s web management task scheduler. Improper input sanitization allows an authenticated attacker to inject malicious commands into the system’s root cron table. This establishes persistent code execution, surviving system reboots and maintaining full control.


These vulnerabilities could collectively transform a vital network security device into a platform for malicious activity, severely threatening the integrity and availability of the industrial network. Siemens has released security advisories[SSA-973901](https://cert-portal.siemens.com/productcert/html/ssa-973901.html) ,[SSA-078743](https://cert-portal.siemens.com/productcert/html/ssa-078743.html) and[SSA-081142](https://cert-portal.siemens.com/productcert/html/ssa-081142.html) to address these issues, which recommend that customers update their affected ROX II devices to firmware version V2.17.1.


Palo Alto Networks customers are better protected against these threats through the following products and services:


- Virtual patching detection signatures available via the[Next-Generation Firewall](https://www.paloaltonetworks.com/network-security/next-generation-firewall) with[Advanced Threat Prevention](https://docs.paloaltonetworks.com/advanced-threat-prevention/administration)
- [OT Device Security](https://www.paloaltonetworks.com/network-security/ot-security-solution)


If you think you might have been compromised or have an urgent matter, contact the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) .


**Related Unit 42 Topics** **[Vulnerabilities](https://unit42.paloaltonetworks.com/category/vulnerabilities/) ,[Zero-day](https://unit42.paloaltonetworks.com/tag/zero-day/) ,[Exploits](https://unit42.paloaltonetworks.com/tag/exploits/)**


## Partnership Overview


The Palo Alto Networks[OT Threat Research Lab](https://unit42.paloaltonetworks.com/threat-bulletin/ot-threat-research-arc/) and Siemens partnered to advance the security and resilience of critical infrastructure through collaborative vulnerability research on the Ruggedcom ROX II platform. We combined the OT Threat Research Lab’s expertise in industrial cybersecurity research with Siemens’ deep product knowledge and the coordination capabilities of Siemens ProductCERT. These teams worked together to identify, validate, remediate and responsibly disclose security vulnerabilities.


This collaboration reflects the growing importance of industry partnerships in securing OT environments. As critical infrastructure enters the AI era, organizations must work together more closely than ever to address emerging threats, accelerate vulnerability remediation and strengthen the security of the technologies that support essential services worldwide. This partnership demonstrates how coordinated research and responsible disclosure can help build a more resilient and secure future for critical infrastructure.


## The Role of OT Switches


The modern OT environment is a complex network of devices working in concert. At the heart of this connectivity are OT switches, which act as the nervous systems of industrial networks, directing communication between critical assets like human-machine interfaces (HMIs) and programmable logic controllers (PLCs).


Protecting the integrity and availability of these switches is paramount for any industrial operation, be it a factory floor or a power plant. For instance, a properly configured OT switch provides crucial network segmentation, which enhances security by isolating different parts of the network while still allowing necessary communication.


However, OT switches designed to secure the network can themselves become attack surfaces. A common misconception is that because these devices are often air-gapped or sit on isolated networks, they’re inherently safe. In reality, they are just as susceptible to software vulnerabilities as any other IT equipment, allowing an unprivileged attacker to exploit software flaws, escalate privileges and disrupt OT communication.


This threat research article demonstrates how seemingly benign flaws can be exploited to initiate a chain of events. In this case, this could lead to full control of the critical OT switch operating system ROX II.


## The Impact: From Innocuous to Hostile


### Arbitrary File Disclosure


The first vulnerability,[CVE-2025-40948](https://www.cve.org/CVERecord?id=CVE-2025-40948) , is an arbitrary file disclosure vulnerability. While not immediately devastating, this flaw provides vital intelligence by revealing sensitive information on the switch operating system (OS), from password hashes to network topology data. This initial foothold is a crucial step in a sophisticated attack.


### Privilege Escalation and Root Access


The second vulnerability,[CVE-2025-40947](https://www.cve.org/CVERecord?id=CVE-2025-40947) , is the pivotal privilege escalation flaw. We identified this vulnerability by carefully analyzing the switch’s feature key functionality, a mechanism designed to unlock optional capabilities. By reverse-engineering this feature, we discovered a way to exploit its internal logic and gain root access. This vulnerability grants an attacker total control, bypassing available security measures and transforming the switch into a platform for malicious activity.


### Persistence via Task Scheduling


The third vulnerability,[CVE-2025-40949](https://www.cve.org/CVERecord?id=CVE-2025-40949) , solidifies the attacker’s control by exploiting the switch’s task scheduling functionality. An authenticated attacker can schedule malicious scripts to run with root privilege at predetermined intervals, ensuring persistence even after a reboot. This allows for ongoing malicious activity, such as data exfiltration or denial-of-service attacks, making the compromise difficult to detect or remove.


## Exploit Chain Part 1: Exploiting CVE-2025-40948, Then Misusing xz for File System Information Disclosure


During the initial analysis of the switch’s publicly available firmware, we worked with Siemens researchers and located a key configuration file associated with a privileged daemon. This file is used by a management and configuration daemon running with root privileges on the switch’s operating system. As a root-privileged process, it can perform any action on the system, including reading and writing any file.


### **Using CVE-2025-40948 to** Mis **use xz for Arbitrary File Disclosure**


The xz


command is a common Linux utility primarily used for compressing files into the[XZ format](https://xz.xz/) with a highly effective compression algorithm. However, xz


can be used with specific parameters to function like the standard Linux cat


command, which is used to print files to standard output. By supplying the parameters -f, -c


and -d


at the same time, an attacker can instruct xz


to view file contents.


The CVE-2025-40948 vulnerability lies in the privileged daemon executing the xz


command with user-provided parameters. Since the process runs as root, an attacker can pass any file path to xz


, allowing it to read any file on the file system, including those normally inaccessible to regular users.


### **The Impact: Unrestricted File Access**


This insecure configuration creates a significant arbitrary file disclosure vulnerability. An attacker can leverage this to:


- Read sensitive configuration and system files containing credential information
- Access private keys or other cryptographic materials
- Gather information about the system and network, paving the way for further attacks


In the case of the ROX II switch, this oversight would have allowed an attacker to leak the contents of any file on the file system. This highlights the importance of carefully vetting all commands executed by privileged processes and ensuring that user input is never used to construct commands insecurely.


## Exploit Chain Part 2: Exploiting CVE-2025-40947, the Feature Key for Root Access


### **The Feature Key Mechanism**


To understand CVE-2025-40947, we must first understand how the Siemens feature key mechanism works. A feature key is a cryptographically signed license that enables specific functionalities on the switch. When a customer purchases a license, Siemens provides a signature (i.e., the feature key) that the customer installs on the device. The switch then uses a pre-installed public key to verify the feature key’s authenticity and enable the corresponding features. This process is intended to be secure, but our analysis revealed a critical flaw in its implementation.


### How the Vulnerability Works


By reverse engineering the feature key handling library (responsible for installation), we identified the CVE-2025-40947 vulnerability in its signature verification function. This function is responsible for validating the signature provided in the feature key. The function involves three important steps:


1. Read and parse: The function reads from the feature key file and parses a signature line containing up to a fixed number of characters
2. Command preparation: It then prepares a Linux command to verify the signature using the gpgv


utility, inserting the parsed signature string directly into the command
3. Command execution: Finally, it executes the constructed command using system()


with root privileges


The code excerpt in Figure 1 shows how the signature is copied into the command string before being executed. This is the root cause of the command injection vulnerability, as there is no sanitization or validation of the input signature before it is inserted into the command string.


Figure 1. Code excerpt showing how the signature is copied into the command string.


### CVE-2025-40947 Exploitation in Practice


To exploit this vulnerability, an attacker needs to craft a payload that fits within the signature field size limit. The exploitation process involves two main steps:


1. File upload: The attacker first uses the web UI’s normal file upload functionality for a feature key to upload a malicious script (e.g., a Python reverse shell) to a writable directory on the switch.
2. Command injection: Next, the attacker crafts a new feature key file where the signature field contains a command injection payload. This payload is designed to execute the malicious script that was previously uploaded. For example, a payload like $(python /tmp/rev_shell.py)


could be used, where /tmp/rev_shell.py


is the uploaded script.


When the attacker uploads this specially crafted feature key, the vulnerable verification function will execute the injected command with root privileges, giving the attacker a reverse shell and full control over the device. This attack vector highlights the importance of robust input validation and secure coding practices, especially when handling external data and executing system commands.


## Exploit Chain Part 3: Exploiting CVE-2025-40949, Persistence via System Scheduling


Following the initial privilege escalation, we discovered a third critical vulnerability, CVE-2025-40949, in the Siemens ROX II switch's system scheduling functionality. This flaw allows an authenticated attacker to establish persistent execution of arbitrary commands with root privileges.


### CVE-2025-40949 Vulnerability Details


The vulnerability resides in the switch’s system task scheduler, which is used to automate periodic command execution. An authenticated attacker can manipulate input fields within the web management interface used to configure scheduled tasks.


Due to improper sanitization and validation of user-supplied data, the attacker can inject control characters and commands into the underlying system configuration file responsible for task execution. This technique results in a command injection attack executed with root privileges. The impact is persistent code execution as the root user, enabling long-term compromise that survives system reboots and maintains control over the device.


## Exploit Chain Proof of Concept (PoC)


Exploiting this vulnerability involves several steps that an authenticated attacker can perform via the web management interface. This high-level summary demonstrates how persistent root access is achieved.


- **Step 1 - Prepare the malicious code:** The attacker prepares malicious code, such as a script designed for communication or system manipulation. This payload is stored in a location accessible by the switch's operating system.
- **Step 2 - Inject the command:** The attacker uses the task scheduler interface to create a new scheduled task. By crafting a special input string for some of the task configuration fields, the attacker is able to inject an execution command. This command is structured to overwrite or bypass the intended task parameters, causing the system to execute the attacker’s payload instead of a legitimate function.
- **Step 3 - Execute and achieve persistence:** Once the configuration is saved, the scheduled task mechanism processes the injected command. This results in the execution of the attacker’s prepared code with root privileges, achieving persistent control over the device. This control remains active through system operations, allowing for ongoing malicious activity.


## Conclusion


The discovery and mitigation of these three chained zero-day vulnerabilities in Siemens ROX II switches highlight the necessity of collaborative vulnerability research between vendors and security researchers. By working together to identify and remediate flaws that could allow full system compromise, the industry can better protect the critical infrastructure that underpins essential services.


To protect the intricate and interconnected OT environment, organizations must execute a defense-in-depth strategy. This methodology must combine timely firmware updates with compensating controls, such as virtual patching.


### Palo Alto Networks Protection and Mitigation


While applying vendor-provided security updates remains the recommended long-term remediation, organizations may require additional time to test and deploy patches within OT environments.


#### Next-Generation Firewall with Advanced Threat Prevention


[Next-Generation Firewall](https://www.paloaltonetworks.com/network-security/next-generation-firewall) with the[Advanced Threat Prevention](https://docs.paloaltonetworks.com/advanced-threat-prevention/administration) security subscription can help block the attacks with best practices via the following Threat Prevention signatures[97246](https://threatvault.paloaltonetworks.com/?query=97246) ,[97250](https://threatvault.paloaltonetworks.com/?query=97250) ,[97249](https://threatvault.paloaltonetworks.com/?query=97249) .


#### OT Security


[OT Device Security](https://www.paloaltonetworks.com/network-security/ot-security-solution) provides deep visibility and AI-powered inline protection for industrial environments, securing critical OT assets and legacy systems without requiring downtime.


If you think you may have been compromised or have an urgent matter, get in touch with the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) or call:


- North America: Toll Free: +1 (866) 486-4842 (866.4.UNIT42)
- UK: +44.20.3743.3660
- Europe and Middle East: +31.20.299.3130
- Asia: +65.6983.8730
- Japan: +81.50.1790.0200
- Australia: +61.2.4062.7950
- India: 000 800 050 45107
- South Korea: +82.080.467.8774


Palo Alto Networks has shared these findings with our fellow Cyber Threat Alliance (CTA) members. CTA members use this intelligence to rapidly deploy protections to their customers and to systematically disrupt malicious cyber actors. Learn more about the[Cyber Threat Alliance](https://www.cyberthreatalliance.org/) .


## Indicators of Behavior


**System behavior indicators:**


- Unusual task scheduler entries:


- Description: Unexpected scripts or commands injected into the switch’s system configuration files or task scheduler configuration. These are typically characterized by the execution of arbitrary commands (e.g., python, bash


or direct system calls) instead of legitimate task functions.


- Abnormal use of the xz


utility:


- Description: Execution of xz


with parameters -f, -c


and -d


by the privileged configuration daemon. This indicates an attempt to read files on the file system that are not normally accessible to the user.


## Additional Resources


- [CVE-2025-40947](https://www.cve.org/CVERecord?id=CVE-2025-40947) – MITRE CVE Program
- [CVE-2025-40948](https://www.cve.org/CVERecord?id=CVE-2025-40948) – MITRE CVE Program
- [CVE-2025-40949](https://www.cve.org/CVERecord?id=CVE-2025-40948) – MITRE CVE Program
- [Siemens ProductCERT and CERT - Hall of Thanks](https://www.siemens.com/en-us/content/cert-services/hall-of-thanks/) – Siemens
- [SSA-078743: Remote Code Execution Vulnerability in Ruggedcom Rox Before V2.17.1](https://cert-portal.siemens.com/productcert/html/ssa-078743.html) – Siemens ProductCERT
- [SSA-081142: Arbitrary Code Execution Vulnerability in Ruggedcom Rox Before 2.17.1](https://cert-portal.siemens.com/productcert/html/ssa-081142.html) – Siemens ProductCERT
- [SSA-973901: Arbitrary File Disclosure Vulnerability in Ruggedcom Rox Before V2.17.1](https://cert-portal.siemens.com/productcert/html/ssa-973901.html) – Siemens ProductCERT


*Adam Robbie, Head of OT Threat Research, Emmanuel Zhou, Sr. Staff Researcher and Rick Wyble, OT Security Researcher, are researchers affiliated with the Palo Alto Networks Advanced Research Center for OT. Miguel Pereira is from Siemens ProductCERT.*
