---
schema_version: "1.0.0"
document_id: "27596eb09cf817bf30093aca4fcb6d09fa881650e8cca2efdedc240ceb22323e"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-rss-2984dc1ab104"
canonical_url: "https://www.tenable.com/sc-report-templates/vulnerability-management-program-health"
published_at: "2026-04-28T12:19:54+00:00"
first_seen_at: "2026-07-20T23:16:56.508607+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:68c0bdc55971712efb201feb6f43f9fd165eafd0ed6ddc4726bdfa09c4e12463"
---

# Vulnerability Management Program Health

by[John Thounhurst](https://www.tenable.com/profile/john-thounhurst)


Organizations deploying vulnerability management programs must reconcile asset discovery results with scanning coverage to ensure critical systems receive complete security assessments that detect installed software, configuration weaknesses, and known vulnerabilities. Infrastructure teams continuously discover new hosts through network ranges and passive monitoring, while security teams depend on successful authentication to those systems for credentialed scans that provide complete visibility into security posture. However, credential failures, insufficient permissions, authentication protocol issues, and scanning infrastructure gaps often create situations where discovered assets cannot be assessed comprehensively, leaving unknown vulnerabilities on systems the team knows exist but cannot fully evaluate.


A common issue is incomplete visibility into discovered asset populations where some systems receive full credentialed scanning while others remain limited to network-based vulnerability detection or receive no assessment at all. Infrastructure and security operations teams must diagnose why authenticated scans fail on specific hosts, why certain credential sets work for some systems but not others, and whether observed scanning gaps result from credential problems, privilege escalation blockers, or infrastructure capacity limitations. Each unauthenticated system represents a scanning coverage gap that could conceal critical vulnerabilities, compliance failures, or security configuration weaknesses that attackers could exploit for initial access or privilege escalation during compromise campaigns.


For organizations and Risk Managers managing diverse asset populations spanning multiple operating systems, authentication protocols, and privilege models, successful scanning requires understanding which discovered assets have received successful credentialed assessment versus those remaining in a discovered-but-unauthenticated state or experiencing intermittent authentication failures. Security teams need insight into whether observed scanning issues affect specific hosts, asset groups, authentication protocols, or scanning infrastructure consistently, enabling them to distinguish between localized credential problems affecting individual systems and systemic issues requiring authentication infrastructure remediation. By focusing credential and privilege remediation efforts on the highest-impact systems and protocols, organizations can expand credentialed scanning coverage incrementally and demonstrate scanning program maturity through increasing percentages of discovered assets receiving authenticated vulnerability assessment.


Solving scanning coverage gaps requires unified visibility into host discovery results, authentication success indicators, and scanning infrastructure diagnostics that collectively answer whether the asset population is being discovered completely and assessed authentically. This report provides infrastructure and security teams with detailed analytics on host discovery progression, scanning authentication success rates across protocols and privilege levels, and scanning infrastructure indicators that surface issues requiring operational remediation. By addressing credential deployment, privilege escalation, and authentication protocol configuration problems that prevent credentialed scans from succeeding, organizations expand the percentage of discovered assets receiving complete security assessment, improve vulnerability detection accuracy, and reduce the risk that critical systems harbor undetected security weaknesses.


## Chapters


**Executive Summary** : This chapter provides infrastructure and security teams with comprehensive host discovery and scanning authentication metrics that collectively demonstrate scanning program maturity and coverage effectiveness. This chapter also displays metrics tracking the relationship between discovered hosts receiving various levels of assessment, including those identified through active scanning with credentialed checks enabled, those discovered but awaiting credentialed assessment, and hosts receiving various scanning configurations.


**Hosts with the Most Vulnerability Scanning Issues:** This chapter focuses on identifying which discovered systems are experiencing the most significant barriers to successful credentialed scanning and complete vulnerability assessment. The chapter highlights assets where authentication failures, insufficient credentials, privilege escalation problems, or scanning infrastructure issues prevent the security team from executing comprehensive local security checks and vulnerability detection. By surfacing the systems experiencing the greatest scanning challenges, this chapter enables infrastructure teams to prioritize remediation efforts on high-impact systems where expanding credentialed scanning coverage would provide the greatest improvement to vulnerability detection accuracy.


**Assets with Local Check Scan Results in Last 30:** This chapter displays assets that have received credentialed scanning with local security checks within the recent assessment window, demonstrating which systems in the asset population are receiving the most comprehensive vulnerability detection assessments. Details within this chapter provide visibility into successful credentialed scanning infrastructure performance and identifies systems that have recently received updated security assessments reflecting current vulnerability exposure.


**Scan Information Summary:** This chapter provides detailed diagnostics about scanning infrastructure health and execution characteristics including scan duration, engine versions, and parallelization parameters affecting assessment coverage. This chapter helps operations teams understand whether observed scanning performance aligns with expected baselines and whether specific hosts or asset groups experience anomalous scanning behavior indicating infrastructure problems or credential issues.


**Vulnerability Scanning Issues** : This chapter aggregates all detected scanning-related issues across the assessed asset population into a consolidated view that helps operations teams understand the types and severity of problems preventing complete credentialed assessment. This chapter distinguishes between authentication failures, insufficient privilege issues, intermittent credential problems, and scanning infrastructure errors to enable targeted remediation investments.


**Assets Seen in Last 30d but Not Assessed with Credentials:** This chapter identifies discovered systems that have not received successful credentialed assessment despite appearing in the asset inventory within the recent assessment window. This chapter highlights assets representing scanning coverage gaps where authentication failures, credential unavailability, or privilege escalation problems prevent credentialed scanning from executing. By identifying these high-priority remediation targets, infrastructure teams can focus credential deployment and privilege escalation efforts on systems that are actively monitored but not completely assessed.
