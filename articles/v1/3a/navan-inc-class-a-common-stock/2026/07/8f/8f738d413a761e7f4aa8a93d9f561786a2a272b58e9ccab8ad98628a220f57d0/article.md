---
schema_version: "1.0.0"
document_id: "8f738d413a761e7f4aa8a93d9f561786a2a272b58e9ccab8ad98628a220f57d0"
company_key: "navan-inc-class-a-common-stock"
company: "Navan Inc."
source_id: "navan-inc-class-a-common-stock-news-import-0382cbfb66e0"
canonical_url: "https://navan.com/blog/how-we-built-the-navan-business-travel-benchmark"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T01:59:35.810604+00:00"
fetched_at: "2026-07-30T01:59:38.986018+00:00"
content_hash: "sha256:3288719c003212da9df68182bb663dc31d05a2ed196f3af44751073d777a56ba"
---

# Inside the cockpit: How we built the Navan Business Travel Benchmark

*Co-authored by Brett Vintch, Senior Director, Data & Product; Mukesh Ganesh, Data Analyst - 2; and Chris Cholette, Senior Vice President, R&D Shared Services*


Business travel moves the economy in ways that are easy to feel and surprisingly hard to measure. The pandemic made that clear: teams drifted apart, conferences moved online, and sales teams lost the full force of being in the room. Work is more distributed now, but the moments that matter most still pull people together.


That was the starting point for the[Navan Business Travel Benchmark (BTB](https://navan.com/btb) ). By isolating business travel activity from consumer travel patterns, the benchmark revealed that business travel kept growing even as broader travel flattened, with its own seasonality and its own industry drivers.


This companion piece is about the work behind the BTB. It details how Navan's data and finance teams, in collaboration with Nasdaq's Economic Research team, turned millions of travel and expense transactions into a repeatable benchmark.


## The Measurement Problem


The core measurement problem is simple: most travel data blends business and leisure. Airline and hotel aggregates are dominated by consumer trips, which follow different economic drivers and different seasonal patterns. Surveys help, but they are indirect. Navan has a first-party view into actual corporate travel and expense activity, so we set out to isolate the business signal from the travel noise.


The inaugural report showed the difference clearly. Business travel climbed 15% year over year while overall travel, measured by TSA foot traffic, dipped 1%. In the[most recent H1 2026 release](https://navan.com/btb) , the gap held: the BTB grew 13.5% year over year and reached an all-time high. Those findings matter, but the more interesting story for us was the craft behind the benchmark: what to include, what to exclude, and how to make the system durable enough to run every month.


## The Data We Used


The BTB starts with Navan's first-party corporate travel and expense data. Every month, millions of transactions flow through the platform as more than 12,500 companies book flights and hotels, reconcile expenses, and pay for everything from client dinners to rideshares with Navan-issued cards. That gives us a direct read on business travel demand rather than a consumer-and-business blend.


From that universe, we narrowed the benchmark to 12 measures: air, hotel, and expense activity across spend and volume, split between U.S. and international markets. Each measure tells part of the story. Together, they create a consistent view of corporate travel intensity.


The harder work was deciding what to leave out. We tested cabin class mix, average ticket prices, cancellation data, and future bookings. All were useful in other contexts. All risked distorting the benchmark. A jump in ticket prices, for example, might reflect fuel surcharges rather than a true increase in travel demand.


Future bookings were the most tempting signal to include. Business trips are often planned weeks or months ahead, and our data shows future bookings rising while cancellations declined, a useful sign that travelers are booking with more intention. But future-booking data leads to actual travel by one to two months, so it can blur the timeline we wanted the BTB to measure. A spike in advance bookings can collide with a seasonal lull in flown travel and make both signals harder to interpret. For this benchmark, we chose actual travel over anticipation.


*Figure 1.* *Domestic air data from January 1, 2024-June 30, 2026 shows future bookings moving ahead of flown travel by one to two months, while cancellations declined over the same period. The BTB counts only flown air volume to preserve seasonality and reduce timing confounds.*


That decision captures the broader philosophy of the BTB: measure what happened, control for what could bias the read, and preserve the patterns that are genuinely part of business travel.


## How The Index Works


We started from the[Conference Board's composite index methodology](https://conference-board.org/) , then adapted it for a fast-growing first-party data set. Three choices mattered most.


- **The first problem was growth bias.** A fixed customer cohort would quickly become stale at a company growing as quickly as Navan. Instead, we rebuild the cohort each month using a stable 24-month activity window, helping ensure the index reflects business travel behavior rather than Navan's own customer growth.
- **The second problem was industry mix.** Business travel patterns differ by sector, and Navan's customer mix can change over time. We use fixed weights to normalize each signal across 14 industries, reducing the influence of any one industry becoming more or less represented on the platform.
- **The third problem was noisy signals.** We calculate monthly changes with symmetric percent change, so increases and decreases are treated consistently. Then we weight each signal by the inverse of its volatility, allowing steadier measures to carry more influence than more variable ones.


The result is a composite index set to 100 in January 2023, our post-pandemic baseline. We intentionally preserve seasonality rather than smoothing it away. For business travel, the spring and fall cycles are not noise. They are part of the signal.


*Figure 2.* *The BTB is anchored at 100 in January 2023 and calculated recursively from monthly symmetric changes, using rolling 24-month cohorts, industry weighting, and inverse-volatility scaling.*


## Is The BTB Just Tracking The Economy?


Business travel is part of the broader economy, so we tested whether the BTB was simply another proxy for familiar macro signals. It was not. Business travel grew rapidly year over year while overall travel, measured by TSA foot traffic, remained largely flat, and their seasonal patterns moved in opposite directions across the analysis.


We also compared the BTB against the S&P 500 average closing price, the University of Michigan Survey of Consumers for Independents, and average Gulf Coast jet fuel prices. The BTB rose alongside broader markets after 2023, but its seasonality did not line up with those comparison series.


That divergence is the point. The BTB captures dynamics that are hard to see in blended travel data or broad economic indicators. It is a distinct view into corporate movement: when companies are sending people to meet customers, gather teams, and build in person.


*Figure 3.* *The BTB compared with selected macro, consumer, and fuel-price indicators (indexed to January 2024). Detrended correlations suggest its seasonal pattern is distinct from the comparison set.*


*Figure 4:* *Correlation coefficients of detrended (slope-and-mean subtracted) signals over the same period. While the seasonality of the S&P 500 and Consumer Sentiment are correlated with each other, no index correlates with the BTB.*


## From Prototype To Pipeline


Designing the index logic was only half the work. The other half was turning it into something the business could trust every month: repeatable, automated, and governed by checks.


The pipeline now runs after each monthly financial close, processing millions of raw booking and expense rows into the BTB and its sub-indices. The workflow is orchestrated by dbt inside Snowflake, with 20 modular, version-controlled models: the first 10 clean and standardize the source data, and the next 10 apply the business logic that calculates the final indices.


Quality checks are built into the flow. The pipeline runs through a 16-point automated checklist across 8 critical stages, flagging anomalies before results are published. The final outputs land in ThoughtSpot liveboards, where stakeholders can explore the benchmark and drill into dimensions without needing to work directly in the warehouse.


That matters for more than convenience. A benchmark is only useful if people trust both the result and the process behind it. The BTB is built to be rerun, inspected, and improved as the data evolves.


## The Bottom Line


The BTB is not just a report. It is a data product: a repeatable way to turn real corporate travel and expense activity into a timely view of how companies are moving through the world.


For customers, it offers a benchmark for understanding travel intensity against the market. For Navan, it reflects the kind of work we want to do more of: combine product data, finance discipline, and engineering rigor to answer business questions that were previously hard to see. The result is a clearer read on business travel and a stronger foundation for the teams building it.


---


Share this article


---


-
-
-


Smarter Travel. Cleaner Books. Happier Teams.


Navan’s all-in-one platform — built to streamline T&E.


[Get started](https://navan.com/get-started)


---


---


Share this article


-
-
-


---


This content is for informational purposes only. It doesn't necessarily reflect the views of Navan and should not be construed as legal, tax, benefits, financial, accounting, or other advice. If you need specific advice for your business, please consult with an expert, as rules and regulations change regularly.
