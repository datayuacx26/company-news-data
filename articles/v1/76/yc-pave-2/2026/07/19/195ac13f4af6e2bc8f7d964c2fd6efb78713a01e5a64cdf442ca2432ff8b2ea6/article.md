---
schema_version: "1.0.0"
document_id: "195ac13f4af6e2bc8f7d964c2fd6efb78713a01e5a64cdf442ca2432ff8b2ea6"
company_key: "yc-pave-2"
company: "Pave"
source_id: "yc-pave-2-news-import-17afd9925d0e"
canonical_url: "https://www.pave.com/blog-posts/how-pave-protects-contributor-data"
published_at: "2026-08-10T15:10:28.506+00:00"
first_seen_at: "2026-07-25T18:54:26.650513+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:f3d7f96c2789f2a4979ed19a507090cde64466a4ebf17a7a44b4b970f0993f5e"
---

# How Pave Protects Contributor Data

Table of contents


NOTE: Table of contents generated on published site only, does not display here. If no H2s are present in the article, the TOC should be turned off in the article colleciton entry.


Featured Products


No items found.


Share this content


Every company that competes for talent relies on[compensation benchmarks](https://www.pave.com/s/salary-benchmarking) . And every benchmark is only as good as the trust behind it: companies contribute their data to Pave because they are confident that no other company will ever see it. Protecting that confidence is a design requirement that shapes every layer of our product.


Here's how Pave protects the data behind every benchmark.


To read more about our broader methodology, visit the[Pave Market Data Methodology page](https://www.pave.com/products/market-data-methodology) .


## How Pave is designed to protect contributor data


Pave delivers compensation benchmarks derived from data collected in near real-time from more than 8,900 companies through automated HRIS and Equity Management System integrations. That data gives our customers visibility into recent macro adjustments to compensation across a range of companies and geographies so they can make informed pay decisions.


Every layer of the product, from ingestion to publication, is designed so that no customer can see or reverse-engineer another company's compensation data. That's what lets customers trust the numbers.


Today there are seven core data guardrails in place:


- **Data is aggregated and de-identified, by contract and by design.** All benchmark data is aggregated and de-identified across employers so that it is not linked to any individual company or employee. This is a contractual commitment we make to every customer in our Master Subscription Agreement, not just an internal policy.
- **A strict minimum-company threshold.** No benchmark is displayed unless at least five companies contribute data to it. This applies uniformly across every job family, geography, and filter combination. If a segment doesn't meet the threshold, no data is shown.
- **Adaptive dominance controls at every level.** No single company can contribute more employees to any benchmark segment than the fourth-most-prevalent company in that segment, enforced through deterministic sampling. In short, no single contributor can dominate a benchmark. Combined with the five-company minimum, this is designed so that no company represents 25% or more of any benchmark at any filter combination. And across the full dataset, our single largest customer represents less than 1.6% of the data.
- **Monthly batch publication.** Although data is ingested continuously, benchmarks are published once per month, bundling thousands of changes from thousands of companies into a single release. Because so many changes land at once, no movement in a benchmark can be traced back to any one company's hiring, departures, or pay changes.
- **No competitor-specific data.** Customers cannot access compensation data from identified competitors, cannot determine whether any specific company contributes to a given benchmark, and cannot monitor a competitor's compensation updates through the platform.
- **Current data only.** Pave's benchmarks reflect current market data only. The product contains no prospective or future wage information. Separately, once new data is published, the old data is no longer available on the platform.
- **Statistical ranges, not data points.** Benchmarks are displayed as percentile distributions with transparent sample-size and company counts, providing market insight without exposing any individual data point.


## Peer groups: the same protections, plus additional safeguards


Like most major compensation data providers, Pave allows customers to create named peer groups to benchmark against a curated set of companies. Peer groups don't weaken any of the guardrails above. Every peer group benchmark is subject to the same data dominance and data sufficiency controls, plus three safeguards specific to the feature:


- **A higher bar for group size.** While many survey providers typically require 10 named companies in a peer group, Pave requires at least 15. And no peer group benchmark will display unless at least 5 companies actually contribute data to it. Combined with the dominance controls, this is designed to prevent a user from isolating any single company's compensation or determining where any company falls within the group's distribution.
- **Variance controls.** Any two active or pending peer groups together must differ by at least three varying companies. If a user attempts to save a peer group too similar to an existing one, Pave blocks the creation outright. This prevents anyone from building marginally different groups to triangulate around a single company.
- **Timing controls.** Once saved, a peer group's composition is locked. If a user deletes a[peer group](https://www.pave.com/blog-posts/peer-groups) and tries to recreate a near-identical one, the new group enters a 90-day cooling-off period before it can generate benchmarks. And an attempt to recreate an identical deleted group is blocked entirely. This is designed to prevent cycling through peer groups over time to extract company-specific data.


Whether a user applies market filters, builds a peer group, or combines the two, the output is always an aggregate market benchmark, never a view into any one company's pay practices.


## Your decisions stay yours


It's also worth being clear about what Pave doesn't do. Pave does not make compensation recommendations, and customers are under no obligation to use Market Data in any particular way. Our benchmarks inform independent decision-making using whatever survey data and compensation methodologies you choose, and Pave does not coordinate compensation strategies across clients. That independence is how the product is meant to work.


## This work doesn't stop


Market expectations and data protection best practices will keep evolving, and our methodology will keep pace. We continuously review our data practices and strengthen these guardrails as the product and dataset grow. We built Pave on the belief that useful[compensation data](https://www.pave.com/products/market-data-pro) and rigorous data protection can coexist. The guardrails above are how.


Want to go deeper on our methodology? Reach out to your Pave account team for our detailed anonymization and privacy documentation.


*This post describes Pave's product design and is provided for general informational purposes only. It is not legal advice and does not describe how any law applies to your use of Market Data. Consult your own advisors regarding your obligations.*


Featured Products


No items found.


Share this content


By


Matthew Schulman


CEO & Founder


Matt Schulman is CEO and founder of Pave, the complete platform for Total Rewards professionals. Prior to Pave, he was a software engineer at Facebook focusing on user-centric mobile experiences. A self-proclaimed "comp nerd," Matt is known for sharing data-driven thought leadership around all things compensation and personal finance.


NOTE: The elements below are only visible in the editor. To place these in articles, use their corresponding short codes. They are made visible here to facilitate editing.


{{mid-cta}}


###


{{signup-cta}}


Compensation Platform


### Access our full platform to build your customized compensation strategy.


Book a demoSign up free


{{signup-cta-narrow}}


##### Access our full platform to build your customized compensation strategy


Book a demoSign up free


{{article-cta}}


Market Data Pro


#### Harness real-time benchmarks. Sync with industry standards


Button


{{newsletter-cta}}


Newsletter


#### Join our newsletter for the most current Pave insights


Thank you! Your submission has been received!


Oops! Something went wrong while submitting the form.


{{article-stats}}


No items found.


{{key-results}}


Key results
