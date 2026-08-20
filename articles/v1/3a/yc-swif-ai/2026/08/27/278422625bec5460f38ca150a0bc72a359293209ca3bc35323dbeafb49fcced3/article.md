---
schema_version: "1.0.0"
document_id: "278422625bec5460f38ca150a0bc72a359293209ca3bc35323dbeafb49fcced3"
company_key: "yc-swif-ai"
company: "Swif.ai"
source_id: "yc-swif-ai-news-import-789a3488158e"
canonical_url: "https://www.swif.ai/blog/linux-malware-and-vulnerability-statistics"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T10:00:33.682122+00:00"
fetched_at: "2026-08-05T10:00:35.732088+00:00"
content_hash: "sha256:83f82c35a29b19724366fd251b1e54f6d0f4ff43e9dfa53ace068eb772270f18"
---

# Linux Malware and Vulnerability Statistics for 2026

Linux quietly runs most of the internet, which is exactly why attackers spent 2025 retooling around it. As of May 2026,[W3Techs](https://w3techs.com/technologies/details/os-linux) reports that Linux powers 61.1% of all websites with an identifiable operating system, and the broader Unix family accounts for 91.5%. The Linux kernel project alone had 3,529 CVEs assigned to it in 2024, roughly a tenfold jump after the kernel team became its own CVE Numbering Authority, according to data tracked by NIST through the[National Vulnerability Database](https://nvd.nist.gov/vuln/search) . And in March 2024, a near-miss supply chain backdoor in XZ Utils (CVE-2024-3094) was rated a maximum CVSS 10.0 by[CISA](https://www.cisa.gov/news-events/alerts/2024/03/29/reported-supply-chain-compromise-affecting-xz-utils-data-compression-library-cve-2024-3094) , a reminder that the open-source software underneath most Linux distributions is now a top-tier target. The figures below are the numbers IT, security, and platform engineering leaders should expect to see cited in 2026 board decks, audit prep, and post-incident retros.


### **Summary: key Linux malware and vulnerability statistics at a glance**


- **61.1%** of all identifiable websites run on Linux as of May 2026 ([W3Techs](https://w3techs.com/technologies/details/os-linux) ), and 83.5% of AWS plus 91.6% of Google Cloud virtual machines run Linux.
- **3,529** Linux kernel CVEs were assigned in 2024, an order-of-magnitude jump after the kernel project became its own CVE Numbering Authority ([NVD](https://nvd.nist.gov/vuln/search) ).
- **CVSS 10.0** for the XZ Utils SSH backdoor (CVE-2024-3094), which CISA called a critical supply chain compromise affecting multiple Linux distributions ([CISA](https://www.cisa.gov/news-events/alerts/2024/03/29/reported-supply-chain-compromise-affecting-xz-utils-data-compression-library-cve-2024-3094) ).
- **20%** of breaches now begin with an exploited vulnerability, up 34% year over year and just behind credential abuse as the top initial access vector ([Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) ).
- **89%** of all endpoint behaviors observed on Linux systems are brute-force attacks, primarily against public-facing SSH ([Elastic 2025 Global Threat Report](https://www.elastic.co/security-labs/elastic-publishes-2025-global-threat-report) ).
- **26%** year-over-year increase in new and unattributed cloud intrusions, with valid account abuse now the top method against (mostly Linux) cloud workloads ([CrowdStrike 2025 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) ).
- **$5M+** average cost of a public cloud breach, and cloud attacks grew 154% year over year in Sysdig telemetry ([Sysdig 2024 Global Threat Year-in-Review](https://www.sysdig.com/press-releases/sysdig-2024-threat-report) ).
- **500** cryptomining containers spun up every 20 seconds in one observed attack on a compromised Linux cloud account ([Sysdig](https://www.sysdig.com/press-releases/sysdig-2024-threat-report) ).
- **22%** of vulnerability-based breaches now target edge devices and VPNs (many of them Linux-based appliances), nearly an eightfold rise in one year ([Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) ).


**31.4 Tbps** peak DDoS attack from a Mirai-derived Linux IoT botnet, one of the largest floods Cloudflare has ever absorbed ([Cloudflare](https://www.cloudflare.com/learning/ddos/glossary/mirai-botnet/) ).


‍


### **Linux runs the infrastructure attackers want most**


The reason Linux malware is no longer a fringe topic is that Linux is no longer a fringe operating system. According to[W3Techs](https://w3techs.com/technologies/details/os-linux) , 61.1% of all websites with an identifiable underlying OS run on Linux, and 91.5% run on a Unix-family system more broadly. The four leading web servers (Nginx, Cloudflare Server, Apache, and LiteSpeed) together account for roughly 99% of the identifiable web-server market, and all four run primarily on Linux.


The cloud picture is even more skewed.[W3Techs cloud OS data](https://w3techs.com/technologies/details/os-linux) shows Google Cloud at 91.6% Linux VM usage, Amazon Web Services at 83.5%, and Microsoft Azure at 61.8%. When the[CrowdStrike 2025 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) notes that cloud intrusions rose 26% year over year and that more than one in three incident response cases tied back to a valid or abused credential, those attacks are landing on Linux first.


The takeaway is structural. Almost every modern enterprise that runs containers, Kubernetes clusters, internet-facing applications, virtualization hosts, or CI/CD pipelines is running a Linux fleet whether or not the IT team thinks of itself as a Linux shop. Linux is now the operating system of the data plane.


### **Linux kernel and CVE volume in 2026**


After the Linux kernel team became its own CVE Numbering Authority in February 2024, the project began assigning CVEs to every backported fix in stable kernel releases. The result was a tenfold jump in reported kernel CVEs almost overnight. The[NVD](https://nvd.nist.gov/vuln/search) recorded 3,529 Linux kernel CVEs in 2024, up from a few hundred in prior years, and disclosure is now running at roughly 8 to 9 new kernel CVEs every day in 2025 and 2026. That is a transparency win, not a quality regression, but it has real operational consequences for any team doing CVE-based patching.


The[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) put vulnerability exploitation at 20% of all breaches, a 34% increase over the prior reporting cycle, and the second-most-common initial access vector behind credential abuse at 22%. Edge devices and VPNs, many of them Linux-based appliances, accounted for 22% of those vulnerability-based breaches, nearly an eightfold rise year over year. Median time to patch an edge device vulnerability was 32 days, and only 54% of those patches were fully deployed across the affected fleet within the year.


Specific kernel bugs continue to show up in active exploitation.[Qualys](https://blog.qualys.com/vulnerabilities-threat-research/2023/10/03/cve-2023-4911-looney-tunables-local-privilege-escalation-in-the-glibcs-ld-so) disclosed "Looney Tunables" (CVE-2023-4911) in October 2023, a glibc dynamic loader flaw that allows local privilege escalation to root on default installs of Fedora 37 and 38, Ubuntu 22.04 and 23.04, and Debian 12 and 13. CISA added it to the Known Exploited Vulnerabilities catalog within days of disclosure and it remained an active target through 2025.


### **The XZ Utils backdoor and the new face of Linux supply chain risk**


The single most consequential Linux security event of the last two years was the XZ Utils backdoor, tracked as CVE-2024-3094.[CISA](https://www.cisa.gov/news-events/alerts/2024/03/29/reported-supply-chain-compromise-affecting-xz-utils-data-compression-library-cve-2024-3094) rated the vulnerability a CVSS 10.0 and issued an emergency advisory after a Microsoft engineer noticed unusually high CPU usage during SSH logins on a Debian testing system. The malicious code, planted in XZ Utils versions 5.6.0 and 5.6.1, modified OpenSSH decryption routines so that a remote attacker holding a specific private key could execute arbitrary commands before authentication completed.


What made the incident generational was the operator behavior. According to the[CISA advisory](https://www.cisa.gov/news-events/alerts/2024/03/29/reported-supply-chain-compromise-affecting-xz-utils-data-compression-library-cve-2024-3094) , the threat actor (operating as "Jia Tan") spent more than two years contributing legitimate-looking patches to the XZ project, slowly building social trust until being granted full maintainer rights. Affected pre-stable distributions included Fedora 41 and Rawhide, openSUSE Tumbleweed, Debian unstable and experimental, and Kali Linux for a three-day window in March 2024. Stable enterprise distributions like RHEL, Ubuntu LTS, and Debian stable were spared, but only because the discovery happened before the malicious release was promoted upstream.


The lesson for 2026 is that the "many eyes" assumption no longer scales for low-traffic but high-impact maintainer projects. Most modern Linux distributions depend on hundreds of single-maintainer compression, parsing, and crypto libraries that look exactly like XZ did in 2022. Software bill of materials (SBOM) tooling, package provenance, and dependency review are now Linux infrastructure concerns, not just developer-tools concerns.


### **Linux malware in 2026: ransomware, brute force, and botnets**


The[Elastic 2025 Global Threat Report](https://www.elastic.co/security-labs/elastic-publishes-2025-global-threat-report) analyzed telemetry from June 2024 through July 2025 and found that Linux accounts for 9% of all malware signature events Elastic observed across customer endpoints. The bigger Linux finding is on the behavior side: 89% of all endpoint behaviors logged on Linux systems were brute-force attempts, overwhelmingly against public-facing SSH services. Linux is what attackers spray credentials at, in volume, every minute of every day.


Ransomware groups have followed the workloads.[Elastic](https://www.elastic.co/security-labs/elastic-publishes-2025-global-threat-report) reports that LockBit, Play, Akira, and the newer Kraken family all ship dedicated Linux encryptors aimed at VMware ESXi virtualization hosts. Compromising a single ESXi hypervisor can encrypt dozens of virtual machines simultaneously, which is why ESXi-targeting Linux ransomware variants now drive a disproportionate share of large-loss incidents. CISA added CVE-2025-22225, a VMware ESXi arbitrary write flaw that grants kernel-level access to the hypervisor, to its Known Exploited Vulnerabilities catalog in 2025 after confirmed active exploitation.


IoT botnets remain the volume story.[Cloudflare](https://www.cloudflare.com/learning/ddos/glossary/mirai-botnet/) has tracked Mirai and its descendants since the original 2016 Krebs and Dyn attacks, and the latest generation has produced the largest DDoS floods on record. Cloudflare reported absorbing a 31.4 terabit-per-second flood and a 14.1 billion packet-per-second assault attributed to the Aisuru-Kimwolf Mirai variant, which is estimated to have compromised between one and four million Linux-based IoT hosts. Aisuru is one of dozens of Mirai derivatives in active use, all of them targeting the same exposed Linux IoT firmware ecosystem.


### **Cloud workloads, containers, and cryptomining**


Linux containers are where opportunistic attackers go for free compute. The[Sysdig 2024 Global Threat Year-in-Review](https://www.sysdig.com/press-releases/sysdig-2024-threat-report) reported that the average cost of a public cloud breach has exceeded $5 million, and cloud attacks rose 154% year over year in Sysdig telemetry. In one Meson Network case Sysdig analyzed, attackers used a compromised cloud account to spin up more than 500 new cryptomining instances every 20 seconds, racking up roughly $22,000 a day in compute charges for the victim.


LLMjacking, where attackers steal AI service credentials and resell access on underground markets, has emerged as an adjacent category.[Sysdig](https://www.sysdig.com/press-releases/sysdig-2024-threat-report) documented an LLMjacking incident that left one victim on the hook for $30,000 in three hours, with the potential to exceed $100,000 per day if undetected. Like cryptomining, LLMjacking lives on poorly secured Linux cloud workloads with permissive identity and access management configurations.


The[CrowdStrike 2025 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) adds that cloud-focused attacks saw a 37% year-over-year increase overall and a 266% surge from nation-state threat groups specifically. China-linked adversaries were responsible for 40% of the rise, with groups like GENESIS PANDA and MURKY PANDA exploiting cloud misconfigurations and abused trust relationships rather than novel kernel exploits. Two-thirds of the vulnerabilities Chinese groups exploited in 2024 yielded immediate system access, and 40% of those exploits targeted internet-edge devices, again mostly Linux.


### **What is new in 2026**


Three patterns are worth flagging for 2026 planning. First, the threat surface is decisively Linux, but the controls are still Windows-centric in most organizations. The[Elastic 2025 Global Threat Report](https://www.elastic.co/security-labs/elastic-publishes-2025-global-threat-report) notes that endpoint detection and response coverage for Linux servers, containers, and virtualization hosts lags badly behind Windows workstation coverage at most enterprises. Security teams whose detection stack indexes on endpoint agents on user laptops are functionally blind to the brute-force, container escape, and ESXi ransomware activity that is now the dominant attack pattern.


Second, the kernel CVE firehose has changed how Linux patching should work. With[NVD](https://nvd.nist.gov/vuln/search) tracking 3,529 kernel CVEs in 2024 and disclosure continuing at 8 to 9 per day, CVE-by-CVE triage is no longer feasible. Live patching, automated kernel updates, and distro-based security stream subscriptions are now the only realistic models for staying current. The[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) finding that only 54% of edge device patches landed within the year, with a 32-day median patch time, is the clearest evidence that traditional patch cycles cannot keep pace.


Third, supply chain risk is now a Linux-native concern. The XZ Utils episode, documented by[CISA](https://www.cisa.gov/news-events/alerts/2024/03/29/reported-supply-chain-compromise-affecting-xz-utils-data-compression-library-cve-2024-3094) , proved that a determined actor can earn maintainer rights on a critical compression library over two years and slip a backdoor into pre-stable Linux distributions. Combined with the[Sysdig](https://www.sysdig.com/press-releases/sysdig-2024-threat-report) cryptomining and LLMjacking findings, the picture for 2026 is one where Linux defenders need to think about package provenance, container image scanning, and identity and access management for cloud workloads in the same breath as kernel patching.


For broader context on the trends above, see our[malware statistics](https://www.swif.ai/blog/malware-statistics) and[hacking statistics](https://www.swif.ai/blog/hacking-statistics) roundups.


### **How swif.ai helps secure Linux fleets**


Swif.ai gives IT and security teams a single console to enforce device, identity, and compliance controls across the macOS, Windows, and Linux endpoints behind the numbers above. Explore[Swif.ai Linux MDM](https://www.swif.ai/products/mobile-device-management-mdm/linux-mdm) to see how it works.


If you are looking for more detailed[Cybersecurity statistics](https://www.swif.ai/blog/cybersecurity-statistics) , check out our detailed article here.
