---
schema_version: "1.0.0"
document_id: "fe5b6144bf4a6f76a402f427aa7b7b575c517337cd7a8b33658b6961f5bf7dda"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-news-import-ac2626a08ad7"
canonical_url: "https://www.spscommerce.com/community/articles/labeling-for-new-suppliers-gs1-128-sscc-and-where-labels-have-to-go"
published_at: null
first_seen_at: "2026-08-06T00:17:50.907215+00:00"
fetched_at: "2026-08-06T00:17:51.589979+00:00"
content_hash: "sha256:2c077f865734842baf2e59ba803a9dcd73e320cc1fe4a84c2d5f2ef72ca65bac"
---

# Labeling for New Suppliers: GS1-128, SSCC, and Where Labels Have to Go

In this article, learn about:


-


The technical structure of GS1-128 and SSCC-18 as a digital license plate for your shipments


-


Why matching physical labels to electronic ASNs is essential for automated receiving and compliance


-


How to navigate retailer-specific placement and quality rules to prevent recurring chargebacks


---


New suppliers often treat shipping labels as a simple formality once they have finalized their product barcodes. This perspective is a common misconception that can lead to immediate and expensive consequences. In the world of retail fulfillment, the shipping label is the identifier doing the actual work once a shipment leaves your dock. While a product UPC identifies the item, the shipping label is the physical link to the electronic record a retailer uses to receive goods automatically.


The industry refers to this label as a license plate because it identifies and tracks shipments throughout the entire supply chain. For a first-time retail supplier setting up a shipping process, getting these labels right is a high-leverage task to complete before the first purchase order (PO) arrives. A labeling mistake is rarely a one-time error; it is usually a systemic process or template error that repeats with every single carton until someone identifies the flaw.


## Decoding the Terms: GS1-128 and SSCC-18


One of the first hurdles for new operations teams is the terminology. It is common to hear the terms[GS1-128](https://www.spscommerce.com/community/articles/gs1-128-barcode-label) and[SSCC-18](https://www.spscommerce.com/community/articles/what-are-the-different-segments-of-a-barcode) used interchangeably, but they represent two different parts of the labeling equation. GS1-128 is the barcode symbology, or the visual format of the barcode itself. The[Serial Shipping Container Code (SSCC)](https://www.gs1us.org/upcs-barcodes-prefixes/serialized-shipping-container-codes) is the specific 18-digit number encoded within that visual format.


Think of the GS1-128 barcode as a container. While it is most commonly used to carry an SSCC, it can also hold other data, such as[Global Trade Item Numbers (GTINs)](https://www.gs1us.org/upcs-barcodes-prefixes/what-is-a-gtin) or batch numbers, using Application Identifiers (AIs). When you see a barcode on a logistics label, the digits 00 at the beginning tell the scanner that the information following it is an SSCC.


The SSCC-18 itself is built from several components:


-


**Application identifier (AI):** The digits 00 which signal an SSCC follows


-


**Extension digit:** A single number used to increase the capacity of the serial reference


-


**GS1 company prefix:** A unique 4-to-12-digit number licensed from GS1 that identifies your company as the entity responsible for the shipment


-


**Serial reference:** A unique number you assign to each individual shipping container


-


**Check digit:** A final digit calculated mathematically to ensure the barcode scans accurately


The most critical rule regarding the SSCC is that it can never be reused, and it must match your electronic documentation exactly.


**Related Reading** :[GS1 Standards for New Suppliers: GTINs, UPCs, and Case Codes](https://www.spscommerce.com/community/articles/gs1-standards-for-new-suppliers-gtins-upcs-and-case-codes)


## Why Your Label Must Match the ASN


The primary purpose of an SSCC label is to connect a physical box to an[advance shipping notice (ASN)](https://www.spscommerce.com/community/articles/edi-856-ship-notice-manifest) . When a retailer like[Amazon](https://www.spscommerce.com/community/articles/asn-submission-and-labeling-workflow) or[C&S Wholesale Grocers](https://www.spscommerce.com/community/articles/how-to-stay-compliant-with-cands-wholesale-grocers) receives your shipment, they don't want to open every box to count the contents manually. Instead, they scan the GS1-128 barcode on the outside of the carton.


This scan triggers a search in their system for the corresponding ASN, which is an electronic document (EDI 856) that tells the retailer exactly what is inside that specific box. If the SSCC on the label does not match the SSCC listed in the ASN, the shipment becomes effectively invisible to the retailer's automated systems. This results in a scan failure, requiring manual intervention, which leads to receiving delays and compliance chargebacks.


At Amazon, this specific failure is often categorized under[ASN accuracy](https://www.spscommerce.com/community/articles/how-to-prevent-amazon-asn-chargebacks) or No Carton Content Label (No CCL). If the label information is missing from the ASN, or if the label doesn't match the ASN SKU due to a typo, the retailer will impose financial penalties to recover the cost of the manual labor required to process the shipment.


## The Rules of Label Placement


Where you place the label on the box is its own compliance category, separate from barcode quality. Retailers use automated scanning tunnels and specific workflows, meaning they expect to find your labels in very specific locations.


Different retailers have different requirements. For instance, C&S Wholesale Grocers requires that GS1-compliant labels be visible on at least two sides of a case. They also mandate that labels include the PO number, case count, and an expiration date in a specific YYYY-MM-DD format.


General GS1 guidelines suggest placing labels in the lower portion of the carton, but you must always consult your specific[retailer routing guide](https://www.spscommerce.com/community/articles/how-to-read-a-routing-guide-without-missing-the-rules-that-cost-you) . A barcode that scans perfectly can still be non-compliant if it is placed over a seam, wrapped around a corner, or covered by shipping tape. Amazon specifically warns against placing barcodes on the corners or curves of a product, as this makes them unscannable for their imaging systems.


## The Financial Cost of Systemic Errors


Labeling errors are particularly dangerous because they are rarely isolated incidents. If your label template has an incorrect company prefix or a serial reference that repeats, every single carton on every shipment will trigger a chargeback until the template is fixed.


The fees can erode margins quickly. C&S Wholesale Grocers, for example, may charge $50 or more for missing or illegible labels. If an incorrect PO number is used on the label, that fee can jump to $150 per instance.[Amazon's chargeback](https://www.spscommerce.com/community/articles/how-to-dispute-an-amazon-chargeback) taxonomy is even more granular. A Case Pack Defect occurs when a barcode for an inner pack is visible on the outside of a master pack.


These penalties can be tiered based on your compliance rate over a rolling period. This means that as your accuracy drops, the cost per error often increases, significantly impacting your bottom line.


## Building a Retail-Ready Process


For a new supplier, the goal is to build a process that prevents these errors from reaching the dock. This starts with quality control and training.


1.


**Verify your prefix:** Ensure you have a licensed[GS1 company prefix](https://www.gs1us.org/upcs-barcodes-prefixes/what-is-a-prefix) . If you have only licensed individual GTINs for your products, you will likely need to license a full prefix to meet SSCC requirements for shipping labels.


1.


**Audit your templates:** Before your first shipment, perform a pre-shipment audit. Print a sample label and ensure it matches the data structure required by the retailer. Verify that the application identifiers and check digits are calculated correctly.


1.


**Coordinate with EDI:** Your labeling accuracy and[electronic data interchange (EDI)](https://www.spscommerce.com/community/articles/what-is-edi) accuracy are two sides of the same coin. The SSCC on the sticker must be the exact same 18-digit string sent in your EDI 856 ASN.


1.


**Check your hardware:** Ensure your printers are in good condition. Faded ink, smudged barcodes, or labels printed on shiny backgrounds can all cause scan failures even if the data itself is correct.


**Related Reading** :[Packaging Guide Glossary](https://www.spscommerce.com/community/articles/packaging-guide-glossary)


## The Dispute Process


No process is completely error free, but even a perfect process may occasionally receive an invalid chargeback. When this happens, documentation is your best defense. Retailers like Amazon allow you to dispute chargebacks, but the burden of proof is on you.


At Amazon,[disputes](https://www.spscommerce.com/community/articles/how-to-dispute-an-amazon-chargeback) go through the Vendor Chargeback Dispute Management (VCDM) team. You typically have only 30 days from the date of the charge to file a dispute. You will need to provide evidence, such as photos of the labeled cartons or records of your ASN transmissions, to prove that the shipment was compliant.


C&S Wholesale Grocers follows a similar document-driven process. To win a dispute there, you will need your original[bill of lading (BOL)](https://www.spscommerce.com/community/articles/bill-lading-bol-supplypike) , a copy of the signed exit pass from the driver, and copies of the case labels used. Proactive record-keeping is a financial necessity for recovering lost revenue.


## Getting It Right the First Time


Labeling and ASN mismatch failures are common sources of retailer chargebacks because they are so easy to get wrong and so expensive when they repeat. For a new supplier, the complexity of GS1-128 and SSCC-18 can be overwhelming. However, understanding that these labels are the digital license plates of your business is the first step toward a compliant, profitable supply chain. By aligning your physical labels with your electronic notices and following retailer-specific placement rules, you protect your shipments from the moment they leave your dock to the moment they arrive at the fulfillment center.


[The Supply Chain Source](https://www.spscommerce.com/community/) is a community-driven platform designed to help professionals navigate the complexities of modern retail and logistics. Whether you are looking for deep dives into EDI structures, tips for negotiating supplier agreements, or the latest updates on retailer compliance programs, our library of over 2,800 resources is built to help you learn faster and lead with confidence. Join over 60,000 industry experts and operators today to stay ahead of the trends shaping the market.
