---
schema_version: "1.0.0"
document_id: "16c1f59be857b93cb56dcdb2cd88fa90d0614196fb7f7a11709c77087cdc0422"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-news-import-ac2626a08ad7"
canonical_url: "https://www.spscommerce.com/community/articles/case-pack-configuration-how-to-build-a-pack-that-works-at-retail"
published_at: null
first_seen_at: "2026-07-23T00:37:49.401871+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:a895824648d3b4bc7ed9b9d30bc112af3577137f5803d523c66754443dbcf895"
---

# Case Pack Configuration: How to Build a Pack That Works at Retail

In this article, learn about:


-


The critical link between physical case builds and digital data accuracy in preventing costly retail chargebacks


-


Industry standards for case packs and stacking, including GS1 GTIN rules and TI/HI pallet mechanics


-


The operational impact of programs like Walmart SQEP and how 2025 freight density changes turn packaging into a cost lever


---


Landing shelf space at a major retailer is a milestone for any brand, but that dream can quickly turn into a logistical nightmare if you overlook the back-of-house details. While new suppliers often view the case pack as a purely physical decision that is centered on how many units fit in a box that can survive a truck ride, retailers experience the case pack as a sophisticated data object. A configuration only works at retail when the physical carton and its digital data record are in total agreement across every touchpoint: the item setup, the[purchase order (PO)](https://www.spscommerce.com/edi-document/edi-850-purchase-order/) , the[advanced ship notice (ASN)](https://www.spscommerce.com/edi-document/edi-856-advance-shipping-notice/) , and the pallet on the warehouse floor.


In the high-stakes world of retail logistics, a case pack is a critical compliance metric. Discrepancies between what was declared in your data and what actually arrives at the distribution center (DC) are the primary drivers of[chargebacks](https://www.spscommerce.com/community/articles/edi-chargebacks-explained) , manual rework fees, and rejected loads. This guide walks you through the discipline of building a configuration from the ground up, ensuring your physical builds and digital footprints remain perfectly aligned.


## Defining the Layers of Packaging


Building an effective configuration starts with understanding the hierarchy of trade items. This layered approach ensures that every person and automated system in the supply chain knows exactly what they are handling.


At the base is the **each** , or the smallest sellable unit purchased by the consumer. Above this, suppliers often use **inner packs** (sometimes called warehouse packs) to bundle eaches together. These groupings allow retailers to handle smaller quantities without breaking down a full case, which is especially useful for modular resets or slower-turning items.


The **master pack** , also known as the **case pack** or **vendor pack** , is the larger carton containing either multiple inner packs or individual eaches.[Walmart’s vocabulary](https://www.spscommerce.com/community/articles/what-are-walmarts-secondary-packaging-standards) can be particularly tricky here as they treat the warehouse pack as an inner grouping and the vendor pack as the primary shipping case. Understanding these distinctions is vital during item setup. Some retailers may require every level of the hierarchy to be declared precisely.


Consider three common ways to configure a 48-unit shipment:


-


**Vendor pack 48:** In this case, a supplier ships one case of 48 units directly to the store. This is cost-effective for high-volume staples like bottled water but offers retailers zero flexibility in replenishment.


-


**Vendor pack 48 / warehouse pack 12:** This case splits into four inner packs of 12 units. This strikes a balance, allowing a store to order just 12 units if sales are moderate.


-


**Vendor pack 48 / warehouse pack 1:** In this scenario, every unit is treated as its own warehouse pack. While this offers the gold standard for flexibility, it is the least cost-effective for suppliers due to increased handling and labeling requirements.


**Related Reading** :[Walmart Secondary Packaging Guide: Supply Chain Standards](https://www.spscommerce.com/community/unlock-your/walmart-secondary-packaging-guide-supply-chain-standards)


## The Identity Layer: Global Trade Item Numbers (GTINs)


The most unmissable rule in case pack configuration is the[GS1 standard](https://www.spscommerce.com/community/articles/gs1-128-barcode-label) for identity: Any change to the number of trade items in a case requires the assignment of a new[Global Trade Item Number (GTIN)](https://www.gs1.org/standards/id-keys/gtin) . This is the single fact that turns a simple decision to go from 12 to 16 units per case into a cross-functional project.


A unique GTIN is required at every level of the packaging hierarchy above the base unit. For Walmart suppliers,[SQEP (Supplier Quality Excellence Program) Phase 2](https://www.spscommerce.com/community/unlock-your/deep-dive-sqep-phases-2-and-3) requires a[GTIN-14](https://www.gs1us.org/upcs-barcodes-prefixes/what-is-a-gtin) , which is essentially a modified UPC that identifies the specific pack configuration. If your item file says you are shipping a 12-pack, but the barcode on the box represents a 10-pack, the scanner at the DC will trigger a defect immediately.


## Physical Build Constraints and the Multi-Retailer Reality


A case pack that satisfies one retailer rarely satisfies everyone. Retailers increasingly disagree on what the ideal case looks like, forcing suppliers to navigate a fragmented landscape of standards.


Walmart generally prefers lightweight, scannable cases that can be scanned from multiple angles in motion. They also utilize the[5 Easies](https://www.spscommerce.com/community/articles/what-are-the-5-easies-of-retail-ready-packaging) framework for shelf-ready packaging: Cases must be easy to identify, open, stock, shop, and dispose of. Conversely,[Costco](https://www.spscommerce.com/community/articles/costco-supplier-requirements) expects sturdy club packs that function as both a shipping container and a shelf display. Target may request smaller inner packs for their modular resets, while[Amazon](https://www.amazon-packaging.com/vendor-self-testing) tests cases against rigorous[ISTA](https://ista.org/) parcel-shipping standards to ensure they survive the small-parcel environment.


Secondary packaging also faces unique pressures regarding cost and sustainability. Materials typically account for 2–15% of a product’s total cost, and retailers like Walmart are now tying packaging standards directly to corporate sustainability targets. Suppliers are increasingly expected to replace non-recyclable materials like PVC or EPS foam with recyclable alternatives.


**Related Reading** :[Retail-Ready Product Requirements for First-Time Suppliers](https://www.spscommerce.com/community/articles/retail-ready-product-requirements-for-first-time-suppliers)


## Connecting the Case to the Pallet: TI/HI Mechanics


The intersection of physical stacking and digital data is most visible in[TI/HI](https://www.spscommerce.com/community/articles/what-is-ti-hi) (Tier by High) configurations. TI refers to the number of cases on a single layer of a pallet, while HI is the number of layers stacked vertically.


Retailers care about these numbers because their automated storage and retrieval systems are calibrated to specific dimensions. If you declare a 10x5 pattern but ship an 8x6 configuration, it disrupts the entire inbound flow, leading to manual rework and[lumper fees](https://www.spscommerce.com/community/articles/what-are-walmart-lumper-fees) . Furthermore, the pallet must be built for stability. This includes placing the heaviest goods on the bottom, using interlocking brick-laying patterns to prevent tipping, and ensuring edge alignment to avoid the overhang trap.


This is where your[electronic data interchange (EDI)](https://www.spscommerce.com/community/articles/what-is-edi) strategy becomes a compliance tool. Your ASN (or EDI 856) must accurately reflect the physical TI/HI on the truck. If the digital ASN and the physical pallet disagree, a compliance defect is triggered, often resulting in receiving delays and chargebacks.


**Related Reading** :[Pallet Building 101: TI/HI, Stacking, and Why Retailers Care](https://www.spscommerce.com/community/articles/pallet-building-101-ti-hi-stacking-and-why-retailers-care)


## Freight Cost and NMFC 2025 Changes


Case pack configuration now has a direct impact on your freight invoice, not just your compliance scorecard. As of July 19, 2025, the[National Motor Freight Classification (NMFC)](https://nmfta.org/standards/classification/nmfc/) shifted many commodities to a density-based classification system.


Under these rules, freight is classed primarily by its weight-to-volume ratio. Denser packaging can earn a lower freight class and lower rates, while light, bulky packaging can lead to higher classes and significantly higher shipping costs. In this environment, every inch of wasted space in a case pack is money leaving your margin.


## The Price of Noncompliance


Getting these details wrong is expensive. Walmart’s[SQEP](https://www.spscommerce.com/community/articles/supplier-guided-checklist-walmart-sqep) enforces these standards with a rigorous fine structure. Packaging and labeling defects typically incur a $200 administrative fee per defect per PO, plus an additional $1 per impacted case.


Common configuration-driven defects include:


-


**Wrong pack:** The vendor pack quantity shipped does not match the PO or the item file


-


**Inaccurate TI:** The cases per layer do not match the item file


-


**ASN mismatch:** The quantities or items on the ASN do not match the physical shipment


When these defects occur, they trigger a waterfall effect. Receiving associates must remove the "problem freight," generate tickets, and wait for disposition from the home office before the goods can even be processed for sale. This leads to lost sales and administrative burdens that often exceed the value of the original fine.


## Building a Pack That Lasts


A case pack that works at retail is one where the physical box and its digital data record never tell two different stories. By mastering the packaging hierarchy, adhering to GS1 GTIN standards, and ensuring your TI/HI patterns match your EDI transmissions, you transform a potential source of chargebacks into a competitive advantage.


As you add retailers and volume, managing these complex fulfillment requirements becomes harder to do manually.[SPS Commerce Fulfillment](https://www.spscommerce.com/products/fulfillment/edi/) helps your team stay aligned to thousands of trading partner requirements, reducing exceptions and ensuring your digital data moves as fast as your physical products. From purchase orders to ASNs and beyond, our network intelligence helps you get it right, protect your bottom line, and scale your business with confidence.
