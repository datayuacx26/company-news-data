---
schema_version: "1.0.0"
document_id: "d590111fe67d53d5766d76460d61316ecf34af2c834aa57852f24afb02c5dadc"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/pos-data"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-22T12:52:59.406044+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:6f2dfe177256b6f35f69297f40e84d631c70454ea313e17fbbf55a278170a621"
---

# POS Data: What Point-of-Sale Data Tells a CPG Brand

POS data, or point-of-sale data, is the record of what actually scanned at the register: the units and dollars of a product sold, broken out by item, store, and time period. It sits under almost every retail metric a CPG brand looks at, and it is the rawest, most trustworthy demand signal a brand can get. Everything else is built on top of it, so it pays to know exactly what POS data says and what it does not.


## What POS data measures


A row of POS data looks trivial: an item, a store, a week, a unit count, a dollar figure. From those five fields a brand derives most of what it tracks.


- Velocity: units sold per store per week, the normalized read on how fast a product moves no matter how many stores carry it.
- Distribution: how many stores, weighted by size, are actually scanning the item. Counting stores with at least one scan is how brands estimate where a product is and is not on shelf.
- Promoted vs. base sales: add a price and a promotion flag and POS data will separate the lift a deal drove from the sales that would have happened regardless.
- Trend: the same item in the same store, week over week, is where declines and accelerations first show their face.


## What POS data cannot see


POS data is a census of transactions, not of shoppers, and that leaves it with three fixed blind spots.


- No shopper. POS data shows what sold, never who bought it. It cannot tell a loyal repeat customer from a one-time deal chaser. For that you need loyalty or panel data.
- No cause. A scan never says why. A spike could be your promotion, a competitor stockout, the weather, or one shopper buying in bulk for an office party.
- No shelf truth. POS data shows what scanned, not what was on the shelf. A zero can mean no demand or an out-of-stock, and those two problems have opposite fixes.


Knowing the blind spots is the difference between reading POS data and being quietly misled by it.


## Where POS data comes from


The same scans reach a brand through different doors, and the door changes the shape of the data.


- Retailer portals like[Retail Link](https://www.cpgscout.ai/blog/retail-link-data) give you first-party POS data straight from one retailer, granular and fast but blind to the rest of the category.
- Syndicated providers (SPINS, NielsenIQ, Circana) pool POS data across retailers, which buys you category context but introduces some lag.
- Distributor portals like[KeHE Connect](https://www.cpgscout.ai/blog/kehe-connect) report distributor movement rather than true store scans.


Because the same metric arrives in formats that do not agree, the real work with POS data is harmonization: mapping item codes and week definitions so a number means the same thing across every source you pull from.


## Frequently asked questions


What is the difference between POS data and syndicated data?POS data is the raw record of what scanned at the register. Syndicated data is POS data aggregated across many retailers by a third party like SPINS or Circana: POS data with category context added, and some lag introduced.


Can POS data tell you why sales changed?No. POS data shows what sold, not why. A change could be a promotion, a competitor out-of-stock, seasonality, or a one-time bulk purchase. Explaining the change takes additional context the scan itself does not carry.


POS data is one of several retailer feeds a brand has to read together. For the full picture, see[What is retailer data?](https://www.cpgscout.ai/retailer-data) .
