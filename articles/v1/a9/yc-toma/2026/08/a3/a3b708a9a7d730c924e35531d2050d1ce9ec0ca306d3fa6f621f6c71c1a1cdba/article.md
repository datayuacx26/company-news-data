---
schema_version: "1.0.0"
document_id: "a3b708a9a7d730c924e35531d2050d1ce9ec0ca306d3fa6f621f6c71c1a1cdba"
company_key: "yc-toma"
company: "Toma"
source_id: "yc-toma-rss-1eeeda85d3f8"
canonical_url: "https://www.toma.com/blog/iso-27001-certification"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:33.368971+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:a852bee49cbdfc00e94d9c6e9a492f17c2b7fd68a6d077c639efac8963d01e44"
---

# Toma is now ISO 27001 certified

[← Blog](https://www.toma.com/blog)


# Toma is now ISO 27001 certified


Toma is now ISO/IEC 27001:2022 certified. Here's what the standard checks and how we protect the data your dealership trusts us with.


By Anthony · May 2026


Dealerships hand Toma some of their most sensitive data: customer names, phone numbers, service history, and a recording of every call. Protecting it is the job behind the job.


Toma is now certified to ISO/IEC 27001:2022, the international standard for information security. An independent auditor, Prescient Security, reviewed how we manage risk, control access, and protect data, and confirmed we meet the standard. It joins our SOC 2 Type 2 report and our work to comply with GDPR.


## What ISO 27001 actually checks


ISO 27001 is not a one-time box to tick. It certifies that a company runs a working information security management system: the policies and controls for handling risk, access, vendors, and incidents, with evidence that they operate every day. An outside auditor tests that evidence before granting the certificate, then again each year to keep it.


Our audit ran in two stages across April and May 2026. The auditor found no deviations and recommended Toma for certification.


## How we protect your data


A certificate means little without the controls behind it. Here is what we do.


**Encryption.** Customer data is encrypted at rest with AES-256 and in transit with TLS 1.2 or higher. That covers call audio moving across telephony networks, not only our APIs.


**Access.** Every person and every service gets the minimum access the job needs, and nothing more. Multi-factor authentication is required for anything that touches production or customer data, and we don't allow shared accounts.


**Monitoring.** We watch production around the clock with automated alerting, and a security alert puts an on-duty engineer on it right away. Security events are logged and kept for 5 years, so we can reconstruct what happened if we ever need to.


## Where your data lives


Toma runs entirely on Amazon Web Services in US data centers. Access to production systems from outside the US is prohibited.


Each customer's data is isolated from every other customer's. Staff at one dealership can see their own customers and no one else's. Critical systems run across multiple availability zones, and backups are encrypted and stored separately, so an outage in one place doesn't take your data with it.


## We don't train models on your data


Your calls and customer records are yours. We do not use them to train models, ours or anyone else's. Our contracts with the AI, speech-to-text, and text-to-speech providers behind Toma bar them from keeping or training on your data. You sign one agreement, with Toma, and we stand behind every vendor in the chain.


We're also now working toward ISO 42001, the standard for managing AI systems responsibly. More on that when it's done.


## Get the report


Customers and prospects can request our ISO 27001 certificate, SOC 2 Type 2 report, and penetration test results under NDA through our trust center at[trust.toma.com](https://trust.toma.com/) . If you're evaluating Toma, that's where your security and procurement teams should start.


## FAQ


**Is my data encrypted?** Yes. At rest with AES-256, and in transit with TLS 1.2 or higher, including call audio.


**Where is my data stored?** On AWS, in US data centers. Access from outside the US is prohibited.


**Do you use my data to train AI models?** No. We don't train on customer data, and our vendor contracts prohibit it too.


**Can I delete my data?** Yes. Emailsupport@toma.com and we delete everything we're not legally required to keep, within 30 days.
