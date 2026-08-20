---
schema_version: "1.0.0"
document_id: "67dd380924a976c658cfae262f371b981fe6e6855c6a07918c3e4cb353ab4464"
company_key: "yc-pointone"
company: "PointOne"
source_id: "yc-pointone-news-import-cfa073ae7806"
canonical_url: "https://pointone.com/blog/ai-timekeeping-security"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T00:40:27.504989+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:adf1e267ae9b509b19b0e3ea6146f70f906405f98ffea4dbbef8b2976f3aaa92"
---

# Is AI Timekeeping Secure? A Law Firm's Guide | PointOne - PointOne

### AI Summary


-


**Three tests decide security.** Independent certifications (SOC 2 Type II, ISO 27001), zero data retention with no model training, and firm-controlled exclusions separate safe vendors from risky ones.


-


**Hosting region shapes risk.** Confirm whether data sits in US or EU data centers and whether on-premise or private hosting exists for government and high-security clients.


-


**Zero data retention is contractual.** Reputable vendors prohibit their AI providers from retaining or training on firm data, in writing, not in a marketing FAQ.


-


**Attorneys control what leaves.** Client-level exclusions, a capture pause, and entry-by-entry approval keep privileged material out of AI processing entirely.


-


**Get it in writing.** Certifications, a data processing agreement (DPA), and the retention policy belong in the contract before any pilot begins.


## Why Law Firms Ask About AI Timekeeping Security


The security review is where most AI timekeeping purchases stall. The upside of AI timekeeping is real: it recovers the[10–20% of billable hours](https://pointone.com/blog/ai-time-capture-revenue-leakage-law-firms) that manual, end-of-day time entry quietly loses. But capturing that time means letting software into sensitive territory.


[Passive time capture](https://pointone.com/time) reads your work activity — emails, documents, calls — to draft time entries. That activity touches privileged client information, which is why AI timekeeping deserves the same security scrutiny you'd apply to your document management system. And the pressure isn't only internal: a growing number of Outside Counsel Guidelines now restrict how firms can use AI on client matters, so clients are raising these questions before you've even finished procurement.


### The three terms that anchor every security review


-


**SOC 2 Type II:** an independent audit that tests a vendor's security controls in operation over months, not a self-assessment.


-


**ISO 27001:** the international standard for running an information security management system.


-


**Zero data retention (ZDR):** a contractual clause prohibiting the AI provider from storing, or training models on, anything it processes for you.


A vendor that can produce all three, plus a data processing agreement (DPA), has cleared the baseline. The rest of this guide covers how to verify it.


## The 6 Security Questions Every Firm Should Ask


Use this as a checklist for any AI timekeeping vendor. Apply the same rigor you would to[billing compliance software](https://pointone.com/blog/best-law-firm-billing-compliance-software) .


1.


**What independent certifications do you hold?** Look for SOC 2 Type II and ISO 27001 at minimum; GDPR compliance if you handle EU data.


2.


**Is my data used to train AI models?** Ask whether the contract includes a zero data retention clause prohibiting model training on firm data. A verbal assurance without the clause carries no obligation.


3.


**Where is my data hosted, and can I choose the region?** US by default for many vendors; ask about EU hosting or on-premise and private options.


4.


**How long is my data retained, and can I restrict it by geography?** Ask for the default retention window and whether it's configurable per firm.


5.


**Who can see my data internally?** Look for role-based access controls, encryption in transit and at rest, and a named, complete subprocessor list.


6.


**Can I exclude privileged or sensitive clients and personal activity?** Attorneys should be able to exclude specific clients and matters and pause capture entirely.


A seventh question worth adding for compliance-heavy firms: ask how the vendor enforces firm policies and[OCG requirements](https://pointone.com/rules) inside the product, since guideline violations create their own data-handling exposure.


## Where Does the Data Go?


AI timekeeping tools process your work activity to draft time entries, so two things matter: what leaves your environment, and what the AI provider is allowed to do with it.


The strongest setups use zero data retention: the AI provider stores and trains on nothing it processes, and the commitment appears in writing. Data is encrypted in transit and at rest, and access inside the vendor is limited by role. For firms with data residency requirements (EU, Canadian, or Australian clients, regulated industries), ask whether the vendor can restrict data to specific geographies rather than a single default region.


### What a strong data-handling posture includes


-


A signed DPA naming every subprocessor, including the AI model provider.


-


ZDR commitments covering those AI providers, not just the vendor itself.


-


A stated, configurable retention window for your firm's activity data.


-


Encryption in transit (TLS) and at rest, with role-based internal access.


-


Data isolation between customers at the database level, and a data residency option for firms that need one.


Firms already[automating other parts of the billing cycle](https://pointone.com/blog/automate-legal-billing-processes) should put these same questions to every vendor in the stack. The timekeeping layer sees the most raw activity, so it warrants the closest look.


## Protecting Privileged and Sensitive Client Data


The most common security concern is privilege. A well-designed tool addresses it in three ways.


1.


**It works from activity signals rather than exposing document contents.**


2.


**It lets attorneys exclude specific clients or matters from AI processing.** Exclusions should live at the client and matter level, with a pause control for personal time.


3.


**It keeps the attorney in control.** Nothing becomes a billable entry, or leaves the firm, until the attorney approves it during[pre-bill review](https://pointone.com/review) .


Litigation practices face the sharpest version of this question, since work product and opposing-party material flow through daily activity. Our guide to[AI timekeeping tools for litigation firms](https://pointone.com/blog/ai-timekeeping-tools-for-litigation-firms) covers the matter-level controls that apply there.


The pattern to look for: the attorney decides what is ever submitted. Capture can be broad, but processing and submission stay firm-controlled.


##
How to Compare AI Timekeeping Vendors on Security


Use this table to score AI timekeeping security across the vendors you're evaluating. Strong answers come with documents; red flags come with reassurances.


**Question to ask**


**Strong answer**


**Red flag**


**What certifications do you hold?**


Current SOC 2 Type II report and ISO 27001 certificate, shared under NDA


"Certification in progress" with no audit date


**Is our data used for model training?**


Written zero data retention covering every AI provider


A privacy policy reserving the right to train on customer data


**Where is our data hosted?**


A named default region, with data residency configurable by geography


One region, no alternatives, no roadmap


**How long is data retained?**


A stated default window, configurable per firm


Undocumented or indefinite retention


**Who can access our data internally?**


Role-based access controls plus encryption in transit and at rest


Broad engineering access to production data


**Can attorneys exclude clients and pause capture?**


Client-level and matter-level exclusions with a pause control


All-or-nothing capture with no matter-level control


Firm size changes budgets, not standards. Smaller firms building a[tech stack on a budget](https://pointone.com/blog/small-law-firm-tech-stack-on-a-budget) should demand the same six answers; certifications and DPAs cost the vendor, not the buyer.


## How PointOne Handles Security


PointOne is built for firms with strict requirements, from boutiques to the AmLaw 100 and government clients like the Minnesota Attorney General's Office. Its security posture includes:


-


SOC 2 Type II, ISO 27001, GDPR, and HIPAA[certifications](https://trust.pointone.com/)


-


Zero data retention with its AI model providers: no customer data or outputs are stored after processing, and models are never trained or fine-tuned on firm data


-


AES-256 encryption at rest, TLS 1.2+ in transit, role-based access controls, and mandatory multi-factor authentication for privileged production access


-


Quarterly access reviews, with employee access revoked within 24 hours of departure


-


A named, complete subprocessor list, and customer data never used in development or testing environments


-


US hosting by default, with data residency configurable by geography for firms with EU, Canadian, or Australian requirements


-


Configurable retention windows, from immediate deletion onward, with data securely purged when a firm leaves


Firms keep full ownership and control of their data, which is logically isolated per customer with row-level database security; enterprise firms can get full account-level segregation. Time data flows into[PointOne Intelligence](https://pointone.com/intelligence) for realization and leakage analytics under the same access controls.


## Your AI-powered
firm starts here


[Book a demo](https://pointone.com/demo-3)


## Your AI-powered
firm starts here


[Book a demo](https://pointone.com/demo-3)


## Your AI-powered
firm starts here


[Book a demo](https://pointone.com/demo-3)


## FAQs About AI Timekeeping Security


Is AI timekeeping secure enough for a law firm?


Yes, when the vendor holds SOC 2 Type II and ISO 27001 certifications, contractually guarantees your data is never used to train AI models, and gives attorneys control over what is captured and submitted. Verify all three in writing before piloting.


Is my firm's data used to train AI models?


With a reputable vendor, no. Look for a written zero data retention agreement that prohibits the AI provider from retaining or training on your data. The clause should cover subprocessors, not just the vendor.


Where is my data stored?


It depends on the vendor. Ask whether hosting is US or EU-based and whether on-premise or private hosting is available for high-security or government clients. Get the answer named in the DPA.


How is privileged client data protected?


Through attorney-controlled exclusions (leave specific clients or matters out), the ability to pause capture, encryption, access controls, and a workflow where the attorney approves every entry before it is submitted.


What certifications should an AI timekeeping vendor have?


SOC 2 Type II and ISO 27001 at minimum, plus GDPR compliance if you handle EU client data. Ask for the reports and a data processing agreement (DPA).


Do exclusions and pause controls reduce how much billable time gets recovered?


Only for the time you choose to exclude. Everything else is still captured passively, which is where the recovered hours come from. Firms weigh a handful of excluded matters against firm-wide productivity gains and consistently keep the exclusions.
