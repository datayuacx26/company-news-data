---
schema_version: "1.0.0"
document_id: "493712252ce038fbd98a83383c7f2feb67f3e5b1db33a63b27660bb164fec410"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/vanta-sca"
published_at: "2025-05-21T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:4b075251b7a19311c93c52866be7958a010e1747cc8b22da2363810ed204a678"
---

# Announcing Vanta integration for DeepSource SCA

Last updated on May 21, 2025


We’re excited to announce that DeepSource now fully integrates Software Composition Analysis (SCA) with Vanta, expanding beyond our previous support for SAST issues. Security and compliance teams can now automatically surface vulnerable open-source dependencies in their Vanta dashboard, giving them a unified view of code and dependency risks.


## The open-source security compliance challenge


As organizations increasingly rely on open-source components, maintaining visibility into supply chain vulnerabilities becomes critical for compliance with standards like SOC 2, ISO 27001, PCI DSS, HIPAA, GDPR, and other regulatory frameworks. These frameworks require organizations to implement controls that promptly identify and remediate security issues in both first-party code and third-party dependencies.


Security and compliance teams face significant challenges:


- Tracking vulnerabilities across hundreds of dependencies across multiple repositories
- Assessing actual risk based on vulnerability reachability and exploitability
- Prioritizing remediation efforts based on severity and impact
- Demonstrating continuous compliance during audits


## How DeepSource SCA works


DeepSource SCA provides a comprehensive solution for securing your open-source dependencies through a multi-layered approach. At its core, the platform continuously monitors package manifests and lock files across all your repositories, ensuring no vulnerable dependency goes undetected.


What sets DeepSource SCA apart is its sophisticated reachability analysis, which determines if vulnerable code is actually used in your application. This drastically reduces noise and alert fatigue by focusing your attention on vulnerabilities that truly matter.


When issues are discovered, DeepSource offers intelligent remediation suggestions that minimize breaking changes, along with Autofix™ AI capabilities that automatically propose dependency updates via pull requests. This approach not only identifies security issues but actively helps resolve them with minimal disruption to your development workflow.


## How DeepSource SCA integrates with Vanta


The Vanta integration creates a seamless connection between your dependency security monitoring and compliance frameworks. All vulnerability findings from DeepSource SCA are automatically reported to your Vanta dashboard without any manual intervention required.


The integration categorizes issues by severity and maps them to relevant controls within your compliance frameworks, whether SOC 2, ISO 27001, or others. Your compliance status updates in real-time as issues are remediated, giving you an accurate picture of your security posture at any moment.


Perhaps most valuable for compliance teams is the automatic collection of audit evidence. The integration documents your security diligence around third-party dependencies, significantly simplifying the often arduous compliance process and reducing the preparation time for audits.


## Setting up the integration


1. Navigate to the Integrations tab in your organization's settings on DeepSource
2. Click on the Vanta integration card and follow the authentication process
3. Ensure you're connecting the correct workspace to your Vanta account
4. Once connected, DeepSource will report all security issues from both SAST and SCA to Vanta


In your Vanta dashboard, these issues will appear under Tests in the sidebar, providing a consolidated view of your security status. The system is designed to alert you immediately if any vulnerabilities cause a compliance control to fail, enabling swift remediation action.


If you're an existing DeepSource customer, you can enable this integration directly from your dashboard. New users can[sign up](https://deepsource.com/signup) with our free plan or[contact sales](https://deepsource.com/contact/sales) for more information.
