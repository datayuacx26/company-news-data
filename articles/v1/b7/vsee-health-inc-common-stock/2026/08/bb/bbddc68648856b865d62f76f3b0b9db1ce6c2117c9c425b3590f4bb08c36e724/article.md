---
schema_version: "1.0.0"
document_id: "bbddc68648856b865d62f76f3b0b9db1ce6c2117c9c425b3590f4bb08c36e724"
company_key: "vsee-health-inc-common-stock"
company: "VSee Health Inc."
source_id: "vsee-health-inc-common-stock-rss-098185c65d89"
canonical_url: "https://vsee.com/blog/fedramp-cloud-inheritance/"
published_at: "2026-08-12T20:12:45+00:00"
first_seen_at: "2026-08-13T11:39:24.272782+00:00"
fetched_at: "2026-08-13T11:39:25.445568+00:00"
content_hash: "sha256:00b848aed6eca26d02f4ff63c21224ca5ee840f7d440b3e5ebd5a5057e448bd5"
---

# Cloud-Inherited FedRAMP: What Azure Government and AWS GovCloud Actually Cover

“We run on a FedRAMP-authorized (or FedRAMP-certified) cloud” — is one of the most common and most misleading claims in federal procurement. It’s really two separate facts stacked together: what the infrastructure inherited, and what your application still has to prove on its own.


Hosting on Azure Government or AWS GovCloud gives you a real head start — the infrastructure layer’s security controls are already assessed and authorized. But it says nothing about whether your own application is authorized or certified. The application still has to be assessed and authorized or certified on top of the cloud before it can legally handle federal data.


## **Government cloud only covers the infrastructure**


Azure Government and AWS GovCloud each hold their own FedRAMP authorizations for the infrastructure layer — the data centers, physical security, hypervisors, and core network. That’s a real and valuable authorization. But it authorizes the platform you build on, not the software you build.


Think of it like a landlord’s building permit: it certifies the building is structurally sound. It doesn’t certify what you install inside your unit.


You’ll also see this loosely described as “FedRAMP compliance” — it’s a phrase competitor sites often use to mean the cloud infrastructure underneath meets FedRAMP requirements, not that the application itself is authorized. “Compliant” and “authorized” get used interchangeably in marketing copy, but only “authorized” (or “certified,” under the newer terminology) means the government has checked and approved the security of the application for all federal agencies to use.


## **The shared responsibility model**


FedRAMP controls split three ways once an application sits on top of a government cloud provider:


- **Inherited —** fully handled by the cloud provider (physical security, environmental controls, core network hardware).


- **Shared** — the provider does part, the vendor does part (for example, parts of configuration management and monitoring).


- **Customer-owned —** entirely the application vendor’s responsibility (application-level access control, application logging, how the software itself handles data).


## **The Customer Responsibility Matrix and SSP**


A vendor only gets credit for what’s actually covered. Everything in the customer-owned bucket still has to be built, documented, and assessed — the cloud provider’s authorization doesn’t touch it.


The document that spells this out is the Customer Responsibility Matrix (CRM), published alongside the cloud provider’s Control Implementation Summary and typically filed as an appendix to the vendor’s own System Security Plan (SSP).


It lists every control in the baseline and assigns it — inherited, shared, or customer — so an Authorizing Official can see at a glance who owns what.


When a vendor claims “FedRAMP because GovCloud,” the CRM is what exposes how many controls they still have to own, document, and get independently assessed themselves. It’s often the first document a reviewer opens, precisely because it’s the fastest way to tell a real authorization from a hosting claim wearing one.


## **Why “hosted on GovCloud” is not the same as “FedRAMP authorized”**


Inheriting infrastructure controls from an authorized cloud is a genuine head start — industry estimates put it at roughly 40–60% of the total baseline. But the remaining application-layer controls are exactly the ones that govern how patient data is accessed, logged, and protected inside the software itself.


Until those application-layer controls are assessed by an independent 3PAO and an agency issues an Authority to Operate (ATO) for the application, the platform isn’t FedRAMP authorized — it’s hosted on something that is. Hosting is the foundation, not the finished building.


## **Can a cloud EHR meet the full baseline? Yes — two ways**


An application can meet the complete FedRAMP baseline through either path below. Both are real, both are complete, and both end in an actual ATO for the application itself:


-


**PMO-published FedRAMP authorization (a.k.a. FedRAMP certification) —** listed on the FedRAMP Marketplace, reusable government-wide under OMB’s “presumption of adequacy.”


-


**Agency ATO assessed against the full baseline —** a federal agency assesses the application directly, inheriting infrastructure controls from the authorized cloud underneath it, and issues its own ATO against the complete FedRAMP baseline.


Either route gets an agency to the same place: an application-level authorization that actually covers the controls patient data depends on — not just a hosting inheritance claim.


## **How to check a vendor’s claim**


If a vendor says “we’re FedRAMP because we run on GovCloud/Azure Government,” these questions cut through it fast. A hosting claim takes thirty seconds to verify once you know what to ask for:


-


**Request the CRM** — not just a statement that the platform “runs on GovCloud.”


-


**Check the authorization boundary** in the SSP — does it include the application, or does it stop at the infrastructure?


-


**Ask who issued the ATO** — for the application specifically, not for the underlying cloud service.


-


**Confirm the impact level matches your data** — an application authorized at Moderate can’t take on High-impact work, regardless of what the cloud underneath it is rated for.


If the answer to any of those is “no” or “we’re not sure,” the claim is describing the infrastructure, not the application.


## **Common mistakes when reading a hosting claim**


-


**Treating “hosted on Azure Government” as a finished credential.** It’s a real control set — but only the infrastructure layer.


-


**Assuming inheritance percentage equals authorization status.** Inheriting 40–60% of controls still leaves the rest unassessed.


-


**Not checking whose ATO covers the application.** A cloud provider’s ATO was never meant to cover a third party’s software.


-


**Skipping the CRM entirely.** It’s the one document that turns a hosting claim into a verifiable fact.


## **FAQs**


### **Does hosting on Azure Government or AWS GovCloud make my application FedRAMP authorized?**


No. It authorizes the infrastructure underneath the application, not the application itself. The application still has to implement and get authorized for its own application-layer controls.


### **What’s the difference between “FedRAMP-authorized infrastructure” and a “FedRAMP-authorized application”?**


Infrastructure authorization covers the data centers, hardware, and virtualization layer a cloud provider like Microsoft or AWS operates. Application authorization covers the application running on top of it — access control, logging, data handling — and requires its own ATO.


### **What does “FedRAMP compliance” actually mean?**


FedRAMP itself doesn’t use “compliant” as an official designation — a system is Ready, In Process, or Authorized (increasingly called “Certified”). Vendors often use “FedRAMP compliant” to mean their infrastructure meets FedRAMP standards, without necessarily having the application itself independently assessed and authorized. Treat “compliant” as a marketing term to ask follow-up questions about, not a credential on its own.


### **What controls do I inherit vs. own?**


Typically the vendor inherits infrastructure-layer controls, shares some configuration and monitoring controls with the cloud provider, and fully owns the application-layer controls. The exact split for any given platform is documented in its Customer Responsibility Matrix.


### **Roughly how much of the FedRAMP baseline can I inherit from a cloud provider?**


Estimates commonly put full infrastructure inheritance at around 40–60% of the total control baseline, though the exact share depends on the cloud service and how narrowly its authorization boundary is scoped. The remaining controls — usually the ones closest to how patient data is actually handled — stay with the application.


### **If a vendor says “we’re FedRAMP because we’re hosted on GovCloud,” is that a red flag?**


It’s worth a follow-up question, not an automatic disqualifier. Ask whether the application itself has been independently assessed and authorized, or whether the claim is describing the hosting layer alone. Request the CRM — it will show exactly which controls the vendor still owns.


### **What is a “leveraged authorization”?**


It’s the formal term for building a system on top of an already-authorized cloud service and inheriting that service’s controls rather than re-implementing them. Leveraging is expected and encouraged — it just doesn’t extend the underlying authorization to cover the application on top.


### **What is a Customer Responsibility Matrix (CRM)?**


### A document within the System Security Plan that lists every control in the FedRAMP baseline and states who is responsible for it — the cloud provider, the customer (application vendor), or both. It’s typically the first document an Authorizing Official reviews when evaluating an authorization package.


### **What is a PMO-published authorization?**


A PMO-published authorization is one reviewed and formally listed by the FedRAMP Program Management Office (PMO) — the team within the General Services Administration (GSA) that runs FedRAMP day to day. Once the PMO publishes it, the authorization appears as “Authorized” on the FedRAMP Marketplace, and any federal agency can reuse that same package under OMB’s presumption of adequacy instead of reassessing it themselves. It’s the government-wide route, as opposed to an agency ATO, which one agency assesses and issues on its own.


### **What is an Authority to Operate (ATO)?**


An ATO is the formal approval a federal agency issues before a system goes live, certifying its security risk has been assessed and accepted. A FedRAMP High ATO means it was assessed against the FedRAMP High baseline.


### **Which EHRs have a FedRAMP High ATO?**


Very few. VSee holds a FedRAMP High ATO issued by HHS ASPR; most others are Moderate, In Process, single-agency, or not FedRAMP.


See the comparison →


### **How do I verify a vendor’s FedRAMP status?**


Check the FedRAMP Marketplace; if a vendor isn’t listed, request its ATO letter, impact level, authorizing agency, and System Security Plan — and confirm it’s the platform’s own ATO, not just its hosting.


### **Can a cloud-based EHR meet the full FedRAMP baseline?**


Yes, through a PMO-published FedRAMP authorization listed on the Marketplace, or through an agency ATO assessed against the full baseline. Either path results in the application itself — not just its hosting — being authorized.


### **Does it matter whether the platform runs on Azure Government vs. AWS GovCloud?**


Not for the underlying question. Both hold their own FedRAMP infrastructure authorizations, and both require anything built on top of them to be separately assessed and authorized. The cloud provider is a foundation either way, not a substitute for application-level authorization.


### **What happens if the application-layer controls were never independently assessed?**


The platform isn’t FedRAMP authorized, no matter how solid its hosting is. An agency can’t legally accept the risk of an unassessed application — the ATO has to cover the system that actually touches the data, not just the cloud underneath it.


### **What’s the difference between FedRAMP Moderate and High for a cloud-hosted application?**


The impact level (Moderate or High) describes how sensitive the data is and is set by the agency under FIPS 199 — it’s separate from how much of the baseline is inherited versus customer-owned. A High-impact application still needs its own High-level authorization on top of the cloud, regardless of what it inherits.


### **How long and how expensive is a FedRAMP authorization?**


Typically 12–18 months plus significant cost and staffing (several hundred thousand to a few million) — which is why deploying a platform that already holds a FedRAMP authorization removes the biggest time and risk from an agency’s project.


## **Where VSee stands**


VSee holds a FedRAMP High ATO from HHS ASPR — the government’s toughest security bar, fully authorized at the application level.


[See VSee’s FedRAMP High ATO details →](https://vsee.com/fedramp)


### ***Sources***


- [Microsoft Learn — Azure Government FedRAMP compliance offering](https://learn.microsoft.com/en-us/azure/compliance/offerings/offering-fedramp)


- [AWS — FedRAMP compliance overview (AWS GovCloud FedRAMP High authorization)](https://aws.amazon.com/compliance/fedramp/)


- [FedRAMP.gov — System Security Plan (SSP)](https://www.fedramp.gov/legacy/playbook/csp/authorization/ssp/)


- [FedRAMP.gov — Legacy Documentation with CIS/CRM workbook](https://www.fedramp.gov/legacy/)


- [FedRAMP Marketplace](https://www.fedramp.gov/marketplace/)


- [NIST SP 800-53 Rev. 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)


- [FIPS 199 — Standards for Security Categorization of Federal Information and Information Systems](https://csrc.nist.gov/pubs/fips/199/final)


- [FedRAMP 20x](https://www.fedramp.gov/20x/)


- [AWS — FedRAMP Customer Responsibility Matrix (CRM) inheritance guidance](https://elevateconsult.com/insights/aws-fedramp-inheritance-maximizing-shared-responsibility/)


- [FedRAMP.gov — Agency Authorization Playbook (PMO-published vs. agency-assessed authorization)](https://www.fedramp.gov/resources/documents/Agency_Authorization_Playbook.pdf)


### Share this:


- [Facebook](https://vsee.com/blog/fedramp-cloud-inheritance/?share=facebook)
- [Twitter](https://vsee.com/blog/fedramp-cloud-inheritance/?share=twitter)
- [LinkedIn](https://vsee.com/blog/fedramp-cloud-inheritance/?share=linkedin)
- Email
-


### Like this:


Like


Loading…


### *Related*
