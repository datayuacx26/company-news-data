---
schema_version: "1.0.0"
document_id: "67adc5b3bdc0230e91a59e3e45906796045eb0837dd40ef1c73aec115bf3947a"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-secure/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:42a62a6699ec82b2ad0117971741fc1f91a32aeeb201527be25bf43a9157efb4"
---

# DASH 2026 Security & Compliance: Guide to Datadog’s newest announcements

Securing modern systems means spotting real risk in a sea of findings, investigating threats faster, and meeting compliance demands without bolting on separate tools. At this year’s DASH, we made announcements that bring AI-assisted investigation and remediation across the security life cycle—from code and cloud to APIs and sensitive data—alongside expanded SIEM coverage and a modernized approach to authentication and governance.


With Datadog, you can now automate threat hunts and SIEM investigations with Bits agents, fix vulnerable dependencies directly from a finding, and secure your most sensitive workloads with FedRAMP High–certified observability. These features and many others help teams stay ahead of threats while keeping security context in familiar workflow territory. Explore everything new in security and compliance below, and see our other roundup posts for the latest in[AI](https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-ai/) ,[observability](https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-observability/) , and[scale](https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-scale/) .


## Prioritize and route real risk with Datadog Security


### Let agents run day-to-day security operations with the MCP Security toolset


The Datadog Security MCP toolset enables AI agents such as Claude Code, OpenAI Codex, and Cursor to securely access Datadog security context through the remote Datadog MCP Server. After launching with read-only capabilities earlier this year, the Security MCP toolset now includes expanded tools for SQL-powered reads, detection rule management, suppressions, triage, and ticketing workflows. With these new capabilities, teams can bring AI-assisted investigation and remediation into their existing security operations. Agents can help surface relevant context, prioritize what needs attention, and take governed actions while Datadog remains the source of truth for security data, detections, and controls.[Read our Security MCP Tools documentation](https://docs.datadoghq.com/security/mcp_server/) to get started.


### Route security findings notifications to the right team automatically


Security findings are most actionable when they reach the team that owns the affected service, resource, or repository. With dynamic routing for security notification rules, Datadog can automatically send finding notifications to the Slack or Microsoft Teams channel configured for the associated[Datadog Team](https://docs.datadoghq.com/account_management/teams/) . This helps security teams reduce manual notification setup and keep routing accurate as their organizations evolve. Instead of updating individual rules every time ownership changes, teams can manage notification channels centrally in Datadog Teams. If a finding is missing ownership information or the team does not have a notification channel configured, Datadog can route the notification to a fallback channel so important issues are still surfaced. Learn more in the[Dynamic routing documentation](https://docs.datadoghq.com/security/notifications/rules/#dynamic-routing) , or[sign up for the Preview](https://www.datadoghq.com/product-preview/dynamic-routing-for-security-notifications/) to get started.


### Automated bidirectional ticket creation for security findings


Security teams can now set up automation rules that create tickets whenever a new security finding matches defined criteria. Tickets can be created in Datadog Case Management or Jira, with bidirectional sync to keep Datadog and downstream ticketing systems aligned as work progresses. This makes it easier to route remediation work into the systems that engineering teams already use while preserving security context in Datadog. This release complements previous ticketing capabilities, including new[public APIs](https://docs.datadoghq.com/api/latest/security-monitoring/#create-cases-for-security-findings) to create and manage tickets, and Workflow Automation actions for teams that need more advanced ticketing flows. Learn more in our[Ticket Creation Rules documentation](https://docs.datadoghq.com/security/automation_pipelines/create_ticket) .


## Detect and investigate threats faster with Cloud SIEM


### Get deeper coverage across your stack with security integrations in Datadog


Since last DASH, we’ve added 30+ security integrations spanning SIEM log sources and threat intelligence feeds, expanding Datadog’s ability to ingest, correlate, and act on security data across the tools your SOC already relies on. Ingest[Jamf Pro](https://docs.datadoghq.com/integrations/jamf-pro/) device compliance, policy enforcement, and inventory data into Datadog Cloud SIEM so security teams can detect unmanaged or non-compliant endpoints, correlate device posture with security events, and close the gap between endpoint management and threat investigation. Stream[Box](https://docs.datadoghq.com/integrations/rapdev-box/) enterprise event logs to detect anomalous data access patterns, flag unauthorized sharing, and give security teams full audit trail visibility. Ingest logs from both[Zscaler Internet Access](https://docs.datadoghq.com/integrations/z-scaler/) and[Zscaler Private Access](https://docs.datadoghq.com/integrations/zscaler-private-access/) to correlate web traffic threats, policy violations, and private app access events in a single view. Bring firewall logs, threat detections, and traffic analytics from[Barracuda SecureEdge](https://docs.datadoghq.com/integrations/barracuda-secure-edge/) and[CloudGen Firewall](https://docs.datadoghq.com/integrations/cloudgen-firewall/) into Datadog Cloud SIEM so teams can correlate network-layer security events with broader infrastructure and application signals. Integrate[Recorded Future](https://www.datadoghq.com/blog/recorded-future-content-pack/) threat intelligence feeds into Datadog to automatically enrich security events with real-time context on indicators of compromise, threat actors, and vulnerability risk. Learn more about[Datadog’s security integrations in our documentation](https://docs.datadoghq.com/integrations/#cat-security) .


### Automate threat hunting with Datadog Cloud SIEM


Bits Threat Hunting is an autonomous agent that runs hypothesis-driven threat hunts across your environment. It reasons with your telemetry—logs, network flows, identity events, and endpoint activity—to surface patterns consistent with known attacker behaviors, emerging threat campaigns, and unusual deviations from your baseline. From there, it can catch indicators of compromise (IoCs) and tactics, techniques, and procedures (TTPs) that existing SIEM detection rules don’t yet cover. It can also recommend and deploy detection rules for IoCs and TTPs based on its available threat intelligence and threat hunt findings. Bits Threat Hunting is available in Preview.[Sign up to get started](https://www.datadoghq.com/product-preview/bits-threat-hunting) or[read our blog post](https://www.datadoghq.com/blog/bits-threat-hunting) .


### Automate investigations on any SIEM with the standalone Bits Security Analyst


Bits Security Analyst is an always-on SOC analyst built to investigate complex threats and triage security alerts. It autonomously investigates alerts and creates actionable reports in minutes, following security investigation best practices. SOC teams can spend less time on false positives and benign activities and focus on real threats. Bits Security Analyst is now available as a standalone solution that you can deploy in popular SIEMs—including Splunk and Microsoft Sentinel—in minutes, delivering value from day one without disrupting existing workflows. Standalone Bits Security Analyst is available in Preview.[Sign up now to start automating your investigations](https://www.datadoghq.com/product-preview/standalone-bits-security-analyst) on any SIEM.


### Monitor Claude Compliance API activity with Datadog Cloud SIEM


Security and compliance teams need a scalable way to monitor AI activity, investigate suspicious behavior, and maintain audit-ready records. Datadog integrates with the Claude Compliance API, bringing Claude activity into Cloud SIEM, including sign-ins, admin API key life cycle events, organization membership changes, SSO/SAML configuration updates, and Claude chat and project access events. Out-of-the-box dashboards and detection rules help teams identify suspicious patterns, validate administrative actions against change records, and investigate AI-related activity alongside the rest of their security telemetry data.


To learn more, read our[Claude Compliance API integration blog post](https://www.datadoghq.com/blog/cloud-siem-claude-compliance-api-integration) .


## Remediate code and cloud risks faster with Code and Cloud Security


### Fix vulnerable dependencies and misconfigurations faster with Bits Code in Datadog Code Security


Bits Code helps engineers remediate Code Security SAST findings by generating pull requests with code fixes. Now, this capability extends to Software Composition Analysis (SCA) and Infrastructure as Code (IaC) findings. Bits Code proposes targeted updates for vulnerable dependencies and misconfigured infrastructure, creating the exact change needed and showing you the reasoning behind it. Teams can apply remediations as single fixes or as batch actions that resolve many findings at once, and engineers review, refine, and merge fixes directly from their existing workflow. This expansion gives security and platform teams a faster path to resolving issues across application code, open source libraries, and cloud infrastructure without leaving Datadog. Learn more in our[dedicated blog post](https://www.datadoghq.com/blog/bitsai-dev-agent-code-security/) .


### Detect risks in your code more accurately with AI-native SAST in Datadog Code Security


Datadog Code Security now includes built-in AI-native SAST capabilities in public Preview, using LLMs to reason about code semantics, call stacks, and data flow, delivering context-aware vulnerability detection. On OWASP benchmarks, it outperforms traditional SAST across nearly every category, with true positive rates up to three times higher for context-dependent issues like SQL and command injection. Incremental analysis keeps scans fast and cost-effective, and each finding includes a clear exploit explanation and suggested fix. Learn more in our dedicated[blog post](https://www.datadoghq.com/blog/open-source-ai-sast/) .


### Detect source code attacks with Datadog Code Threat Detection


Datadog Code Threat Detection helps engineering teams catch malicious code changes before merging. Developed in partnership with Datadog Security Research, it automatically analyzes every pull request for threats that traditional vulnerability scanners miss, including supply chain attacks, suspicious dependencies, and obfuscated code. Reviewers get clear, contextual findings directly in their workflow, and each flagged change includes an explanation of why it was flagged and recommended next steps. By surfacing risks at review time, teams can stop malicious code from reaching production. Code Threat Detection is now available for Datadog Code Security customers. To request early access,[sign up for the Preview](https://www.datadoghq.com/product-preview/malicious-pr-protection/) . For more information,[read our blog post](https://www.datadoghq.com/blog/datadog-code-threats) .


### Evaluate code risks with confidence using Bits Assessments in Datadog Code Security


Available for Code Security customers, Bits Assessments reduce noise in static code analysis by classifying SAST findings as likely true or false positives, so your team can focus on the vulnerabilities that actually matter. Each evaluation includes a confidence score and a short reason citing the relevant code context, helping developers trust the verdict. Findings flagged as false positives can be automatically filtered from PR comments and PR gates, keeping pull requests clean without blocking valid fixes. Bits Assessments also learns from your team’s past false positive reports, using them as context to improve future classifications over time.[Learn more in our documentation](https://docs.datadoghq.com/security/code_security/static_analysis/) .


### Detect and block malicious open source packages with Supply Chain Firewall


Supply Chain Firewall (SCFW) is an open source CLI tool from Datadog Security Research that blocks malicious and vulnerable open source packages before they install. It supports npm, pip, and Poetry, checking every dependency against Datadog’s malicious packages dataset and[OSV.dev](https://osv.dev/) . Known malicious packages are blocked outright, and vulnerable packages prompt for user confirmation. Datadog customers can now forward SCFW activity to their Datadog account through a local Agent or the HTTP API, giving security teams visibility into developer install activity alongside their existing telemetry. You can learn more about SCFW in our[dedicated blog post](https://securitylabs.datadoghq.com/articles/introducing-supply-chain-firewall/) ; to set up the integration,[read our documentation](https://docs.datadoghq.com/integrations/supply-chain-firewall/) .


### Identify security posture risks in Oracle Cloud Infrastructure with Datadog Cloud Security


Maintaining consistent security posture across a multi-cloud footprint can be challenging. Often, security teams are left with fragmented visibility and manual compliance checks across different providers and tools. To help you secure your entire infrastructure in one place, Datadog Cloud Security now supports Oracle Cloud Infrastructure (OCI), expanding our coverage across all four major cloud providers. Now, you can automate compliance monitoring using 45 out-of-the-box rules mapped to the[CIS OCI 3.0.0 benchmark](https://www.cisecurity.org/benchmark/oracle_cloud) to identify and remediate misconfigurations instantly; build custom security logic by writing tailored Rego rules against 40 different OCI resource types to meet your organization’s specific security requirements; and gain unified visibility into multi-cloud risk by analyzing OCI security findings alongside AWS, Azure, and Google Cloud data within the Cloud Security Summary, Findings, and Compliance pages, and via the API. Set up[OCI in Cloud Security](https://docs.datadoghq.com/security/cloud_security_management/setup/cloud_integrations?tab=oraclecloud) to get started, and[read more about securing OCI infrastructure in our blog](https://www.datadoghq.com/blog/cloud-security-oci/) .


## Secure your APIs from finding to fix with App and API Protection


### Remediate API security issues directly from findings with Bits Code


Bits Code enables backend engineers to take immediate action on API findings by generating pull requests with concrete fixes. Leveraging production signals and source code context, it streamlines remediation and reduces manual effort, helping teams resolve issues faster without leaving Datadog.[Learn more in our Bits Code documentation](https://docs.datadoghq.com/bits_ai/bits_ai_dev_agent/) .


### Proactively validate your API attack surface with API Security Testing


API Security Testing brings active validation to API security by continuously testing endpoints for[OWASP API risks](https://owasp.org/www-project-api-security/) . This helps teams uncover vulnerabilities and misconfigurations that passive monitoring may miss, and turns API inventory into actionable security findings, enabling faster remediation and stronger, more reliable API posture. To get started,[request access to the Preview](https://dd-corpsite.datadoghq.com/forms/share/089713597d47b7d548fba7b8c28c39e5852580c313f6ac3c1cb0be6c2fa1efb8) .


## Find and fix data leaks at the source with Sensitive Data Scanner


### Keep sensitive data out of your logs with Bits Code and Sensitive Data Scanner


Redaction and access controls reduce exposure to sensitive data leaks, but only a code change eliminates them. Now, you can launch a Bits Code coding session from any finding in Datadog Sensitive Data Scanner. Bits AI will locate the offending log line in your code and propose a fix that removes the sensitive field at the source, so you can remediate the leak regardless of how the log is processed downstream. From there, you can review the change in Datadog and open a pull request to commit the fix in a few clicks. Contact your Datadog account team to request access.


### Detect and resolve sensitive data leaks with the new SDS Findings Explorer


[Sensitive Data Scanner](https://www.datadoghq.com/blog/scaling-sensitive-data-scanner/) helps teams detect and resolve leaks of sensitive data, such as PII, secrets/credentials, and financial information, in their telemetry data in order to meet security and compliance requirements. The new SDS Findings Explorer now groups every match by specific data patterns so you can precisely see where leaks originate, how often they occur, and which services are responsible. Each finding includes a seven-day trend chart, sample log events with sensitive content highlighted, rule and ownership context, and recommended remediations to resolve the leak at the source. The Findings Explorer is available in preview for logs, with RUM and APM support to follow later in 2026. Contact your Datadog account team to request access.


## Modernize authentication and meet compliance demands with Governance


### Introducing Datadog’s new API authentication model


Modern infrastructure depends on automated systems, AI agents, and cloud-native workloads that need secure, auditable access to APIs. Datadog’s new API authentication model modernizes how teams authenticate to Datadog APIs by introducing four purpose-built credential types: Personal Access Tokens (PATs), Service Access Tokens (SATs), Workload Identity Federation, and customer-managed OAuth clients. These new authentication methods provide scoped, identity-aware access for developers, CI/CD pipelines, autonomous AI agents, and cloud provider workloads without relying on long-lived shared credentials. PATs, SATs, and Workload Identity Federation for AWS workloads are generally available. OAuth client support is planned for release later this year. Application keys will continue to work after Q3 2026, but they will be considered legacy features and no new capabilities will be added. Learn more about which scoped credential is best for your use case in[our dedicated blog](https://www.datadoghq.com/blog/datadog-api-authentication) .


### Monitor and secure high-impact workloads with FedRAMP® High-certified observability


Datadog for Government has achieved FedRAMP® High certification, enabling federal agencies and regulated organizations to secure their most sensitive mission-critical workloads within the US1-FED GovCloud environment. As FedRAMP’s most stringent security baseline, High certification supports organizations with rigorous requirements for confidentiality, integrity, availability, and continuous monitoring, while giving teams the flexibility to scale as their needs evolve. With this milestone, public-sector teams, as well as organizations in industries such as healthcare and financial services, can use Datadog’s unified observability and security platform to monitor, troubleshoot, and secure sensitive workloads, without introducing separate tools or workflows. To learn more, read our[blog post](https://www.datadoghq.com/blog/datadog-achieves-fedramp-high-certification/) on Datadog for Government achieving FedRAMP High certification.


### Connect Azure to Datadog with Secretless authentication


Telemetry access from your Azure environments has traditionally required a client secret, which forces periodic rotation to keep telemetry flowing. With Secretless authentication, you can connect your subscriptions using workload identity federation instead, removing the need to rotate credentials before expiration. To get started, follow the[Secretless authentication setup guide in the Azure integration docs](https://docs.datadoghq.com/integrations/azure/) .


-
-
-
