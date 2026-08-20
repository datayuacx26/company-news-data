---
schema_version: "1.0.0"
document_id: "294223c4f6b0b4fde22b6e3a47de889946167f2f99719569ae808ef7bc13002c"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/xaoc-mulo-explained"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-24T00:54:45.437387+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:fd47f81eb00bc3fbba0406eee301155488ecff084b12b8b2dfd8cb9502d647d0"
---

# xAOC, MULO & FDMx: CPG Market Definitions

xAOC stands for "extended All Outlet Combined" and is NielsenIQ's label for its broadest total-US retail measurement: grocery, drug, mass, club, dollar, military, and a wide set of other food and non-food retailers, all combined into one national benchmark. If you have ever pulled a Nielsen report and seen "Total US xAOC" at the top of the market selector, that is the view.


This post covers what xAOC includes (and what it deliberately leaves out), how Circana's[MULO](https://www.cpgscout.ai/learn/what-is-mulo) and MULO+ compare, what FDMx was and why most teams have moved on from it, and how to choose between these views depending on your brand's channel footprint. It is aimed at brand managers and insights analysts who are new to[syndicated data](https://www.cpgscout.ai/blog/what-is-syndicated-data) or who are switching between data providers and need to align terminology.


## What is xAOC?


NielsenIQ defines xAOC as the "extended All Outlet Combined" universe. The "extended" part is meaningful: it goes beyond the traditional food-drug-mass (FDM) footprint and includes club stores (Costco, Sam's Club), dollar stores (Dollar General, Dollar Tree, Family Dollar), and military commissaries. The result is a panel that covers roughly 90% of US food and non-food CPG retail dollars, depending on the category.


To make that concrete: a brand selling a $6.99 pasta sauce might see roughly $280M in measured retail sales under Total US xAOC. If you stripped out club and dollar, you would lose maybe $40-50M of that, because club over-indexes on bulk pack sizes and dollar stores now move meaningful grocery volume in lower-income geographies. Leaving those channels out of your benchmark understates the real competitive picture.


### What xAOC includes


- Grocery (supermarkets, natural/specialty chains)
- Drug (CVS, Walgreens, Rite Aid)
- Mass merchandisers (Walmart, Target)
- Club (Costco, Sam's Club)
- Dollar stores (Dollar General, Dollar Tree, Family Dollar)
- Military commissaries
- Select other non-traditional retailers


### What xAOC leaves out


xAOC does not include convenience stores (c-stores), e-commerce (Amazon), foodservice, or Walmart's online channel unless you add the relevant panel extensions. NielsenIQ sells these as separate data sources. If your brand does significant volume through 7-Eleven or Circle K, your xAOC number will look light compared to your internal shipment data, and you need the c-store panel to reconcile it.


## What is MULO (and MULO+)?


MULO stands for "Multi-Outlet" and is Circana's (formerly IRI) label for its total-market view. Conceptually it serves the same purpose as xAOC: one number that summarizes national retail performance across the main measured channels. The channel list is nearly identical to xAOC, covering grocery, drug, mass, club, and dollar. The differences are primarily definitional and methodological, not strategic.


MULO+ adds convenience stores on top of MULO. So if you see a Circana report with "MULO+C" or "MULO+Conv", that is the convenience-store extension. NielsenIQ handles the same expansion differently: xAOC+C is a common pull label in Nielsen syndicated reports. The plus-C convention has largely converged across both providers, even if the base market names differ.


### MULO vs MULO+ in practice


Whether to use MULO or MULO+ depends on your category. Energy drinks, water, salty snacks, and tobacco all see substantial c-store volume. For a brand like a sparkling water that does 25% of its volume through c-store, leaving that out of your benchmark hides a real competitive pressure point. For a premium pasta sauce that sells almost nothing through Circle K, the c-store add-on just adds noise and makes comp shopping across datasets messier.


## FDMx and what replaced it


FDMx stands for "Food, Drug, Mass plus x" where the x indicated additional channels (typically club or dollar, depending on the vintage of the report). It was a common market definition in the early 2010s when club and dollar were not yet treated as first-class channels in syndicated data. You still encounter FDMx in older white papers, competitive benchmarks from trade associations, and sometimes in legacy templates at large CPG companies.


In practice, FDMx has been replaced by xAOC (Nielsen) and MULO (Circana) for almost all modern reporting. The main reason is that dollar stores grew enormously through the 2010s and into the 2020s. Dollar General alone now operates over 20,000 stores in the US, and for many CPG categories it is a top-three account by door count. Leaving dollar out of a total-market view the way older FDMx definitions did produces a number that understates the mass-value end of the market.


If you inherit a workbook using FDMx, the safe move is to rebuild the benchmark in xAOC or MULO and document the breakage point. Time-series trend lines that cross the switch will look like a market share jump or drop that is purely definitional, not real. Flag that to your leadership team before presenting the trend.


## Nielsen vs. Circana market naming


The biggest confusion in cross-vendor reporting is that NielsenIQ and Circana use different names for views that cover roughly the same geography and channel set. A brand that buys both panels will get xAOC from Nielsen and MULO from[Circana](https://www.cpgscout.ai/blog/circana-data-explained) , and needs to know they are the same idea before trying to reconcile the numbers.


The numbers themselves will differ even for the same category, the same market, and the same time period. The two providers use different store panels, different projection methodologies, and different account coverage agreements. For most large CPG categories the difference is small (within 5-8% on dollar sales), but for smaller or highly fragmented categories the gap can be larger. Neither number is "right"; they are different samples of the same universe.


NielsenIQ name Circana name What is included


Total US xAOC MULO Grocery, drug, mass, club, dollar, military


Total US xAOC+C MULO+C (MULO+) All of the above plus convenience stores


FDM (legacy) FDM (legacy) Grocery, drug, mass only (no club, no dollar)


FDMx (legacy) FDMx (legacy) Grocery, drug, mass plus selected other channels (definition varied by report era)


Total US Food Total US Food Grocery and natural/specialty only


Total US Drug Total US Drug Drug channel only (CVS, Walgreens, Rite Aid, etc.)


One practical note: NielsenIQ and Circana do not cover exactly the same retailers even within the same channel. Walmart's supercenter volume, for example, has historically been reported differently across the two providers due to data-sharing agreements. If a retailer withholds scan data from one provider but grants it to the other, your share numbers will not reconcile even when the market definition is identical on paper.


## Which market view to use when


There is no single correct answer, but there are some clear default rules that most brand analytics teams follow.


### Use xAOC or MULO as your standard benchmark


For almost any mainstream CPG category (food, beverage, household, personal care), xAOC or MULO should be your headline market view. It gives you the broadest, most stable denominator for share calculation. If you are presenting to a buyer, they will generally expect to see category share stated in xAOC or MULO, not a narrower cut.


### Add the c-store extension for impulse categories


If more than 10-15% of your category's volume runs through convenience, use xAOC+C or MULO+. Energy drinks and ready-to-drink (RTD) beverages are the clearest cases. A brand with 18% c-store share that reports on MULO base only will consistently look like it is punching above its weight in grocery and below it in total market. The distortion is not small.


### Use narrower views for account-specific pitches


When pitching a specific retailer, total US xAOC is less useful than the relevant channel cut. A Kroger buyer wants to see grocery-channel share, not a number that dilutes Kroger's importance by averaging in mass and dollar. Use xAOC as context, then drill to the channel-level view for the actual pitch numbers.


### Be consistent within a workbook


The most common mistake is mixing market definitions inside the same analysis: total trend line in xAOC, account-level comparison in FDM, competitive share in MULO. The numbers do not add up and the share reconciles to nothing. Pick one base market, document it at the top of the workbook, and build every chart from the same denominator. This is a discipline problem more than a data problem, but catching it early saves painful post-meeting corrections.


Platforms that harmonize data across vendors, like Scout, surface xAOC and MULO side by side with consistent labeling so you can see where the two panels agree and where they diverge. That kind of alignment view is particularly useful when you are onboarding a second data provider and need to understand how much of the difference is definitional versus panel-coverage.


For a broader orientation to CPG data acronyms and time period labels that appear alongside xAOC in syndicated reports, see the guide to[WTD, CWW & Other CPG Data Acronyms](https://www.cpgscout.ai/blog/wtd-cww-cpg-acronyms) .


## Frequently asked questions


What does xAOC stand for?xAOC stands for "extended All Outlet Combined." It is NielsenIQ's label for its broadest total-US retail panel, covering grocery, drug, mass, club, dollar, and military channels. The "extended" refers to the inclusion of club and dollar on top of the legacy food-drug-mass (FDM) base. For more background on how syndicated data panels work, see[What Is Syndicated Data?](https://www.cpgscout.ai/blog/what-is-syndicated-data) .


What does the MULO acronym mean?MULO stands for "Multi-Outlet" and is Circana's equivalent of xAOC. It covers the same core channels (grocery, drug, mass, club, dollar) and is the default total-market benchmark in Circana reports. MULO+ (sometimes written MULO+C) adds convenience stores. You can read more about Circana's data structure in[Circana Data Explained](https://www.cpgscout.ai/blog/circana-data-explained) .


What happened to FDMx?FDMx (Food, Drug, Mass plus x) was a common market definition through the early 2010s that predated the full inclusion of club and dollar stores in syndicated panels. As Dollar General and Dollar Tree grew to over 20,000 combined US stores, leaving dollar out of a total-market benchmark became increasingly misleading. Both NielsenIQ and Circana shifted to xAOC and MULO respectively as their standard total-market views, effectively retiring FDMx as a primary benchmark. You may still encounter FDMx in legacy templates and older industry white papers.


What is the difference between xAOC and MULO?xAOC (NielsenIQ) and MULO (Circana) are different names for conceptually similar total-US market views. Both cover grocery, drug, mass, club, and dollar channels. The differences are methodological: the two providers use different store panels, projection models, and retailer coverage agreements, so the dollar sales and share numbers will not match exactly even for the same category and time period. Neither is definitively correct; they are different estimates of the same universe.


What is what is xAOC Nielsen specifically compared to older Nielsen views?NielsenIQ has used several total-market labels over the years: FDM (food-drug-mass only), FDMx (FDM plus selected extended channels), and ultimately xAOC as the modern standard. xAOC is the broadest and is now the default "total US" benchmark for most categories in Nielsen syndicated data. If you are pulling historical data that spans a period when Nielsen changed its panel definitions, you may see a discontinuity in the trend line at the transition point.
