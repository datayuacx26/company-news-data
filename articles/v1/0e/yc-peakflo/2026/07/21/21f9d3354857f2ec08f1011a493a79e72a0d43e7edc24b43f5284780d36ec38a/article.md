---
schema_version: "1.0.0"
document_id: "21f9d3354857f2ec08f1011a493a79e72a0d43e7edc24b43f5284780d36ec38a"
company_key: "yc-peakflo"
company: "Peakflo"
source_id: "yc-peakflo-news-import-4ba227f4ca0c"
canonical_url: "https://peakflo.co/blog/invoicenow-ap-automation-gap-singapore-food-manufacturers"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-31T22:55:40.942818+00:00"
fetched_at: "2026-07-31T22:55:41.445460+00:00"
content_hash: "sha256:c1819df5d6bd0677a6febfd94decab7cf94231b72a1e959efc7522838ac2ae3b"
---

# We're Already on InvoiceNow — So Why Are We Still Processing 600 Invoices Manually?

**TL;DR:** InvoiceNow is Singapore’s PEPPOL-based e-invoicing network. It provides a standardised delivery channel for invoices from registered suppliers — not AP automation. Singapore food manufacturers who go live on InvoiceNow often discover that 60–75% of their invoices still arrive outside the network (email PDFs, WhatsApp, overseas vendors), and that even InvoiceNow-delivered invoices still require manual GL coding, PO matching, exception handling, approval routing, and payment processing. AP automation closes both gaps: it handles non-InvoiceNow invoice capture via OCR and AI, and it automates the downstream processing workflow for all invoices regardless of source.


## The Expectation Gap That Every Finance Controller Recognises


A Singapore food manufacturer completes its InvoiceNow implementation. The project took four months — ERP configuration, supplier communication, PEPPOL access point setup, user training. The finance team expects to see meaningful reduction in manual invoice processing.


Three months later, the AP supervisor is still manually processing the same number of invoices per week. Some invoices now arrive via InvoiceNow, which is an improvement — no more re-keying invoice header data from PDFs. But the coding still needs to be done. The PO matching still needs to be checked. The exception invoices still need to be resolved with suppliers. The approval workflow still runs on email. And 65% of suppliers are still not on InvoiceNow at all.


This scenario is not a project failure. The InvoiceNow implementation worked exactly as designed. The expectation gap exists because InvoiceNow is solving a different problem than AP automation — and the distinction is not always communicated clearly when businesses sign up.


## What InvoiceNow Actually Does


InvoiceNow is Singapore’s implementation of the PEPPOL (Pan-European Public Procurement On-Line) e-invoicing standard, administered by IMDA (Infocomm Media Development Authority). It provides a secure, standardised network for transmitting structured invoice data between connected businesses.


When a supplier on the InvoiceNow network sends an invoice to a connected buyer, the invoice is transmitted as a structured XML document (PEPPOL BIS Billing 3.0 format) through registered access points. The buyer’s ERP receives the structured data, which can be ingested directly without manual data entry.


The benefits of this are real: elimination of re-keying, reduction in OCR errors for InvoiceNow-delivered invoices, standardised invoice format, and an immutable digital record of what was transmitted.


What InvoiceNow does not do: code the invoice to the right GL account and financial dimensions, match it against the purchase order and goods receipt, detect and route exceptions for human resolution, manage the approval workflow, execute the payment, or reconcile the payment back to the invoice. These steps — which collectively constitute accounts payable processing — remain entirely manual unless a separate system handles them.


## The Two Gaps InvoiceNow Leaves Open


**Gap 1: The supplier coverage gap**


InvoiceNow works only for invoices where both the supplier and the buyer are registered on the network. In a food manufacturing supply chain, this is rarely more than 30–40% of total suppliers.


The remaining suppliers include:


- Overseas suppliers in Malaysia, China, Indonesia, or Australia who are not on Singapore’s PEPPOL network
- Small local suppliers (fresh produce vendors, artisan food producers, local logistics contractors) who have no ERP or accounting software and therefore cannot connect to InvoiceNow
- Service providers (utilities, facilities management, professional services) who may be large enough to have accounting software but have not yet prioritised InvoiceNow connectivity
- Group entities invoicing intercompany (intracompany transactions are not delivered through InvoiceNow)


All invoices from these sources continue arriving by email, WhatsApp, fax, or physical delivery. They still require manual data entry or OCR-based extraction.


**Gap 2: The downstream processing gap**


Even for the 30–40% of invoices that do arrive through InvoiceNow, the structured data delivery solves only the first step. Everything that happens next — GL coding, PO matching, exception handling, approval, payment, reconciliation — still needs to be done, and InvoiceNow provides no capability for any of it.


A food manufacturer processing 600 invoices per month who achieves 35% InvoiceNow coverage has eliminated manual data entry for approximately 210 invoices. But those 210 invoices still need to be coded, matched, approved, and paid. And the other 390 invoices still need to be captured from email or document first.


Invoice Source % of Total (Typical SG Food Mfr) InvoiceNow Covers? Downstream Processing Automated?


Registered SG domestic suppliers 30–40% Yes No — requires separate AP automation


Overseas suppliers (MY, CN, APAC) 20–30% No No


Small/local suppliers without ERP 15–20% No No


Service/utility providers 10–15% No No


Intracompany invoices 5–10% No No


## What Manual Processing Still Looks Like After InvoiceNow


For the AP team at a food manufacturer after InvoiceNow implementation, the daily workflow changes at the very beginning — the data entry or PDF reading step — but reverts to manual work almost immediately after.


When an InvoiceNow invoice arrives, the ERP receives the structured data and creates a draft bill record. The AP staff member opens that draft record and must:


1. Verify the GL account assignment (InvoiceNow delivers the supplier’s own account coding, not the buyer’s chart of accounts — these must be mapped)
2. Check or correct the financial dimension coding (department, cost centre, location)
3. Trigger the three-way match against the purchase order and goods receipt
4. Resolve any matching exceptions (PO line item code differs from invoice line item code, quantity slightly over the GR amount, price above the PO price by more than tolerance)
5. Route to the appropriate approver based on amount and expense type
6. Follow up if the approver has not responded within the approval SLA
7. Trigger payment and mark the invoice as paid
8. Confirm payment reconciliation in the bank statement


Steps 1 through 8 are identical whether the invoice arrived through InvoiceNow or by email PDF. InvoiceNow eliminates step 0 (extracting the data from the PDF). Every step thereafter remains manual unless AP automation is in place.


## The Specific Case of Large Food Manufacturers With ERP Integration


Food manufacturers who have gone through the investment of integrating their ERP with InvoiceNow — a project that typically takes three to six months and requires both IT and finance resources — have a reasonable expectation that invoice processing will become substantially more automated.


The ERP integration does deliver real value: invoices arrive as structured data, draft records are created automatically, and the supplier’s invoice data is available in the ERP without manual transcription.


But large food manufacturers using SAP S/4HANA or Microsoft Dynamics 365 Business Central often have additional complexity that the ERP alone cannot handle: multi-dimensional financial coding requirements, three-way matching with complex PO structures, approval workflows that cross multiple entities and geographies, and payment runs that need to coordinate across Singapore and Malaysia bank accounts.


SAP’s native AP capabilities handle matching and coding within a single entity. But when the approval workflow requires routing to a manager in Kuala Lumpur for invoices above a certain threshold, or when an exception needs to be communicated to a supplier’s account manager in Shanghai, the built-in ERP workflow is insufficient — and gaps must be filled by email, WhatsApp, or manual intervention.


[AP automation purpose-built for food manufacturing](https://peakflo.co/industries/manufacturing) plugs directly into the ERP’s InvoiceNow-delivered data and adds the workflow layer that the ERP does not provide: intelligent exception routing, multi-entity approval chains, supplier communication automation, and cross-bank payment orchestration.


## What Full AP Automation Adds to an InvoiceNow Foundation


When an AP automation platform sits on top of an InvoiceNow-connected ERP, the processing capability expands across all five categories that InvoiceNow leaves unaddressed:


**Universal capture:** Email AP inboxes, WhatsApp forwarding, scanned documents, and supplier portal uploads all feed into the same processing pipeline as InvoiceNow. A food manufacturer’s 600 monthly invoices — regardless of source — are processed through a single workflow. The source channel is logged but does not change the processing steps.


**AI GL coding:** For both InvoiceNow invoices (where the supplier’s coding needs to be translated to the buyer’s chart of accounts) and non-InvoiceNow invoices, AI assigns the correct GL account and financial dimensions based on vendor history and invoice context.[AI-powered GL coding](https://peakflo.co/blog/ai-gl-coding-automation-non-po-invoices) reaches 85–95% touchless accuracy within 90 days of supervised operation.


**Three-way matching with exception management:** Invoice quantities and prices are matched against the PO and GR automatically. Exceptions are routed to the responsible buyer or procurement manager with context: the exact line item discrepancy, the PO that was referenced, and the vendor contact for resolution. Exceptions are resolved faster because they go directly to the person who can act on them, not to the AP queue.


**Approval workflow:** Approval rules are configured per entity, per expense type, and per amount threshold. Mobile approval interfaces allow Finance Directors to approve payment batches from any location. Approval SLA monitoring automatically escalates overdue approvals before they delay payment runs.


**Payment and reconciliation:**[Cross-border vendor payments](https://peakflo.co/blog/singapore-malaysia-cross-border-vendor-payment-automation-food-manufacturing) are executed through bank connectors that handle GIRO, IBG, and international wire formats automatically. Payment confirmations are matched back to invoices in the ERP, closing the AP cycle.


Processing Step InvoiceNow Only InvoiceNow + AP Automation


Invoice capture (structured, from registered suppliers) Automated Automated


Invoice capture (PDF email, non-registered suppliers) Manual Automated (OCR + AI)


GL account coding Manual AI-automated (85–95% touchless)


Financial dimension coding Manual AI-automated


Three-way PO matching Manual in ERP Automated with exception routing


Approval workflow Manual (email/ERP) Automated with mobile access


Payment execution Manual (bank portal) Automated (bank connectors)


ERP reconciliation Manual Automated


## How Peakflo Closes the InvoiceNow Gap for Singapore Food Manufacturers


Peakflo’s[accounts payable automation](https://peakflo.co/accounts-payable) integrates with both InvoiceNow-connected ERPs and the full range of alternative invoice channels that food manufacturers use. The platform connects to SAP S/4HANA and Microsoft Dynamics 365 Business Central, receiving structured invoice data from InvoiceNow deliveries while also processing email PDFs, scanned documents, and supplier portal uploads through AI extraction.


For Singapore food manufacturers, Peakflo’s support for[InvoiceNow and PEPPOL e-invoicing](https://peakflo.co/blog/invoicenow-peppol-e-invoicing-singapore-guide) means InvoiceNow-delivered invoices receive the benefit of structured data quality (no OCR extraction variance) while still being routed through the full AP workflow: coding, matching, approval, and payment.


Non-InvoiceNow invoices — which typically represent the majority of a food manufacturer’s invoice volume — are processed through Peakflo’s[AI OCR invoice processing](https://peakflo.co/blog/ai-ocr-invoice-processing-accuracy-food-manufacturing) pipeline, which handles multi-vendor format variance, multi-language invoices (English and Chinese from Singapore/Malaysia suppliers), and complex invoice layouts.


For food manufacturers processing 600 invoices per month, the combined InvoiceNow + Peakflo workflow delivers meaningful reduction in manual processing within the first month, with[automated invoice processing](https://peakflo.co/blog/automated-invoice-processing-food-manufacturing-complete-guide) reaching optimal performance within 90 days.


For Singapore businesses qualifying for government support, the[Productivity Solutions Grant](https://peakflo.co/productivity-solutions-grant) covers AP automation platforms that are IMDA-listed, substantially reducing the net investment for going beyond InvoiceNow to full AP automation.


## Our Verdict: Is AP Automation the Right Next Step After InvoiceNow?


### The gap is real if:


- You went live on InvoiceNow but still process more than 100 invoices manually per month
- Your suppliers are not fully InvoiceNow-registered and invoices still arrive by email or WhatsApp
- InvoiceNow delivers invoice data to your ERP but GL coding, PO matching, and approval still run manually
- Your AP team spends significant time resolving exceptions and following up on approvals
- Month-end close is delayed because invoice coding and matching are not complete


### InvoiceNow alone may be sufficient if:


- More than 80% of your suppliers are InvoiceNow-registered and no overseas suppliers
- You have fewer than 50 invoices per month and the manual downstream steps are manageable
- Your ERP has native AP workflow capabilities that already handle your matching and approval requirements


**Verdict:** For food manufacturers processing 300 or more invoices per month, InvoiceNow is the right starting point but is insufficient as a complete AP automation solution. The supplier coverage gap (60–75% of invoices arriving outside the network) and the downstream processing gap (all invoices still requiring coding, matching, approval, and payment) mean that the manual AP workload remains substantial after InvoiceNow adoption. AP automation that complements InvoiceNow by handling non-network invoices and automating downstream processing is the complete solution.


## Conclusion


InvoiceNow is a meaningful step toward digital invoice processing for Singapore food manufacturers. It eliminates the data entry problem for connected suppliers and creates an auditable digital record of invoice transmission. These are genuine improvements.


But InvoiceNow is not AP automation. It is a delivery channel standard that solves one dimension of a multi-step AP processing problem. The other dimensions — how invoices are captured from non-network suppliers, how they are coded, matched, approved, and paid — require additional capability that InvoiceNow was never designed to provide.


Food manufacturers who have completed their InvoiceNow implementation and still find their AP teams processing hundreds of invoices manually each month are not experiencing a failure. They are experiencing the predictable gap between what InvoiceNow delivers and what full AP automation covers.


Closing that gap is the next step — and for most Singapore food manufacturers, the step with the greatest impact on AP efficiency.


To see how AP automation builds on your InvoiceNow foundation to automate the downstream processing steps,[request a demo](https://peakflo.co/request-demo) and walk through a live invoice processing session.


---


## Frequently Asked Questions


**Does Peakflo work as an InvoiceNow access point?**


Peakflo integrates with InvoiceNow-connected ERPs and can receive invoices delivered through the PEPPOL network into its processing workflow. For food manufacturers whose ERP is already connected to InvoiceNow, Peakflo receives the structured invoice data and processes it through the downstream AP workflow (coding, matching, approval, payment) — closing the gap that InvoiceNow leaves open.


**How does AP automation handle invoices that arrive in both InvoiceNow format and email PDF simultaneously?**


Duplicate detection is built into AP automation platforms. If a supplier sends the same invoice through InvoiceNow and also by email (which happens when suppliers are uncertain whether the InvoiceNow transmission was received), the system detects the duplicate based on invoice number, supplier ID, and amount, flags it, and presents it for human review before creating a duplicate payable.


**What happens if the InvoiceNow invoice data contains errors?**


InvoiceNow delivers the invoice data as submitted by the supplier. If the data contains errors (wrong PO reference, incorrect tax calculation, price that doesn’t match the agreed rate), the errors are flagged during AP automation’s matching step. The exception is routed to the AP team with context, and a communication can be triggered to the supplier requesting a corrected invoice or credit note. InvoiceNow does not validate invoice accuracy against the buyer’s ERP data — that validation requires an AP automation layer.


**My ERP already has built-in invoice approval workflow. Why would I need separate AP automation?**


Built-in ERP approval workflows handle the basic approval step, but typically lack: mobile-first approval interfaces (most ERP portals are desktop-only), automatic escalation for overdue approvals, multi-entity approval chains that cross organisational boundaries, supplier communication automation for exception resolution, and consolidated AP visibility across entities. AP automation adds these capabilities on top of the ERP’s core approval functionality.


**How do I know if my current InvoiceNow implementation is working correctly?**


Key indicators that InvoiceNow is working correctly: registered suppliers’ invoices arrive in your ERP without manual data entry, invoice transmission is logged in the PEPPOL access point records, and no structured data errors are reported during ingestion. If invoices are arriving with blank fields or incorrect data, the supplier’s access point may have a configuration issue that should be investigated with your InvoiceNow implementation partner.


**Is there a minimum invoice volume for AP automation to be cost-effective?**


For Singapore food manufacturers, AP automation typically becomes cost-effective at 150 or more invoices per month, accounting for PSG grant support that reduces the net investment. Below that volume, the manual processing burden may not justify the implementation cost. Above 300 invoices per month, the cost-benefit case is strongly positive within the first year.


**Can AP automation help us get more suppliers onto InvoiceNow?**


AP automation platforms often include supplier onboarding features that can encourage InvoiceNow registration among suppliers. Some platforms send registration invitations to suppliers who currently submit invoices by email, offer portal-based submission as an intermediate step for suppliers who are not yet InvoiceNow-ready, and track InvoiceNow adoption rates across the supplier base. Increasing InvoiceNow coverage reduces OCR processing requirements over time.


**What is the PEPPOL BIS Billing 3.0 format and does it matter for AP automation?**


PEPPOL BIS Billing 3.0 is the international XML standard for structured invoice data that InvoiceNow uses. It defines specific fields, validation rules, and coding schemas for invoice elements. For AP automation purposes, what matters is that PEPPOL-format invoices arrive with more complete and accurate data than OCR-extracted PDF invoices, resulting in higher initial coding and matching confidence. AP automation platforms that support PEPPOL ingestion natively can process InvoiceNow invoices without the OCR extraction step.


**Does InvoiceNow cover government procurement invoicing in Singapore?**


Government agencies in Singapore are progressively requiring suppliers to use InvoiceNow for invoicing government entities. If your food manufacturing business supplies to government buyers (public institutions, statutory boards, government-linked companies), InvoiceNow connectivity is increasingly a commercial requirement for those customer relationships, separate from your own internal AP efficiency goals.


**What is the PSG grant coverage for AP automation that includes InvoiceNow capability?**


The Productivity Solutions Grant covers qualifying AP automation platforms listed by IMDA. Grant coverage is typically 50–70% of qualifying costs for eligible Singapore businesses. The exact coverage level and eligibility criteria are reviewed periodically by IMDA. To understand current grant rates and application requirements, consult the IMDA website or speak with an IMDA-listed vendor.
