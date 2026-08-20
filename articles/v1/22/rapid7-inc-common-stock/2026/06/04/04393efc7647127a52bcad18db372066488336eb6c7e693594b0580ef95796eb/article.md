---
schema_version: "1.0.0"
document_id: "04393efc7647127a52bcad18db372066488336eb6c7e693594b0580ef95796eb"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/ai-modernizing-global-vulnerability-standards"
published_at: "2026-06-29T12:37:04+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:7819b223bbaf9fd5cbe00f92447168e7a89cb462120f194f2d6606d0e1ee488a"
---

# Modernizing Global Vulnerability Standards For The Age Of AI

As AI-driven vulnerability discovery accelerates, the cybersecurity ecosystem is being forced to examine whether the standards, disclosure processes, and prioritization frameworks defenders rely on can still keep pace. Many of those systems were built around human-speed discovery, manageable vulnerability volumes, and exploitability confirmed after the fact, which leaves them under increasing strain as frontier AI capabilities mature.


During a private sector consultation with the White House in June, Corey Thomas and I presented Rapid7’s new policy paper,


[Modernizing Global Vulnerability Standards](https://www.rapid7.com/resources/modernizing-global-vulnerability-standards) , which lays out where today’s vulnerability management infrastructure is breaking under AI-era conditions and what governments, security companies, and frontier AI providers need to do next.


In recent guidance, the


[Five Eyes cyber security agencies](https://www.ncsc.gov.uk/news/the-ai-shift-in-cyber-risk-why-leaders-must-act-now) warned that AI is rapidly transforming cyber risk by increasing the speed, scale, and sophistication of threats, lowering barriers for malicious actors, and requiring leaders to reassess long-standing assumptions about resilience and accountability.


## AI vulnerability discovery is changing the rules


In April 2026, Anthropic, OpenAI, and Google DeepMind each announced production-grade AI systems capable of discovering, chaining, and, in some cases, remediating software vulnerabilities at machine speed. In the same period, the Stanford HAI AI Index 2026 Cybench benchmark showed unguided AI agent solve rates on cybersecurity tasks rising from 15% to 93% in a single year.


These are deployed capabilities on a steep improvement curve. Faster discovery can help security teams identify weaknesses earlier, validate risk more effectively, and improve remediation workflows. It also increases the pressure on every system that decides how vulnerabilities are verified, scored, disclosed, prioritized, and fixed.


## Vulnerability management standards were built for human speed


For decades, the security community has depended on shared infrastructure to make vulnerability management work. CVE identifiers, CVSS scoring, the National Vulnerability Database, the CISA Known Exploited Vulnerabilities catalog, and the Exploit Prediction Scoring System all help organizations understand what a vulnerability is, how severe it may be, whether it is being exploited, and how urgently it should be addressed.


Those systems were built around several assumptions: vulnerability discovery would be human-led, volume would remain manageable, exploitability would usually be confirmed after the fact, and organizations would have time to assess and respond. As AI-driven discovery challenges each of those assumptions, existing strain across the vulnerability ecosystem becomes much harder to absorb.


CVE submissions already grew 263% between 2020 and 2025 from human-speed growth alone. NIST acknowledged in April 2026 that the National Vulnerability Database can no longer keep pace and is shifting to risk-based triage. If AI-driven discovery dramatically increases volume, the prioritization problem becomes even more acute.


The issue for defenders is whether organizations can understand which vulnerabilities are actually exploitable, which are reachable in their environments, which can be chained together, and which require immediate action.


## AI-era vulnerability prioritization needs reform


The paper argues that the prioritization gap is the most urgent and least addressed part of the problem. Traditional severity scores can miss the way attackers chain multiple lower-severity issues into a serious compromise. KEV remains one of the strongest signals available to defenders, but it is retrospective by design because it depends on confirmed exploitation in the wild. EPSS is trained on historical attacker behavior, which may not reflect what AI-assisted attackers can now do.


To close that gap, we propose reforms that would help move vulnerability prioritization closer to real-world risk. These include recognizing verified AI-demonstrated exploitability, adding chaining-risk metadata to vulnerability records, and requiring reachability guidance alongside AI-discovered findings.


The goal is to help organizations understand how dangerous a vulnerability is in practice, in their environment, rather than relying only on abstract severity.


## AI vulnerability policy needs verification, access, and accountability


The paper also outlines a broader policy agenda - we call for updates to the Vulnerabilities Equities Process, investment in CVE and NVD infrastructure, standardized capability disclosure from AI labs, stronger international coordination, and clear CISA leadership.


We also propose three access and verification standards for the security community:


-


Independent verification before access expansion


-


Broad but curated access through transparent processes


-


Rigorous data standards for published capability claims


The frontier model providers building these capabilities deserve credit for acting responsibly as they develop programs in real time. But individual access programs cannot carry the weight of ecosystem governance on their own. The security community needs shared standards backed by independent verification and institutional accountability.


## The next phase of cybersecurity resilience


This paper is part of a wider conversation we recently explored on Rapid7’s


[Experts on Experts: Commanding Perspectives](https://www.rapid7.com/blog/post/it-experts-video-series-ai-compliance-force-new-security-operating-models) , where Corey and I discussed AI, compliance, industry accountability, and the shift toward more resilient security operations.


AI-driven vulnerability discovery has crossed a threshold. The question now is whether the policy, standards, and operational systems around it can adapt quickly enough to help defenders use these capabilities safely and effectively.


Read the full paper,


[Modernizing Global Vulnerability Standards](https://www.rapid7.com/resources/modernizing-global-vulnerability-standards) , to explore Rapid7’s recommendations for verification, access, disclosure, prioritization, and institutional accountability in the age of AI-driven vulnerability discovery.
