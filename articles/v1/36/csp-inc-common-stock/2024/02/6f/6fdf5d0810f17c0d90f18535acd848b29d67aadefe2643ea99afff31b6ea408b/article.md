---
schema_version: "1.0.0"
document_id: "6fdf5d0810f17c0d90f18535acd848b29d67aadefe2643ea99afff31b6ea408b"
company_key: "csp-inc-common-stock"
company: "CSP Inc."
source_id: "csp-inc-common-stock-news-import-a1f8bc775bf8"
canonical_url: "https://blog.ariacybersecurity.com/blog/how-to-protect-against-supply-chain-attacks-aria-cybersecurity"
published_at: "2024-02-05T00:00:00+00:00"
first_seen_at: "2026-07-23T06:52:08.583478+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:e2f960ef61d77857738269fec76d25df49697950443cdb36f78830364c5a024e"
---

# How to protect against supply-chain attacks

### SUNBURST (SolarWinds) – How ARIA would have stopped the supply-chain cyberattack of the decade – and others like it


In the 2021 supply-chain attack propagated by SolarWinds, a major US-based IT management software company, hackers reportedly linked to the Russian state injected malicious code into software updates for SolarWinds’ Orion platform. This backdoor enabled an attack known as SUNBURST to bypass the traditional cloud-based defenses – in some cases even disabling them.


The compromised Orion updates were signed with valid digital certificates, making them appear legitimate and able to bypass the leading vendors’ security checks. Because the malware-adulterated update package was delivered by certified software, it could evade “allow listing” defenses, enabling entry into critical applications. Thousands of organizations – including operational technology (OT) environments and government institutions – unwittingly installed the infected updates, providing the attackers with a foothold into their systems.


Once active inside the target networks, the attackers launched a sophisticated attack that was able to programmatically pull down and launch executables, run shell code and perform injections to gain full application access, and escalate privileges from user to system level, thereby gaining full control and data access. It required a cybersecurity firm (FireEye) to be[infiltrated itself](https://www.industrialcybersecuritypulse.com/facilities/throwback-attack-fireeye-the-cyberattack-that-started-solarwinds/) , and by chance notice its critical tools being exfiltrated, for the alarm to be raised.


The SolarWinds propagated breach demonstrated that even the most secure organizations can fall victim to highly sophisticated attacks. Today’s best-in-breed next-generation antivirus (NGAV) solutions and other tools were ineffective at protecting critical systems from being compromised by the malicious software, let alone able to detect it. Leading vendors admitted that their software was circumvented and, in some cases, temporarily disabled, leaving the systems vulnerable to the unfettered use of the malicious techniques being deployed. The attackers utilized a patient and methodical approach, remaining undetected for an extended period. This underscores the need for continuous active monitoring to find the generic techniques deployed by attackers, rather than specific attack by attack behaviors known as indicators of compromise (IoCs). The breach served as a wake-up call, prompting both governments and private enterprises to reassess their cybersecurity postures, and for the cybersecurity industry to respond with a solution to stop these emerging attacks.


This is exactly what[ARIA Cybersecurity Solutions](https://www.ariacybersecurity.com/cybersecurity-products/solutions/) did. We started with a clean sheet of paper and built a solution that protects critical assets from executing untrusted or adulterated code, using AI to detect and block generic attack techniques as they appear.


This solution became[AZT PROTECT](https://finance.yahoo.com/news/aria-cybersecurity-unveils-revolutionary-ai-130000533.html) .


Our analysis of the timeline of the SUNBURST attack is shown in the graphics below, which highlights the stages and techniques of the attack and where AZT PROTECT would have stopped it.


In the first stage of the attack the malicious payload is delivered in a signed Orion software update from the publisher (SolarWinds). This merely provides an automated mechanism that, once active, will attempt to open a back door to allow access for further code to automatically be loaded to activate the next phases of the attack. It is noted that this software update would have been allowed to execute by the AZT agent running on the machine because of the verified trusted vendor relationship. However, because the[AZT agent](https://www.ariacybersecurity.com/aria-azt-protect/) is continuously watching for code execution in memory, our Malicious Shellcode countermeasure detects and blocks the first step of the attack, which uses shell code to open the back door and to search for a command-and-control site. Stopping this halts the execution of the Sunburst attack before any harm can be done. For illustration purposes, we set the[AZT policy](https://www.ariacybersecurity.com/cybersecurity-products/all-products/zero-trust-security-gateway/) to “Detect” from the default “Prevent” policy to expose the attack techniques used, but not stop them. The graphic highlights all the techniques used throughout the attack and shows how the AZT agent countermeasures and AI triggers at each stage to detect and (if set to “Prevent”) blocks execution.


This attack started with the embedded malware becoming active. Its only role was to execute and try and open a backdoor. As it executed its use of Shellcode was picked up by AZT PROTECT. If set to the normal “Prevent” mode of operation the attack would have failed at that point, but we continued to let it run. A series of our other countermeasures began to fire as the backdoor was opened, and the next set of malicious code was brought in. Noting that the attack was constructed to run leveraging techniques used at the process level. These attack techniques are not normal – but they cannot be easily detected unless the devices DRAM memory is watched continuously. These processes also often can only be seen interacting at the Kernel level. AZT PROTECT utilizes a patented approach to run at the OS kernel level to observe the processes and code used, as they execute in memory. The solution is therefore able to uniquely detect the list of generic attack techniques shown to the right of the diagram.


Contact ARIA if you would like to see a[demo](https://info.ariacybersecurity.com/azt-demo) of how to stop this attack as it runs.


The key takeaway is that AZT PROTECT would have blocked this attack before it had a chance to execute. Our approach is generic enough that if other attacks using different malware delivery techniques tried to execute, the built-in countermeasures will also detect and block them before they execute.


The generic nature of our countermeasures, and this is important, allow us to detect and prevent attacks that have never been seen before. This provides protection from zero-day attacks on day zero, not shortly thereafter. We don’t first need to determine a pattern of behavior (the IOCs) of an attack and then figure out a means to block it up in the cloud. This approach results in delays in OT environments as the company must wait for a production maintenance window to process the download and update the endpoint agents.


Instead, AZT PROTECT protects against these type of application exploits out-of-the-box – never needing updates or any form of cloud or internet connectivity. Likewise, we can also detect when a critical application’s code has been adulterated via an exploit of the application’s vulnerabilities. This avoids the need to have CVEs discovered and subsequent security patches or updates to be deployed when eventually available from the vendor.


The SolarWinds breach reshaped the geopolitical landscape of cybersecurity. While the attribution of the attack points to a nation-state actor, the incident highlighted the blurred lines between state-sponsored and criminal cyber activities. The use of sophisticated techniques and the targeting of government agencies and critical infrastructure underscore the potential for cyber operations to have far-reaching geopolitical consequences. As a result, nations are reevaluating their cyber defense strategies, including introducing additional cybersecurity compliance regulations for critical infrastructure as well as public companies. Cyber insurance providers are also adding additional requirements before providing insurance. This is becoming even more of a focus for insurance providers as they see further examples similar to[Merck’s $1.4B cyberattack related claim](https://news.bloomberglaw.com/litigation/merck-1-4-billion-cyberhack-settlement-ends-warlike-act-claim) .


The SolarWinds incident also accelerated the adoption of[zero-trust security](https://www.ariacybersecurity.com/cybersecurity-products/all-products/zero-trust-security-gateway/) models. Traditional network-based security approaches were insufficient to detect the attackers within compromised networks.


In conclusion, SolarWinds has had a profound and lasting impact on cybersecurity. It exposed vulnerabilities in software supply chains, eroded trust in digital infrastructure, reshaped geopolitics in cyberspace, and prompted a reevaluation of security practices and strategies. As a result, industries are realizing they need a new approach that augments their network-based and traditional host-based protections, which are missing this decade’s wave of supply chain-based sophisticated intrusion attacks.


[ARIA AZT PROTECT](https://www.ariacybersecurity.com/cybersecurity-products/all-products/aria-azt-protect/) uses a patented approach to lock down critical applications from exploit, while blocking the execution of zero-day malware, and automatically stops the sophisticated techniques used by the most advanced nation-state sponsored attackers. It also deploys in minutes and instantly provides full protection, requiring no configuration to stop these attacks. It works in true air-gapped infrastructures and deploys on legacy, out-of-support OS-based critical devices in addition to today’s latest systems. Its ability to utilize less than 2% of a single core of CPU and less than 100 MB of device memory ensures it never disrupts or adds latency to the critical applications it protects. Furthermore, it deploys without requiring any reboots of the protected device OS or the hosted applications.


*If you need support building-out a simple to implement active defense, please contact* *ARIA Cybersecurity at*


Sales@ariacybersecurity.com


**


[Click here for more information on AZT PROTECT.](https://www.ariacybersecurity.com/cybersecurity-products/all-products/aria-azt-protect/)
