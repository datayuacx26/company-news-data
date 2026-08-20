---
schema_version: "1.0.0"
document_id: "3b036695437c193d40fa86b10a0ecdc98e1b3122a9ae17d4346386bdedc61e5c"
company_key: "yc-pave-2"
company: "Pave"
source_id: "yc-pave-2-news-import-17afd9925d0e"
canonical_url: "https://www.pave.com/blog-posts/market-data-antitrust-data-protection-requirements"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-25T18:54:26.650513+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:00ba2169741eb8be4f47121561064912aa5224c7daf33f056688192d04d9d76c"
---

# How Pave's Market Data Is Built for Today's Antitrust and Data Protection Requirements

Table of contents


NOTE: Table of contents generated on published site only, does not display here. If no H2s are present in the article, the TOC should be turned off in the article colleciton entry.


Featured Products


No items found.


Share this content


Every company that competes for talent relies on[compensation benchmarks](https://www.pave.com/s/salary-benchmarking) . Regulators know this too, and over the past few years there has been more information coming from the FTC and DOJ regarding how competitively sensitive information, including compensation data, can be shared. Pave follows the guidance coming from the FTC and DOJ and adapts as the guidance develops.


Here's where the guidance stands today and what Pave does to stay aligned with it.


*To read more about our broader methodology, visit the*[Pave Market Data Methodology](https://www.pave.com/products/market-data-methodology) *page.*


## The background on antitrust guidance in compensation


For decades, HR teams worked from a set of rules written in the 1990s. In 1996, the DOJ and FTC published a healthcare-industry “safety zone” for compensation surveys built on three prongs:


1. The survey is managed by a third party
2. The data is more than three months old
3. It draws from at least five entities, with no single entity representing more than 25% of any benchmark and no way to identify any individual entity's data.


The agencies' 2016 Antitrust Guidance for Human Resource Professionals extended those concepts beyond healthcare, and the “three months old” rule of thumb became gospel in many HR departments.


In February 2023, the[DOJ formally withdrew the 1996 safe harbor](https://www.justice.gov/archives/opa/pr/justice-department-withdraws-outdated-enforcement-policy-statements) , including, perhaps most notably, the “three months old” restriction. The[FTC followed in July 2023](https://www.ftc.gov/news-events/news/press-releases/2023/07/federal-trade-commission-withdraws-health-care-enforcement-policy-statements) , withdrawing the same guidance. Then in January 2025 the DOJ and FTC issued new[Antitrust Guidelines for Business Activities Affecting Workers](https://www.ftc.gov/system/files/ftc_gov/pdf/p251201antitrustguidelinesbusinessactivitiesaffectingworkers2025.pdf) , replacing the 2016 guidance.


Though issued on a divided vote and still debated, the latest 2025 Guidelines have not been withdrawn and remain the agencies' most recent guidance on the subject. These 2025 Guidelines focus not on the “recency” of the data but instead on how compensation benchmarks are aggregated and delivered.


This brings us to how Pave is designed to avoid antitrust risk.


## Pave’s data guardrails


Pave delivers compensation benchmarks derived from data collected in near real-time from more than 8,900 companies through automated HRIS and Equity Management System integrations. That data gives our customers visibility into recent macro adjustments to compensation across a range of companies and geographies so they can make informed pay decisions.


Every layer of the product, from ingestion to publication, is designed so that no customer can see or reverse-engineer another company's compensation data. That's what current guidance calls for, and it's also what lets customers trust the numbers.


Today there are seven core data guardrails in place:


1. **Data is aggregated and de-identified, by contract and by design** *.* All benchmark data is aggregated and de-identified across employers so it cannot be linked to any individual company or employee. This is a contractual commitment we make to every customer in our Master Subscription Agreement, not just an internal policy.
2. **A strict minimum-company threshold** . No benchmark is displayed unless at least five companies contribute data to it. This applies uniformly across every job family, geography, and filter combination. If a segment doesn't meet the threshold, no data is shown.
3. **Adaptive dominance controls at every level.** No single company can contribute more employees to any benchmark segment than the fourth-most-prevalent company in that segment, enforced through deterministic sampling. Combined with the five-company minimum, this mathematically ensures no company ever represents 25% or more of any benchmark at any filter combination. And across the full dataset, our single largest customer represents less than 1.6% of the data.
4. **Monthly batch publication** . Although data is ingested continuously, benchmarks are published once per month, bundling thousands of changes from thousands of companies into a single release. Because so many changes land at once, no movement in a benchmark can be traced back to any one company's hiring, departures, or pay changes.
5. **No competitor-specific data.** Customers cannot access compensation data from identified competitors, cannot determine whether any specific company contributes to a given benchmark, and cannot monitor a competitor's compensation updates through the platform.
6. **No forward-looking data** *.* Pave's benchmarks reflect current market data only. The product contains no prospective or future wage information, a category the agencies have flagged as especially sensitive. And, once new data is published, the old data is no longer available on the platform.
7. **Statistical ranges, not data points** . Benchmarks are displayed as percentile distributions with transparent sample-size and company counts, providing market insight without exposing any individual data point.


## Peer groups: the same protections, plus additional safeguards


Like most major compensation data providers, Pave allows customers to create named[peer groups](https://www.pave.com/blog-posts/peer-groups) to benchmark against a curated set of companies. Peer groups don't weaken any of the guardrails above. Every peer group benchmark is subject to the same data dominance and data sufficiency controls, plus three safeguards specific to the feature:


1. **A higher bar for group size** . While many survey providers typically require 10 named companies in a peer group, Pave requires at least 15. And no peer group benchmark will display unless at least 5 companies actually contribute data to it. Combined with the dominance controls, this means a user cannot isolate any single company's compensation or determine where any company falls within the group's distribution.
2. **Variance controls** . Any two active or pending peer groups together must have at least three varying companies. If a user attempts to save a peer group too similar to an existing one, Pave blocks the creation outright. This prevents anyone from building marginally different groups to triangulate around a single company.
3. **Timing controls.** Once saved, a peer group's composition is locked. If a user deletes a peer group and tries to recreate a near-identical one, the new group enters a 90-day cooling-off period before it can generate benchmarks. And an attempt to recreate an identical deleted group is blocked entirely. There's no way to cycle through peer groups over time to extract company-specific data.


Whether a user applies market filters, builds a peer group, or combines the two, the output is always an aggregate market benchmark, never a view into any one company's pay practices.


## Your decisions stay yours


It's also worth being clear about what Pave doesn't do. Pave does not make compensation recommendations, and customers are under no obligation to use Market Data in any particular way. Our benchmarks inform independent decision-making using whatever survey data and compensation methodologies you choose, and Pave never coordinates strategies across clients. That independence matters under the current guidelines, and it's how the product is meant to work.


## This work doesn't stop


The guidance will keep changing, and our methodology will keep pace. We actively monitor developments from the FTC and DOJ and review our data practices against the current state of the law. We built Pave on the belief that[compensation data](https://www.pave.com/products/market-data-pro) and antitrust discipline can coexist. The guardrails above are how.


Want to go deeper on our methodology? Reach out to your Pave account team for our detailed anonymization and privacy documentation.


*This post is provided for general informational purposes and is not legal advice. Companies should consult qualified antitrust counsel regarding their own use of compensation benchmarking data.*


‍


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
