---
schema_version: "1.0.0"
document_id: "e6c29fea980d12ce8508dcdda053a99bc5dff1eababb865b13eb1ee3eac65956"
company_key: "yc-verbiflow"
company: "Verbiflow"
source_id: "yc-verbiflow-news-import-392b5e1f9020"
canonical_url: "https://verbiflow.com/blog/every-security-guard-firm-in-the-us"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-22T18:39:30.762214+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:8fde2405207f0b3b2cb1c4e8967ddc00c5c7fc8c19186b4146aaace7743c054a"
---

# How we found every regional security-guard firm in the US (and surfaced the ones losing customers)

A customer selling software into physical-security firms had a familiar problem: the buyer is real, but no vendor sells a clean list of them. ZoomInfo and Apollo have a handful. Crunchbase has none. The data is sitting in Google Maps and the US Census, and nobody has bothered to assemble it. So we did.


1,935


US cities (all 51 states + DC), tier A/B/C by population


7 stages


Census load → seed → subareas → find → classify → reviews → negatives


1 SQLite DB


resumable, restartable, no orchestration framework


## Why this play exists


Physical-security firms are local. The buyer isn’t a household name. The category doesn’t map cleanly to the NAICS codes vendor databases lean on, so the standard B2B list-buying motion returns garbage. The customer would have spent three months stitching exports together and still missed half the market.


So we built a pipeline that walks the country city by city, finds every guard firm operating there, classifies each one, and ranks them by how unhappy their recent customers sound.


## The pipeline


- **Load Census data.** Population, place names, lat/lng for every city in the US. Filter to the cities that actually have a market.
- **Seed cities.** 1,935 cities across all 51 states, tiered A/B/C by population.
- **Generate sub-areas.** For metros with more than 1M people, ask Claude to split the metro into neighborhoods (Manhattan / Brooklyn / Queens / ...). Necessary because Google Maps caps results at ~120 per query and the largest cities have thousands of firms.
- **Find companies.** Apify Google Maps actor, query each sub-area for security-guard businesses.
- **Classify with Claude.** Filter out franchises, training schools, alarm installers. Keep the actual local guard providers.
- **Fetch reviews.** Pull recent Google Maps reviews for each surviving company.
- **Surface negative reviewers.** Find the firms whose recent reviews skew negative. These are the customers most likely losing accounts. Sequence them first.


Why SQLite, again


Every stage reads and writes to the same database. Kill any step mid-run, re-run it, it picks up where it left off. No orchestration framework, no temporal worker. The whole pipeline is seven Python scripts and a DB.


## What the customer actually got


- A ranked list of physical-security firms in their target region, deduped, classified, freshness-stamped.
- The right contact at each one, sourced via the standard Verbiflow contact-finding tools.
- A “churn-risk” score per firm based on recent review sentiment. The customer sequenced the high-score firms first, with copy that referenced the visible problems.


Reply rate on the high-score segment was meaningfully better than the rest, because the cold email opened with something the buyer was already worried about.


> Vendor lists are static. A pipeline you can re-run is worth more than a list you bought once.


## What this is a template for


Any local-services category. Pest control. Towing. Locksmiths. Roofing contractors. Independent restaurants by region. Same pipeline shape: Census + Google Maps + a Claude classifier + a reviews-based ranking step.


Vendor lists miss these categories because the firms don’t sit in Crunchbase. Plays catch them because the data is public if you go look for it.
