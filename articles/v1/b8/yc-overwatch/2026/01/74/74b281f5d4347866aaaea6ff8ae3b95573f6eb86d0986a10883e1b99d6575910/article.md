---
schema_version: "1.0.0"
document_id: "74b281f5d4347866aaaea6ff8ae3b95573f6eb86d0986a10883e1b99d6575910"
company_key: "yc-overwatch"
company: "Overwatch"
source_id: "yc-overwatch-news-import-3db2f07671db"
canonical_url: "https://www.overwatchdata.ai/blog/comparing-fraud-taxonomies"
published_at: "2026-01-29T00:00:00+00:00"
first_seen_at: "2026-07-22T07:59:37.209926+00:00"
fetched_at: "2026-07-28T22:22:56.687510+00:00"
content_hash: "sha256:bb2951bd5d26bd45b31a6a510f542767e6a5dc1a85726f46a82588ce2955fee7"
---

# Comparing Fraud Taxonomies and How to Use Them

By Arjun Bisen, CEO & Co-Founder | January 29, 2026


‍


I’ve been in many conversations with cyber, intel, and fraud experts where they’ve praised taxonomies like the MITRE ATT@CK framework and sharing of Indicators of Compromise (IOCs) that are commonly understood in cyber intelligence and wished that the fraud taxonomy was as well developed. That would allow them to throw all the fraud accounts/ IOCs into a centralized system to take automated action/ scoring.


That said, there are some very useful fraud taxonomies that are being worked on by some of the best in the business, which could have a big impact on how we organize. Each of the major frameworks focus on solving a different problem.


The[Fed’s FraudClassifier](https://fedpaymentsimprovement.org/strategic-initiatives/payments-security/fraudclassifier-model/) has become a foundational standard for banks and payment providers. Its strength is simplicity: a clear, payment-agnostic way to distinguish authorized vs. unauthorized fraud and report losses consistently across the industry.


The **Fed’s ScamClassifier** builds on this by addressing a major blind spot, authorized scams. By breaking scams into concrete types (romance, investment, impersonation, etc.), it added much-needed clarity to an area that was previously vague, on the rise, and inconsistently labeled. The Fed has a[useful guide](https://www.frbservices.org/news/fed360/issues/071625/industry-perspective-institutions-use-scamclassifier-model) on how to leverage this classifier.


The most tactical of all, **the FT3 framework,** built by[Vincent Passaro](https://www.linkedin.com/in/vincentpassaro/) and the good people at[Stripe](https://www.linkedin.com/company/stripe/) , takes a very different approach. Modeled after cybersecurity frameworks like MITRE ATT&CK, it classifies fraud by the tactics and techniques used by attackers. This is powerful for detection and threat intelligence, but also more complex to operationalize. However, as a technology company, we’ve found useful ways to leverage their[Github repo](https://github.com/stripe/ft3) to automate the classification fraud materials, leveraging our AI agents. We’re always happy to share our approach with anyone interested.


Finally,[FS-ISAC](https://www.linkedin.com/company/fs-isac/) **’s Cyber Fraud Prevention Framework** focuses less on classification and more on coordination. By mapping fraud across stages, from reconnaissance to monetization, it helps break down silos between fraud, cyber, and AML teams. It's especially useful and organizing info sharing via FS-ISAC's platform.


**The takeaway:** there’s no single “best” taxonomy. The strongest fraud programs combine:


• Fed models for standardization and reporting


• FT3 for understanding adversary behavior


• FS-ISAC’s framework for cross-team action


**How are you all using these taxonomies today? What did I miss?**


I generally feel like the data we need to tackle fraud exists but often sits in silos, is unstructured, and could really benefit from tighter classification that would allow it to be operationalized in a simple way.
