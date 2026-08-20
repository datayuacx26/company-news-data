---
schema_version: "1.0.0"
document_id: "2e79c6372ebe209f10f370bd4b93c09adca761eb66b1428b1978f11471775f5f"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-news-import-ac2626a08ad7"
canonical_url: "https://www.spscommerce.com/community/articles/gs1-standards-for-new-suppliers-gtins-upcs-and-case-codes"
published_at: null
first_seen_at: "2026-07-31T22:54:45.639349+00:00"
fetched_at: "2026-07-31T22:54:46.737046+00:00"
content_hash: "sha256:f8b230015f625c1224a554d04cfb743edbe0f1de7d18b9b5ef6be80a39a9ddbf"
---

# GS1 Standards for New Suppliers: GTINs, UPCs, and Case Codes

In this article, learn about:


-


The distinct roles of GTINs, UPCs, and case codes across the retail supply chain


-


How to license legitimate identifiers from GS1 US and why avoiding resold codes is critical


-


The downstream impact of identification accuracy on retail fulfillment and compliance chargebacks


---


Many first-time suppliers view barcoding as a simple checkout formality, but this assumption often leads to technical hurdles for brands transitioning into retail. Item identification is one of the most critical technical decisions a brand will make because it serves as the foundation for every downstream transaction, from[purchase orders (POs)](https://www.spscommerce.com/community/articles/what-is-a-purchase-order) to inventory tracking and[advance ship notices (ASNs)](https://www.spscommerce.com/community/articles/edi-856-ship-notice-manifest) .


Getting this right before your first PO is a matter of practical prevention. Retailers have become increasingly strict about validating ownership and accuracy of these identifiers, and errors often surface as rejected item setups or expensive compliance chargebacks. To navigate this landscape, you must understand that **Global Trade Item Numbers (GTINs)** , **Universal Product Codes (UPCs)** , and **case codes** are not interchangeable names for the same barcode. These terms are distinct identifiers doing different jobs at various levels of the supply chain.


## What Is a GTIN?


The[Global Trade Item Number, or GTIN](https://www.gs1.org/standards/id-keys/gtin) , is the umbrella term for the entire family of[GS1](https://www.gs1us.org/upcs-barcodes-prefixes/what-is-a-gtin) data structures. Think of a GTIN as a unique digital fingerprint for your product that allows it to be identified anywhere in the world. It is used for items sold, ordered, or invoiced at any stage, from the manufacturing floor to the store shelf.


While people often use the word GTIN to refer to a specific barcode, it actually refers to the number itself. The most common formats you will encounter are:


-


**GTIN-12** : primarily used in North America as the UPC


-


**GTIN-13** : common in Europe as the EAN


-


**GTIN-14** : used for cases and master packs


Every variation of a product — every size, color, style, or packaging configuration — requires its own unique GTIN. For instance, if you sell a shirt in three sizes and three colors, you have nine distinct identification requirements.


## What Are Universal Product Codes (UPC)?


The Universal Product Code is the 12-digit identifier that most consumers recognize. In the[GS1](https://www.gs1.org/home) system, this is technically a GTIN-12. It is assigned to individual selling units, which are the smallest units of a product a consumer can buy, such as a single bottle of shampoo or a box of cereal.


The UPC consists of two parts:


-


The machine-readable[barcode](https://www.spscommerce.com/community/articles/what-are-the-different-segments-of-a-barcode) , which is the familiar series of black bars


-


The unique 12-digit number printed below the black bars


When a retailer’s point-of-sale (POS) system scans this code, it pulls the item data, description, and price from the retailer’s database. For a new supplier, the UPC is often the first step in product setup.


**Related Reading** :[UPC, GTIN, and Style Number Basics for Apparel and Footwear](https://www.spscommerce.com/community/articles/upc-gtin-and-style-number-basics-for-apparel-and-footwear)


## What Are Case Codes (GTIN-14)?


While the UPC handles the consumer interaction, the case code (or GTIN-14) handles the logistical journey. A case code is assigned to the vendor pack configuration, which is the box or carton you use to ship multiple individual units to a retailer’s distribution center (DC).


Many new suppliers mistakenly apply the consumer UPC to the outside of a shipping carton. But this is both incorrect and a recipe for possible compliance fines. If a warehouse associate scans a box of 12 items and the scanner reads it as a single consumer unit, your inventory records will immediately degrade.


Retailers like[Walmart require a separate GTIN-14](https://www.spscommerce.com/community/articles/what-are-walmarts-gtin-standards) at the case level, often applied via an ITF-14 or[GS1-128](https://www.spscommerce.com/community/articles/gs1-128-barcode-label) barcode format on the carton itself. This allows the retailer to know exactly what is inside a carton without ever opening it, facilitating efficient receiving and cross-docking.


## How to Obtain Legitimate Identifiers


The only way to ensure your product identifiers are accepted by major retailers and marketplaces is to license them directly from GS1 US. Retailers increasingly check new supplier identifiers against the[GS1 global registry](https://www.gs1us.org/tools/gs1-company-database-gepir) to confirm that the brand owner listed in the registry matches the company applying for onboarding.


When you go to GS1 US, you generally have two paths:


1.


**GS1 company prefix** : This is a unique number assigned to your company that forms the base of all your GTINs. Licensed prefixes come in different capacities, allowing you to identify anywhere from 10 to 100,000 products.


1.


**Single GS1 US GTIN** : For very small brands or those launching with only one or two products, you can license a single GTIN for a flat fee (currently $30) with no annual renewal. This identifies your company as the brand owner.


It is important to avoid cheap and/or resold UPCs. Some third-party websites sell "discounted" barcodes that were originally licensed to other companies. Because major retailers like Amazon and Walmart now validate prefix ownership, using a resold code can lead to your item setup being rejected or your products being delisted entirely.


**Related Reading** :[What are the Different Item Codes in the Retail Industry?](https://www.spscommerce.com/community/articles/what-are-the-different-item-codes)


## Retailer-Specific Standards and Hierarchy


Identification requirements evolve as retailers invest more in automation. Walmart, for example, recently shifted its policy to require GTINs at every single level of the packaging hierarchy: the each, the warehouse pack, the[case pack](https://www.spscommerce.com/community/articles/case-pack-configuration-how-to-build-a-pack-that-works-at-retail) , and the[pallet](https://www.spscommerce.com/community/articles/pallet-building-101-ti-hi-stacking-and-why-retailers-care) .


This hierarchy is essential for accurate ordering and tracking. A typical setup might look like this:


-


**Each:** The individual unit (GTIN-12/UPC)


-


**Case:** A carton containing multiple eaches (GTIN-14)


-


**Pallet:** A logistical unit containing multiple cases (often identified with a serial shipping container code or[SSCC](https://www.gs1us.org/upcs-barcodes-prefixes/serialized-shipping-container-codes) )


If you change your case pack quantity or the net content of your product, you cannot simply update your records. GS1 standards require you to issue a new GTIN. Reusing a GTIN from a retired product is also generally prohibited. Once a GTIN is assigned to a product, it is permanent.
Reassignment can lead to[ghost data](https://www.spscommerce.com/community/articles/dont-get-spooked-by-phantom-inventory) in retailer systems where an old product description might overwrite a new one.


## Why Identification Accuracy Matters Downstream


The consequences of a missing or incorrect case code extend far beyond a rejected setup form. In an omnichannel environment, item data supports a massive web of automated systems, including warehouse robots, replenishment algorithms, and digital shelf displays.


When item data is wrong, it creates cascading failures. For example, Amazon maintains a GTIN-14 gold list. If your case-level barcode scans correctly but the number is not registered on Amazon’s list, their receiving process will fail, triggering a compliance chargeback. Similarly, if your case dimensions or weights are submitted incorrectly during setup, the retailer’s warehouse management system (WMS) will assign products to the wrong bin locations, leading to bottlenecks and manual workarounds.


Furthermore, your GTIN is the primary key for[electronic data interchange (EDI)](https://www.spscommerce.com/community/articles/what-is-edi) . When you send an EDI 856/ASN, the GTIN-14 on the physical carton must match the data in the electronic message. If there is a mismatch, the retailer’s automated receiving system will flag an exception, often resulting in a fine and a strained trading partner relationship.


**Related Reading** :[2D Barcodes & RFID: Terms, Roles, and Responsibilities](https://www.spscommerce.com/community/articles/2d-barcodes-and-rfid-terms-roles-and-responsibilities)


## Building a Foundation for Growth


Correctly identifying your products is a business process that uses technology, not an IT task to be checked off and forgotten. It requires executive sponsorship to set standards and operational ownership to maintain them as your catalog grows.


By treating GTINs, UPCs, and case codes as strategic assets rather than administrative burdens, you position your brand for a smoother onboarding experience and more efficient operations. Your identification strategy is the foundation that your entire retail fulfillment story sits upon. Once your identifiers are accurate and registered, you can move forward with confidence into the worlds of EDI,[fulfillment](https://www.spscommerce.com/products/fulfillment/edi/) automation, and scalable retail growth.


Success in the retail supply chain starts with understanding the rules of the game. At[The Supply Chain Source](https://www.spscommerce.com/community/) , we provide the community knowledge, expert insights, and practical resources you need to turn complex mandates into competitive advantages. Whether you are navigating your first item setup or optimizing an existing network, our library of webinars, cheat sheets, and articles is designed to help you learn faster and think bigger.
