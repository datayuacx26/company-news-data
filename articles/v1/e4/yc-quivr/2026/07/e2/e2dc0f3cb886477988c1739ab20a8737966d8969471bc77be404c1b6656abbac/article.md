---
schema_version: "1.0.0"
document_id: "e2dc0f3cb886477988c1739ab20a8737966d8969471bc77be404c1b6656abbac"
company_key: "yc-quivr"
company: "Quivr"
source_id: "yc-quivr-news-import-c3f3e0926723"
canonical_url: "https://www.quivr.com/blog/quivr-is-soc2-compliant"
published_at: null
first_seen_at: "2026-07-27T04:35:10.435547+00:00"
fetched_at: "2026-07-28T21:16:46.713740+00:00"
content_hash: "sha256:ece1fb3bea5077ef24f724be3a289814dbe18c2deaae4fc0bd1826d4ccfe6ad3"
---

# What is SOC2?

Feature


### Quivr is SOC2 Compliant


Quivr is now SOC2 Type 1 compliant. Let's explore what this means and walk through the process we followed to achieve compliance. If you're building a SaaS product, this blog post will serve as a practical guide to obtaining your SOC2 certification.


# **What is SOC2?**


SOC2 is a compliance standard developed by the American Institute of CPAs (AICPA). Yes, you read that right—accountants created what has become the leading security standard for SaaS companies. SOC2 evaluates companies based on five Trust Services Criteria: security, availability, processing integrity, confidentiality, and privacy. Companies can achieve two types of compliance, each requiring an external audit:


- Type 1: checks if a company is compliant at a point in time.
- Type 2: checks if a company stays compliant during an observation window (typically 6 months to a year).


# **Why did we get SOC2 certified?**


The main reason to pursue SOC2 certification is straightforward—customers demand it. Our team was spending significant time completing security questionnaires and having calls with potential customers to explain our security practices and data protection measures. As a RAG company, we handle sensitive customer data, which made SOC2 certification a natural priority since it specifically addresses data handling practices. It's also far more efficient to implement and maintain SOC2 compliance by establishing proper standards and practices from the beginning.


# **Choosing a compliance monitoring tool**


Several tools integrate with cloud providers, task management systems, identity providers, and HR systems to simplify monitoring, collecting, and submitting evidence to auditors. We evaluated[Vanta](https://www.vanta.com/) and[Oneleet](https://www.oneleet.com/) . Our stack is fairly standard (AWS, GSuite, and Github), and both tools integrated well with these systems. During evaluation, we found some tools requested excessive permissions when integrating with our vendors (like read/write access to Github), which immediately disqualified them. Some offered a lightweight agent for Mobile Device Management (MDM), but otherwise, the products were quite similar. We ultimately chose Oneleet, with Vanta as a close runner-up. Oneleet offered broader compliance standard coverage at the time and, as a fellow YC company, felt like a natural fit. Vanta's poor support reviews from early YC founders also influenced our decision. Today, the feature gap between these tools has narrowed significantly, as they all support the compliance standards we're targeting.


# **Choosing an Auditor**


After setting up Oneleet (which involved persuading developers to install a security agent on their laptops) and completing the SOC2 task checklist, we began our search for auditors. Oneleet simplified this process—they handled everything, and we didn't even need to meet the auditor directly. Since our auditors were already familiar with Oneleet's system, this streamlined the audit process. The process began with an initial call to review which systems would be included in the audit's scope. We then established a specific audit date. This Type 1 audit would verify our compliance with stated policies at that particular moment in time.


# **Off to the races**


Once the audit date was set, we rushed to get everything in order. Oneleet has a lightweight, read-only agent that verifies important security features for all team members, such as hard disk encryption. While it supports Mac, Windows, and Linux, hard disk encryption remains challenging on some Linux distributions. To simplify this, we standardized on Mac and Windows. Though a more powerful Mobile Device Management (MDM) solution would allow remote wiping of stolen computers, we determined our existing protections were adequate. We developed several essential policies, including Access Control, Data Management, and Asset Management. Oneleet provided templates that we customized to our needs. Since our auditors were familiar with these templates, the policy language review went smoothly. We provided comprehensive evidence of our cloud activity monitoring. Our Cloudwatch logs are retained for at least a year, and we monitor Cloudtrail logs with Slack notifications for critical events—such as root AWS account logins, public bucket creation, and IAM policy changes. Enabling GitHub's branch protection ensures code can't be merged without prior review. We had already implemented other security measures: data encryption at rest and in transit, MFA enforcement where possible, password manager standardization, and elimination of long-lived SSH keys. Finally, we evaluated all vendors with access to confidential data—now it was our turn to request SOC2 reports and send security questionnaires 😈..


# **Making the SOC2 process work for you**


We made a conscious effort to avoid implementing processes solely to meet SOC2 requirements. Each addition needed to meaningfully improve our security or reduce risk. This often meant working with auditors to demonstrate how we satisfied controls in our own way, even if it differed from their standard approach. We leveraged the SOC2 process to formalize the security practices we were already following informally at Quivr. We did encounter some unavoidable "check-the-box" requirements—security awareness training being a prime example. Despite our team's strong security practices, we still had to complete these formalities, even though watching videos about password safety seemed redundant for our security-conscious team.


# **A few surprises**


We were surprised by the amount of manual evidence we had to collect—yes, including screenshots—even with a tool like Oneleet. Some things simply can't be automated. For example, we had to provide evidence of proper onboarding and offboarding checklists, and demonstrate how we resolve security issues from penetration tests within specific timeframes. Since Type 1 audit only requires evidence at a single point in time, a tool like Oneleet offers limited value. However, for Type 2 certification, which requires proving ongoing compliance, a monitoring tool becomes essential. If you're pursuing multiple certifications, these tools are particularly valuable since frameworks overlap significantly—you'll only need to collect evidence once. Interestingly, many controls that other companies described as challenging during their SOC2 journey weren't blockers for us. The specific requirements depend on your systems' scope, architecture, and auditor. The key lesson? Connect with your auditor early to understand what's relevant for your audit. Unlike pentesters or bug bounty hunters, auditors take a more collaborative approach. When you show them a PR that addresses a security issue, they *trust* your implementation. They won't try to bypass your fix to find exceptions. This highlights why SOC2 certification is just one piece of security—what satisfies an auditor often differs from what would impress a penetration tester.


# **Security Center**


Today we are launching our[security center](https://trust.quivr.com/) . This portal showcases our data protection measures and provides access to our audit reports.


# **What's Next**


SOC2 certification is just the beginning. We're now working toward HIPAA certification and would welcome any discussions about our security initiatives. In the coming months, we'll be implementing additional security enhancements to Quivr.
