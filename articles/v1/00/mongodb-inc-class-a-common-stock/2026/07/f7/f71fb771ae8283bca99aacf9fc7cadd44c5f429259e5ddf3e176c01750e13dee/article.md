---
schema_version: "1.0.0"
document_id: "f71fb771ae8283bca99aacf9fc7cadd44c5f429259e5ddf3e176c01750e13dee"
company_key: "mongodb-inc-class-a-common-stock"
company: "MongoDB Inc."
source_id: "mongodb-inc-class-a-common-stock-news-import-efe1743dc302"
canonical_url: "https://www.mongodb.com/company/blog/product-release-announcements/mongodb-atlas-delivers-greater-capacity-reliability-operational-confidence"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-24T04:07:24.248205+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:a121b25279f31d24b47c78127275508c780c3f19c1e8851c8fcdf87e216e60a1"
---

# MongoDB Atlas Delivers Greater Capacity, Reliability, and Operational Confidence

Production reliability is rarely lost in a single dramatic moment. More often, it is challenged by the realities operators face every day: cloud capacity constraints that delay critical scaling operations or the need to recover a single collection without disrupting an entire environment.


To help organizations navigate these challenges,[MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database) is introducing two new capabilities to strengthen production resilience:[Adaptive Capacity](https://www.mongodb.com/docs/atlas/reference/microsoft-azure/#adaptive-capacity) , now generally available for supported Azure Atlas clusters, and[Collection- and Database-Level Restores](https://www.mongodb.com/docs/atlas/backup/cloud-backup/restore-from-db-coll/?interface-default-atlas-cli=admin-api&restore-type=snapshot) , now generally available across Atlas.


Together, these capabilities help organizations remain resilient when infrastructure constraints arise and recover data with greater precision when issues occur.


## Scale with confidence, even when cloud capacity is constrained


Cloud capacity constraints are an industry-wide reality, and MongoDB Atlas continues to add flexibility and operational safeguards to help organizations scale more predictably when they occur. For supported MongoDB Atlas clusters on Microsoft Azure,[Adaptive Capacity](https://www.mongodb.com/docs/atlas/reference/microsoft-azure/#adaptive-capacity) automatically uses qualified alternative instance families when preferred capacity is unavailable, enabling new cluster provisioning and cluster scaling to continue with fewer disruptions. Once the preferred capacity becomes available again, Atlas automatically returns clusters to the standard hardware.


The result is greater business continuity: organizations can keep critical applications available, reduce delays to infrastructure operations, and continue scaling to meet demand despite underlying cloud capacity constraints. By absorbing more of the complexity of cloud infrastructure availability, Atlas minimizes operational interruptions and frees infrastructure teams to focus on higher-value work instead of troubleshooting capacity shortages.


Adaptive Capacity also includes built-in safeguards. Atlas maintains visibility into these transitions and prioritizes safe operations when workloads are already under pressure, delivering greater resilience without sacrificing operational awareness or control. Adaptive Capacity is enabled by default for supported Atlas tiers on Azure at no additional cost.


## Minimize interruption with targeted recovery


When a data issue affects a single collection or database, restoring an entire cluster can be unnecessarily disruptive. Historically, organizations have had to choose between time-consuming manual recovery processes and broad environment-level restores that impact applications beyond the affected data. That approach is increasingly difficult to justify in modern production environments, particularly in multi-tenant architectures where a localized issue should not disrupt unrelated users and workloads.


With[Collection- and Database-Level Restores](https://www.mongodb.com/docs/atlas/backup/cloud-backup/restore-from-db-coll/?interface-default-atlas-cli=admin-api&restore-type=snapshot) , Atlas provides a more targeted approach to recovery. Administrators can restore only the specific collection or database directly into a live Atlas cluster through the Atlas UI or API, reducing the recovery blast radius while keeping unaffected applications online.


By limiting recovery to only the affected data, organizations can simplify recovery workflows, reduce operational complexity, and minimize disruption. Database administrators and DevOps teams spend less time managing recovery operations, while architects building multi-tenant applications can recover individual tenants without affecting neighboring workloads. The result is a more precise, efficient recovery experience that aligns with the needs of modern production environments.


## Reliability extends beyond uptime


Operational confidence comes from more than simply keeping systems online. Organizations also need the ability to scale when demand changes, recover efficiently when issues occur, and manage operational changes with consistency and control.


Adaptive Capacity helps organizations continue provisioning and scaling during temporary Azure infrastructure capacity shortages, reducing operational delays caused by cloud infrastructure availability. Collection- and Database-Level Restores enable more precise recovery when data issues arise. Together, these capabilities make it easier to maintain availability while reducing operational overhead.


This becomes increasingly important as applications grow more distributed, customer expectations continue to rise, and platform teams are asked to support greater scale without introducing additional risk.


## Built for modern production operations


Every production environment faces unexpected challenges, whether related to infrastructure availability or data recovery. With[Adaptive Capacity](https://www.mongodb.com/docs/atlas/reference/microsoft-azure/#adaptive-capacity) and[Collection- and Database-Level Restores](https://www.mongodb.com/docs/atlas/backup/cloud-backup/restore-from-db-coll/?interface-default-atlas-cli=admin-api&restore-type=snapshot) now generally available, MongoDB Atlas provides organizations with greater resilience, more precise recovery options, and stronger operational confidence.


And this is just the beginning. Adaptive Capacity is currently available for Azure, with support for AWS and GCP coming soon, extending these resilience capabilities across the major cloud providers.


Because reliability is about more than keeping systems running. It is about helping organizations adapt, recover, and move forward with confidence.


###### Next Steps


To learn more, explore the MongoDB Atlas documentation for[Collection- and Database-Level Restores](https://www.mongodb.com/docs/atlas/backup/cloud-backup/restore-from-db-coll/?interface-default-atlas-cli=admin-api&restore-type=snapshot) and[Adaptive Capacity](https://www.mongodb.com/docs/atlas/reference/microsoft-azure/#adaptive-capacity) .


Ready to start building?[Register for a free MongoDB Atlas account today](https://www.mongodb.com/cloud/atlas/register) .
