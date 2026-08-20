---
schema_version: "1.0.0"
document_id: "ffe3f66fcdd732be151a70124d841d41f446957b70f11c6daca3f15d7e4cc3c1"
company_key: "avepoint-inc-class-a-common-stock"
company: "AvePoint Inc."
source_id: "avepoint-inc-class-a-common-stock-news-import-1c9c9e9520bc"
canonical_url: "https://www.avepoint.com/blog/protect/avepoint-and-native-express-protection-entra-id"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T12:04:13.404697+00:00"
fetched_at: "2026-08-19T12:04:14.249644+00:00"
content_hash: "sha256:77df9d510ada51cb83836abd18255324a44896635be3d8140d4c989ec30be1a9"
---

# AvePoint and Native: Express Protection for Entra ID

Identity is the control plane of your business. When users can’t sign in, applications lose trust or policies stop working correctly. The impact is immediate. Microsoft detects an average of[38 million identity threats per day](https://www.microsoft.com/en-us/corporate-responsibility/topics/cybersecurity/reports/microsoft-digital-defense-report-2025/?msockid=00a96fda8437626707fd795185aa6376) , underscoring why identity resilience has become a foundational part of business continuity planning.


Protecting your identity framework should be the first step in resilience planning, and that’s why Microsoft’s new native Entra backup and recovery capability is such an important development. It represents meaningful progress in helping organizations recover identity data faster after an incident, especially when time matters most.


A question customers ask is, *“If Microsoft can back up my Entra ID data, why do I still need AvePoint?”*


Rather than viewing native recovery and third-party backup as competing approaches, the strongest resilience strategy is one that brings both together: Microsoft’s native recovery speed combined with AvePoint’s broader protection, longer retention, and operational flexibility. Together, they help organizations recover faster, bring back your Minimum Viable Company, and better align identity protection to real-world business and compliance needs.


## What Microsoft’s Native Solution Brings to the Table


Microsoft’s native Entra ID recovery capability is designed to help restore identity data quickly at scale. This solution was built with one thing in mind – speed. For organizations facing a large-scale data loss event, that speed can be incredibly valuable.


Microsoft’s solution, like AvePoint’s, helps to protect core Entra ID objects such as:


- Users
- Groups
- Applications
- Access Policies
- Named Locations
- Authentication Method Policies


For the right recovery scenario, native recovery offers a fast path to getting critical identity services back online. In situations where mass recovery is the goal, where speed and continuity need to be prioritized first, this new functionality is essential. Native Entra ID backup was purpose-built for mass data loss scenarios and for large, enterprise-sized tenants. In scenarios where organizations need to recover a large number of objects and need to do so quickly, Microsoft’s first-party offering is a very compelling option.


### What Native Entra ID Backup Doesn’t Cover


Native recovery is a strong foundation, but it was optimized for speed and for large-scale data loss scenarios. Comprehensive Entra ID protection requires more than speed alone. That’s where[Cloud Backup for Entra ID](https://www.avepoint.com/products/cloud-backup/microsoft-entra-id) starts to extend native capabilities.


### 1. Protected Objects


Native protection was designed to protect a core selection of Entra ID objects, with a greater focus on the identity graph and policies that make up Entra. There are, however, more components that make up a full Entra ID environment, components that AvePoint’s solution can protect.


For full Entra ID coverage, neither solution is enough in isolation — customers need both native and AvePoint Cloud Backup for Entra ID:


**Object** **Native Entra ID Backup** **AvePoint Cloud Backup**


User ✅


✅


Group ✅


✅


Application ✅


✅


Service Principal ✅


✅


Authentication Process ✅


✅


Conditional Access Policies ✅


✅


Named Location Policies ✅


✅


Authorization Policies ✅


❌


Administrative Units ❌


✅


Roles and Administrators ❌


✅


Devices ❌


✅


*BitLocker Recovery Keys can be protected with Cloud Backup*


Audit Logs and Sign-In Logs ❌


✅


### 2. Deleted Objects Recovery


Native recovery for Entra ID objects is limited to those that have only been modified or that have been “soft-deleted”. That’s to say, any objects that have been deleted but still exist in your recycle bin. Anything that has been “hard deleted” – removed from the recycle bin – cannot be recovered with the native feature.


AvePoint Cloud Backup supports recovery of objects regardless of their status. Even if Entra ID objects were deleted months ago, AvePoint’s solution can help you recover the exact version you need.


### 3. Backup Frequency and Scheduling


Native Entra backup restricts organizations to a single backup job per day. For those massive data loss scenarios, this isn’t a dealbreaker; when recovering mass amounts of data, speed is often favored over granularity and flexibility. For operational recovery, though, once-daily backups may not be enough. Identity data changes constantly, and users or applications can have multiple versions within a 24-hour period. That’s why AvePoint Cloud Backup supports up to four incremental backup jobs per day. When it comes to day-to-day restores, the ability to choose specific versions of an object makes all the difference.


### 4. Retention Limitations


With the native solution, customers can retain up to seven days of backup history. When it comes to disaster recovery, that limited retention is usually enough. If you’re rolling back entire tenants or recovering thousands of objects, you often don’t need to go back in time more than a day or two. However, for organizations with long-term retention requirements, this doesn’t fit the bill.


AvePoint Cloud Backup supports flexible retention for Entra ID data, from one year to unlimited retention, depending on how the deal was structured. Customers with strict retention requirements, whether based on regulatory compliance or internal preferences, can hold onto data for as long as needed.


### 5. Independent Copy and Storage Flexibility


Native Entra backup stores backup copies within the Microsoft trust, in the same geo-location as the protected Entra tenant. This helps the solution achieve faster recovery speeds, but it means there is no independent copy of data outside of Microsoft’s infrastructure.


With AvePoint Cloud Backup, customers have their backup stored in a separate location. The solution supports AvePoint-hosted storage options (Azure Blob Storage, Google Cloud Storage, or Amazon S3), as well as customer hosted Bring Your Own Storage (BYOS) support for the following storage types:


- Microsoft Azure Blob Storage
- Amazon S3 Storage
- Amazon S3-Compatible Storage
- FTP
- SFTP
- IBM Storage Protect - S3
- IBM Cloud Object Storage
- Google Cloud Storage


This independent backup copy, when paired with native Entra protection, can be an essential piece of regulatory compliance. With this, customers can more easily implement a[3-2-1 backup strategy](https://www.avepoint.com/blog/backup/3-2-1-backup-rule) for their critical identity data.


## Speed from Microsoft. Depth and Flexibility from AvePoint. Express Protection with Both.


Entra data is some of the most critical in a modern enterprise. Often, the first point of entry for an attacker is an organization’s identity deployment, and because identity often governs access to the rest of the cloud environment, many recovery plans – including the[AvePoint Rapid Recovery System](https://www.avepoint.com/blog/backup/introducing-avepoint-rapid-recovery-system-business-continuity) – prioritize restoring identity services early on. That’s why organizations shouldn’t think about AvePoint vs Native for Entra protection. There doesn’t need to be a tradeoff between speed and flexibility.


In practice, most organizations need a combination of capabilities:


- Fast recovery for large-scale incidents
- Granular restore options for everyday operational issues
- Longer retention for audit, investigation, and compliance
- Broader object and attribute coverage
- A consistent recovery experience across workloads


This broad coverage and the ability to confidently recover from any type of Entra outage only comes when AvePoint Cloud Backup and Native Entra protection work together.


## AvePoint and Native Integration: The Better Together Story in Action


Most organizations do not want to choose between recovery speed and recovery flexibility. They want access to both.


That's why AvePoint Cloud Backup for Entra ID has integrated directly with Native Entra Recovery to bring the Express Recovery story to identity workloads. Rather than forcing administrators to work across multiple consoles and disconnected recovery workflows, AvePoint can surface native Microsoft recovery points directly within the Cloud Backup solution, allowing teams to see and leverage both protection layers from a single interface. Organizations using both solutions have the freedom to choose their recovery point and restore strategy based on their needs:


- **Use Entra Express Recovery** for large-scale data loss events and rapid recovery
- **Use AvePoint Cloud Backup** for day-to-day recovery, independent data copies, and long-term retention
- **Use Both** for fast rollback, 3-2-1 style backup strategies, and complete Entra resilience


By bringing native Microsoft recovery points and AvePoint Cloud Backup together in a single experience, organizations no longer have to choose between recovery speed and recovery flexibility. Instead, they gain a recovery strategy that combines Microsoft's speed with AvePoint's depth, delivering a more resilient approach to protecting the identity layer that modern business depends on.
