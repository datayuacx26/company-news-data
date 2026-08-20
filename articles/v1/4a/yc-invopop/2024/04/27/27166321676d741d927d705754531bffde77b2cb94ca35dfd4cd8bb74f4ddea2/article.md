---
schema_version: "1.0.0"
document_id: "27166321676d741d927d705754531bffde77b2cb94ca35dfd4cd8bb74f4ddea2"
company_key: "yc-invopop"
company: "Invopop"
source_id: "yc-invopop-news-import-7a590a12f7de"
canonical_url: "https://www.invopop.com/blog/germany-b2b-einvoicing"
published_at: "2024-04-24T00:00:00+00:00"
first_seen_at: "2026-07-22T22:36:25.084833+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:0b369496baf8b6423732cc6711d4b4ad65b5c122f90e6572bdcf3eff2612b10a"
---

# Germany approves B2B e-invoicing mandate

On March 27, 2024, Germany’s Bundesrat approved the Growth Opportunities Act, which introduces an obligation to use e-invoices in domestic business-to-business (B2B) transactions. Starting in 2025, e-invoicing will coexist with paper invoices as the default format; other formats will be accepted if the buyer agrees. Progressively through 2028, every invoice format except e-invoices will be phased out. Unlike in other EU countries that have recently announced e-invoicing plans (such as France, Poland, and Spain), this law doesn’t include any obligation to report data to the tax authority or any requirements related to the channels used to transmit invoices. It focuses strictly on the invoice format, making paper and simple PDF invoices obsolete in favor of e-invoices. The text does mention that e-reporting may be introduced later.


## Scope


The Growth Opportunities Act affects only invoices for domestic B2B transactions. Notably, business-to-consumer (B2C) transactions are beyond the act’s scope. Also, any transaction currently exempt from issuing invoices will remain unaffected. This is the case for transactions under €250 (unless the customer requests an invoice), exempt supplies, and passenger transport. Non-resident businesses that are just VAT-registered in Germany will potentially also be excluded from this requirement.


## Timeline


The new law differentiates e-invoices and “other invoices.” It defines an e-invoice as an invoice that is issued, transmitted, and received in a structured electronic format that allows for electronic processing and complies with the European e-invoicing standard ([EN 16931](https://ec.europa.eu/digital-building-blocks/sites/display/DIGITAL/Navigating+the+eInvoicing+standard+documentation) ) and the corresponding syntaxes ([Directive 2014/55/EU of 16 April 2014](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32014L0055&qid=1713970432358) ).


EN 16931 is a standard, developed by the European Commission, that describes the semantic data model (that is, an invoice’s mandatory and optional fields and the values those fields can contain) and two supported syntaxes: UBL (Universal Business Language) and CII (UN/CEFACT Cross Industry Invoice), which are both based on XML files.


Invoice Type 2025 2026 2027 2028


Paper invoices Default Default +€800k Not allowed Not allowed


PDF invoices Allowed Allowed <€800k Default Not allowed


EN16931 e-invoices Allowed Default +€800k Not allowed Default


Non-EN16931 e-invoices (e.g., EDI) Allowed Allowed Allowed Not Allowed


These are the deadlines established by the Growth Opportunities Act:


- Before Jan 1, 2025: Paper invoices are the default format. Sellers can send any other format if the buyer agrees, including PDF, e-invoice formats, and EDI.
- Jan 1, 2025: EN 16931–compliant e-invoice formats will join paper invoices as the default format. This means that companies can start sending EN-16931 e-invoices without buyer consent. Other formats like PDF or EDI will still be allowed with customer permission.
- Jan 1, 2027: Businesses with turnover above €800k must use EN 16931 e-invoices. Paper or PDF invoices will no longer be allowed, with one exception: businesses using EDI can continue to do so, even if their implementation does not comply with EN 16931, always with customer agreement.
- Jan 1, 2028: Businesses with turnover below €800k must also use EN 16931 e-invoices and won’t be allowed to use paper or PDF invoices. The EDI exception will be eliminated. Sellers can continue to use EDI — if the customer agrees — but their EDI implementation must comply with EN 16931 or at least guarantee interoperability with it.


## ZUGFeRD and XRechnung formats


ZUGFeRD and XRechnung are two e-invoice formats already used in Germany, especially in business-to-government (B2G) transactions, where e-invoicing is mandatory in some states. The Federal Finance Ministry has confirmed that both formats meet the criteria for e-invoices. XRechnung is an XML document format that conforms to EU requirements. ZUGFeRD (Zentraler User Guide des Forum Elektronische Rechnung Deutschland) is a hybrid format that consists of an A3 PDF with embedded XML, meeting European standards. Both are based on and interoperable with UBL and CII.


If you ask me, ZUGFeRD seems like the number one candidate for Germany's de facto invoice format, since it combines a human-readable PDF with a machine-readable e-invoice.


## The EDI exception


The law defines an e-invoice as one that contains either a UBL or a CII file compatible with EN 16931, but there’s an exception. Suppose the invoice issuer and recipient agree on using a different structured format. In that case, an invoice in this format will be accepted as an e-invoice as long as it allows the extraction of all the information required by EN 19631 or is interoperable with it.


This exception was introduced primarily to accommodate the Electronic Data Interchange (EDI) protocol, which some sectors have used for years. Automotive, pharmaceutical, and logistics companies use the EDI protocol to electronically exchange orders, invoices, and other messages with customers and suppliers. Companies define their own EDI implementation and can use different invoice syntaxes. EDIFACT is a widely used EDI invoice syntax.


These sectors already exchange millions of invoices in structured electronic formats, so forcing them to instantly switch to UBL or CII doesn’t seem fair. Consequently, the law allows companies to use any EDI e-invoices until 2028, regardless of whether they comply with EN 16931, as long as the buyer and customer agree. This exception will be eliminated in 2028, when EDI invoices will have to comply with EN 16931 or, if the buyer agrees, at least contain all required information and be interoperable with the standard.
