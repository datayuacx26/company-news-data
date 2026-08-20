---
schema_version: "1.0.0"
document_id: "884ceab960fbcb07d4c8fd50570a61c2f48bfe217f11b620ebb4c78dd754085f"
company_key: "yc-meadow"
company: "Meadow"
source_id: "yc-meadow-news-import-8815cf464307"
canonical_url: "https://getmeadow.com/blog/metrc-retail-id"
published_at: "2026-07-28T21:39:04.425+00:00"
first_seen_at: "2026-07-24T11:04:17.621611+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:9019db275e8e2bd3d92ec5da53dd3c5fe519e769adcdf41def66fae1fb5c4cca"
---

# Metrc Retail ID: What Dispensaries Need to Know in 2026

Metrc Retail ID is a serialized, item-level QR code that Metrc issues to producers and distributors, who print it directly on packages before those packages reach a dispensary. Scan the code and you get the exact Metrc package behind that unit, no manual lookup required. The same code also works for your customers: they can scan it with a phone to confirm the product came from the regulated market, check lab results, and see the Certificate of Analysis (COA), and in some markets, check for active recalls.


Historically, getting a package to that scannable state has fallen to the retailer: printing a label for every single product so staff have something to scan at point of sale. That's hours of labeling labor, plus the ongoing cost of labels and label-printing hardware, on top of everything else a dispensary already handles. Retail ID moves that labeling upstream, so as more of the supply chain adopts it, dispensaries need to label fewer and fewer of their own packages over time.


**This guide covers how Metrc Retail ID works, where it's required or gaining ground, why it's worth building into your workflow, and how to scan Retail ID QR codes in Meadow today.**


#### In This Post


- [What Is Metrc Retail ID?](https://getmeadow.com/blog/metrc-retail-id#what-is-metrc-retail-id)
- [Why Retail ID Matters for Dispensary Operations](https://getmeadow.com/blog/metrc-retail-id#why-retail-id-matters-for-dispensary-operations)
- [Where Retail ID Is Required or Gaining Ground](https://getmeadow.com/blog/metrc-retail-id#where-retail-id-is-required-or-gaining-ground)
- [Where You Can Scan a Retail ID in Meadow](https://getmeadow.com/blog/metrc-retail-id#where-you-can-scan-a-retail-id-in-meadow)
- [What to Know Before You Rely on Retail ID Scanning](https://getmeadow.com/blog/metrc-retail-id#what-to-know-before-you-rely-on-retail-id-scanning)
- [Getting Started with Retail ID Scanning](https://getmeadow.com/blog/metrc-retail-id#getting-started-with-retail-id-scanning)
- [Common Questions](https://getmeadow.com/blog/metrc-retail-id#common-questions)


## What Is Metrc Retail ID?


In Metrc, every package is assigned a 24-character UID. Each time a package is split, new UIDs are assigned. This meant that it fell on the retailer to add a label to their products if they wanted the correct UID displayed.


Retail ID solves that by encoding the source package identity into a QR code applied at the unit level, upstream of a retailer's store. When a retailer scans the Metrc Retail ID QR, the system selects the correct package in their inventory.


A few things worth knowing:


- **Metrc issues the codes.** Producers and distributors generate and print Retail IDs on packaging before transfer. A dispensary can't create a Retail ID itself.
- **One scan, two audiences.** A retailer's team scans a code to identify the package in POS or inventory workflows. A customer can scan that same code to see a verification page confirming the product is from a licensed supply chain, has passed lab testing, and (in markets that support it) isn't under recall.
- **It supplements existing identifiers.** Retail ID sits alongside product barcodes and Metrc UIDs, which still work everywhere they did before, even if a product is missing its Retail ID.


Meadow has been working with Metrc on Retail ID since it was first announced. For background on how it came about, see Meadow's coverage of the[California Metrc User Exchange](https://getmeadow.com/blog/metrc-announces-retail-id-and-universal-product-catalog-at-user-exchange) and[Michigan Metrc User Exchange](https://getmeadow.com/blog/michigan-metrc-user-exchange-event-retail-id-and-universal-product-catalog) events where it was first previewed. Wherever Metrc issues Retail ID codes, Meadow reads them, and Meadow adds coverage as Metrc expands the program to new markets.


## Why Retail ID Matters for Dispensary Operations


**Less labeling labor and cost.** Labeling every product by hand is a real, recurring cost: staff time spent printing and applying labels, plus the ongoing spend on labels and label-printing hardware, for a step that exists purely so packages can be scanned later. As more of the supply chain adopts Retail ID, that labor and cost shrinks, since the package already arrives with a scannable code applied upstream. Per Metrc's own initial time trials, scanning saves roughly 60 seconds of labor per item, which the company estimates works out to about 2,000 hours of labor savings per year, per retailer, as adoption grows (source: Metrc, Retail ID launch announcement, August 2024).


**Selling from the right package.** Wrong-package sales are one of the most common sources of Metrc discrepancies. They tend to surface weeks later as inventory that doesn't reconcile, and they're painful to untangle during an audit. Scanning ties the physical unit in a staffer's hand to the exact package in Metrc, so a sale, pack, or count lands on the right record the first time, instead of relying on a manually applied UID label.


**Customer confidence in quality and authenticity.** Licensed retailers compete with an illicit market that can't offer verified lab results or recall status. Retail ID puts that proof directly in a customer's hand at the shelf, which is a tangible answer to "why buy legal" and a layer of protection for both the customer and the retailer's license if a recall hits.


## Where Retail ID Is Required or Gaining Ground


Retail ID's rollout varies significantly by state, and it's worth knowing where things stand before building it into your SOPs.


### New York: Required


New York made Retail ID (called "Retail Item ID" in the state's Metrc rollout) a required part of its seed-to-sale system. Per Metrc's New York guidance, processors have been required to affix Retail Item IDs to each unit before transferring finished goods to distributors since **November 7, 2025** , and distributors have been required to include Retail Item IDs on transfers to dispensaries since **February 28, 2026** . Retail Item IDs are digital-only QR codes.


New York is also the only Metrc state that charges licensees for Retail Item IDs, at $0.10 per code under Metrc's contract with the state. To offset the initial cost, OCM and Metrc provided a one-time allocation of 20 million Retail Item IDs to licensed processors at no cost, distributed evenly at 30,000 per processor and available through 2026 (per OCM's December 2025 announcement).


The practical upshot for a New York dispensary: as of the writing of this article, products arriving from distributors should already carry a Retail Item ID, and Meadow can scan them across POS, packing, and inventory workflows.


### California: Voluntary, and Growing Fast


California launched Retail ID in partnership with the Department of Cannabis Control (DCC) as a voluntary program in November 2024. Adoption has grown steadily since: as of Metrc's August 2025 announcement, roughly 20% of the state's weekly product volume carried a Retail ID QR code. That same month, Metrc and the DCC added direct-to-consumer recall notifications, so a customer scanning a code can see instantly whether that specific product has been recalled, in addition to confirming it's from the licensed market and has passed lab testing. The feature is additive: the DCC's official recall list remains the authoritative public record, and the QR lookup gives customers a fast way to check a specific product at the shelf.


For California retailers, that means a meaningful and growing share of inventory is already scannable without printing a single label, and the recall-lookup feature is a real point-of-sale trust signal worth mentioning to customers.


### Other States


Retail ID is expanding fast across Metrc's footprint. By mid-2025, Metrc reported more than 1 million Retail ID QR codes generated in a single week across 21 markets (per Metrc's June 2025 announcement). Among the states Meadow serves, Massachusetts has begun adopting Retail Item IDs on an optional basis; they are not required there. Rollout pace varies market to market, so if you operate outside New York or California, ask your distributors whether Retail ID codes are appearing on product in your market, and watch your state regulator's bulletins for updates, since more states may follow New York's lead in making it mandatory. Wherever the codes show up, Meadow can read them; where they don't yet, Meadow's own package labels fill the gap.


💡 **How Meadow handles this:** Wherever Metrc issues Retail ID codes, Meadow reads them across POS, packing, and inventory, so your coverage grows automatically as your distributors adopt it, with no setup work on your end.


## Where You Can Scan a Retail ID in Meadow


Retail ID scanning works anywhere a dispensary already looks up a product or package by hand, across both the iPad app and Meadow Admin.


### In the iPad App


- **Build a cart in POS.** Scan a package to add the matching product straight to a customer's cart.
- **Pack an order.** In the packing tool, scan to confirm or swap the package fulfilling an order, so budtenders sell from the correct package every time.
- **Add to a cycle count.** Scan a package to add its product to an open cycle count without searching for it manually.


### In Meadow Admin


- **Create a reconciliation.** Scan a package to start a reconciliation for it.
- **Add to a cycle count.** Scan to add a product to an open count from the admin side.
- **Create an inventory transfer.** Scan a package to start a transfer for it.
- **Search received packages.** On the Metrc received packages list, scan into the search bar to find a package, at either the product level or the package level.
- **Search inventory transactions.** Scan into the search bar on the inventory transactions list to find matching transactions.


💡 **How Meadow handles this:** Meadow's scanning already covers Metrc UIDs and product barcodes everywhere in POS and Admin, and now reads Retail ID the same way, so operations teams don't have to learn a second workflow. For the packing side specifically, see[Use Scanning at POS and the Packing Tool](https://intercom.help/meadow-admin/en/articles/5340068-use-scanning-at-pos-packing-tool-to-make-sure-you-re-selling-from-the-right-packages) .


## What to Know Before You Rely on Retail ID Scanning


A few realities are worth planning around before a team leans on Retail ID as its primary workflow:


- **Adoption is uneven.** Not every brand or distributor applies a Retail ID QR code yet, even in states where the program is live or required. Plan on a mix of manufacturer codes and Meadow-printed labels for a while. If a package doesn't have one, print a Meadow package label for it instead; it generates its own scannable QR code and works the same way a manufacturer-applied Retail ID would.
- **One code can point to more than one package.** Metrc sometimes issues the same Retail ID QR code to multiple packages. If a scan matches more than one, Meadow shows the matching options and asks staff to choose the correct product or package before continuing, so the wrong package never gets selected silently.
- **Scanning is additive, not required.** Retail ID doesn't replace scanning by product barcode or Metrc UID. It's a faster way to identify a package, layered on top of methods that already work.
- **There's a learning curve.** Any new identifier introduces some risk of mistakes during the transition. Rolling out workflow by workflow, and double-checking counts closely in the first few weeks, keeps small errors from compounding.


💡 **How Meadow handles this:** Meadow's[cycle count tools](https://intercom.help/meadow-admin/en/articles/13766508-meadow-s-all-inclusive-guide-to-cycle-counts) and[reconciliation workflows](https://intercom.help/meadow-admin/en/articles/13836349-how-to-use-reconciliations) already build in the same package-matching logic, so multi-package Retail IDs get resolved consistently no matter which workflow a scan starts from.


## Getting Started with Retail ID Scanning


Getting a team scanning day one doesn't take new hardware for most retailers already using a Socket scanner or an iPad camera with Meadow.


1. **Check your incoming product.** Look for Retail ID QR codes on deliveries and ask distributors which brands are already printing them.
2. **Confirm your scanning setup.** Any integrated scanner or iPad camera already configured for barcode or Metrc UID scanning in Meadow will read Retail ID codes the same way.
3. **Print Meadow labels for gaps.** For packages without a Retail ID, generate a Meadow package label from the package's menu in Admin so scanning still works downstream.


For the full walkthrough, see Meadow's help guide on scanning Metrc Retail ID QR codes.


#### More Resources for Dispensaries


- [Metrc in New York: What Dispensaries Need to Know](https://getmeadow.com/blog/new-york-dispensaries-transition-to-metrc)
- [Why Integrated Cannabis POS Software Is Essential for Compliance](https://getmeadow.com/blog/why-integrated-cannabis-pos-software-compliance)
- [New York Dispensary Software](https://getmeadow.com/new-york-dispensary-software) , for New York operators evaluating a Metrc-ready POS


## Common Questions


**What is Metrc Retail ID?** Metrc Retail ID is a serialized QR code that Metrc issues to producers and distributors, who apply it to a package before it reaches a retailer. Scanning it identifies the package's Metrc UID instantly, without a manual lookup.


**Do I need Retail ID to use scanning in Meadow?** No. Scanning by product barcode or Metrc UID still works everywhere it did before. Retail ID is an additional, faster way to identify a package, not a replacement for existing scanning methods. All other scanning still works the same, such as scanning UPCs at the SKU level, or scanning package IDs.


**Is Retail ID required in my state?** New York requires Retail Item ID QR codes on cannabis product packaging as of the 2025 to 2026 Metrc rollout. California's program is voluntary but widely adopted and growing. In other states, check with your regulator and your distributors for current status.


**What happens if a package doesn't have a Retail ID?** Retailers can't create a Retail ID themselves since Metrc issues it upstream. Instead, print a Meadow package label from the package's menu in Admin. It generates its own scannable QR code that works the same way.


**What happens if a Retail ID scan matches more than one package?** Meadow shows every matching product or package and asks staff to choose the correct one before continuing, so a scan never resolves to the wrong package automatically.


**Can customers scan a Retail ID too?** Yes. The same QR code that identifies a package for retail staff also lets consumers scan it for product information like lab results and Certificate of Analysis (COA), and in California, real-time recall status.


**Does Meadow support Retail ID scanning in my state?** Wherever Metrc issues Retail ID codes, Meadow reads them, and Meadow adds coverage as Metrc expands Retail ID to new markets.


**Where can I scan a Retail ID in Meadow?** In the iPad app, scanning works in POS cart building, the packing tool, and cycle counts. In Meadow Admin, it works for reconciliations, cycle counts, inventory transfers, and searching received packages or inventory transactions.


---


## Stay Compliant Without the Manual Work


Metrc Retail ID is one more way Meadow keeps inventory accurate without adding steps for your team. If you're evaluating a POS built to handle Metrc compliance end to end, from receiving to reconciliation, let's talk.


[Book a Call with Meadow to Learn More](https://getmeadow.com/schedule-demo)
