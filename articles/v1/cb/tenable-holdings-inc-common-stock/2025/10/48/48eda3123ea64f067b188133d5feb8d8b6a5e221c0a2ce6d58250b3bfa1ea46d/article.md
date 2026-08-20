---
schema_version: "1.0.0"
document_id: "48eda3123ea64f067b188133d5feb8d8b6a5e221c0a2ce6d58250b3bfa1ea46d"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-rss-2984dc1ab104"
canonical_url: "https://www.tenable.com/sc-report-templates/monitoring-internal-scans-for-pci-11-3-1"
published_at: "2025-10-20T12:00:00+00:00"
first_seen_at: "2026-07-20T23:16:56.508607+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:b7e0de24ebb77f9ea06b1e16723ba0b1d22174031b020b4663876c1045c3550e"
---

# Monitoring Internal Scans for PCI 11.3.1

by[Cody Dumont](https://www.tenable.com/profile/cody-dumont)


The Payment Card Industry Security Standards Council (PCI SSC) maintains, evolves, and promotes Payment Card Industry standards for the safety of cardholder data across the globe. The PCI SSC provides technical and operational requirements for organizations accepting or processing payment transactions. The guidance also applies to software developers and manufacturers of applications and devices used in those transactions.


PCI DSS helps entities understand and implement standards for security policies, technologies and ongoing processes that protect their payment systems from breaches and theft of cardholder data. The standards have historically been revised on a 2-3 year cycle, but the PCI SSC is transitioning to a posture of revising the PCI DSS as required based on changes to the current threat landscape. The current standard revision is PCI DSS Version 4.x. Any organization that handles payment card information must comply with the PCI DSS and must demonstrate compliance annually. Tenable Security Center is able to help organizations monitor ongoing PCI DSS compliance.


As part of the PCI DSS version 4.x the requirement for authenticated internal vulnerability scanning was introduced. Tenable has always emphasized that credentialed scanning is required to get the most accurate information, now the PCI Council requires credentialed scanning where possible. The Council recognizes that all systems may not be accessible as part of a credentialed vulnerability scan, but those systems must be clearly documented. As part of the vulnerability scanning, Tenable uses two methods to perform elevated vulnerability scans, Nessus and Nessus Agents. Nessus vulnerability scans access the system over a network protocol such as SMB, SSH, and etc, while the Nessus Agents run a local version of the Nessus scan engine as a system level service. (Note: When using Nessus Agent, uncredentialed port scans are still required to identify open ports) There are benefits to each method, however each provides the ability to enumerate vulnerabilities based on the operating system, system configurations, and installed software.


As part of the requirement 11.3.1.2 (Internal vulnerability scans are performed via authenticated scanning), the internal systems located within the Cardholder Data Environment (CDE) are to be documented as accessible with and without credentials. Using Nessus to scan devices on the network will provide the necessary information as to the accessibility of a system using the defined protocols and supplied credentials. Nessus will report on the success of authentication and the status of collecting vulnerabilities. Once authenticated, Nessus will enumerate vulnerabilities found on the system. The vulnerabilities detected are identified using industry-recognized vulnerability databases and our research teams.


As directed in the 11.3.1 (Internal vulnerability scans are performed), the organization must conduct internal scans every three months and perform rescans to confirm all high and critical vulnerabilities are resolved. Tenable Security Center supports the scheduling of scans, allowing the assessment teams to continuously monitor the CDE accordingly. The report provides a high level summary covering the systems and related vulnerabilities that will follow in the pages to come. Following the affected systems table, is an iterative list of all systems along with several tables that identify the vulnerabilities with different risk levels and factors. The first table is a detailed list of the scan health and assessment plugins. Each plugin has the plugin description along with relevant plugin output so the assessor is able to verify the scan of the system. The following tables show the finding, severity, state, and detection dates. The tables provide a list of critical and high vulnerabilities, along with accepted and recast findings. The last two tables provide a list of the mitigated findings, along with first seen and fixed dates.


Chapters


**Executive Summary** - As directed in the 11.3.1 (Internal vulnerability scans are performed), the organization must conduct internal scans every three months and perform rescans to confirm all high and critical vulnerabilities are resolved. Tenable Security Centers supports the scheduling of scans, allowing the assessment teams to continuously monitor the CDE accordingly. The report provides a high level summary covering the systems and related vulnerabilities that will follow in the pages to come. This chapter provides an executive level review of the vulnerabilities and scan health related findings.


**Authentication Summary** - The chapter establishes host counts in local authentication as well as overall host counts of authentication status. Local checks in Windows systems and other systems are also highlighted. Finally, this chapter serves as a preamble to chapter three, the detailed chapter.


**Risk Finding Per Asset** - As directed in the 11.3.1 (Internal vulnerability scans are performed), the organization must conduct internal scans every three months and perform rescans to confirm all high and critical vulnerabilities are resolved. Tenable Security Center supports the scheduling of scans, allowing the assessment teams to continuously monitor the CDE accordingly. This chapter provides a high level summary covering the systems and related vulnerabilities that will follow in the pages to come. Following the Identified Assets table, is an iterative list of all systems along with several tables that identify the vulnerabilities with different risk levels and factors.
