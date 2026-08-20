---
schema_version: "1.0.0"
document_id: "7473ed9160e85f95e4f15543610be7ab331f03fa8269916e75c90cd2aec9c5c0"
company_key: "zscaler-inc-common-stock"
company: "Zscaler Inc."
source_id: "zscaler-inc-common-stock-news-import-d539219e7401"
canonical_url: "https://www.zscaler.com/blogs/product-insights/hidden-threat-your-software-development-lifecycle-why-self-hosted-runners"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-10T22:30:00.076900+00:00"
fetched_at: "2026-08-11T00:00:01.092994+00:00"
content_hash: "sha256:f47294affd9cb630a857ba413655d6cb0646d8f9f3dcbaa5fbca64ccb7cffd8c"
---

# [Securing Self-Hosted GitHub Runners with Zero Trust]

## Deep Dive: Securing the GitHub Runner’s Ecosystem


Securing a self-hosted runner requires a dual-pronged approach: safeguarding its communication with the public internet (SaaS and public registries) and controlling its access to internal, highly sensitive private resources. ZTC excels at both.


**1. Public SaaS & Registry Access: Blocking Exfiltration**


ZTC continuously monitors all outbound traffic whenever GitHub Runners interact with public applications like ServiceNow or Slack. The traffic is routed from a service endpoint to Zero Trust Exchange via Zero Trust Gateway. ZTC provides cloud-scale TLS inspection and inline data protection. It ensures that the GitHub workloads remain entirely hidden from the public internet.


**2. Private Resource Access: Isolating the Crown Jewels**


During the code review, testing and deployment phases, runners must inevitably connect to private internal resources, such as artifact repositories (like JFrog Artifactory or Confluence), or internal databases.


Traditionally, this meant placing the runner in the same network segment as these sensitive assets, or configuring complex router ACLs and internal firewalls. If an attacker compromised the runner, they had an open pathway to probe your internal infrastructure.


ZTC enforces least-privilege access allowing only 1-to-1 interactions based on workload identity and security policies. There are 2 scenarios:


**2.1. When the private apps are hosted in the same region:**


Under this scenario, the private application VPC is located within the same cloud region as the GitHub Runner VPC. Traffic originating from the GitHub Runner is directed to the Zero Trust Gateway via service endpoint / intercept, where local inspection is conducted before granting access to the private application.


Figure: Secure connectivity to private apps when they are hosted in the same region


**2.2. When the private apps are hosted in different regions, availability zones (AZ), or cloud:**


In scenarios where the private application VPC and the GitHub Runner are distributed across distinct cloud providers, regions, or AZs, traffic travels from a service endpoint through the Zero Trust Gateway to the Zero Trust Exchange prior to private application access being authorized.


Figure: Secure connectivity to private applications when it is hosted in a different region
