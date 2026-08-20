---
schema_version: "1.0.0"
document_id: "584d3cecf7be3167b97b21852c997327dd3e7a84e48c916fa246820c4f797974"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/incoterms"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-08-09T23:40:56.159161+00:00"
fetched_at: "2026-08-09T23:40:57.548506+00:00"
content_hash: "sha256:d463c582c8c0d95e5a7301ea2334dbfa2d44f1e0153877a493ab1e3f477d5cab"
---

# What Are Incoterms? The 2020 Rules, Explained for Buyers

Incoterms are a set of 11 standardized three-letter trade terms published by the International Chamber of Commerce that define the responsibilities, costs, and risk transfer points between a buyer and a seller in an international (and increasingly domestic) sales contract. The current version is Incoterms 2020. They tell you who pays freight, who handles customs, who insures the goods, and exactly where the risk of loss shifts from seller to buyer.


The whole reason Incoterms exist is to remove ambiguity. You write "FOB Shanghai" on a PO and there's a published, internationally recognized definition of what that means. No interpretation needed. Get the term wrong, though, and you find out three weeks later when a freight invoice you didn't expect lands in your inbox.


## The 11 Incoterms 2020 Rules at a Glance


Two groupings: rules for any mode of transport, and rules for sea and inland waterway only.


**Any mode of transport (use these for containerized ocean, air, road, rail, multimodal):**


- **EXW (Ex Works)** : Seller makes goods available at their premises. Buyer does literally everything else, including loading. Maximum buyer responsibility.
- **FCA (Free Carrier)** : Seller delivers goods to a named carrier or place. Risk transfers when the carrier takes possession.
- **CPT (Carriage Paid To)** : Seller pays freight to a named destination. Risk transfers when the seller hands goods to the first carrier (not at destination, despite what the name implies).
- **CIP (Carriage and Insurance Paid To)** : Same as CPT but seller also pays for insurance. Insurance level was upgraded in Incoterms 2020 (more on that below).
- **DAP (Delivered at Place)** : Seller delivers to a named destination, ready for unloading. Buyer handles import clearance and unloading.
- **DPU (Delivered at Place Unloaded)** : Seller delivers AND unloads at the named place. New name in 2020 (was DAT in 2010).
- **DDP (Delivered Duty Paid)** : Seller pays everything, including import duties and taxes. Maximum seller responsibility.


**Sea and inland waterway only (do not use these for containers loaded at a terminal):**


- **FAS (Free Alongside Ship)** : Seller delivers goods alongside the vessel at the named port.
- **FOB (Free On Board)** : Seller delivers goods on board the vessel at the named port. Risk transfers once goods are on board.
- **CFR (Cost and Freight)** : Seller pays freight to destination port. Risk transfers when goods are on board at origin.
- **CIF (Cost, Insurance and Freight)** : Same as CFR but seller also pays insurance.


Common mistake: writing "FOB Shanghai" on a PO for a container shipped via ocean. Technically FOB is for non-containerized cargo loaded at the port. For containers, FCA is the correct term because containers are typically delivered to a terminal, not loaded directly onto a vessel by the seller. ICC has been telling people this since 2010 and most procurement teams still use FOB out of habit. It rarely causes a problem, but it's wrong.


## What Changed From Incoterms 2010 to 2020


Three substantive changes worth knowing.


**DAT became DPU.** Delivered at Terminal was renamed Delivered at Place Unloaded. Same idea but the new name makes clear it works at any unloading location, not just a terminal.


**CIP insurance level went up.** Under 2010 rules, both CIF and CIP only required minimum coverage (Institute Cargo Clauses C). Under 2020, CIP requires the higher Clauses A coverage (all-risks), while CIF still defaults to Clauses C. If you're shipping high-value goods CIP, you now get better coverage by default. If you're shipping by sea CIF, you still need to negotiate up to Clauses A in the contract.


**FCA gets an "on board" option.** A new feature lets buyer and seller agree that the carrier will issue an on-board bill of lading after loading. This matters for letter-of-credit transactions where banks require an on-board BOL.


There were also clarifications around buyer-arranged transport (FCA, DAP, DPU, DDP), seller-arranged transport, and security-related obligations under all rules. None of those changed the substance of risk transfer, just the documentation around it.


## Which Incoterm to Use When


Forget the chart. Here's the practical version.


**You're a buyer and you want maximum control.** Use FCA at the seller's named place, or EXW if you have a freight forwarder you trust. You arrange freight, you negotiate the rate, you hold the relationship with the carrier. Most established procurement teams default here for ocean freight from Asia because they have better freight rates than the suppliers do.


**You're a buyer and you want zero hassle.** Use DDP. Seller delivers it to your door with duties paid. You'll pay more (the seller bakes freight, insurance, and duty into the unit price plus a margin), but you don't have to think about logistics. Useful for low-volume or first-time international purchases.


**You're a buyer and you want a middle ground.** DAP works well. Seller arranges freight and clearance at origin, buyer handles import clearance at destination. You see the freight cost embedded in the price but you control customs.


**Avoid CIF and CFR for containerized ocean unless your supplier insists.** They're sea-only terms designed for bulk cargo. Use CIP or CPT for containers. Same idea, but the right term.


**Avoid EXW for international shipments unless you have a freight forwarder at the supplier's origin.** Technically EXW puts loading and export clearance on the buyer, which the buyer can't usually do remotely. In practice, most "EXW" deals end up with the seller doing the export clearance anyway, which creates legal ambiguity. Either accept that and write FCA, or have a forwarder on the ground.


**Sea-only terms (FOB, CFR, CIF, FAS) for non-containerized cargo only.** Bulk grain, oil, project cargo. Not containers.


## Where Buyers Get Burned


The pattern is almost always the same. The PO says one Incoterm. The supplier ships. Something goes wrong (port delay, theft, damage, customs hold). And the question comes up: who's responsible, and who pays?


The answer should be in the Incoterm. But it isn't always, because of two issues.


The Incoterm wasn't actually negotiated. The buyer used the same template they always use, the supplier signed it without flagging that they expected something different, and now there's an argument about whether DAP includes the supplier paying duty (it doesn't, that's DDP) or whether FOB means the supplier pays freight to destination (it doesn't, that's CFR or CIF).


The Incoterm was right but the contract didn't specify enough else. Incoterms cover risk and cost transfer. They don't cover quality, payment terms, dispute resolution, force majeure, or warranty. A PO that says "FOB Shanghai" and nothing else is missing 80% of the actual contract.


The opinion: most procurement teams don't take Incoterms seriously enough until they get burned, and then they overcorrect. Pick a default term that fits your business (we like FCA for ocean from Asia, DAP for road from Mexico/Canada), train your buyers on it, and write down the few exceptions where you'd use something else. Don't pretend everyone on your team knows the difference between CIF and CIP without training.


## What Incoterms Don't Cover


This is the part that bites people. Incoterms only define:


- Who pays for transport
- Where risk transfers
- Who handles import/export clearance
- Who's responsible for insurance (under CIF and CIP)


Incoterms do not define:


- Title transfer (governed by sales contract law)
- Payment terms (Net 30, LC, advance)
- Quality specifications
- Warranty terms
- Damages or remedies for non-performance
- Tax treatment (sales tax, VAT, GST)


If your only contractual document with a supplier is a PO with an Incoterm and a price, you have a contract that doesn't address most of the questions that come up when something goes wrong. Add terms and conditions. Have legal write a master supply agreement for high-value or high-risk relationships.


## How Lumari Handles Incoterms in Day-to-Day Procurement


Lumari reads supplier emails, parses ASNs, ship dates, and freight notifications, and ties them back to the PO so the buyer can see whether shipments are tracking against the agreed Incoterm. When a supplier emails saying "we'll cover the freight on this one" but the PO is FCA, Lumari flags the deviation instead of letting it slip through to the invoice mismatch six weeks later.


If your team is finding Incoterm discrepancies at invoice match instead of at shipment,[see how Lumari catches them upstream](https://lumari.ai/) .
