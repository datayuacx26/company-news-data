---
schema_version: "1.0.0"
document_id: "6cb8dfbb93b2432535a4522d05c09dbefebc88d4fcd4e92c318efc3a6a4130c4"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/repeat-purchase-rate"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-24T00:54:45.437387+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:37cdc1143783fe955e87034ada0bcaa241fd0c71eddca8622856ece1b88db2bf"
---

# Repeat Purchase Rate: Measuring CPG Loyalty

Repeat purchase rate is the share of buyers who purchase a product more than once within a defined time window. In CPG, it is typically derived from household panel data and answers a single question: of the households that tried your product, how many came back? A high repeat rate signals genuine product satisfaction; a low one usually means you are burning marketing dollars to acquire buyers who never return.


This post explains how to calculate repeat purchase rate under both the CPG panel definition and the broader e-commerce definition, walks through worked examples, compares it to related metrics like retention rate and purchase frequency, and provides realistic category benchmarks brand managers can use as a sanity check.


## What Is Repeat Purchase Rate?


At its core, repeat purchase rate measures buyer loyalty over time. The term gets used in two distinct contexts, and it is worth being precise about which one you mean before pulling data.


In CPG and retail analytics, repeat purchase rate refers specifically to a panel-based metric: the fraction of households that made a trial purchase and then made at least one follow-on purchase during the measurement period. This definition shows up in SPINS, NielsenIQ, and Circana outputs and is the primary lens brand managers use to evaluate new item performance.


In e-commerce and direct-to-consumer settings, repeat purchase rate is calculated from transaction records: the share of your total customer base that has placed more than one order ever (or within a rolling window). Shopify analytics surfaces this version by default. The two definitions ask similar questions but use different data foundations, so benchmark comparisons across contexts are almost always apples-to-oranges.


## The Repeat Purchase Rate Formula


Both formulas are simple. The complexity is in defining the denominator carefully.


### CPG / Panel-Based Formula


Repeat purchase rate (CPG) = Households that bought the item 2+ times in the period / Households that bought the item at least once in the period


The "period" is usually 52 weeks for an established item or a defined launch window (often 13 or 26 weeks) for a new item. The numerator counts only households that came back, not their total trips.


### Worked CPG Example


A better-for-you snack brand launches in natural grocery in Q1. Over the following 26 weeks, 42,000 projected households (panel-projected) purchase the item at least once. Of those, 14,700 make a second or subsequent purchase. Repeat purchase rate = 14,700 / 42,000 = 35%. The brand's category average for similar new items in the same channel is roughly 28%, so 35% is a healthy early signal.


### E-Commerce / Generic Formula


Repeat purchase rate (e-commerce) = Customers with more than 1 order in the period / Total customers with at least 1 order in the period


Some teams use a lifetime window (all orders ever), while others use a rolling 12-month window to avoid counting lapsed buyers as loyal. The rolling-window approach is generally more actionable because it surfaces churn.


### Worked E-Commerce Example


A DTC supplement brand has 18,500 customers who placed at least one order in the trailing 12 months. Of those, 9,435 placed a second order within the same window. Repeat purchase rate = 9,435 / 18,500 = 51%. For a consumable supplement with a 30-day supply size, this is on the lower end, suggesting the brand should look hard at its post-trial email sequence and subscription conversion rate.


## Repeat Purchase Rate vs. Retention Rate vs. Purchase Frequency


These three metrics are frequently conflated. They measure related but distinct things, and mixing them up leads to the wrong diagnosis.


Metric What It Measures Typical Data Source Primary Use Case


Repeat purchase rate Share of buyers who return at least once Panel or transaction records New item trial-to-loyalty health


Retention rate Share of customers active in one period who are still active in the next Transaction records, subscription data Cohort survival, subscription churn


Purchase frequency Average number of trips or orders per buyer in a period Panel or transaction records Category engagement, volume driver analysis


A practical way to keep them straight: repeat purchase rate tells you whether buyers come back at all. Retention rate tells you whether they are still around period over period. Purchase frequency tells you how often the returners actually shop. You want all three to be healthy, but for a new item launch in CPG, repeat purchase rate is the first gate to clear.


Repeat purchase rate also connects directly to[Household Penetration](https://www.cpgscout.ai/blog/household-penetration) . Penetration measures how many unique households buy you at all; repeat rate measures whether those households are worth acquiring. A brand with soaring penetration and a 15% repeat rate is essentially running a sampling program, not building a business.


## Category Benchmarks for Repeat Purchase Rate


There is no universal benchmark. Repeat purchase rates vary widely by category, channel, pack size, and price point. A few structural factors explain most of the variance.


### What Drives Benchmark Differences


- Consumption rate: daily-use items (coffee, protein bars) naturally see higher repeat rates than seasonal or occasion-driven items (holiday candy, sports drinks tied to events).
- Pack size and price: a 24-count value pack bought once represents more forward inventory, so the household may not repurchase within the 52-week window even if satisfied.
- Category substitutability: in highly fragmented categories with many similar SKUs, buyers may be satisfied with your item but rotate to a competitor on the next trip.
- Channel: natural/specialty retail skews toward more intentional buyers who often repeat at higher rates than mass-channel impulse purchasers.


### Illustrative Ranges by Category


Category Typical 52-Week Repeat Rate (New Item) Notes


Refrigerated beverages 25-35% High turn, moderate switching


Better-for-you snacks 22-32% Flavor fatigue a real factor


Coffee / RTD coffee 38-50% Daily-use consumption lifts rate


Personal care (natural) 30-42% Routine-based; strong if efficacy evident


Frozen entrees 18-28% High variety-seeking, freezer space limits


Supplements (DTC) 45-60% Subscription conversion inflates figure


These ranges are illustrative, not sourced from a single panel study. Use your category data from SPINS, Circana, or NielsenIQ as the ground truth, and compare your item against its direct subcategory, not the broad category average. A 28% repeat rate in frozen pizza looks different from 28% in a highly differentiated probiotic soda.


Understanding panel-derived metrics like repeat purchase rate requires clean household panel data. For context on how panel data is structured and projected,[Household Panel vs. Consumer Panel Data](https://www.cpgscout.ai/blog/household-panel-data) is a useful primer before interpreting any vendor output.


## How to Improve Repeat Purchase Rate


Before pulling levers, diagnose where in the post-trial funnel buyers are dropping off. A low repeat rate can mean the first experience was not good enough, the price-to-value ratio failed at the second purchase, distribution gaps made a second buy hard, or variety-seekers simply moved on. The fix depends on the root cause.


### Product and Pack Adjustments


If trial buyers lapse after a single purchase, the most direct lever is product quality or format. Post-purchase surveys (via Suzy, Voxpopme, or a simple post-shipment email) catch product-fit issues early. Pack size is also a factor: if your item has a 60-day supply size and you are measuring at 13 weeks, the repeat window may simply not be wide enough to capture natural repurchase cycles. Adjust the measurement window before concluding the repeat rate is low.


### Pricing and Promotion Strategy


Trial via deep promotion can inflate your first-buy count with price-sensitive buyers who will never pay full retail. This artificially deflates repeat rate. If your repeat rate spikes on non-promoted weeks and collapses after a TPR, you likely have a price-perception problem rather than a product problem. Consider whether introductory pricing is attracting the right buyer, or whether a lower everyday price point would raise sustained repeat rates.


### Distribution and Shelf Placement


A motivated repeat buyer who cannot find the item gives up. Spotty distribution, out-of-stocks, or poor shelf placement can kill repeat intent that already exists. Pull your ACV-weighted distribution data alongside repeat rate. If distribution is below 60% ACV and repeat rate is low, distribution is likely a contributing factor. The[Household Penetration](https://www.cpgscout.ai/blog/household-penetration) metric can help you identify geographic pockets where buyers are not returning.


### CRM and Retention Programs (DTC and Omnichannel)


For DTC brands where you own the customer relationship, the post-trial email or SMS sequence is the highest-impact repeat-rate driver. A win-back flow triggered at 25-30 days (when the supply of a typical first purchase is running low) can lift second-purchase rates by 8-15 percentage points in well-run programs. Subscription or loyalty program enrollment is the most durable fix for a low repeat rate because it converts a purchase decision into a default.


### Variety and Line Extension


In flavor-forward categories, a buyer who loved the original but wants variety will lapse from your brand if there is nothing adjacent to try. Repeat rate at the brand level (summing across all SKUs) is typically 10-20 points higher than at the item level in these categories. Before pulling product or marketing levers, check whether brand-level repeat is healthy even when item-level repeat looks soft.


## Tracking Repeat Purchase Rate in Practice


In a[syndicated data](https://www.cpgscout.ai/blog/what-is-syndicated-data) workflow, repeat purchase rate typically lives inside your panel report alongside trial rate, average items per buyer, and dollar sales per buyer. In Circana and NielsenIQ panel outputs, it is usually expressed as a percentage of all buyers who also appear as repeat buyers in the same period.


The challenge most brands face is that panel data arrives with a lag (usually 4-6 weeks) and cuts are run quarterly or on a project basis, making real-time monitoring difficult. Platforms that harmonize panel, POS, and retailer-portal data can triangulate repeat-purchase signals more continuously. Scout surfaces repeat rate alongside household penetration and purchase frequency in a single view, so brand teams do not need to stitch together three separate exports to see the full buyer funnel.


For brands that also sell DTC, reconciling the panel-based repeat rate with the transaction-based rate is worth the effort. The two numbers will not match (they measure different buyer populations), but a consistent directional gap between them often reveals channel mix effects: a high DTC repeat rate with a low panel rate may indicate your loyal buyers have shifted to your own site, while your retail presence is running on trial buyers only.


## Frequently asked questions


How do you calculate repeat purchase rate?In CPG, repeat purchase rate = households with 2+ purchases in the period / households with at least 1 purchase in the same period. In e-commerce, it is customers with more than 1 order / total customers with at least 1 order. The formula is the same in structure; what differs is whether your data source is household panel data or a transaction database.


What is a good repeat purchase rate for CPG brands?There is no single universal benchmark. For a new item in its first 52 weeks, a repeat rate above 25-30% is generally considered acceptable across most food and beverage categories, with daily-use categories like coffee or supplements often running 40%+. The most meaningful comparison is against your item's direct subcategory using the same panel source (SPINS, Circana, NielsenIQ), not an industry-wide average.


What is the repeat purchase rate formula for e-commerce?Repeat purchase rate (e-commerce) = customers with more than 1 order in the measurement window / total customers with at least 1 order in the same window. Most teams use a rolling 12-month window rather than a lifetime window to avoid treating lapsed buyers as active loyal customers. For consumable products, a window of 60-90 days is often more diagnostic because it aligns with natural repurchase cycles.


Is repeat purchase rate the same as retention rate?No. Repeat purchase rate measures whether a buyer comes back at least once. Retention rate measures what fraction of customers who were active in one period are still active in the next period (a cohort-survival concept). A customer could have a repeat purchase (met your repeat rate threshold) but still be counted as churned if they stopped buying in a later period. Retention rate is more common in subscription or SaaS contexts; repeat purchase rate is more common in CPG and e-commerce.


How does repeat purchase rate relate to household penetration?They are complementary metrics.[Household Penetration](https://www.cpgscout.ai/blog/household-penetration) measures how many households buy your product at all (breadth of reach). Repeat purchase rate measures whether those households come back (depth of loyalty). A new item launch is healthy when penetration is growing AND repeat rate is at or above category average. High penetration with a low repeat rate usually signals that marketing is reaching the right households but the product or value proposition is not converting them to loyal buyers.
