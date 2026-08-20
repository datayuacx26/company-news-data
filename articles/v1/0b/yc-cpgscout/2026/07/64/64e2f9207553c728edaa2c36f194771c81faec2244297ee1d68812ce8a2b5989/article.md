---
schema_version: "1.0.0"
document_id: "64e2f9207553c728edaa2c36f194771c81faec2244297ee1d68812ce8a2b5989"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/nielsen-scantrack-scan-data"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-24T00:54:45.437387+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:a1910b0317b2ce2350cc525fef26ebbe78cb8f462494893b954947b9544d7dd0"
---

# Nielsen Scantrack & Scan/POS Data 101

Scantrack is NielsenIQ's retail measurement service that collects point-of-sale scanner data from participating retailers and aggregates it into weekly sales reports covering dollars, units, volume, and pricing at the category, brand, and item level. If your brand manager has ever pulled a Nielsen report showing "12 weeks ending" sales and ACV-weighted distribution, that data came through Scantrack.


This post walks through what scan data is, how Scantrack specifically works, the key differences between scan data and household panel data, and where the coverage gaps are that every CPG analyst needs to know about.


## What Is Scantrack?


Scantrack is the brand name NielsenIQ has used for its retail point-of-sale measurement product for decades. At its core, the service works like this: retailers share their raw register transaction files with NielsenIQ on a weekly basis, NielsenIQ maps every UPC to a product hierarchy (brand, segment, category, department), and then publishes aggregated performance metrics back to subscribing manufacturers and retailers.


The output is a syndicated data subscription. "Syndicated" means the same underlying retail panel is resold to many buyers. A $30 yogurt brand and a $2B cereal company both subscribe and both get reports built from the same store-level sales feed. This is fundamentally different from a custom data pull or retailer portal export, which is proprietary to one buyer. For more on that distinction, see[What Is Syndicated Data?](https://www.cpgscout.ai/blog/what-is-syndicated-data) .


Scantrack covers what NielsenIQ calls "all-outlet combined" (AOC), meaning the service attempts to capture sales across grocery, drug, mass, club, dollar, military, and convenience channels, weighted and projected to represent the full U.S. market. In practice the coverage is not truly universal, a distinction worth understanding carefully.


## Scan/POS Data Basics


Scan data, also called POS data, starts at the checkout lane. Every time a UPC barcode is scanned at a register, the transaction is logged: product, quantity, price paid, store, date. That raw record is the foundation of all retail measurement services, including Scantrack.


NielsenIQ collects those raw files from participating retailers, cleans and normalizes the data, applies projection factors to account for non-reporting stores, and then organizes everything into a consistent hierarchy. The result lets a brand manager ask questions like: in the 52 weeks ending June 7, 2026, how many dollar sales did SKU X achieve in the grocery channel, and what was its ACV-weighted distribution?


### Key metrics Scantrack reports


- Dollar sales and unit sales, the primary volume metrics, available by week
- Equivalized volume (EQ volume), which normalizes across package sizes using a common unit like ounces
- Average selling price (ASP) per unit, used to track price trends
- % ACV distribution, the share of stores carrying an item weighted by each store's total grocery sales
- Velocity (dollars or units per point of ACV), which normalizes sales for how widely distributed an item is
- Promotional metrics including % ACV on promotion, price reduction depth, and feature and display support


A worked example: suppose your frozen pizza SKU sold $480,000 in the four weeks ending June 7 across the grocery channel. Its % ACV distribution was 60%, meaning it was stocked in stores representing 60% of all ACV in that channel. Velocity = $480,000 / 60 = $8,000 per point of ACV. A competitor doing $540,000 with 80% ACV has a velocity of $6,750 per point. Despite higher total sales, the competitor is underperforming on a velocity basis, which suggests your SKU has stronger turns per store. That kind of comparison is a standard Scantrack analytical move.


## Scantrack vs. Household Panel Data


This is the question CPG analysts get asked constantly, especially when presenting to a brand team that has both a Nielsen retail subscription and a panel subscription. The short answer: scan data tells you what sold, where, and at what price; panel data tells you who bought it, how often, and whether they came back.


Dimension Scantrack (Scan/POS) Household Panel


Source Retailer register transactions Shopper purchase diaries / loyalty scan


Unit of measurement Store, chain, market, channel Household, demographic segment


Core questions How much sold? At what price? In which channel? Who bought? How often? What else did they buy?


Buyer insights None directly Trial rate, repeat rate, basket composition


Promotion measurement Price paid, feature/display flags Whether promo drove new buyers or just loyal ones


Update cadence Weekly Weekly (aggregate), household trips variable


Coverage Primarily measured retail; projects to AOC Panel sample projected to universe (smaller n)


Typical use Sales tracking, distribution, velocity, pricing Buyer equity, loyalty, switching, penetration


A common mistake is using scan data to answer buyer questions. If your brand's dollar sales declined 8% last quarter, Scantrack can tell you it was driven by a drop in % ACV on promotion and a lower average selling price. It cannot tell you whether that decline came from losing light buyers or from your heaviest buyers trading down to a private label. That question belongs to panel. For a deeper look at how panel data works, see[Household Panel vs. Consumer Panel Data](https://www.cpgscout.ai/blog/household-panel-data) .


The two data sets do intersect. NielsenIQ sells a service called Total Consumer Measurement that attempts to reconcile scan and panel, but most mid-market CPG brands work with them as complementary rather than merged sources.


## Coverage and Limitations


Scantrack is not a complete census of all retail transactions in the U.S. Understanding the gaps is important before presenting findings to leadership.


### Participating vs. non-participating retailers


Retailers must agree to share their POS files with NielsenIQ and in many cases pay for access to the resulting analytics. Large chains typically participate, but smaller regional retailers, many online pure-play retailers, and some club and specialty formats either do not participate or are reported at a lower coverage level. NielsenIQ fills these gaps through projection models, not direct data collection. Projections are reasonable for major channels but add measurement uncertainty, especially for smaller or faster-growing channels like e-commerce.


### Random-weight items


Products sold by weight at retail, like fresh meat from the service case, deli items, or produce sold loose, typically do not carry a scannable UPC. They are either excluded from Scantrack reporting or captured imperfectly through store-specific PLUs (price look-up codes). For CPG brands in center store this rarely matters, but for brands in fresh, deli, or specialty meat it is a significant blind spot.


### Untracked and underdeveloped channels


- E-commerce: tracked in part through separate NielsenIQ e-commerce products, not fully integrated into the Scantrack AOC figure
- Foodservice: restaurants and institutional buyers are outside the Scantrack scope entirely
- Direct-to-consumer (DTC): sales through a brand's own website are not captured
- International: Scantrack is a U.S.-focused measurement; NielsenIQ offers separate services by market globally


The practical implication: if your brand does 20% of volume through Amazon or DTC, Scantrack will understate total performance, and your internal sell-in conversation with buyers will need to account for that gap. Circana (formerly IRI) faces the same structural limitation. If you want to compare the two services head to head,[Circana Data Explained](https://www.cpgscout.ai/blog/circana-data-explained) covers how Circana's retail panel is structured and where it overlaps with or diverges from Scantrack.


### Lag and restatement


Scantrack reports are published weekly with roughly a two-week lag from the sales period close. Retailers occasionally restate historical weeks when they correct submission errors, which means a Scantrack pull on two different dates can show slightly different historical numbers. This is normal, but it means you should always note which pull date your report was based on when sharing externally.


For brands that report numbers up the chain, that restatement window is worth flagging: a figure quoted on Monday can move slightly once the prior week is finalized, so label the earliest reads as preliminary and reconcile them after the data settles.


## NielsenIQ Today


The Nielsen name carries decades of history in CPG data, but the current corporate structure is worth clarifying. In 2023, the legacy Nielsen retail measurement business (formerly part of Nielsen Holdings) was rebranded as NielsenIQ. It operates as a standalone data and analytics company with private equity backing. The Scantrack product line continues under the NielsenIQ brand, along with broader platform investments like NielsenIQ Discover and their data harmonization layer.


The confusion between "Nielsen" and "NielsenIQ" comes up regularly in client conversations. Nielsen Holdings (now Nielsen) retained the media measurement business, TV ratings and the like. NielsenIQ owns the retail measurement and consumer intelligence products, which is what CPG brands are referring to when they say "our Nielsen data."


NielsenIQ also competes directly with Circana (the combined IRI and NPD Group), and in natural and specialty channels with[What Is SPINS Data?](https://www.cpgscout.ai/blog/what-is-spins-data) . Each service has its own retailer coverage strengths, price points, and refresh cadences, which is why larger brands often subscribe to more than one.


When brands work with Scout, all of these data sources, Scantrack, Circana, SPINS, plus retailer portal data, are normalized into a consistent schema. That means a single dashboard can show Kroger portal velocity alongside Scantrack national ACV without a manual reconciliation step.


## Frequently asked questions


What is Nielsen Scantrack data, exactly?Nielsen Scantrack data is a syndicated retail measurement feed published by NielsenIQ. It aggregates point-of-sale scanner transactions from participating retailers and projects them across channels to produce weekly sales, distribution, pricing, and promotion metrics at the category, brand, and item level. It is the primary source when a CPG brand talks about "our Nielsen numbers."


What is scan data in CPG?In CPG, scan data refers to sales information derived from checkout scanner records, where a product UPC is scanned at the register. Scan data (also called POS data) captures what sold, in what quantity, at what price, and in which store. It does not capture who bought the product. Services like Scantrack and Circana aggregate scan data from many retailers into a standardized weekly report format used for category and brand performance tracking.


What is the difference between scan data and panel data?Scan data measures sales at the store level: units, dollars, distribution, and pricing. Panel data, like the NielsenIQ Homescan panel, tracks the same purchases at the household level. Scan data answers "how much sold and where?" while panel answers "who bought, how often, and are they coming back?" The two are complementary. For a full comparison, see[Household Panel vs. Consumer Panel Data](https://www.cpgscout.ai/blog/household-panel-data) .


What are the main limitations of Scantrack coverage?The main gaps are: non-participating retailers (projected rather than directly measured), random-weight items like fresh deli and produce (no UPC to scan), e-commerce (partially covered by separate NielsenIQ products), foodservice (out of scope entirely), and DTC sales through a brand's own website. Brands with significant volume in these channels will see Scantrack understate their total performance.


Is it Nielsen or NielsenIQ?For CPG retail measurement, the correct current name is NielsenIQ. The company was rebranded in 2023 when it separated from Nielsen Holdings, which retained TV and media measurement under the Nielsen brand. When your buyers, brokers, or category managers say "Nielsen data," they mean NielsenIQ's retail measurement products, including Scantrack.
