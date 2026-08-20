---
schema_version: "1.0.0"
document_id: "4ad1618fd411cb23ee8429c32da274d64cbc0c828a36a372606225f023ad993e"
company_key: "yc-carbonfact"
company: "Carbonfact"
source_id: "yc-carbonfact-rss-e3c23fd0e117"
canonical_url: "https://www.carbonfact.com/blog/platform/heuristics-and-rules"
published_at: "2026-06-25T10:45:31+00:00"
first_seen_at: "2026-07-24T22:18:04.567508+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:642f30927d76163007c10d8d72606d326050f43ed9c6f12891e3465ab5d99815"
---

# Carbonfact Explains: Heuristics and Rules

## **What are Carbonfact's Bespoke Emission Factors?**


*Part of Carbonfact’s*[Data Engine](https://www.carbonfact.com/data-management-software)


Heuristics and Rules are how Carbonfact produces a product footprint when your data is incomplete. We built them because no brand has a perfect Bill of Materials (BoM) for every SKU – we've frequently seen that component weights are missing, yarn densities aren't tracked, and factory locations are blank for a third of the supply chain.


Heuristics fill those gaps using patterns drawn from the 2 million+ LCAs we run each day across the 300+ brands we serve. Rules fill them using your brand's own knowledge – the things your team understands about your products that don't sit in any BoM column. The result? A defensible first product footprint in weeks, with a clear list of which gaps to close to make it more precise as your primary data improves.


#### TL;DR


Heuristics and Rules are how Carbonfact produces a defensible product footprint when your data is incomplete. Heuristics fill the gaps using patterns from the 2M+ LCAs we run each day; Rules fill them using the knowledge that lives inside your company.


## **How do Heuristics and Rules work?**


When Carbonfact calculates a product footprint, every input is resolved through a four-tier hierarchy:


1.


Primary data


– something you or your supplier directly provided. A weighted garment, a measured dtex, a named factory in a known city.


2.


Rules


– brand-specific knowledge encoded as a rule, applied wherever the rule matches. For example: “this supplier always uses 100% recycled polybags,” “every T-shirt ships in a 10g polybag regardless of what the export says.”


3.


Carbonfact Heuristics


– a data-backed estimate drawn from the patterns of the 2M+ weekly LCAs we run for our customers. Segmented by category, material, product class, and, where useful, by supplier.


4.


Third-party databases


–


[Ecoinvent, EF 3.1, Base Empreinte, ADEME, and many more.](https://www.carbonfact.com/data-management-software) Used when no relevant pattern exists in the Carbonfact dataset, typically for upstream chemistry and emission factors rather than for product attributes.


Every data point’s source is flagged in the platform. You can always see whether a number is yours, a rule, a heuristic, or a database value. And because Carbonfact’s Heuristics are trained on real product data rather than generic industry averages, the median value they produce is usually closer to your reality than a third-party database.


## **How Heuristics and Rules Show Up in Your Data**


You work in sustainability at a sneaker brand. You’ve onboarded a season’s worth of BoMs into Carbonfact, and the platform runs a full LCA on every SKU. Behind that LCA, Heuristics and Rules are doing real work in three specific places.


### 1. Example one: Missing component weights


Most BoMs list placements – upper, lining, midsole, insole – but only some carry an explicit mass in grams. In a recent gap analysis with one large customer, only 39% of BoM components had an explicit weight. For the rest, Carbonfact applies Heuristics that distribute the finished product weight across placements based on patterns from comparable products in the dataset. The split for a lightweight running sneaker isn't the split for a hiking boot, and the heuristic knows that.


For products where even the finished weight is missing, Carbonfact applies category-level defaults trained on real brand data: a T-shirt is 170g, a coat is 950g. These defaults aren’t guesses – they’re the median of every comparable product Carbonfact has measured.


#### A Rule for Component Weights


A brand might know from their R&D data that the midsole on their running sneakers is always 35% of the finished weight – not the 30% the heuristic estimates from comparable products. A rule like "for our running sneakers, midsole = 35% of finished weight" replaces the heuristic-derived split for every matching SKU, and scales correctly across sizes.


### 2. Missing yarn density (dtex)


Dtex – the linear density of a yarn, in grams per 10,000 meters – drives the energy footprint of yarn formation*. It’s almost never in a brand’s BoM. In the same customer dataset above, dtex coverage was 0%.


What Carbonfact does know: the material type and the spinning method (Ring Spun, Open End, Filament) are almost always in the BoM. The heuristic ties the dtex default to those known variables, so a ring-spun cotton thread in a knit T-shirt gets a different default than a filament polyester used in a woven shell. The result is closer to reality than a one-size-fits-all textile-industry average, even when nothing about dtex was ever supplied.


*The finer the yarn (lower dtex), the more spinning it takes to produce a kilogram, which means more machine time and more energy per kg of yarn.


#### A Rule for dtex


A brand might know that the polyester in their main fabric is always 50 dtex. A rule that says "polyester in this fabric = 50 dtex" replaces the material-type default everywhere that fabric appears, without having to update every BoM line.


### 3. Missing process step locations


A dyeing process step without a country attached can’t be matched to a heat and electricity grid factor, and the grid drives a large share of the dyeing footprint. Coverage gaps here are common: in the same customer dataset, 49% of process steps had no location. Carbonfact’s heuristic fills the gap in cascading order – first the Tier 2 country if it’s known on the same component, then a regional default tuned to the brand’s broader sourcing pattern, then a global average as a last resort. The same logic runs for Tier 3 (yarn/spinning) and Tier 4 (raw material) – tiers where almost no brand has visibility today.


#### A Rule for Process Step Locations


A brand might know that all their denim is woven in Türkiye, even when the BoM doesn't carry the country. A rule like "for denim fabric, weaving country = Türkiye" applies that location to every matching product – instead of the regional or global default the heuristic would have used.


Each of these Heuristics is flagged in Explorer – Carbonfact’s dashboarding tool. You can filter your product LCAs by data source and see exactly which values came from your data, which came from a heuristic, and which came from a third-party database like Ecoinvent or EF 3.1. The Uncertainty metric quantifies how much of your footprint depends on Heuristics versus primary data, so you can prioritize which gap to close first.


## **Heuristics and Rules in a Nutshell**


Heuristics are what Carbonfact brings: patterns drawn from the 2M+ LCAs we run every day across 300+ brands. Rules are what you bring: the knowledge of your brand that lives within your company. Together they let you start with a defensible product footprint in weeks rather than the six months it can take to chase every Tier 2 factory in your supply chain if you're not using


[Carbonfact for Suppliers](https://suppliers.carbonfact.com/) . Every assumption is flagged, quantified by Uncertainty, and replaceable as your primary data improves.


So when a stakeholder, auditor, or regulator asks where each number came from, you have a clear answer: primary data where you have it, your own rules where you've encoded what you know, transparent heuristics where neither applies, and a measured path to close the gap from there.
