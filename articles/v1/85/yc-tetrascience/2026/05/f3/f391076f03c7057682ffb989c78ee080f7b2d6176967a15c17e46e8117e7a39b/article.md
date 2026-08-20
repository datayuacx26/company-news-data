---
schema_version: "1.0.0"
document_id: "f391076f03c7057682ffb989c78ee080f7b2d6176967a15c17e46e8117e7a39b"
company_key: "yc-tetrascience"
company: "TetraScience"
source_id: "yc-tetrascience-news-import-63b926bd0a66"
canonical_url: "https://www.tetrascience.com/blog/tetra-os-platform-updates-help-scale-scientific-data-infrastructure-with-confidence"
published_at: "2026-05-06T00:00:00+00:00"
first_seen_at: "2026-07-22T16:09:26.237182+00:00"
fetched_at: "2026-07-28T22:12:46.079122+00:00"
content_hash: "sha256:9f4cf1f9d9d037c655bdaa656d07913ef77d5c27ae8182b0ab9c75ca42609c59"
---

# Tetra OS Platform Updates: Scale Scientific Data & Optimize Costs

*In this post we share how we are:*


1. *Giving scientists their own experience*
2. *Connecting the details of each run*
3. *Giving you workflow super-powers ( In Review → Approved states)*


Scaling scientific data is no small feat. Platform admins and IT teams are constantly juggling the need for total visibility without getting bogged down in manual work. At the same time, data engineers are trying to optimize compute costs without sacrificing speed, and developers need safe places to test new code before it hits production. We’ve been listening to these challenges and working closely with our partners to make these complex workflows much smoother.


Today, we're shipping **a platform update** that will reduce the time and risk associated with running scientific data infrastructure across the Tetra OS’ Scientific Data Foundry and Use Case Factory. Leading organizations are already using these new capabilities in production across discovery, development, and manufacturing.


The release introduces capabilities that fall into three core areas: **‍**


#### **1. Science at Your Fingertips**


Lab scientists now have a dedicated, role-appropriate interface to the Tetra OS. This streamlined experience offers simplified search, curated apps relevant to their work, and bulk file actions, removing platform administration noise. IT controls the UI version scientists see independently of platform upgrades. This reduction in friction increases data adoption, enabling the Tetra OS to deliver the scientific intelligence it was built for. **‍**


#### **2. Control Across the Deployment Lifecycle**


**Seeing What's Actually Deployed:** Scientific use cases span multiple artifacts (connectors, pipelines, agents, applications). **Use Case Manager** provides a single view showing how these artifacts fit together, tracking deployment status, and providing architecture documentation. Teams can define use cases as collections of artifacts and export definitions to help promote to other environments (e.g. migrate from DEV to TST and PROD).


**Admin-Controlled Artifact Version Management:** Tetra OS' **Artifact Version Manager** provides structured approval workflows with role-based permissions. Artifacts move through New → In Review → Approved → Active states, allowing administrators to roll back to previous versions when necessary. This ensures controlled, audit-ready software rollouts at enterprise scale.


**Testing Before Production:** To prevent buggy pipeline releases from breaking scientific workflows, the platform now supports **pre-release** versions for self-service pipelines, schemas, connectors, and data apps. Teams can publish and test prerelease pipeline artifacts (e.g., 1.2.0-beta.1) in controlled environments, promoting them to full release only when confident.


#### **3. Operational Confidence at Scale**


**Centralized Job Monitoring:** The new **Jobs Dashboard** provides centralized, real-time visibility into all Lakehouse and AI Services jobs. This includes status tracking, integrated logs, and performance metrics in one filterable, paginated table. Administrators can quickly drill down to specific execution logs and track performance patterns over time to optimize scheduling.


**Right-Sizing Compute Resources:** Data processing varies, requiring different compute resources for different workload types. Tetra OS’ new **Cluster Sizing** provides expanded configurability across all Lakehouse job types, ensuring right-sizing for performance and cost optimization. In early testing, one customer reduced their monthly compute spend by 28% while maintaining throughput.


**Platform Health Visibility:** Access to Tetra OS’ **Operational Insights** dashboard, showing platform adoption and usage metrics, is now democratized. Administrators can access the dashboard directly from the UI without external coordination, as the menu option appears for all customers by default. This provides instant visibility to monitor user engagement and platform ROI, ensuring your digital transformation stays on track.


### **Data Integrity and Workflow Completion**


The platform now supports more of the data lifecycle by allowing downstream analysis results to live alongside source data. **PROCESSED files** can be uploaded via API or the File Details page, automatically inheriting metadata to preserve data lineage. For example, this means a lab scientist can upload the final processed results of a chromatographic analysis via the File Details page, and the platform will automatically link it to the source RAW file, ensuring complete data lineage for future audits or AI/ML training. Furthermore, **Write-back capability** allows Data Apps to write processed results back as managed tables. This turns in-app analysis into a permanent, searchable, lineage-tracked part of the scientific record, enabling end-to-end automated workflows.


### **Vocabulary as Infrastructure**


Consistent metadata is the foundation of trustworthy scientific data. **Attribute Management** now displays both customer-defined and TetraScience-defined value sets, using a Publisher column to identify the origin of each label. This establishes the foundation for a shared scientific vocabulary where terms like "Sample Type: Plasma" mean the same thing across all of your data systems, enabling cross-study scientific intelligence.


### **Getting Started**


These updates are part of **Tetra Data Platform v4.5** . The updates ship on April 29, 2026. Components marked as Limited Availability in the release notes (Scientist UX, Use Case Manager, Artifact Version Manager, Data Apps Write-back, Prerelease Pipelines) are available by contacting your customer account leader. All other capabilities are generally available to all customers.


Complete technical details, including GxP impact assessment, known issues, and upgrade considerations, are in the[full release notes](https://docs.google.com/document/d/1OIejqL9XH5JQdjNxnSazbn03bvygjbtf8mDeQ0_4JUM/edit?tab=t.0) .


We built these capabilities based on direct input from platform administrators, data engineers, Scientific IT teams, and developers. We build the operating system for Scientific Intelligence by systematically removing friction from production deployments.


‍ *Questions about this release? Contact your customer account leader. For technical documentation, see the*[TetraScience Help Center](https://help.tetrascience.com/) *.*
