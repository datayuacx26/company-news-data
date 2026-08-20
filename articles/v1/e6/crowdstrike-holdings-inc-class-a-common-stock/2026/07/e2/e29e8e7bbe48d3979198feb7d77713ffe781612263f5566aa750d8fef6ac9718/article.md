---
schema_version: "1.0.0"
document_id: "e29e8e7bbe48d3979198feb7d77713ffe781612263f5566aa750d8fef6ac9718"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/new-in-falcon-cloud-security-expanding-multi-cloud-coverage/"
published_at: null
first_seen_at: "2026-07-20T23:21:19.193454+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:47b9b09daf708ab534c76050f4a5212788e680ba9b313bbf8c628295d24d6108"
---

# Falcon Cloud Security June 2026 Release: Updates for Azure and Google Cloud

Identities, permissions, exposed resources, and sensitive data can all contribute to risk regardless of whether they reside in AWS, Microsoft Azure, or Google Cloud. However, security teams often encounter uneven visibility and coverage across disparate cloud environments, and face difficulty in consistently understanding risk across a multi-cloud estate.


This month's CrowdStrike Falcon® Cloud Security innovations expand its capabilities in real-time cloud security posture management (CSPM), the Cloud Risks feature, data security posture management (DSPM), and cloud infrastructure entitlement management (CIEM) across Azure and Google Cloud. With these expansions, key security workflows are now available across AWS, Azure, and Google Cloud.


## Real-Time CSPM for Google Cloud


Security teams rely on CSPM to identify misconfigurations and internet exposures before they can be exploited. However, traditional CSPM approaches often rely on periodic snapshot scanning, which creates a delay between when a risk is introduced and when it becomes visible to defenders. During this window, newly created resources, permission changes, and misconfigurations may go unnoticed.


CrowdStrike introduced real-time CSPM for AWS and Azure to enable security teams to identify cloud changes and misconfigurations within minutes. We're now extending those capabilities to Google Cloud.


Across Google Cloud environments, Falcon Cloud Security provides near real-time visibility into new assets, asset updates, misconfigurations, and cloud risks that emerge from combinations of cloud exposures. This helps teams identify potential breach paths shortly after they are introduced.


By surfacing findings as they happen, security teams can investigate and remediate cloud risks sooner using a consistent workflow across AWS, Azure, and Google Cloud.


Figure 1. CrowdStrike detects and updates cloud misconfigurations as they happen


## Correlated Cloud Risks for Azure and Google Cloud


Security teams often struggle to determine how individual findings relate to one another. A single misconfiguration or excessive permission may not represent meaningful risk on its own, but when combined with other exposures, it can create a viable path to sensitive resources, critical workloads, or administrative control.


Cloud Risks, a feature in Falcon Cloud Security, addresses this challenge by correlating multiple cloud exposures into potential breach paths. Falcon Cloud Security is now extending Cloud Risks coverage, previously only available for AWS environments, to Azure and Google Cloud.


By combining signals such as identity permissions, exposed resources, and cloud misconfigurations, Cloud Risks helps organizations understand how individual findings connect and where attackers could move through an environment.


CrowdStrike is also expanding[adversary-informed risk prioritization](https://www.crowdstrike.com/en-us/blog/crowdstrike-advances-cnapp-with-industry-first-adversary-informed-risk-prioritization/) , a capability introduced earlier this year that maps known adversary activity to relevant cloud risks. This provides additional context into which attack paths align with real-world attacker behavior and helps teams incorporate threat intelligence into cloud risk investigations. With the recent expansion of this capability, organizations can now identify cloud risks consistently across AWS, Azure, and Google Cloud using the same workflow and prioritization model.


Figure 2. Cloud risk detections surfaced in Azure


**See it in action:**


## DSPM for Google Cloud Storage


Cloud infrastructure findings are often difficult to evaluate without understanding the data they expose. A storage bucket, permission issue, or attack path may appear low risk until security teams discover it provides access to sensitive information.


Falcon Cloud Security's DSPM capabilities help organizations discover and classify sensitive data, and understand how cloud risks may provide access to that data. Now, that DSPM coverage extends to Google Cloud Storage, building on existing DSPM capabilities for AWS and Azure.


Organizations can agentlessly discover cloud data stores, classify sensitive information, identify sensitive data exposures, and understand how cloud risks may create paths to sensitive data. By combining cloud infrastructure context with data security insights, teams can more quickly determine which findings create meaningful business risk and where to focus remediation efforts.


Figure 3. Sensitive data classification


## CIEM for Azure


Cloud permissions tend to accumulate over time. Access is granted to support projects, integrations, and operational requirements, but those permissions are not always reviewed or removed as environments evolve. Organizations often struggle to understand who can access what resources and whether that access is required.


CIEM helps address this challenge by providing visibility into cloud permissions and identifying excessive access. Falcon Cloud Security now extends CIEM capabilities to Azure, building on existing AWS coverage.


Security teams can analyze permissions, understand effective access, and identify excessive entitlements across AWS and Azure environments, applying the same entitlement management workflow across both cloud providers. This helps organizations reduce identity-related risk, identify opportunities to enforce least-privilege access, and gain a more complete view of cloud permissions across their environments.


Figure 4. Cloud Identities dashboard displays key identity risks in Azure


## Windows Container Image Assessment


Container image security is most effective when issues are identified before workloads reach production. Security teams routinely assess container images for vulnerabilities, malware, and compliance violations, but organizations running both Linux and Windows containerized workloads often require separate processes.


Falcon Cloud Security already provides container image assessment for Linux-based workloads. It’s now extending those capabilities to Windows container images.


Security and platform teams can assess Windows container images for vulnerabilities before deployment to help identify issues earlier in the software development lifecycle. This expansion enables organizations to apply a consistent image security process across both Linux and Windows container environments.


Figure 5. Windows container image with vulnerabilities and detections


## Consistent Security Across Multi-Cloud Environments


Multi-cloud environments introduce enough complexity on their own. Security teams shouldn't have to navigate different workflows for understanding cloud risk, securing sensitive data, or governing identities depending on which cloud provider they're using.


These Falcon Cloud Security enhancements expand several core capabilities across Azure and Google Cloud, helping organizations apply more consistent security practices across cloud environments. The result is broader coverage and a more unified approach to securing multi-cloud environments.


Interested in seeing these capabilities in action? Request an unlimited[15-day free trial](https://www.crowdstrike.com/en-us/products/trials/try-falcon-cloud-security/) of Falcon Cloud Security.


#### Additional Resources


- *Be part of[Fal.Con 2026](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) and connect with 10,000+ cybersecurity professionals shaping the future of the industry.*
- *See CrowdStrike Falcon Cloud Security in action and how it helps you identify and[remediate cloud risks](https://vid.crowdstrike.com/watch/2aXxC2wWqZRTQ4M8GfTZv3) .*
