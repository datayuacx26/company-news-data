---
schema_version: "1.0.0"
document_id: "764d6e8b787016ebda06e0074e2db0581c2266585ab6c492c7c619c0524fcb7d"
company_key: "yc-toma"
company: "Toma"
source_id: "yc-toma-rss-1eeeda85d3f8"
canonical_url: "https://www.toma.com/blog/pci-dss-certification"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:33.368971+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:ae3c07966cb5f9fb395b208c47f6a817dbfb0c7a9606b337f4ef54df524f6113"
---

# Toma is now PCI DSS certified

[← Blog](https://www.toma.com/blog)


# Toma is now PCI DSS certified


As of July 16, 2026, Toma is certified to the PCI DSS standard. Here's what that covers and how we protect card data when a caller pays over the phone.


By Anthony · July 2026


When a customer calls a dealership to pay a service bill or put down a deposit, a card number changes hands. That number is some of the most sensitive data a caller can give you, and it now moves through an AI on the call. Protecting it is not optional.


As of July 16, 2026, Toma is certified to the Payment Card Industry Data Security Standard (PCI DSS v4.0.1). An independent assessor reviewed how we handle, transmit, and protect cardholder data and confirmed we meet the standard. It joins our SOC 2 Type 2 report, our ISO 27001 certification, and our work to comply with GDPR.


## What PCI DSS actually checks


PCI DSS is the security standard every company that touches payment card data has to meet. It sets rules for how card numbers are stored, encrypted, transmitted, and access-controlled, and it requires evidence that those controls run every day, not just on audit day. An assessor tests that evidence before validating compliance, then again each year to keep it.


The standard is specific about one thing above all: the less card data you keep, the smaller the risk. So the first question an assessor asks is not "how do you protect the data" but "why do you have it at all."


## How we protect card data


**We keep as little as possible.** The strongest control is not holding card numbers in the first place. Where a payment flows through Toma, we minimize what touches our systems and hand card data to certified payment processors rather than storing it ourselves.


**Encryption.** Cardholder data is encrypted at rest with AES-256 and in transit with TLS 1.2 or higher. That covers card details spoken on a call, not only what moves across our APIs.


**Access.** Every person and every service gets the minimum access the job needs, and nothing more. Multi-factor authentication is required for anything that touches production or payment data, and we don't allow shared accounts.


**Monitoring.** We watch production around the clock with automated alerting, and a security alert puts an on-duty engineer on it right away. Security events are logged and kept for 5 years, so we can reconstruct what happened if we ever need to.


## We don't train models on your data


Your calls and customer records are yours, and that includes anything a caller says about a payment. We do not use them to train models, ours or anyone else's. Our contracts with the AI, speech-to-text, and text-to-speech providers behind Toma bar them from keeping or training on your data. You sign one agreement, with Toma, and we stand behind every vendor in the chain.


We're also working toward ISO 42001, the standard for managing AI systems responsibly. More on that when it's done.


## Get the report


Customers and prospects can request our PCI DSS attestation, SOC 2 Type 2 report, ISO 27001 certificate, and penetration test results under NDA through our trust center at[trust.toma.com](https://trust.toma.com/) . If you're evaluating Toma, that's where your security and procurement teams should start.


## FAQ


**Does Toma handle payments over the phone?** Where a dealership uses Toma to take a payment, card data is handled under PCI DSS and passed to certified payment processors.


**Is card data encrypted?** Yes. At rest with AES-256, and in transit with TLS 1.2 or higher, including card details spoken on a call.


**Do you use my data to train AI models?** No. We don't train on customer data, and our vendor contracts prohibit it too.


**Can I delete my data?** Yes. Emailsupport@toma.com and we delete everything we're not legally required to keep, within 30 days.
