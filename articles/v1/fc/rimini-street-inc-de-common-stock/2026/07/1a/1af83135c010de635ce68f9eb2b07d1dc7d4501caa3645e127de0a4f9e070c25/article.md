---
schema_version: "1.0.0"
document_id: "1af83135c010de635ce68f9eb2b07d1dc7d4501caa3645e127de0a4f9e070c25"
company_key: "rimini-street-inc-de-common-stock"
company: "Rimini Street Inc. (DE)"
source_id: "rimini-street-inc-de-common-stock-rss-0d4ff267e309"
canonical_url: "https://www.riministreet.com/blog/how-to-proactively-address-vmware-hypervisor-security-risks/"
published_at: "2026-07-16T14:54:21+00:00"
first_seen_at: "2026-07-20T23:22:59.677568+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:568c07edd6e5565a39ddbe8e33d83fecd37d7c1b6b0826ee88bdb5be3c2b9f97"
---

# Virtual Infrastructure Under Attack: How to Proactively Address Risk

The hypervisor layer is critical to modern data centers and cloud computing infrastructure operations. A hypervisor is the software engine that enables virtualization, allowing a single physical server to act as multiple, independent “virtual machines” (VMs). If the hypervisor layer is compromised by a security breach, security experts often describe it as a “Tier-0” disaster. A “Tier-0” disaster could instantly paralyze an entire organization.


In 2025 and 2026, the hypervisor has shifted from being a “safety net” to a primary target for ransomware and state-sponsored attacks. Security researchers have noted that incidents targeting hypervisors (like VMware ESXi)[rose from nearly zero to 25% of all ransomware cases in late 2025.](https://cloudsecurityalliance.org/blog/2025/10/30/hypervisor-security-in-finance-why-virtual-infrastructure-is-a-growing-ransomware-target#:~:text=At%20the%20core%20of%20this,prime%20targets%20for%20ransomware%20attacks.)


## How hypervisor attackers operate


The math for an attacker is simple: if you compromise a single virtual machine (VM), you own one workload. From there, you can take over the entire hypervisor layer. If you compromise the hypervisor layer, here is what an attacker can do within your organization.


- **Gain total infrastructure control:** By gaining administrative access to the virtualization layer, attackers can effectively destroy or encrypt an organization’s entire infrastructure from the bottom up.
- **“Live off the land”:** Attackers are increasingly using the hypervisor’s own built-in management utilities to execute attacks. This means they don’t need to download detectable malware; they simply use the tools already present to thrive or “live off the land.”
- **Bypass firewall and endpoint protection:** This sophisticated method allows an attacker to break out of a specific virtual workload (also known as ‘escape-to-host’) and gain control of the underlying hypervisor, escaping traditional endpoint/XDR security and firewalls entirely.


## Notable breaches adversely impacting hypervisors


Several high-profile hypervisor campaigns that redefined the threat landscape include:


**1. Scattered Spider:** Known as “initial access brokers,” this group specializes in breaching perimeters to deploy ransomware. Their attacks on consumer goods and grocery chains demonstrated how shutting down a private cloud can instantly vaporize hundreds of millions in revenue.


**2. The BRICKSTORM Campaign:** A nation-state-sponsored effort that leveraged supply chain vulnerabilities. Instead of immediate destruction, these actors linger on these compromised hypervisors for months, siphoning off virtual hard disks containing credit card data and intellectual property.


**3. The MITRE Breach:** MITRE is the organization that maintains the global knowledge base for cyber adversary behavior. A significant cyberattack was carried out through a chain of exploits that breached the Networked Experimentation, Research, and Virtualization Environment (NERVE), a research network managed by MITRE. This has led to the inclusion of VMware ESXi-specific tactics in the MITRE ATT&CK framework.


## The new hypervisor security standard: runtime defense protection


“Check-box” security standards (or “compliance-led security”) do not go far enough to stop or remediate modern hypervisor attacks. To combat this, the shift must move toward proactive, preemptive runtime defense protection. Here is what it means:


- **Exploit prevention:** Through runtime monitoring, detection of 100% of the tactics, techniques and procedures used by threat actors to exploit ESXi\[1\] , the[Rimini Protect™ Advanced Hypervisor Security, powered by Vali Cyber®](https://www.riministreet.com/solutions/vmware/esxi/#lac-rimini-protect-for-vmware) solution blocks the behavior of a vulnerability (CVE) before a patch is even available.
- **Zero-day protection:** Monitoring the hypervisor at the process level to stop “escape-to-host” or “living off the land” tactics in real-time.
- **Telemetry and visibility:** Providing security teams with the data to see the entire “kill chain” so they can identify exactly how an attacker got in.


While firewalls, endpoint protection and network micro-segmentation are still necessary, they are no longer sufficient. If an attacker steals your organization’s credentials, they can walk right through the front door.


Security in a virtualized world is too complex to manage in silos. Rimini Protect™ Advanced Hypervisor Security, powered by Vali Cyber® delivers proactive, advanced, agent-based runtime security across any Linux- or KVM-based hypervisor. IT infrastructure and security leaders can detect, isolate and remediate threats in real-time – even before a vendor patch is available.


## More insights from security experts on how to protect your organization


To find out how your organization can move from a posture of “hoping the firewall holds” to “actively defending the core,” watch the Virtual Infrastructure Under Attack: How to Proactively Address Risk webinar, featuring an expert-led discussion with[Gabe Dimiglio, CISO and SVP for Rimini Street](https://www.riministreet.com/company/leadership/#gabe-dimeglio) , and[Austin Gadient, CTO and Co-Founder of Vali Cyber](https://valicyber.com/company/) , offering insights on:


- The evolving threat landscape for virtual environments
- Use case examples of the evolution and escalation evidenced by recent exploits
- The imperative for proactive, data-driven, risk-based exposure management


[Watch now](https://www.riministreet.com/resources/webinar/vmware-virtual-infrastructure-security-under-attack-how-to-address-risk/)


Concerned about the security of your VMware environment? Visit[Rimini Street solutions for VMware](https://www.riministreet.com/solutions/vmware/) to learn more about how we can help.


\[1\] Rimini Street blog[Easily addressing New MITRE ATT&CK® Updates for VMware ESXi](https://www.riministreet.com/blog/easily-addressing-new-mitre-attack-updates-for-vmware-esxi/) , May 25, 2025.
