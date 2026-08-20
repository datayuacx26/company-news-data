---
schema_version: "1.0.0"
document_id: "1bc24c9d33de2f07dbbd53a8d964efbe4bc5abab677182f8004b0b241471a99d"
company_key: "yc-invopop"
company: "Invopop"
source_id: "yc-invopop-news-import-7a590a12f7de"
canonical_url: "https://www.invopop.com/blog/verifactu-invoicing-system"
published_at: "2023-12-14T00:00:00+00:00"
first_seen_at: "2026-07-22T22:36:25.084833+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:cc55fd75fcd13f2ff5c8339552ee653319e2c1308bc33e43d7e982e2f743aed0"
---

# Verifactu: Spain’s E-Invoicing System Explained

## Background


On December 6, 2023, Spain published[Royal Decree 1007/2023](https://www.boe.es/diario_boe/txt.php?id=BOE-A-2023-24840) , which outlines the technical requirements for computerized invoicing systems, or “sistemas informáticos de facturación (SIF).” This regulation, colloquially known as Veri*Factu, aims to combat fraud by guaranteeing the integrity, traceability, and auditability of the systems that businesses use to issue invoices.


By July 1, 2025, every business in Spain must use invoicing systems that comply with this regulation. A few exceptions to this are businesses that use the Suministro Inmediato de Información (SII) or TicketBAI system, since they serve a similar purpose, and businesses that are exempt from issuing invoices.


The government hasn’t yet published the complete specifications for Veri*Factu, but a first more information can be found[here](https://www.agenciatributaria.es/AEAT.desarrolladores/Desarrolladores/_menu_/Documentacion/IVA/Sistemas_Informaticos_de_Facturacion_y_Sistemas_VERI_FACTU/Sistemas_Informaticos_de_Facturacion_y_Sistemas_VERI_FACTU.html) .


## What’s a computerized invoicing system (CIS)?


The regulation defines “sistema informático de facturación” — or computerized invoicing system (CIS), for the lack of a better translation — as any hardware or software that Spanish taxpayers use to issue invoices.


Consequently, this broad category includes ERP software, POS systems, and any invoicing or billing software. More specifically, the regulation defines a computerized invoicing system as any system that allows taxpayers to issue invoices by:


- Allowing the entry of invoice information through any method.
- Retaining invoice information, either by storing it in the system itself or by exporting it to any physical or electronic media.
- Processing invoice information, regardless of where this processing takes place.


These systems must store invoice records in a standard format and follow specific technical standards, which we’ll discuss later in this post.


## What’s a Veri*Factu system?


The regulation introduces the possibility of invoicing systems electronically sending each invoice record in real time to the Spanish tax authority (AEAT). However, live sending of invoice records to the tax authority will be voluntary for taxpayers.


The regulation refers to the invoicing systems that report invoice records to the tax authority in real time as “Veri *Factu systems” or “verifiable invoices systems.” Even though these systems are just a subset of the computerized invoicing systems mandated by the regulation, the whole regulation is also colloquially referred to as the “Veri* Factu regulation,” which can be misleading.


In the words of the AEAT,


> This regulation is colloquially known as the 'Veri*Factu Regulation,' referring to the simpler and more efficient method to comply with the rule, which involves electronically sending invoice records to the tax agency at the time of their production. (Source:[Agencia Tributaria: El “Reglamento Veri*Factu](https://sede.agenciatributaria.gob.es/Sede/normativa-criterios-interpretativos/analisis/El__Reglamento_Veri_factu_.html) )


## Scope of the Veri*Factu regulation


### Am I required to use a computerized invoicing system?


If your business is liable for tax in Spain, you must use a CIS that complies with this regulation. In more technical terms, the regulation applies to every business with a VAT-fixed establishment in Spain, including resident and non-resident companies.


There are a couple of exceptions to this rule:


- Companies that report invoices to the Suministro Inmediato de Información (SII) system, mandatory for companies with annual revenues higher than six million euros, don’t need to change.
- Companies in the Euskadi region (Vizcaya, Guipúzcoa, and Álava) are already complying with the TicketBAI regulation, which is analogous to Veri*Factu, and therefore don’t need to change.
- Companies exempt from issuing invoices under the Regulation of Invoicing Obligations ([Royal Decree 1619/2012](https://www.boe.es/eli/es/rd/2012/11/30/1619/con) ), including the agriculture, livestock, and fishing sectors, are exempt from the Veri*Factu regulation.


### Am I required to use a Veri*Factu system?


There is no obligation to use a Veri*Factu system; the requirement is to use a compliant CIS. Live reporting of invoices to the tax authority may have some advantages, such as simplified procedures and transparency with the tax authority.


Presumably, the tax authority has introduced this possibility as a first step toward a future where businesses won’t need to submit periodic models such as VAT returns, because the tax authority will have all the necessary information.


## Timeline for adopting Veri*Factu


Here is the timeline for complying with the regulation:


- The ministry is expected to publish a complementary document with the technical details for invoicing systems. We expect this to happen during the first quarter of 2024.
- Invoicing software must conform to the technical details within nine months of the ministry publishing them. Similarly, the tax authority will have nine months to build the system to receive invoice records electronically.
- Businesses must adopt a compliant CIS by July 1, 2025.


## Veri*Factu, TicketBAI, SII, and e-invoicing: What’s the difference?


If you follow the[regulations](https://www.invopop.com/invoicing-guides/spain) that pertain to invoices in Spain, you may be confused about the difference between the various systems and regulations that the government has introduced, so let’s do a quick review.


### Systems currently in place


We’ll start with the already enforced systems: the Suministro Inmediato de Información (SII), FacturaE, and TicketBAI.


**Suministro Inmediato de Información (SII)**


SII is an invoice reporting system introduced in Spain in 2017. SII requires that certain taxpayers electronically submit an XML file to the tax authority with information about their VAT transactions (invoices issued and received). The system is mandatory for taxpayers with an annual turnover of more than six million euros, those in certain VAT groups, and those under the monthly refund regime.


These taxpayers must report their invoicing records in almost real time, typically within four days of issuing or receiving an invoice. Since the SII system already gives the tax authority a detailed view of a company’s VAT position, businesses that must report to SII are exempt from Veri*Factu.


**FacturaE**


The FacturaE system in Spain is an electronic invoicing format mandated for transactions with public sector entities. Introduced in 2015, it requires that all suppliers and service providers who invoice public administrations in Spain issue and send invoices electronically in a specific XML-based format known as FacturaE.


**TicketBAI**


TicketBAI is the equivalent of Veri *Factu in the Euskadi region of Spain, also known as the Basque Country. TicketBAI has been progressively implemented in each of the three provinces in Euskadi, starting from 2021, with complete implementation expected by 2024. Like Veri* Factu, TicketBAI requires all business and professional taxpayers to use certified invoicing software, which must comply with specific technical requirements.


TicketBAI invoicing software must include a unique QR code and an electronic signature on each invoice. Since the TicketBAI regulation is analogous to VeriFactu, businesses that must comply with it won’t need to worry about VeriFactu.


### B2B invoicing and Veri*Factu


Finally, how does Veri*Factu compare with Spain's[upcoming B2B e-invoicing regulation](https://www.invopop.com/blog/spain-draft-royal-decree-b2b-e-invoicing) ? The two requirements are compatible and will exist together in the future.


The key difference is that Veri*Factu mainly introduces requirements for what information invoicing software should store and how to store it, but does not regulate the format (PDF or XML, for instance) or the channel (paper, email, or government channel) used to send the invoice. On the other hand, the e-invoicing regulation will require that businesses use a specific electronic format (not PDF) and channel to send those invoices.


Hence, companies in Spain must soon comply with both regulations: using certified invoicing software that meets Veri*Factu security standards, that can issue e-invoices for B2B sales, and that can include QR codes on B2C invoices.


## Technical requirements for invoicing systems


### Invoice registry


Computerized invoicing systems must record an entry for each invoice issued to a customer, before or at the moment of issuance. In Spanish, these entries are called “registros de facturación de alta.”


The regulation requires a standard format and structure for each entry and specific technical details to ensure that each registry can be traced and cannot be altered without leaving a mark in the system.


Specifically, each invoice registry must include:


- All standard invoice fields, such as supplier and buyer tax data, issue date, and line items. All currency fields must be in euros.
- A "digital fingerprint" or "hash" that links consecutive registries.
- An electronic signature when invoices are not submitted directly to the tax office. In other words, e-signatures are not required if the system is a Veri*Factu system.
- An ID code that identifies the system used to produce the invoice.
- Digital timestamp.
- If associated with a corrective invoice, a digital reference to it.


### QR codes on invoices


Like TicketBAI, the Veri*Factu regulation also requires that invoices include a QR code containing the most relevant fields of the invoice. This affects complete invoices and simplified invoices, what we usually call tickets or receipts.


In addition, if the invoice has been live reported to the tax authority — that is, generated by a VeriFactu system — it must include the text “Factura verificable en la sede electrónica de la AEAT,” or “VERIFACTU.”


This will let customers know that they can scan the QR code to check the invoice's validity directly with the tax authority.
