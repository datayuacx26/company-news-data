---
schema_version: "1.0.0"
document_id: "c50ef891aa87a9798648ac01030b4fcb95a4cba30ad54817f09b3cd53b32e47b"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T17:52:30.959585+00:00"
fetched_at: "2026-07-31T17:52:31.997851+00:00"
content_hash: "sha256:83791974f40df8a2f0af9184f9e379fc1fa6e4d492eee31a958534f6be935279"
---

# What's new in Elastic Defend: 800+ vulnerable driver rules, automated troubleshooting, and ARM support

31 July 2026 •


[Pedro Jaramillo](https://www.elastic.co/security-labs/author/pedro-jaramillo) •


[Roxana Gheorghe](https://www.elastic.co/security-labs/author/roxana-gheorghe) •


[Mia LaVada](https://www.elastic.co/security-labs/author/mia-lavada)


# What's new in Elastic Defend: 800+ vulnerable driver rules, automated troubleshooting, and ARM support


Elastic Defend automatically generates and instantly deploys vulnerable driver YARA rules from VirusTotal, LOLDrivers and Microsoft's blocklist, closing the gap BYOVD attacks depend on. Plus a new troubleshooting skill and ARM endpoint protection.


5 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates) ,


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering)


We know you’re tired of hearing how every vendor is going to finally help you solve alert fatigue. Well, one way we’re improving alert fatigue is from a slightly different angle, better prevention at the endpoint. Because stopping more at the endpoint means fewer alerts ever raised.


We have three endpoint enhancements, all contributing to better endpoint prevention:


1. To be even more proactive about Bring Your Own Vulnerable Driver (BYOVD) attacks, we’re continuously monitoring public vulnerable driver disclosures and automatically generating endpoint protections
2. To improve your endpoint management efficiency, Automatic Troubleshooting is now available as a skill via Elastic Agent Builder
3. To expand our coverage surface, Elastic Defend is now available for Windows on ARM


Let’s dig into each one.


##


What is a BYOVD attack and how does it bypass endpoint protection?


BYOVD is a technique attackers use to gain kernel-level access on Windows machines by abusing legitimately signed drivers, letting them bypass defenses meant to block unauthorized code. Windows requires low-level software drivers that run in the kernel to be digitally signed, so rather than trying to sneak in something unsigned, attackers bring a driver that's already signed and trusted, but that has a known security flaw. That flaw is enough to disable security software or tamper with memory, and once an attacker has that level of access, security tools can no longer reliably protect the host.


This combination is why BYOVD has become so appealing to ransomware operators. The technique started as tradecraft mostly reserved for advanced state actors and red teams. Elastic Security Labs has tracked its shift into a routine step ransomware crews now use to tamper with or shut down endpoint security software before deploying their payload, as detailed in[Stopping Vulnerable Driver Attacks](https://www.elastic.co/security-labs/stopping-vulnerable-driver-attacks) .


Now, why does timing matter here? BYOVD attacks have depended on one thing for years: the delay between a vulnerable driver's public disclosure and a vendor shipping coverage for it. The moment a vulnerable driver becomes public knowledge, attackers already know about it. When it takes a vendor an entire product release to ship a protection, that gap is exactly what the technique depends on.


To close this gap, Elastic Security Labs Threat Command, Elastic's security research team now continuously monitors public vulnerable driver disclosure sources, including VirusTotal, the LOLDrivers catalog, and Microsoft's Vulnerable Driver Block List, and automatically generates and instantly deploys detection rules. Because we know any delay could be the difference between an exposed endpoint and a secured one, we’ve decoupled this coverage from any release cycle and publish the protections in the open.


##


How Elastic automatically generates vulnerable driver YARA rules


Elastic Security Labs has published[detection coverage for vulnerable drivers](https://www.elastic.co/security-labs/invisible-miners-unveiling-ghostengine) for years. That coverage now runs through an always-on process that adds new drivers to the protections library as they're disclosed. An always- on process means coverage ships continuously, not whenever the next major release happens to land, and it doesn’t require an update or setting change. A driver flagged today becomes a driver Elastic Defend recognizes.


Elastic Security Labs Threat Command monitors three public sources for newly disclosed vulnerable and malicious drivers:


- VirusTotal
- The community-run[LOLDrivers](https://www.loldrivers.io/) catalog
- Microsoft's Vulnerable Driver Block List


No single source catches everything, so the system checks all three, filters out drivers Elastic already covers, and builds new detection rules from the driver's digital signature and file characteristics.


One example: Avast's signed anti-rootkit driver (` aswArPot.sys` ), which was abused to terminate protected processes from the kernel and has been leveraged in Cuba ransomware intrusions as well as[GHOSTENGINE](https://www.elastic.co/security-labs/invisible-miners-unveiling-ghostengine) campaigns. Elastic generates detection coverage for weaponized drivers like these automatically as soon as they surface in the wild.


Every rule this process generates is public. Coverage lands in Elastic's open[protections-artifacts](https://github.com/elastic/protections-artifacts) repository alongside the rest of Elastic's detection content, so a security team can verify it directly before applying it to their systems. The sources are named and the rules themselves are published in the open, unlike a vendor's private threat feed. You can see which driver triggered a rule, which source flagged it, and inspect the detection logic itself.


This process has taken coverage from an initial 65 rules in 2023 to more than 800 known vulnerable drivers today, and the number keeps growing. These protections ship through Elastic Security's[malware protection](https://www.elastic.co/docs/solutions/security/configure-elastic-defend/configure-an-integration-policy-for-elastic-defend#malware-protection) , so make sure it's enabled and set to **Prevent** to get the full benefit.


Signature coverage is also just one layer though. This automated coverage sits alongside protections Elastic Defend has carried for years: validating drivers against a blocklist before they're allowed to load and flagging drivers the moment they're seen for the first time in an environment. A newly disclosed driver doesn't have to wait on a signature alone to be caught doing something suspicious, and a driver built to slip past one layer still has to get past the others built to detect it.


##


Automatic endpoint troubleshooting in Elastic Agent Builder, now available as a skill


The[automatic troubleshooting skill in Elastic Agent Builder](https://www.elastic.co/docs/solutions/security/ai/agent-builder/skills-use-cases) flags policy and performance issues, bringing automation and natural language chat to endpoint diagnosis. Just like the existing[Automatic Troubleshooting feature](https://www.elastic.co/docs/solutions/security/manage-elastic-defend/automatic-troubleshooting) scans for and surfaces known endpoint issues, this skill also lets you ask questions, get a diagnosis, and receive specific remediation guidance. It handles the failures that consume the most investigation time: third-party antivirus conflicts, policy application failures, and the errors that typically send analysts into logs for hours.


The skill runs continuously to identify issues’ root causes, tell you what to fix, what commands to run, and what data to collect, all available the second you’re aware of an issue. The existing automatic troubleshooting feature remains available; this skill sits alongside it as a faster path to resolution, specifically tailored for teams that want to work through issues conversationally.


##


Windows on ARM: Elastic Defend coverage for Snapdragon and Copilot+ PCs


With the increased popularity of ARM processors, Snapdragon laptops, Copilot+ PCs, ARM workstations are more commonly showing up in enterprises’ fleets. If your endpoint protection doesn't cover them, they're unmonitored, and an unmonitored endpoint is a gap an attacker can use. That’s why Elastic Defend has now expanded to cover Windows on ARM. ARM workstations, Snapdragon laptops, and Copilot+ PCs can enroll under your existing policy with the same detection rules and telemetry as x64 endpoints. As you add ARM devices to your fleet, they can enroll automatically to your existing policies.


##


Get started with Elastic Security today


Elastic Security has significant enhancements to endpoint protections, efficiencies for diagnosing and resolving performance issues, and expanding coverage to new systems, all aimed at shifting defenses earlier, to prevention at the endpoint.


Elastic Security Labs Threat Command now continuously monitors public vulnerable driver disclosure sources and automatically generates and instantly deploys protection rules, decoupled from any release cycle and published in the open. That speed matters when AI-driven attacks can move from one machine to the next in under a minute, faster than any response workflow can react.


Browse the rules directly in the[elastic/protections-artifacts](https://github.com/elastic/protections-artifacts) repo, alongside the rest of Elastic's open detection content and test out the[automatic troubleshooting skill](https://www.elastic.co/docs/solutions/security/ai/agent-builder/skills-use-cases) through Elastic Agent Builder.


#### Jump to section


- [What is a BYOVD attack and how does it bypass endpoint protection?](https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd#what-is-a-byovd-attack-and-how-does-it-bypass-endpoint-protection)
- [How Elastic automatically generates vulnerable driver YARA rules](https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd#how-elastic-automatically-generates-vulnerable-driver-yara-rules)
- [Automatic endpoint troubleshooting in Elastic Agent Builder, now available as a skill](https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd#automatic-endpoint-troubleshooting-in-elastic-agent-builder-now-available-as-a-skill)
- [Windows on ARM: Elastic Defend coverage for Snapdragon and Copilot+ PCs](https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd#windows-on-arm-elastic-defend-coverage-for-snapdragon-and-copilot-pcs)
- [Get started with Elastic Security today](https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd#get-started-with-elastic-security-today)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=What%27s%20new%20in%20Elastic%20Defend:%20800+%20vulnerable%20driver%20rules,%20automated%20troubleshooting,%20and%20ARM%20support&url=https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd&title=What%27s%20new%20in%20Elastic%20Defend:%20800+%20vulnerable%20driver%20rules,%20automated%20troubleshooting,%20and%20ARM%20support)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd&title=What%27s%20new%20in%20Elastic%20Defend:%20800+%20vulnerable%20driver%20rules,%20automated%20troubleshooting,%20and%20ARM%20support)
