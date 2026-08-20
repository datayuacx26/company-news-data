---
schema_version: "1.0.0"
document_id: "de03511bc1dadd6c42c4ab52a110b0455dd6a7e6847dd38f40aa111584318820"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/customer-managed-encryption-keys-cloud-database-cmek/"
published_at: "2026-06-11T14:24:18+00:00"
first_seen_at: "2026-07-22T13:37:02.744795+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:0d13a3880a4e492e8700cd65ea8b701864104addb2515fdd03341295791bb928"
---

# Cloud Database Encryption Keys and CMEK | SingleStore

# The Encryption Control Spectrum


## From platform-managed keys to full data sovereignty - what enterprise buyers need to understand about who actually controls their data.


Jun 11, 2026


•


7 min read


•


[Siqing Zheng](https://www.singlestore.com/blog/author/siqing-zheng/) ,


[Jay Bhatt](https://www.singlestore.com/blog/author/jay-bhatt/)


"We encrypt everything" is one of the most common statements in cloud vendor security documentation. It is also one of the least informative.


Encryption by itself tells you that data cannot be read without a key. What it does not tell you is who holds that key, who can access it, under what circumstances it can be revoked, and what happens to your data when it is. For most enterprise workloads, that question does not need to go further than the vendor's standard offering. For regulated industries, high-sensitivity data, and organizations with data sovereignty requirements, the answer is the entire conversation.


There is a spectrum of encryption control available in cloud database platforms, spanning platform-managed encryption, customer managed keys, cloud KMS integrations, database encryption, and encrypted cloud storage architectures. Understanding where a vendor sits on that spectrum - and where your workload needs to sit - is a prerequisite for a meaningful security evaluation.


## **How Platform-Managed Encryption Works**


In the standard configuration, SingleStore Helios encrypts all data at rest using AES-256, industry-standard encryption at rest using cloud-managed KMS keys (AWS KMS, Azure Key Vault, or GCP KMS). The encryption key is managed by the cloud provider's Key Management Service (KMS).


This means: the data is encrypted, the key is controlled by the cloud provider on your vendor's behalf, and access to the key is governed by the IAM policies of the cloud account that holds your data. For most enterprise workloads, this is a strong, well-understood security baseline.


This covers both block storage (persistent VM disks attached to cluster nodes) and object storage (S3, Azure Blob, or GCS buckets where database data and backups are held). Both storage types are encrypted by default across all three cloud providers - this is true whether you use platform-managed or customer-managed keys. What CMEK changes is not the presence of encryption, but who controls the keys doing the encrypting.


## **The Limits of Platform-Managed Encryption**


Platform-managed encryption is appropriate when you are comfortable with the following condition: the cloud provider - and by extension, your database vendor - has the technical ability to access the encryption key and therefore the data, subject to their own access controls and policies.


For many organizations, that condition is acceptable. For others, it is not - and the reasons are specific:


**Scenarios that require customer-managed keys**


-


**Data sovereignty requirements:**


Some regulatory environments or contractual obligations require that encryption keys never leave the customer's control.


-


**GDPR right to erasure:**


Revoking access to an encryption key provides a mechanism for cryptographic erasure - making data permanently inaccessible without physically deleting every copy. This is increasingly relevant for compliance with


[GDPR Article 17](https://gdpr.eu/article-17-right-to-be-forgotten/)


- **Sector-specific requirements:**


Certain financial services and healthcare regulations require demonstrable, independent control over the encryption keys protecting regulated data.


- **Contractual obligations:**


Some enterprise contracts with customers or partners require key sovereignty as a condition of data processing.


## **Customer-Managed Encryption Keys (CMEK) - What CMEK Actually Means**


Customer-Managed Encryption Keys (CMEK) - also referred to as Customer Managed Keys or bring your own key (BYOK) - is available in the Enterprise edition of SingleStore Helios. It allows customers to supply their own encryption key, managed through their cloud provider's key vault:


[AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) ,


[Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/) , or


[GCP KMS](https://cloud.google.com/kms/docs) .


A useful analogy: think of it as a safety deposit box at a bank. In the standard configuration, the bank holds a master key and can access the box on your behalf. CMEK is the equivalent of bringing your own key. The bank provides the vault and the box. You control the key. If you take the key away, nobody - not the bank, not the database vendor - can open the box.


With CMEK enabled, both the block storage volumes attached to the cluster and the object storage holding your data and backups can be encrypted under your key.


Here is the technical architecture that makes that guarantee work:


**Your key (KEK)**


You create a symmetric encryption key in your cloud provider's KMS. You retain control of this key. SingleStore never stores it, and it never leaves your cloud account.


**Key Encrypting Key**


Your key functions as a Key Encrypting Key (KEK). It does not directly encrypt your data - it encrypts the data encryption keys that do. This is standard envelope encryption architecture, consistent with NIST SP 800-57 guidelines.


**Data encryption keys**


The data encryption keys (DEKs) that directly encrypt your data at rest are generated and managed within your cloud provider’s KMS. SingleStore itself never touches the underlying encryption keys.


**Access at runtime**


SingleStore does not perform the encryption or decryption itself, and it never handles your key directly. SingleStore uses the reference to your key from your cloud provider, configures it on your resource, and calls the cloud provider’s API with it. The cloud provider then performs the cryptographic operation directly. If SingleStore cannot use your key, the related operations, such as reading or writing data, will fail.


**Revocation**


If you remove SingleStore's permission to use your KEK - or delete the key entirely - all DEKs wrapped by that key become permanently inaccessible. The data is there, but it cannot be read. By anyone.


## **The Sovereignty Guarantee - and What It Costs**


The revocation guarantee is real. If you revoke SingleStore's access to your KMS key, data protected by that key can no longer be decrypted, which makes it inaccessible until access is restored. This is the feature. It is what gives organizations with genuine sovereignty requirements the assurance they need.


But it carries an important caveat that any organization considering CMEK must understand clearly:


**Critical: the data loss risk of CMEK**


-


If you revoke SingleStore’s access to the KEK, SingleStore can no longer access the data. The encryption is working as designed, and there is no back door.


-


If you delete the key from your KMS, the data can never be decrypted. SingleStore has no recovery path, no copy of the key, and no ability to assist.


-


Deleted customer-managed keys are the customer's sole responsibility. This is not a support limitation - it is the architecture.


- Key management best practices per


[NIST SP 800-57](https://csrc.nist.gov/publications/detail/sp/800-57-part-1/rev-5/final) matter enormously here: use KMS rotation policies, maintain key backups where your provider supports them, implement deletion protection, and restrict key access to the minimum required set of identities.


## **How to Decide Which Encryption Model Is Right for You**


Most enterprise workloads do not require CMEK. The question is whether yours does:


- **Use platform-managed encryption if:**


your workload has standard enterprise data security requirements, your organization does not have specific key sovereignty obligations, and your cloud provider's KMS controls are sufficient for your compliance framework.


- **Consider CMEK if:**


you have regulatory, contractual, or governance requirements that mandate customer-controlled encryption keys; you handle data subject to GDPR erasure requests at scale; or your security policy requires the ability to unilaterally terminate all data access without vendor involvement.


- **Before enabling CMEK:**


Ensure your key management process is mature. Define who owns the key, what the rotation policy is, what approvals are required to revoke it, and what the escalation path is if a key is accidentally deleted.


Whether you choose platform-managed encryption or customer managed keys, understanding how cloud KMS services, cloud storage encryption, and database encryption contribute to overall data security is essential for making informed architecture decisions.


**Download the Full SingleStore Helios Cloud Security Whitepaper**


The SingleStore Helios Cloud Security White Paper covers the complete security architecture in depth - including platform architecture, network security, identity and access management, cryptography, logging and monitoring, SDLC practices, and incident management.


[Download the whitepaper](https://www.singlestore.com/resources/whitepaper-singlestoredb-cloud-security)


**This Article Is Part of a Series**


*Enterprise Security with SingleStore Helios - 7 articles exploring every layer of cloud database security.*


**


**1**


[The Compliance Question Enterprises Always Ask First](https://www.singlestore.com/blog/cloud-database-compliance-certifications)


**2**


[Enterprise Identity, Your Way](https://www.singlestore.com/blog/enterprise-database-sso-scim-identity-integration)


**3**


[Zero Trust Isn't a Checkbox](https://www.singlestore.com/blog/zero-trust-isnt-a-checkbox/)


**4**


[What Happens When Something Goes Wrong](https://www.singlestore.com/blog/cloud-database-disaster-recovery-rpo-rto)


**5**


**The Encryption Control Spectrum \[You are here\]**


**6**


[Why Shared Responsibility Isn't a Risk Transfer](https://www.singlestore.com/blog/cloud-database-shared-responsibility-model)


**7**


[Security Engineering, Not Just Security Features](https://blog/cloud-database-security-engineering-sdlc)


*Questions about security on SingleStore Helios? Contact*


security@SingleStore.com


## On this page


- How Platform-Managed Encryption Works
- The Limits of Platform-Managed Encryption
- Customer-Managed Encryption Keys (CMEK) - What CMEK Actually Means
- The Sovereignty Guarantee - and What It Costs
- How to Decide Which Encryption Model Is Right for You


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fcustomer-managed-encryption-keys-cloud-database-cmek%2F)


Last modified - June 24th, 2026


[Product](https://www.singlestore.com/blog/category/product/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Open Intelligence, Open Data: SingleStore delivers Zero-copy Compute on Apache Iceberg Product](https://www.singlestore.com/blog/open-intelligence-open-data-singlestore-delivers-zero-copy-compute-on-apache-iceberg/)


[Blog Security Engineering, Not Just Security Features Product](https://www.singlestore.com/blog/cloud-database-security-engineering-sdlc/)


[Blog Data Lakehouses Are Being Redefined by Real-Time Expectations Product](https://www.singlestore.com/blog/data-lakehouses-are-being-redefined-by-real-time-expectations/)


[Blog The Art of Possibility — Building the Enterprise Intelligence Plane Product](https://www.singlestore.com/blog/the-art-of-possibility-building-the-enterprise-intelligence-plane/)


[Blog Postgres Performance Issues and How to Scale Enterprise Databases Product](https://www.singlestore.com/blog/postgres-performance-issues-and-how-to-scale-enterprise-databases/)


[Blog Stop Waiting for the Future: Real-Time Data in One Platform Is Already Here Product](https://www.singlestore.com/blog/stop-waiting-for-the-future-real-time-data-in-one-platform-is-already-here/)
