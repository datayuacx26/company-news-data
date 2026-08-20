---
schema_version: "1.0.0"
document_id: "e990047940e96ce9099d25b30fec7805585c60066ebb2f726ac0f71e8f735a2a"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/deepsource-vanta-integration-soc2"
published_at: "2023-07-19T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:dd6393ba412636a6ca802232f3f8209db39c728851bfdb6a1c9be9b2dfe7c6ee"
---

# Announcing DeepSource’s Vanta integration

Last updated on Jul 19, 2023


Compliance with frameworks like OWASP® Top 10 and CWE/SANS Top 25 for first-party source code is a critical part of monitoring and improving the overall security posture of your organization. Application security teams often struggle with this --- gathering security issues manually across hundreds of repositories, sorting by severity, and tracking remediation efforts over time. Compliance standards like SOC 2 and ISO 27001 consist of several controls that require prompt remediation of first-party security violations, and lack of visibility makes it difficult to take action on time.


We've partnered with Vanta to solve this.


Vanta is an industry leader in compliance automation and is trusted by thousands of companies. They simplify the complex, time-consuming process of preparing for SOC 2, ISO 27001, and several other compliances, and automate the implementation and monitoring of controls, which not only reduces the risk of data breaches but also gives businesses the security credibility they need. We're excited to announce our official integration with Vanta, which will allow companies to ensure they're compliant with the controls related to source code security by discovering these issues directly in their Vanta dashboard.


## How it works


- Go to the Integrations tab in your organization’s settings on DeepSource, and click on the Vanta integration.
- Grant permissions to DeepSource to your Vanta account. Make sure that you’re connecting the right workspace to your Vanta account. Once the connection is successful, DeepSource will report all security issues found in all active repositories to Vanta.
- On your Vanta dashboard, these issues will show up under Tests, in the sidebar. If any of these issues cause a SOC 2 or ISO 27001 control to fail, you’ll get notified. You can then navigate to the repository on DeepSource and fix them.
