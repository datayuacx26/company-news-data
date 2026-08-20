---
schema_version: "1.0.0"
document_id: "115b2bdca457ff86b0d10b4a610bf30f5b55c63f0d52967a2664f07660d26cdd"
company_key: "vsee-health-inc-common-stock"
company: "VSee Health Inc."
source_id: "vsee-health-inc-common-stock-rss-098185c65d89"
canonical_url: "https://vsee.com/blog/is-fedramp-required-for-ehr/"
published_at: "2026-07-20T15:51:59+00:00"
first_seen_at: "2026-07-24T06:40:24.837691+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:dd4987b96c321fee963bcdd645dfe75df14b86e521a3507280072cab7fe29fc8"
---

# Is a FedRAMP EHR Required for Federal Agencies?

Short answer: Yes. If an EHR runs in the cloud and touches federal health data, it must be FedRAMP authorized — or have an agency ATO that meets the FedRAMP bar. No law spells out “EHR” by name, but federal law makes


FedRAMP


mandatory for the cloud services EHRs run on. The one real exception is an on-premise system.


Here’s the policy chain that gets you there, step by step.


## **FISMA: the root requirement**


The[Federal Information Security Modernization Act](https://security.cms.gov/learn/federal-information-security-modernization-act-fisma) (FISMA)


is the foundation everything else sits on. It requires every federal information system — anything touching government data — has to be risk-assessed and formally signed off before it can go live. That sign-off is the Authority to Operate (ATO). No ATO, no launch. This rule applies whether the system is cloud-based or not.


## **The FedRAMP Authorization Act of 2022**


FISMA says systems must be authorized. The


[FedRAMP Authorization Act of 2022](https://www.congress.gov/bill/117th-congress/house-bill/21) (tucked into the FY23 NDAA, started life as H.R. 21) fills in the “how” for anything cloud-based. It made FedRAMP the official, government-wide way to authorize cloud products and services handling unclassified federal data. Bottom line: if it’s delivered from the cloud and it touches federal info, FedRAMP is the yardstick.


## **OMB M-24-15 and the “presumption of adequacy”**


In 2024,


[OMB Memo M-24-15](https://www.fedramp.gov/2026/authority/m-24-15/) updated how agencies are supposed to use the program. Two things matter here if you’re buying an EHR. First, it tells agencies to lean on FedRAMP-authorized cloud services. Second, it sets up a “presumption of adequacy” — once a service has a FedRAMP authorization, other agencies are generally expected to accept that existing package instead of re-doing the whole assessment themselves. That’s exactly what makes an existing authorization worth something: it’s reusable, not a one-agency favor.


## **How this plays out for EHRs and PHI**


A cloud EHR handling protected health information (PHI) for a federal agency checks every box — it’s a cloud service, it processes federal data, and that data is sensitive. It lands squarely inside FedRAMP’s scope. Same goes for cloud telehealth platforms, patient portals, and pretty much any clinical SaaS a federal agency deploys.


## **FedRamp Moderate or High — which one do you actually need?**


FedRAMP has three impact levels, set under FIPS 199 based on how bad a breach would be. Moderate (~325 controls) covers a lot of federal health data. High (400+ controls) is for data where a breach would be severe or catastrophic — which includes health information in a lot of federal programs. The agency categorizes its own data, and that decision sets the bar: if the workload is High-impact, it legally can’t run on a platform that’s only authorized at Moderate.


For a walk-through of reading a vendor’s level and status, see


FedRAMP Authorized vs. In Process vs. Moderate →


## **FedRAMP is not HIPAA**


Your vendor being HIPAA-compliant doesn’t get you there on its own. HIPAA governs how PHI is handled; FedRAMP authorizes the cloud security of a federal system — two different jobs. A federal health deployment generally needs both, plus a signed BAA. HIPAA alone won’t clear a cloud EHR for federal use.


## **The on-premise exception**


FedRAMP only covers cloud services. A fully on-premise, agency-hosted EHR is authorized under FISMA directly, through the agency’s own ATO process — no FedRAMP needed. That’s why you’ll still find some long-standing federal EMRs running without a FedRAMP authorization: they took a different route. For anything new and cloud-based, that route isn’t on the table.


## **FAQs**


### **What is a FedRAMP EHR?**


A FedRAMP EHR is a cloud-based electronic health record or telehealth platform authorized under FedRAMP — the government’s standardized cloud-security program — to handle federal health data, via an Authority to Operate (ATO) at a FedRAMP impact level (Moderate or High).


### **Is a FedRAMP EHR required for government agencies?**


Yes, if the EHR is cloud-based and handles federal data. FISMA, the FedRAMP Authorization Act of 2022, and OMB M-24-15 together make FedRAMP mandatory for cloud services processing federal information.


### **Does FedRAMP apply to on-prem EHRs?**


No. FedRAMP covers cloud services. On-premise systems are authorized under FISMA through the agency’s own ATO process — though they still need an ATO.


### **Is FedRAMP the same as HIPAA?**


No. HIPAA governs how protected health information is handled; FedRAMP authorizes the cloud security of a federal system. A federal health platform generally needs both, plus a BAA.


### **Who decides Moderate vs. High?**


The agency does, by categorizing its data under FIPS 199 based on breach impact. That categorization determines which FedRAMP baseline the platform has to meet.


### **What does “FedRAMP High” mean?**


FedRAMP High is the most stringent impact level (400+ NIST 800-53 controls), reserved for the government’s most sensitive unclassified data, including health information used in federal programs.


### **What is an Authority to Operate (ATO)?**


An ATO is the formal approval a federal agency issues before a system goes live, certifying its security risk has been assessed and accepted. A FedRAMP High ATO means it was assessed against the FedRAMP High baseline.


### **Which EHRs have a FedRAMP High ATO?**


Very few. VSee holds a FedRAMP High ATO issued by HHS ASPR; most others are Moderate, In Process, single-agency, or not FedRAMP.


See the comparison → ****


### **How do I verify a vendor’s FedRAMP status?**


Check the FedRAMP Marketplace; if a vendor isn’t listed, request its ATO letter, impact level, authorizing agency, and System Security Plan — and confirm it’s the platform’s own ATO, not just its hosting.


***Evaluating a FedRAMP EHR for your agency?*** *VSee is an EHR and telehealth platform running under a FedRAMP High ATO issued by HHS ASPR — assessed against the government’s toughest baseline. See[VSee’s FedRAMP High ATO details →](https://vsee.com/fedramp)*


Sources:


- [FedRAMP Authorization Act (H.R. 21)](https://www.congress.gov/bill/117th-congress/house-bill/21)


- [OMB Memo M-24-15](https://www.whitehouse.gov/wp-content/uploads/2024/07/M-24-15-Modernizing-the-Federal-Risk-and-Authorization-Management-Program.pdf)


- [FedRAMP scope guidance](https://www.fedramp.gov/docs/authority/scope/)


- [CMS: FISMA overview (suggested replacement per editor note above)](https://security.cms.gov/learn/federal-information-security-modernization-act-fisma)


### Share this:


- [Facebook](https://vsee.com/blog/is-fedramp-required-for-ehr/?share=facebook)
- [Twitter](https://vsee.com/blog/is-fedramp-required-for-ehr/?share=twitter)
- [LinkedIn](https://vsee.com/blog/is-fedramp-required-for-ehr/?share=linkedin)
- Email
-


### Like this:


Like


Loading…


### *Related*
