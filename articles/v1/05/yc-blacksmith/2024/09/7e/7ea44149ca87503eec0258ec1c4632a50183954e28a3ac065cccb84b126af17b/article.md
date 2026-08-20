---
schema_version: "1.0.0"
document_id: "7ea44149ca87503eec0258ec1c4632a50183954e28a3ac065cccb84b126af17b"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/blacksmith-achieves-soc-2-type-2-compliance"
published_at: "2024-09-19T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:5805c0b84e175baaae2e036e6a1822aae4341caada858812e85f6839f763b67e"
---

# Blacksmith achieves SOC 2 Type 2 compliance

At Blacksmith, we're committed to providing our customers with the highest level of security and trust. We're proud to announce that Blacksmith has successfully passed its SOC 2 Type 2 audit and observation period. This achievement builds upon our[SOC 2 Type 1 compliance](https://www.blacksmith.sh/blog/blacksmith-is-soc2-type-1-compliant) , which we attained in early May 2024, demonstrating our ongoing dedication to maintaining robust security practices.


## What is SOC 2 Type 2?


SOC 2 is one of the world's highest recognized standards for information security compliance. Developed by the American Institute of CPAs (AICPA), it allows an independent third-party auditor to validate a service company's internal controls with respect to information security.


While SOC 2 Type 1 assesses the design of security processes at a specific point in time, Type 2 reports on the operational effectiveness of those controls over a period of time (typically 6-12 months). This provides a higher level of assurance to our customers about the consistent application of our security practices.


## Our commitment to security


Achieving SOC 2 Type 2 compliance is a significant milestone in our ongoing commitment to security. Blacksmith has been built from the ground up with security in mind, and we've implemented several advanced measures to protect our customers' data and workflows:


1. **Ephemeral VMs with Firecracker** : We use Firecracker to manage ephemeral virtual machines for executing GitHub Actions. Firecracker, maintained by AWS, provides KVM hardware isolation and runs on a memory-safe stack. This technology, which powers millions of untrusted workloads for AWS Lambda and Fargate, ensures that each job runs in complete isolation, with all state destroyed upon completion.
2. **Minimal Data Retention** : We store only essential metadata related to job executions. Our GitHub app doesn't have access to your secrets, ensuring that sensitive information remains protected.
3. **Access Control** : We maintain rigorous access management policies and procedures to ensure that only authorized personnel can access critical systems and data.
4. **Incident Response** : We have a robust incident response plan in place to address any potential security issues promptly and effectively.
5. **Disaster Recovery** : We've implemented a comprehensive disaster recovery plan for our database and backend instances to safeguard against potential outages and ensure business continuity.


## What this means for our customers


Our SOC 2 Type 2 compliance provides our customers with:


- Assurance that we have rigorous security controls in place
- Confidence in our ability to protect their sensitive information
- Validation of our commitment to maintaining the highest standards of security and compliance


## Looking ahead


While achieving SOC 2 Type 2 compliance is a significant accomplishment, we view it as just one step in our ongoing journey to provide the most secure and reliable service possible. We are committed to continually improving our security posture and will undergo regular audits to maintain our compliance.


For more information about our security practices or to request a copy of our SOC 2 Type 2 report, please contact us athello@blacksmith.sh .
