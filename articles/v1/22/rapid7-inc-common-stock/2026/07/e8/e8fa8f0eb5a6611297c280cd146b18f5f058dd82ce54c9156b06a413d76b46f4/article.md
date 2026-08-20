---
schema_version: "1.0.0"
document_id: "e8fa8f0eb5a6611297c280cd146b18f5f058dd82ce54c9156b06a413d76b46f4"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/pt-new-products-services-q2-2026-mdr"
published_at: "2026-07-22T13:28:02+00:00"
first_seen_at: "2026-07-22T14:45:20.673401+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:382ced8b81585a55e8418e194bd8fbc0582fb22e34d8385732d737eb5c9b46a4"
---

# What’s New in Rapid7 Products and Services: Q2 2026 in Review

If Q1 set the pace for Rapid7's tools, Q2 accelerated it. This quarter brought a steady stream of product enhancements, platform investments, and customer-driven innovation across Rapid7’s portfolio. Each release was designed with a clear goal in mind: helping security teams reduce complexity while increasing speed, context, and confidence in their day-to-day operations. Here’s a closer look at what launched in Q2.


## Detection and response


### Streamline investigations with bidirectional and enriched Microsoft Defender alerts


Bidirectional synchronization and enriched alert context for Microsoft Defender is now generally available for SIEM and


[MDR](https://www.rapid7.com/services/managed-detection-and-response-mdr/) customers, enabling security teams to automatically synchronize alert status between Rapid7's


[SIEM](https://www.rapid7.com/products/siem) and the Microsoft Defender console. With added process tree and user identity context, analysts can investigate threats more efficiently while reducing manual effort.


### Confidently scale detection engineering with Detection as Code


[Detection as Code](https://www.rapid7.com/blog/post/dr-scaling-engineering-detection-as-code) enables security teams to build, test, version, and deploy detections using Terraform and modern engineering workflows. Built-in validation, guardrails, and version control help teams deliver higher-quality alerts, maintain more consistent coverage, and scale detection engineering more effectively.


*Figure 1: Rapid7's Detection as Code methodology.*


### Strengthen ransomware resilience with Ransomware Prevention for Incident Command


Ransomware Prevention for


[Incident Command](https://www.rapid7.com/products/siem) adds an intent-based layer of protection designed to stop ransomware encryption and endpoint damage before they disrupt operations. Built into the Insight Agent, this capability strengthens ransomware resilience while working alongside existing endpoint security investments, without adding operational complexity.


## Compliance


### New solutions webpages


Across the globe, cybersecurity regulation is shifting away from static compliance checklists and toward ongoing risk management that blends proactive defense with effective detection and response. Rapid7’s


[platform](https://www.rapid7.com/platform) , which brings exposure management and CTEM together with detection, response, and MDR, is well positioned to help organizations operationalize compliance across mandates such as NIS2, NIST CSF 2.0, DORA, HIPAA, HITRUST, and GovRAMP. To support that effort, Rapid7 has launched an updated library of dedicated compliance solution pages that map platform capabilities to the requirements that matter most across industries and regions. The first set of pages is live now, with more to follow in the coming weeks.


-


[NIST CSF 2.0](https://www.rapid7.com/solutions/compliance/nist-csf-2)


-


[HIPAA](https://www.rapid7.com/solutions/compliance/hipaa)


-


[HITRUST](https://www.rapid7.com/solutions/compliance/hitrust)


-


[NIS2](https://www.rapid7.com/solutions/compliance/nis2)


-


[GovRAMP](https://www.rapid7.com/blog/post/pt-new-products-services-q2-2026-mdr/www.rapid7.com/solutions/compliance/govramp)


*Figure 2: Rapid7's new GovRAMP compliance solutions page.*


## Exposure management


### Turn prioritized exposures into remediation progress


We improved Remediation Hub to help teams turn prioritized exposures into more actionable remediation progress. Updates to the Top Remediations Report add asset-level context, including operating system, IP address, cloud provider, tags, endpoint protection, and patch management details, so teams can better understand what needs to be fixed and who needs to act.


With clearer patch and endpoint coverage signals, reboot status, customizable filters, exportable reports, and scheduled email delivery, teams can spend less time assembling manual updates and more time tracking the remediation work that reduces risk. Read the full


[blog](https://www.rapid7.com/blog/post/em-path-from-prioritized-exposures-to-remediation-progress) to learn more about how Exposure Command helps teams move from prioritized exposures to remediation progress.


### AI pre-triage for AppSec findings


Rapid7 is also making application security testing faster and more focused with AI vulnerability pre-triaging for InsightAppSec. Available now for


[AppSec](https://www.rapid7.com/products/insightappsec) customers in supported regions, the capability uses AI to automatically remove false positives during the scan process, helping teams spend less time manually reviewing findings and more time remediating actual risk.


Initial coverage started with BlindSQL, and the latest engine release adds AI validation for BlindNoSQL findings, including content-based and timing-based detections. The result is a cleaner, more confident view of application risk, so security teams can focus on high-impact vulnerabilities and accelerate remediation with less manual effort.


## Attack surface management


### Open-source MCP Server and Agent Skill


We are delighted to announce the introduction of a free, open-source MCP Server and Agent Skill for Bulk Export. Bulk export is a highly efficient way to access all your Rapid7 vulnerability and exposure data to AI assistants and custom AI workflows. Built as an open-source bridge, it helps customers bring their Rapid7 data into the tools and experiences that work best for their teams. Check out our


[blog](https://www.rapid7.com/blog/post/em-bulk-export-ai-ready-security-workflows-open-source-mcp-server-agent-skill) for more detail.


*Figure 3: Agent Skill for Bulk Export.*


### Turn exposure filters into live dashboards


[Surface Command](https://www.rapid7.com/products/command/attack-surface-management-asm/) also made exposure reporting easier with filter-based dashboard widgets. Teams can now turn saved asset and identity filters into live dashboards without writing Cypher queries, making it faster to track high-risk internet-facing assets, identity-driven exposure hotspots, unmanaged cloud infrastructure, and business-unit risk.


For continuous threat exposure management programs, this helps teams move from one-off reporting to repeatable, always-on views of exposure risk and remediation progress. Read this


[blog](https://www.rapid7.com/blog/post/em-operationalizing-ctem-building-surface-command-dashboards) to learn more.


## Platform and Labs


### Rapid7 Command Platform


#### Cyber GRC


Rapid7 introduced


[Cyber GRC](https://www.rapid7.com/about/press-releases/rapid7-launches-cyber-governance-risk-and-compliance-grc-early-access-program-to-unify-security-data-risk-context-and-compliance-workflows) to select customers in Q2, giving teams an early look at a new way to connect security, risk, compliance, and third-party risk management in one program. Available to both Exposure Management and Detection and Response customers, Cyber GRC brings governance and compliance workflows closer to the security data teams already use every day.


Cyber GRC will be broadly available in late July. It helps organizations move toward continuous compliance by mapping controls to real environment telemetry, automating evidence collection, and prioritizing risk with live attack surface context. That means teams can spend less time chasing audit artifacts, screenshots, and vendor risk details, and more time understanding which controls, assets, third parties, and risks need attention now.


### Rapid7 Labs


#### Rapid7 Quarterly Threat Landscape Report


The Rapid7 Quarterly Threat Landscape Report examines the key trends shaping today's threat landscape, drawing on MDR incident response, vulnerability intelligence, ransomware monitoring, and dark web telemetry. Q1 2026 data highlights the growing dominance of vulnerability exploitation as an initial access vector, the rise of zero-click vulnerabilities, evolving ransomware operations, and the accelerating pace at which attackers operationalize newly disclosed vulnerabilities. Read the


[report](https://www.rapid7.com/research/report/threat-landscape-report-2026-q1) to explore all key findings and takeaways.


*Figure 4: Rapid7's quarterly threat report.*


### The latest threat research


Rapid7 researchers explored emerging trends shaping the threat landscape, including the growing commercialization of


[criminal AI-as-a-Service](https://www.rapid7.com/blog/post/tr-criminal-ai-underground-market-operationalizing-cybercrime-2026) and the evolving tradecraft of advanced threat actors. From the underground adoption of AI tools for fraud and social engineering to an


[in-depth analysis of the Dropping Elephant malware campaign](https://www.rapid7.com/blog/post/tr-malware-tracking-dropping-elephant-tradecraft-china-themed-loader-chain) , these reports provide actionable intelligence on how attackers are adapting their techniques and what defenders can do to stay ahead.


#### Emergent Threat Response


This quarter's Emergent Threat Response (ETR) coverage highlights a sustained wave of high-impact vulnerabilities affecting widely deployed enterprise technologies, including


[Oracle PeopleSoft](https://www.rapid7.com/blog/post/etr-active-exploitation-of-oracle-peoplesoft-zero-day-cve-2026-35273) ,


[Palo Alto Networks PAN-OS](https://www.rapid7.com/blog/post/etr-cve-2026-0265-authentication-bypass-in-palo-alto-networks-pan-os) ,


[Check Point VPN](https://www.rapid7.com/blog/post/etr-critical-check-point-vpn-zero-day-exploited-in-the-wild-cve-2026-50751) ,


[Ivanti Sentry](https://www.rapid7.com/blog/post/etr-cve-2026-10520-cve-2026-10523-multiple-critical-vulnerabilities-affecting-ivanti-sentry) ,


[cPanel/WHM](https://www.rapid7.com/blog/post/etr-cve-2026-41940-cpanel-whm-authentication-bypass) , and


[Nginx UI](https://www.rapid7.com/blog/post/etr-cve-2026-33032-nginx-ui-missing-mcp-authentication) . For each of these CVEs, Rapid7 tracked active exploitation and rapidly evolving attacker activity to provide timely guidance to help defenders assess risk and respond quickly. See all the details, and our latest ETR coverage,


[here](https://www.rapid7.com/blog/tag/emergent-threat-response) .


From strengthening detection and response to advancing exposure management, expanding governance capabilities, and delivering actionable threat intelligence, Q2 demonstrated Rapid7’s continued focus on helping security teams do more with less complexity. Every enhancement this quarter was designed to reduce manual effort, surface the context that matters, and help organizations make faster, more confident security decisions. We’re carrying that momentum into the rest of the year, so stay tuned to our blog and releases as we continue building the security operations platform that helps defenders stay ahead of what’s next.
