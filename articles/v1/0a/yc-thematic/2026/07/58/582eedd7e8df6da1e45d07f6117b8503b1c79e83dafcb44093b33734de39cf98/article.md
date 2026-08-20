---
schema_version: "1.0.0"
document_id: "582eedd7e8df6da1e45d07f6117b8503b1c79e83dafcb44093b33734de39cf98"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/customer-feedback-data-residency-eu"
published_at: null
first_seen_at: "2026-07-31T23:56:58.141334+00:00"
fetched_at: "2026-07-31T23:56:59.512489+00:00"
content_hash: "sha256:4c644e82a63ca3d9f2d665c139945968d81accde93236b3e1aa77a3cbc8ef00c"
---

# Where Is Customer Feedback Data Stored, and Can You Keep It in the EU?

Open-ended customer feedback is some of the most personal data an enterprise holds. People put their names, order numbers, health complaints, and account details into free-text boxes without being asked to. So when a European privacy reviewer asks where that data will be stored, they're not filling in a form. They're deciding whether the vendor creates a cross-border transfer problem they'll have to defend later.


Yes, you can keep it in the EU. Thematic is hosted on Amazon Web Services in three regions: US, EU-Frankfurt, and ANZ-Sydney. A customer's data and processing are confined to their region. Each account is assigned one geographic region, and the API and assistant endpoints are region-locked, so a European customer's feedback is stored and analyzed in Frankfurt rather than transferred to the US and protected by contract afterward.


That distinction is the whole answer. Below is what residency covers, what European reviewers are really testing for, where feedback platforms tend to fall short, and the one question worth asking any vendor before you sign.


## What "data residency" actually means for feedback data


Data residency is the commitment that a defined set of data stays inside a defined geography. It sounds like a single yes-or-no property. It's really six, and vendors often answer the easy ones and stay quiet on the rest:


- **Storage.** Where the database sits.
- **Processing.** Where the analysis runs, which isn't automatically where the data is stored.
- **Sub-processing.** Where any third party in the chain operates, including AI model providers.
- **Backups.** Whether snapshots replicate to another region.
- **Access.** Where the vendor's own staff and support tooling reach in from.
- **Telemetry and logs.** What metadata leaves the region even when the feedback doesn't.


A vendor can honestly say "we have an EU region" while processing runs elsewhere, backups replicate across an ocean, or a model provider sits outside the boundary. Ask about all six.


## What EU buyers are actually asking when they ask about residency


**Whether you create a Chapter V problem.**[Chapter V](https://gdpr-info.eu/chapter-5/) of the General Data Protection Regulation (GDPR), Articles 44 to 50, governs transfers of personal data to third countries, including onward transfers, so that GDPR-level protection is not undermined. If feedback never leaves the EU, Chapter V does not engage. If it does leave, the controller now owns a transfer question.


**Whether their compliance rests on a mechanism under appeal.** Under[Article 45](https://gdpr-info.eu/art-45-gdpr/) , a European Commission[adequacy decision](https://commission.europa.eu/law/law-topic/data-protection/international-dimension-data-protection/adequacy-decisions_en) means no further safeguard is needed. The EU-US Data Privacy Framework adequacy decision is in force. On 3 September 2025 the General Court of the European Union[dismissed a challenge](https://iapp.org/news/a/european-general-court-dismisses-latombe-challenge-upholds-eu-us-data-privacy-framework) to it brought by Latombe, a member of the French parliament, confirming the decision's validity. Latombe appealed on 31 October 2025. That[appeal](https://www.wilmerhale.com/en/insights/blogs/wilmerhale-privacy-and-cybersecurity-law/20251201-european-court-of-justice-to-review-challenge-to-eu-us-data-privacy-framework) is registered at the Court of Justice as Case C-703/25 P, and as of May 2026 no hearing date had been announced. A merits ruling against the framework would be the third consecutive invalidation of a transatlantic transfer arrangement after Schrems I and Schrems II.


This is why sophisticated European buyers prefer in-region processing over a transfer mechanism. In-region removes the question. A mechanism answers it, for now.


**Whether Article 46 work lands on them.** Absent adequacy,[Article 46](https://gdpr-info.eu/art-46-gdpr/) requires appropriate safeguards, most often Standard Contractual Clauses (SCCs). Since Schrems II, SCCs are expected to come with a Transfer Impact Assessment. That's real work for the controller, and avoiding it is a legitimate procurement goal.


**Which regions actually count.** Adequacy isn't intuitive. New Zealand holds a full EU adequacy decision. So do the United Kingdom, Japan, Switzerland, Canada for commercial organizations, and South Korea. Australia does not. A team that assumes ANZ is one regulatory unit will get this wrong.


## Where most feedback platforms fall short


Almost every vendor lists an EU region. The gaps are underneath:


- Storage is in-region but analysis runs in a single home region.
- The AI model provider sits outside the residency boundary, and nobody mentions it.
- Backups replicate cross-region by default for durability.
- Region is a deployment detail rather than an account-level guarantee, so nothing stops data moving later.
- Support staff and internal tooling reach into the region from anywhere, with no residency controls on the access path.


One question exposes most of this:[ask the vendor](https://getthematic.com/insights/customer-feedback-analytics-rfp-requirements) to name every sub-processor that touches feedback text, and the region each one operates in. A vendor that can answer has done the work. A vendor that pivots to "we're SOC 2 certified" has not.


## Where Thematic stores customer feedback data


**Three regions, one per account.** Thematic runs on AWS in three regions. A customer's data and processing are confined to their region, and each customer is assigned a single geographic region rather than spread across several.


Region Location Typical buyer


EU Frankfurt EU and European Economic Area (EEA) organizations avoiding a Chapter V transfer


ANZ Sydney Australian and New Zealand organizations, including Australian entities with no adequacy decision to rely on


US United States US and other organizations without an in-region requirement


**Region-locked endpoints, not just region-locked storage.** Thematic publishes three server URLs, one per region, and they keep data inside the region the account is hosted in. An account is active in exactly one region at a time, and moving requires an explicit switch rather than happening silently. This extends to AI assistants: when teams query Thematic through its[Model Context Protocol server](https://getthematic.com/insights/thematic-mcp-server) , data leaves the regional server only at the moment the calling assistant reads it into context for a specific answer.


**Processor status and contracts.** Thematic acts as a[data processor under GDPR](https://getthematic.com/insights/gdpr-feedback-personal-data) . The customer, as controller, decides what to upload, from whom, and why. Thematic processes it only on the customer's instructions and signs a contract governing the processing of EU personal data.


**Encryption and isolation.** Data at rest is encrypted with FIPS-validated AES, and data in transit uses TLS 1.3. Customers are isolated at the database level. Thematic holds[SOC 2 Type II](https://getthematic.com/insights/vendor-security-certifications) , audited annually by A-LIGN against the Security, Availability, and Confidentiality criteria, with the reporting period closing 28 February each year.


**Minimizing what sits in the region at all.** PII redaction is available as a priced, best-effort add-on that masks emails, names, phone numbers, addresses, URLs, and encoded values before analysis, and strips quoted email history. Reducing the personal data in scope is often a stronger control than arguing about where it lives.


**Deletion.** Customer data is retained for the life of the contract and deleted within 30 days of termination using NIST 800-88 sanitization, with backups purging on their own rotation. Customers can also modify and delete individual datasets, which supports erasure requests.


**Two honest boundary notes.** First, Thematic's internal agent workers are region-locked, and the only thing that crosses a region boundary is error categories, which carry no customer feedback. Second, on sub-processors: rather than take a blanket claim on trust, ask for the current sub-processor list and the region each entry operates in. Thematic notifies customers of changes to that list under its sub-processor notification policy. The list, not a marketing sentence, is the artifact your privacy team should review.


## What this means for a European or Australasian buyer


For an EU or EEA organization, the practical effect is that[feedback analysis](https://getthematic.com/insights/customer-feedback-analysis) stops being a transfer question. Data is stored and processed in Frankfurt, the account can't drift to another region without an explicit change, and the reviewer's Chapter V analysis gets shorter rather than more creative.


For Australasian organizations the calculus differs by country, which is worth spelling out internally. A New Zealand entity operates from a jurisdiction the EU has already found adequate. An Australian entity does not, so in-region processing in Sydney does more work for it than adequacy ever will.


Residency is one input to a controller's compliance decision, not a substitute for it. Nothing here is legal advice, and no hosting arrangement makes an organization compliant on its own.


## A buyer's checklist for feedback data residency


1. Which regions do you operate, and where exactly are they hosted?
2. Is processing confined to the same region as storage, or only storage?
3. Is region an account-level guarantee, or a deployment default that can change?
4. Name every sub-processor that touches feedback text, and the region each operates in.
5. Do backups replicate outside the region?
6. What metadata, telemetry, or error data leaves the region, and does any of it contain customer feedback?
7. Where do your support staff and internal tooling access the region from?


## The short answer


Yes. Thematic stores and processes customer feedback in the region assigned to your account, on AWS in the US, EU-Frankfurt, or ANZ-Sydney, with region-locked endpoints and one active region per account. For an EU organization that means feedback stays in Frankfurt, so GDPR Chapter V does not engage and your compliance does not depend on a transfer framework currently under appeal at the Court of Justice. The test to run on any vendor: ask them to name every sub-processor that touches feedback text and the region each one operates in.
