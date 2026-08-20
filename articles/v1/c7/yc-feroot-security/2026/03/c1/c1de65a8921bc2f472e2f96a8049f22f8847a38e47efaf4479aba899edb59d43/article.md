---
schema_version: "1.0.0"
document_id: "c1de65a8921bc2f472e2f96a8049f22f8847a38e47efaf4479aba899edb59d43"
company_key: "yc-feroot-security"
company: "Feroot Security"
source_id: "yc-feroot-security-news-import-5cc226b40d32"
canonical_url: "https://feroot.com/blog/hipaa-pci-compliance-healthcare-billing/"
published_at: "2026-03-25T18:33:05+00:00"
first_seen_at: "2026-07-22T07:46:19.024825+00:00"
fetched_at: "2026-07-28T22:17:49.677260+00:00"
content_hash: "sha256:8c36d00a8fff95bccbf47cdb08f2c55fe4b5bfe0dd073633055b8ceaf4d08b3b"
---

# HIPAA + PCI for Healthcare Billing: Protecting Both PHI and Payment Card Data

When a patient logs into a billing portal, two of the most heavily regulated data types in the U.S. end up in the same browser session. PHI like health history, insurance providers, and diagnoses, renders right alongside the card entry fields they’ll use to pay.


And with them load the third-party scripts that marketing manages. Analytics, heatmaps, A/B testing, conversion tracking. These tools are how growth teams optimize revenue and product teams improve the experience.


But the problem is that once they load in a patient’s browser at run-time, they can read everything in that DOM. Including the PHI and the payment fields.


Unless someone has deliberately governed what those scripts are allowed to touch, they see everything.


That’s the risk both PCI DSS 4.0.1 and OCR under HIPAA are now doubling down on. But they approach it differently, with different controls, different cadences, and different enforcement teeth. This article is about navigating both at once.


## What you’ll learn


- Where HIPAA and PCI DSS overlap and where they don’t
- What run-time visibility actually looks like in practice, and how it maps to PCI DSS 6.4.3, 11.6.1, and OCR’s tracking guidance
- How to leverage the differences and similarities to unify HIPAA + PCI compliance for healthcare billing


## Where HIPAA and PCI DSS overlap


Although HIPAA and PCI DSS emerged from different policy processes, on healthcare pages, they mitigate the same categories of threat, like unauthorized access, tampering, and e-skimming, with access controls, encryption, audit logging, and vendor agreements


The language differs, but both of them demand all of it to reduce the likelihood of compromising sensitive data.


Let’s look at the areas where they converge in detail:


### Access controls to restrict unauthorized use


Both frameworks require that only authorized users and software can reach sensitive data. How they implement it differs slightly.


HIPAA mandates unique user IDs, automatic logoff, and access controls tied to specific roles. On the other hand, PCI demands least-privilege access, role-based controls, and MFA for anyone with administrative access to the cardholder data environment.


However, there are nuances you need to watch out for. Having MFA and RBAC for the systems that store PHI and card data is not enough.


HIPAA cares equally about who can access PHI in transit or in ephemeral browser views. Any third-party like tags would need a[BAA](https://www.feroot.com/blog/hipaa-baa-checklist-website-technology-vendors/) to justify the data use.


Likewise, PCI 4.0.1 pulls scripts running in the consumer’s browser into scope via requirements 6.4.3 and 11.6.1, even when no staff member ever logs into those scripts. And those scripts need to be governed, inventoried, and continuously monitored for changes.


### Encryption for data at rest and in transit.


HIPAA treats encryption as addressable, which means you can implement it where reasonable and appropriate, or document why you didn’t.


PCI is more direct and all-encompassing. Strong cryptography for cardholder data in transit over open networks is mandatory. Period.


That means, if you design your encryption controls for PCI DSS, you’ll most likely meet HIPAA’s requirements as well.


However, what most teams don’t get is that data is decrypted once the customer’s browser receives it. Which means, it can be seen by unauthorized scripts on the website. Both frameworks’ intent extends beyond the wire to what happens after decryption.


### Audit logging and monitoring


HIPAA requires audit controls that record and examine activity in systems containing ePHI. PCI requires detailed logging of access to cardholder data, security events, and administrative actions, with daily review.


Centralizing logging across your payment portal stack, like web servers, API gateways, identity providers, and payment microservices, serves both frameworks simultaneously.


But logs almost never extend into the browser. The scripts executing in a patient’s session, reading PHI, and hovering over payment fields, leave no trace in a traditional SIEM. Closing that gap requires purpose-built run-time visibility that both frameworks can trust.


### Vendor agreements


HIPAA requires Business Associate Agreements with any vendor that creates, receives, maintains, or transmits PHI on your behalf. PCI requires service provider agreements that define compliance responsibilities and evidence expectations like AOCs and attestations.


The challenge is that on a payment page, a single vendor often needs to be both. A portal platform that renders patient billing information and hosts payment functionality is simultaneously a business associate and a PCI service provider.


Those two contracts get reviewed by different teams, negotiated on different timelines, and rarely align on what happens when something goes wrong in the browser.


### Incident response and risk assessments


Both frameworks require formal risk assessments, documented incident response procedures, and regular testing. HIPAA’s Security Management Process demands an accurate and thorough assessment of risks to ePHI, with ongoing risk management.


On the other hand, PCI expects forensic readiness and a documented ability to contain and eradicate compromises in the cardholder data environment.


That’s both an overhead and an opportunity. A unified risk analysis covering the payment portal, explicitly modeling threats to both PHI and cardholder data in that shared space, can serve both frameworks at once.


Mitigating risks emerging from third-party scripts is a new addition of expectation from both frameworks, though. And that’s where the unified approach matters the most.


## HIPAA + PCI compliance: Key difference in approach


Though they share a common goal of protecting data on healthcare payment pages, the way they prescribe implementation and what data they aim to protect differ.


PCI DSS now spells out what “good” looks like for client‑side script governance. It’s prescriptive, and lays clear expectations of controls and policies that should be implemented.


HIPAA offers you more flexibility with your implementation. What it does is lay out first principles, mark the boundary you need to respect, and point you towards the outcomes. You still have the option to reason from first principles and translate it into controls to protect ePHI from tracking scripts.


Let’s look at these differences closely.


### PCI DSS 4.0.1 prescribes how you govern the digital experience layer


Requirements 6.4.3 and 11.6.1 address protection for browser-based payment pages, targeting the attack surface on the client end.


While **Requirement 6.4.3** focuses on prevention by mandating that every script on a payment page is authorized, verified, and documented, **Requirement 11.6.1** focuses on detection. It requires continuous monitoring and alerts for unauthorized changes to scripts or security-impacting HTTP headers.


### HIPAA expects the same, without explicitly stating how


HIPAA expects you to identify risks emerging from tracking technologies via risk analysis and then take appropriate measures to mitigate them.


There is no regulation that explicitly mandates a script inventory or tamper detection for pages that expose PHI. But[OCR’s tracking guidance](https://www.feroot.com/blog/hipaa-ocr-investigation-website-tracking/) forces you to the same conclusion through risk analysis.


If third-party scripts on your portal can access PHI in the DOM and send it to external domains, your risk management program must treat that as a potential unauthorized disclosure. The controls aren’t spelled out. The obligation is.


### Scope: HIPAA and PCI cover different data types


Put simply, PCI primarily includes the cardholder data environment in the scope. That means any webpage or iFrame that receives card data, or any system that stores, processes, or transmits come under PCI’s scope.


HIPAA scopes all PHI, in any format, across the covered entity and its business associates.


On a payment portal, PCI might formally scope only the endpoints and payment widgets that handle card data. HIPAA scopes the entire portal, including every billing workflow and every tracker that touches patient information, even if it never sees a card number. Thus, the same page sits inside both scopes simultaneously.


### Assessment frequency


The next difference between HIPAA and PCI is how often they expect you to review your security controls and scan your environment.


PCI draws a minimum expectation of quarterly vulnerability scans and annual assessments. Along with that, it requires you to periodically pen-test your environment, and under 11.6.1, continuously monitor payment pages for unauthorized changes and tampering.


HIPAA requires ongoing risk analysis and periodic evaluations, but sets no fixed cadence. OCR expects updates when your environment or threat landscape changes.


That mismatch can have repercussions for portals that are constantly evolving with UX updates, tag rotation, and the addition of new scripts by marketing. In that situation, you could still be satisfying the minimum criteria for PCI DSS, but due to the dynamic nature of the page, you could be failing both HIPAA and PCI for not detecting risks early.


### Breach notification


HIPAA’s breach notification rule is statutory. And it gives you around sixty days from the moment you discover a breach to notify affected patients, HHS, and, in large incidents, the media.


However, the anchor is when you discover the breach, or when you were reasonably supposed to discover it with well functioning compliance program. The only time you can cross that notification deadline without repercussions is when the number of affected people is less than 500. Beyond that, OCR has gone after entities for delays, treating each day past that deadline as its own separate violation.


PCI notification is contractual, but no less serious. You need to notify your acquirer and payment brands promptly, cooperate in forensic investigations, or face escalating per-card fines and potential loss of processing privileges.


In the end, both frameworks are governing the same page, but they’re not governing it the same way. Here’s a side-by-side look at where they diverge.


**Requirement Area** **HIPAA** **PCI DSS 4.0.1**


**Scope** All PHI across the organization, including billing data, identifiers, and any tracker that touches patient information Cardholder data environment. But 6.4.3 and 11.6.1 pull all payment-page scripts into scope regardless of who hosts the card fields


**Technical Specificity** Flexible, risk-based safeguards. You define the how. OCR judges the outcome Prescriptive and testable. Specific controls, specific cadences, specific evidence expected


**Script Monitoring** Implicit via risk analysis and OCR tracking guidance. The obligation is there; the prescription isn’t Explicit. 6.4.3 requires script inventory and integrity checks. 11.6.1 requires tamper detection, at a minimum, weekly


**Assessment Frequency** Ongoing, no fixed cadence. OCR expects updates when your environment or threat landscape changes Annual ROC or SAQ, quarterly vulnerability scans, periodic pen tests, recurring payment page tamper checks


**Breach Notification** 60 days from discovery, or when you were reasonably supposed to discover it. Each day past that is a separate violation Contractual, driven by your acquirer and card brand scheme rules. Delays compound fines and risk processing privileges


**Vendor Agreements** Business Associate Agreements covering permitted uses, breach notification, and downstream subcontractors Service provider agreements defining compliance responsibilities, AOC evidence, and incident cooperation


**Penalties** Tiered civil penalties up to $1.9M per violation category annually, plus state AG actions and class-action exposure Contractual fines from card brands, per-card penalties post-breach, higher transaction fees, or loss of processing privileges


## The healthcare payment page problem


When you watch what loads on the browser at run-time, you’ll see your portal’s own scripts, your payment gateway, and a dozen third-party calls to analytics platforms, tag managers, and advertising pixels, firing in the background as a patient navigates their bill.


That becomes a problem when you factor in that these pages are often designed for the best UX possible. It opts for a flow that removes friction with one continuous process from authentication to payment. That means PHI and card data, the two most protected data types in the US, are side-by-side, on the same webpage.


Every script that loaded into that session can see all of it.


### Tracking scripts create a dual violation risk


A marketing pixel transmitting PHI to an advertising platform is a HIPAA violation. A skimming script exfiltrating card data from the same page is a PCI violation. On a healthcare payment page, both can happen simultaneously, triggered by the same JavaScript ecosystem, invisible to the same compliance teams.


Tracking pixels, analytics SDKs, and tag manager scripts monitor how users move through billing portals. Which links they click, what amounts they see, how far they scroll, and sometimes what they type. Tools like Meta Pixel can capture URLs with query parameters that include account IDs or service types, page titles that reference conditions or departments, and form field values if configured to do so.


### iFrames don’t solve it either


The assumption is that because the PSP hosts the card fields in an iframe, the payment page is effectively out of PCI scope. The reality is that other scripts on the page can still observe DOM changes, watch behavioral patterns, and scrape surrounding PHI regardless of where the card fields are hosted.


In some cases, when the scripts that load at run-time can call compromised dependencies in a downstream attack. That malicious script can steal and skim data in unpredictable ways, by replacing the iFrames with malicious ones, capturing the keystrokes, and more.


### Configuration drifts


Tags and pixels for marketing purposes can’t differentiate between PHI, cardholder data, or any other sensitive data in real time. Even when just set for high-level analytics, it captures what happens in the DOM and sends it to the third-party servers for analysis.


Configuring them to not fire on certain pages, not record certain sessions, and not capture certain fields is possible. But those configurations drift over time as marketing rotates tags or vendors push updates. Without continuous monitoring, what was compliant on day one quietly stops being compliant as the page evolves around it.


## Unified compliance for payment pages


In the span of a single page load, patients enter their name, their account number, their diagnosis code, and their credit card. Two federal frameworks govern that moment.


When two separate teams work in silos, neither team’s scope gets drawn to include what the other is watching. Scripts that touch PHI get reviewed for payment security. Trackers that sit on payment pages get reviewed for privacy risk.


That’s a gap. A patient billing page is not a HIPAA asset that sometimes processes payments, and happens to display some health information. It is for every session.


That means:


- Scoping billing and payment pages as part of the PHI environment under HIPAA risk analysis and Security Rule safeguards.
- scoping them as in-scope payment pages under PCI DSS 4.0.1, with full 6.4.3 and 11.6.1 coverage, including referring pages and single-page application views that load payment fields dynamically.


### Defaulting to a stricter standard leaves blindspots


The instinct to default to whichever framework is more demanding sounds reasonable. But it falls apart in practice, as both frameworks are stricter in different areas.


PCI DSS is strict on run-time governance. HIPAA is stricter on everything that counts as regulated health data.


That means payment-related billing identifiers, account numbers, service descriptions, and procedure codes are all PHI under HIPAA’s broad definition. PCI’s scope stops at cardholder data. If you lean on PCI’s narrower data lens, you are under-classifying the information your payment page actually handles.


### Vendor agreements should be unified, too


[Any vendor that can see both patient information and card data in the same browser session needs to be treated as both a business associate and a PCI service provider.](https://www.feroot.com/blog/hipaa-tracking-pixels-without-vendor-baa/) If your contracts do not reflect that, your risk register won’t stand scrutiny.


For portal and UI platform vendors, that means a BAA that explicitly covers ePHI in the portal, Security Rule safeguard obligations, and breach notification responsibilities to you. It also means a PCI service provider agreement that defines their 6.4.3 and 11.6.1 responsibilities, evidence expectations, and roles in incident response.


One contract or the other is not enough. You need both.


## How feroot enables dual compliance


The tools most healthcare organizations rely on today were built for a threat model that starts at the server. WAFs protect the network perimeter. SIEM platforms aggregate server-side logs. Vulnerability scanners assess infrastructure.


None of them see what executes inside the browser, which scripts load on a payment page, what data they access, and where they send it.


That is the gap Feroot is built to close.


### Feroot delivers continuous run-time visibility


It automatically discovers every script executing on payment pages and maps it directly to PCI DSS 6.4.3 and 11.6.1 requirements. As scripts get discovered, it maintains the inventory, enforces authorization workflows, and monitors continuously for drifts or behavioural changes, constantly verifying their integrity.


When something on the payment page shifts, the platform detects it, flags it, and produces the audit-ready evidence QSAs expect.


### Feroot monitors the browser environment for PHI exposure


AI automatically identifies PHI across web forms, medical search queries, and appointment scheduling systems, and maps every location where tracking technologies could access protected health information.


This way, it alerts and blocks when data moves out to unauthorized third-party destinations via tracking scripts. With a unified, centralized inventory, BAAs get streamlined, and documentation gets simplified.


Together, these tools cover the entire surface needed to satisfy PCI and HIPAA compliance.


## The bottom line


Healthcare payment pages sit at the intersection of two regulatory frameworks, and managing them as separate programs leaves the most dangerous surface ungoverned. Securing that data requires a compliance posture that covers both.


[Schedule a demo](https://www.feroot.com/demo/) to evaluate your current payment page security against both frameworks and explore how Feroot’s combined platform provides visibility and protection that satisfies PCI and HIPAA requirements together.
