---
schema_version: "1.0.0"
document_id: "6a8ebb7941801e422e885b3165161ead23e2c841602397536d28f9db568566b6"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-d1030f25037f"
canonical_url: "https://blog.qualys.com/product-tech/2026/07/09/cisa-bod-26-04-3-day-remediation-sla"
published_at: "2026-07-09T19:30:37+00:00"
first_seen_at: "2026-07-20T03:32:58.378885+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:fad64712ca9dfc558adfb9ae1965b8ec6bd05fc07a5f6abf976d7ea3cd4edc19"
---

# How to Meet a 3-Day Remediation SLA & Comply with CISA BOD 26-04

#### Table of Contents


- Risk-Based Remediation Changes Where You Start
- The 90/10 Reality
- Asset Exposure is the Single Biggest SLA Driver
- Patching Is Not the Only Remediation Option
- How to Structure Remediation to Meet the Need
- Close Exposure Before the Patch Arrives
- How TruRisk Eliminate Empowers Your Team
- Frequently Asked Questions (FAQs)


---


#### Key Takeaways


- CISA BOD 26–04 mandates remediation of the publicly exposed, highest-risk, known-exploited vulnerabilities within 3 days.
- The directive applies a risk-based model evaluating exposure, KEV status, automation potential, and technical impact.
- Most high-risk vulnerability instances reside on non-critical endpoint assets, which are suited for automated patching at scale.
- Patchless remediation techniques can reduce exposure when no vendor patch is yet available.


---


***BOD 26-04 changed vulnerability management from patching everything to prioritizing what matters most. But many teams are still starting in the wrong place.***


Three days. That’s the number everyone remembers from CISA’s new[Binding Operational Directive (BOD) 26-04](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) . Publicly exposed, known-exploited vulnerabilities that meet the directive’s highest-risk criteria now require remediation within 72 hours.


The deadline is real. But if your first instinct is to throw your entire remediation team at your most critical production systems, you’re likely making the problem harder, not solving it faster. The biggest opportunity isn’t the 10% of systems that are the hardest to fix. It’s the other 90%.


---


**Join us on July 15 at 9 am Pacific, as GuidePoint Security and Qualys unpack what BOD 26-04 requires and why agencies need a Risk Operations Center (ROC).**


[Register Now](https://www.brighttalk.com/webcast/11673/671704)


---


## Risk-Based Remediation Changes Where You Start


BOD 26-04 replaces the one-size-fits-all remediation timelines of BOD 22-01 with a **risk-based model** that considers the factors of asset exposure, CISA KEV status, automation potential, and technical impact.


CVSS alone is no longer enough to determine the urgency. It needs to weigh real-world exploitability and exposure, so teams know which vulnerabilities demand a 72-hour response.


The question is no longer: **“How quickly can we patch everything?”** It’s: **“Where do we reduce the most risk first?”**


## The 90/10 Reality


When organizations analyze their KEV exposure, a familiar pattern often emerges.


Most vulnerability instances reside on endpoints, employee laptops, and workstations, which represent the largest portion of the environment. These systems are generally well-suited for automated patch deployment with minimal operational risk. A much smaller portion resides on production servers, internet-facing infrastructure, and business-critical systems where every deployment requires greater scrutiny. That distinction changes the remediation strategy.


The endpoint population is where organizations can automate at scale and rapidly reduce exposure. Critical infrastructure still demands rapid action, but often through staged deployments, validation, and change control rather than broad automation. Trying to solve both problems the same way slows everything down.


## Asset Exposure is the Single Biggest SLA Driver


The same CVE on an internet-facing asset vs. an internal asset carries completely different remediation timelines. Agencies that don’t know which assets are publicly exposed cannot comply with this directive. Asset visibility is a prerequisite, not optional.


Qualys External Attack Surface Management (EASM) provides you with a complete view of internet-exposed assets, along with numerous data points regarding attribution to your organization and subsidiaries, domains and subdomains, DNS misconfigurations, hosting providers, asset types, certificates, exposed ports and services, and vulnerabilities – all important cybersecurity risk factors that hackers are looking at to identify weaknesses to exploit.


## Patching Is Not the Only Remediation Option


BOD 26-04 explicitly defines remediation as any action that eliminates the vulnerability, including asset isolation, mitigation, or uninstall. TruRisk Eliminate goes beyond patching to deliver a complete remediation lifecycle, reducing risk faster without disruption.


## How to Structure Remediation to Meet the Need


The instinct under a three-day deadline is understandable: Start with the highest-risk servers. In practice, those are also the systems that require the most planning, testing, and coordination. Meanwhile, thousands or millions of vulnerabilities on endpoints remain untouched even though they could be remediated automatically.


A more effective approach is to run **two remediation tracks in parallel** :


- **Automate** low-risk endpoint remediation wherever possible.
- **Carefully orchestrate** deployments on production systems using staged rollouts and validation.


This isn’t delaying critical systems. It ensures that every remediation path aligns with the operational risk of the asset being patched. That’s how organizations reduce overall exposure while maintaining stability.


Over the last year, across more than 40 million autonomous patch deployments, Qualys has observed rollback rates below **0.1%** , demonstrating that large-scale automation can dramatically reduce operational effort while maintaining reliability. Automation isn’t replacing change management. It allows teams to reserve manual effort for the systems that truly require it.


## Close Exposure Before the Patch Arrives


Patching isn’t the only way to eliminate a vulnerability. BOD 26-04 defines remediation as any action that eliminates risk: patching, mitigation, isolation, or uninstalling vulnerable software altogether. The right path depends on the situation: whether a patch doesn’t exist yet, would carry too much risk to deploy, or would disrupt a system; patching wasn’t worth the disruption. Waiting for a fix is not a strategy. Exposure doesn’t pause for vendors, and neither should you. The moment a validated option exists, it should close exposure, not weeks later when a patch finally ships. Every path is held to the same standard: reduce risk, or don’t deploy it.


## How TruRisk Eliminate Empowers Your Team


TruRisk Eliminate was built to enable organizations to automate remediation across large endpoint populations while orchestrating staged, confidence-based deployments for production systems. And when no vendor patch exists yet, it supports patchless remediation techniques that reduce exposure while organizations wait for an official fix.


The goal isn’t simply to meet a deadline. It’s to reduce cyber risk as quickly and as safely as possible.


---


**Start your 30-day trial of Qualys TruRisk Eliminate and learn how you can operationalize continuous risk reduction and meet the requirements of BOD 26-04.**


[Start Free Trial](https://www.qualys.com/free-trial-new/trurisk-eliminate/)


---


## Frequently Asked Questions (FAQs)


**Q: What does BOD 26-04 change?**


A: It replaces blanket remediation timelines with a risk-based model. Only the highest-risk, publicly exposed, known-exploited vulnerabilities now require a 72-hour response. Lower-risk issues can follow normal change management processes.


**Q: Should we stop patching everything?**


A: No. You should continue patching, but prioritize the highest-risk exposures first. Automate what you can on endpoints, so your team can focus manual effort on production systems that truly need it.


**Q: What if there’s no patch available yet?**


A: BOD 26-04 allows mitigation, isolation, or other compensating controls. TruRisk Eliminate supports patchless remediation techniques to reduce exposure while you wait for a vendor fix.


**Q: How mature is large-scale automation?**


A: Very mature. Over the last year, Qualys has executed more than 40 million autonomous patch deployments with rollback rates below 0.1%. Automation is now reliable enough to safely handle the majority of endpoint remediation.
