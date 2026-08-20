---
schema_version: "1.0.0"
document_id: "b283179593a0f823c75ca6297502a64533baacd6f43a6eaa7c645b48cdd61a86"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-news-import-ac2626a08ad7"
canonical_url: "https://www.spscommerce.com/community/articles/supply-chain-management-from-order-to-fulfillment"
published_at: null
first_seen_at: "2026-07-31T22:54:45.639349+00:00"
fetched_at: "2026-07-31T22:54:46.737046+00:00"
content_hash: "sha256:07e93f744bb1d3cd6233f6f181b2108053dd8d8c245d01fd798a835df35110f5"
---

# The Supply Chain Management Process From Order to Fulfillment

[Explore SPS Commerce](https://www.spscommerce.com/)[Contact Sales](https://www.spscommerce.com/contact)


# The Supply Chain Management Process From Order to Fulfillment


By[Peter Spaulding](https://www.spscommerce.com/community/profile/peter-spaulding) , *Sr. Content Writer*


Last Updated July 31, 2026


9 min read


In this article, we cover:


-


- The key steps in the supply chain management process


How purchase orders, advance ship notices (ASNs), and invoices work together


How a digital supply chain surfaces exceptions before they become chargebacks


Why supply chain visibility and orchestration matter for suppliers


Most content about the supply chain management process explains the concept. Far less of it walks through what actually happens between the moment a retailer’s purchase order arrives and the moment a product reaches a shelf. This article maps that sequence step by step: order receipt, acknowledgment, allocation, the advance ship notice, the handoff to a carrier or third-party logistics (3PL) provider, receiving at the retailer’s distribution center (DC), and the invoice that closes the loop.


Two things move through each of these steps, the physical goods and the data describing them. The process tends to work or break at the points where one party’s output becomes the next party’s input.


## What Are the 2 Tracks of a Digital Supply Chain?


A digital supply chain runs on two parallel tracks. One is physical: raw materials become finished goods, get picked and packed, ship, and arrive at a store or DC. The other is data: a purchase order becomes an acknowledgment, then a shipping notice, then an invoice. These steps map to the standard process components of planning, sourcing, fulfillment, and returns described by the Association for Supply Chain Management (ASCM).


The data track exists so each party can act on the physical track before a problem compounds. If a supplier can only fill part of an order, the data track is what tells the retailer’s warehouse to expect a shortfall before a truck arrives without enough product on it. When that data moves on schedule, an exception is a quick correction. When it doesn’t, the same exception travels downstream until someone finds it the hard way, usually at receiving or on an invoice.


## How Does a Purchase Order Become a Shipment?


Each step in this process pairs a physical action with a document, and naming both shows exactly where the process is built to catch problems.


Order receipt starts the sequence. A retailer sends a purchase order, formatted as an electronic data interchange (EDI) transaction known as an EDI 850, specifying items, quantities, pricing, and delivery requirements. Everything downstream measures itself against this document.


Acknowledgment follows. The supplier responds with an EDI 855 purchase order acknowledgment, confirming it can fill the order as written or flagging a change in quantity, price, or timing. This is the cheapest point in the process to catch a mismatch. A correction made here is a one-line update. The same mismatch caught later is a short shipment.


Allocation and fulfillment happen next, on the physical side. The supplier checks available inventory, allocates stock to the order, and the warehouse picks and packs it.


The advance ship notice, or EDI 856, tells the retailer’s DC what’s coming before the truck arrives: item counts, pallet configuration, carton labeling, and expected arrival time. The ASN is what lets a receiving team scan a pallet and know immediately whether it matches what was promised.


A carrier or 3PL handoff is the point where the supplier’s responsibility for the physical goods ends and a logistics partner’s begins. The data track keeps pace through carrier confirmations and tracking updates, so both sides know where a shipment stands without picking up the phone.


Receiving and reconciliation happen at the DC, where the ASN is checked against what physically arrived. A match closes the loop quietly. A mismatch becomes an exception that someone, on one side or the other, now has to resolve by hand.


Invoice and payment matching close out the sequence. The supplier sends an EDI 810 invoice, and the retailer matches it against the original purchase order and the ASN before paying. Together, these four EDI transactions form the core document flow used by retailers and suppliers.


For a fuller walkthrough of how these documents work together, SupplierWiki has a plain-English breakdown of the full sequence.


## What Does Orchestration Mean at Each Handoff?


Orchestration gets used as a label for “things are connected” without saying what’s connected or how. In this process, it has a specific meaning: each handoff is the point where one party’s data becomes another party’s working instructions. A supplier’s ASN becomes the DC’s receiving plan. A 3PL’s delivery confirmation becomes the supplier’s shipment record. Orchestration is what a digital supply chain is for.


When that data passes automatically, an exception surfaces at the handoff, while it’s still cheap to fix. When it doesn’t, and someone has to re-key a number or wait on an email, the same exception travels downstream until it shows up as a deduction, a chargeback, or an out-of-stock. The mechanism doesn’t change between those two outcomes. What changes is whether the data arrives in time for someone to act on it.


The cost of letting that happen has gone up because major retailers have formalized the penalty for it. Walmart’s Supplier Quality Excellence Program (SQEP) rolls out defect categories in phases, covering purchase order accuracy and missing or late ASNs in an early phase and barcode and labeling errors in a later one, each carrying chargebacks separate from On-Time In-Full (OTIF) penalties, according to a compliance guide from Productiv. OTIF misses are billed at roughly 3% of the value of non-compliant cases, and the two programs can stack on the same shipment. For a supplier shipping $50,000 a week, a 10% miss compounds to roughly $78,000 a year, according to a separate compliance breakdown from Daserv. Other major retailers and distributors run comparable programs.


## Where Do Small Errors Turn Into Chargebacks?


Three short examples show how the same kind of error costs differently depending on where it gets caught.


A quantity mismatch is the simplest case. If a supplier can only fill 8,000 of 10,000 units on a purchase order and flags that on the EDI 855, the retailer adjusts its expectations before the shipment ever leaves the warehouse. If the same mismatch isn’t flagged until the goods arrive at the DC, it becomes a short shipment, which falls under OTIF.


A missing or late ASN turns a routine receipt into a manual one. Without an ASN to scan against, a DC has to verify the shipment by hand, which slows down the dock and can trigger an SQEP defect on top of any OTIF penalty already in play.


A labeling error compounds further downstream. A barcode that doesn’t match the case contents, or isn’t placed on the required side of the pallet, can cause an automated DC system to reject the pallet outright, turning a packaging mistake into a full return.


The underlying error is small in each case. What determines the cost is how many steps it travels before someone catches it.


## What Can KFC and Zara Teach Suppliers?


Two cases from outside the SPS Commerce network show what sits at either end of this process. Neither involves an SPS Commerce customer; they’re useful because they show the same mechanism running at a scale most suppliers will never operate at.


### KFC’s 2018 Distribution Failure


In February 2018, KFC’s UK business switched its logistics partner from a company running six distribution centers to a single new warehouse operated by DHL, with the cutover happening overnight and no period where the old and new systems ran side by side. Within days, more than two-thirds of KFC’s 900 UK restaurants were closed or running on a limited menu, and the disruption cost the business a 2% drop in same-store sales and a 5% hit to operating profit in the first quarter, according to Supply Chain Dive. Supply problems continued for weeks afterward, including a gravy shortage reported well into the recovery.


The failure traces back to that single overnight cutover. When the new warehouse ran into delivery delays in its first days of operation, there was no connected visibility between the supplier side, KFC, and DHL to catch the problem while restaurants still had product on hand. By the time anyone outside the warehouse knew something was wrong, it had already shown up as closed restaurants.


### Zara’s Continuously Connected Supply Chain


Zara, the flagship brand of Inditex, runs on close to the opposite principle. Designers, market specialists, production planners, and store managers exchange sales data and design feedback continuously, which lets the company move a new design from sketch to store in roughly two to three weeks, against an industry norm closer to several months, according to Harvard Business Review’s 2004 analysis of the model, an assessment that has held up well over the two decades since. Store managers report what’s selling, that data feeds back to design and production almost immediately, and stores receive new shipments multiple times a week instead of on a seasonal cycle.


None of this requires becoming Zara. The mechanism behind its speed is the same one behind an accurate ASN: data moves between every party without a built-in delay, so a problem surfaces as soon as it happens instead of weeks later as a stock-out. Zara just runs that mechanism at a faster cadence and a larger scale than most suppliers need to.


## How Can You Audit Your Supply Chain Management Process This Week?


A supplier can hold their own version of this process up against five questions.


-


- Do you acknowledge every purchase order, or only the ones that need a change?


Does your ASN reflect what’s physically on the pallet, or what the order said it should be?


How do you currently find out about an exception: from your own data, or from a retailer’s deduction notice?


How many separate systems does a single order touch between receipt and invoice?


Who notices first when a shipment is running late, your team or your retailer?


A shortage reported first through a deduction notice traces back to one of the handoffs above, the point where the data should have surfaced the problem earlier.


## Reduce Friction Between Every Supply Chain Handoff


With[SPS Commerce Fulfillment](https://www.spscommerce.com/products/fulfillment/edi/) , the network manages retailer spec changes and monitors your transactions proactively, so your team isn’t debugging EDI workflows and can focus instead on moving the business forward. See how Fulfillment connects the process.
