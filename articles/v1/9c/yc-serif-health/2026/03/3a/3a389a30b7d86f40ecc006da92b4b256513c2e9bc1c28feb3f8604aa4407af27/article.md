---
schema_version: "1.0.0"
document_id: "3a389a30b7d86f40ecc006da92b4b256513c2e9bc1c28feb3f8604aa4407af27"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/aetna-aca-exchange-exit-rate-analysis"
published_at: "2026-03-01T11:15:00+00:00"
first_seen_at: "2026-07-24T01:12:39.675945+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:72dbd9380a5c55108d6a9e9f12397d238497f7c829fc50c3349dee53222826d8"
---

# How Price Transparency Data Explains Aetna’s ACA Market Exit

## Individual, ‘Obamacare’ Markets in Focus


Coming out of last year’s government shutdown and the ongoing policy cliff around Affordable Care Act (ACA) Exchange plan subsidies and affordability, the individual insurance market remains in the news each week. While questions around the extension of subsidies and impacts on enrollment remain, over 20 million Americans are expected to receive individual insurance through an Exchange plan[in 2026](https://www.reuters.com/legal/litigation/obamacare-enrollment-drops-about-23-million-people-2026-2026-01-29/) . At the same time, Aetna announced last year that it will exit the ACA Exchanges for 2026 even as the Marketplace has grown into a record-sized program ([24.3M enrollees received coverage in 2025)](https://www.kff.org/affordable-care-act/enrollment-growth-in-the-aca-marketplaces/) .


Aetna/CVS’ CEO David Joyner described the decision to exit the Exchange market last summer, noting broadly on an[investor call](https://www.beckerspayer.com/payer/aetna-to-exit-individual-market/#:~:text=On%20a%20call%20with%20investors,Joyner%20said.) that there was “not a near or long-term pathway for Aetna to materially improve its position in the market.” That combination of record enrollment, policy volatility, and Aetna’s full market exit sets up a technical question we can explore with price transparency data: **in the Exchange markets Aetna is leaving, how do its negotiated provider rates, networks, and rate growth compare to peers, and what do those differences suggest about Aetna’s decision?**


In this analytical blog, the first in our *TiC Takes* series this year, we utilize Serif Health’s comprehensive national Transparency-in-Coverage (TiC) data to examine Aetna’s performance in the Exchange markets. In main analyses, we analyze Exchange in-network reimbursement and premium growth dynamics across 55 Exchange plan networks spanning 16 states, including Aetna and a set of large individual-market competitors (e.g., UnitedHealthcare, Cigna). We highlight the challenges that commercial payers, including Aetna, face in rising in-network rates, particularly for institutional (inpatient) services.


## Low, Low Prices


Since being introduced in 2014 through the ACA, the individual health insurance market’s total annual enrollment, particularly through federal or state ‘exchanges’ or ‘marketplaces’, has[steadily grown over time](https://www.kff.org/affordable-care-act/enrollment-growth-in-the-aca-marketplaces/) . And across the TiC data we’ve standardized, 209 individual networks in total, Exchange networks consistently reimburse in-network providers less than Group insurance HMO and PPO plans. For the same medical service billed through a CPT/HCPCS code, Exchange plans tend to reimburse providers at a lower percent of Medicare than group insurance plans do.


The figure below shows this directly. We summarize negotiated rates as **percent of Medicare** across network classes, and the Exchange class’ median rates sits meaningfully below Group HMO and Group PPO. Why normalize to Medicare?[Because Medicare](https://www.serifhealth.com/blog/commercial-rates-benchmarking-medicare) provides a stable benchmark that helps standardize across services of different costs. The Medicare benchmark is fixed, so the gap isn’t a coding artifact or driven by high-cost services, but rather a contracting outcome and consistent with[prior studies](https://www.healthaffairs.org/doi/10.1377/hlthaff.2024.00913) .


Exchange plans are priced into a more premium-sensitive environment, with medical loss ratio requirements, annual repricing, and intense benchmark-driven competition. With[premium subsidies](https://www.congress.gov/crs-product/R48290#_Toc216780458) historically set based on the second lowest issuer in the market *and* medical costs driving the majority of premiums, the easiest lever to stay competitive is one of the oldest in insurance: lower, or at least maintain, unit costs.


## Aetna’s Exchange Plans Face Higher Contracted Rates


While Exchange rates overall reflect a lower percent of Medicare overall, some notable differences across payers within the Exchange market emerge with further stratification. In our standardized, 16 state TiC sample, Aetna’s Exchange negotiated rates run materially higher than peer Exchange payers. Put simply: **Aetna is often paying more for the same services in a market that punishes higher unit costs** . While variation exists across states, relative to other payers in state-based Exchange markets, Aetna pays about 20% more than other common Exchange payers.


As shown below, differences in institutional rates, the category that often drives the largest share of medical spend, between Aetna and peers Cigna and UnitedHealthcare show a wide spread: Aetna’s median Exchange in-network rate is 134% of Medicare, versus 99% for Cigna and 97% for UnitedHealthcare. Professional services are tighter but still meaningfully higher for Aetna (122%) than peers (118% and 110%), reinforcing that Aetna’s Exchange pricing pressure isn’t limited to hospital care.


Drilling into specific state insurance markets, Aetna has faced varying in-network rates but rarely a rate advantage vs. other payers.


## Aetna Exchange Provider Networks Expanded in 2025, Unlike Other Payers


Interestingly, we also observe big differences in Aetna’s approach to provider network size versus strategies pursued by other payers in 2025. A second lever Exchange market insurers often pull is around network breadth and design. Narrower networks can reduce total spending per member (or at least steer volume), while[broader networks](https://www.healthaffairs.org/doi/abs/10.1377/hlthaff.2016.0693) can improve attractiveness but often increase contracting complexity and cost.


In 2025, we observe **Aetna expanding Exchange provider network size** relative to peers, who were more stable or more selective. This isn’t “wrong” necessarily, but a strategy choice to improve its network quality (or composition). It can also be expensive if it increases exposure to higher-price contracting.


While differences in network size can also be explained through differences in enrollment, markets covered, and other characteristics, growth rate estimates from the TiC data suggest that Aetna substantially expanded it’s provider networks in 2025 while other national payers, include Cigna and UnitedHealthcare either maintained or shrunk their networks. Results are consistent considering both institutional and professional rates, and considering either in-network organizations (EINs) or unique provider identifiers (NPIs).


For Aetna, most networks increased in-network providers for both types of services; for example, its Nevada exchange product reported a third more (34%) in-network institutional providers (based on EIN) and double (104%) the professional providers by January 2026 compared to January 2025. United Healthcare reported substantially lower in-network providers in its Exchange network TiC disclosures beginning around August 2025, perhaps reflecting[anticipated 2026 enrollment decreases](https://www.reuters.com/business/healthcare-pharmaceuticals/unitedhealth-offer-rebates-its-obamacare-customers-2026-01-21/#:~:text=In%20a%20third%2Dquarter%20earnings%20call%20the%20company%20said%20it%20expects%20its%20ACA%20enrollment%20to%20be%20reduced%20by%20about%20two%2Dthirds.) . Taken together, results are also consistent with academic research on individual plans: a 2024[Health Affairs](https://www.healthaffairs.org/doi/10.1377/hlthaff.2024.00913) study found that Marketplace contracting both in terms of rate levels and network breadth can look meaningfully different from employer and other group insurance coverage.


## Aetna's Rate Position has Worsened Over Time


Assessing year over year trends, did Aetna’s network changes meaningfully improve its in-network rate position? Unfortunately not! In fact, Aetna’s in-network rates have continued to increase as a percent of Medicare over time.


We can directly tie in-network provider rate increases to premium increases for Aetna Exchange networks, as we show below sourcing premium growth data directly from CMS’[Uniform Rate Review](https://www.cms.gov/marketplace/resources/data/rate-review-data) program. Across states, Aetna’s median in-network institutional rate growth averages around 22% in both years, while premium growth from 2024 - 2025 (the last year of data available) sits in a similar range (averaging 11.6%). That’s directionally consistent with the idea that Exchange pricing is tight: plans can’t just pass through large cost increases without losing competitiveness.


Not all payers experienced the same sharp increases in in-network rates like Aetna. Across the overlapping states United Healthcare operated Exchange plans, for example, median in-network rates dropped slightly on average (-1%) from 2024-2025 across states, and increased much less than Aetna’s networks in total from 2025-2026 (8% vs. 21%). These efficiencies allowed United to increase premiums less than Aetna in many states.


Putting the pieces together on Aetna’s 2025 Exchange market outlook:


1. **Exchange rates are clearly lower** than in group insurance markets. Estimated individually for comparable Aetna HMO networks operating in the same states as its Exchange products, median institutional rates in 2025 were 216% relative to the 133% median reimbursed by it's Exchange networks.
2. **Aetna’s Exchange rates are higher** than peer Exchange payers, especially national competitors Cigna and United Healthcare.
3. **Network breadth expanded for many of Aetna networks** in 2025, which can compound rate pressure if members can more immediately utilize care.
4. **Rates increased substantially in every state** where Aetna stayed contracted.
5. **Aetna’s premiums often increased more than competitors** , putting it at a disadvantage for enrollment.


Contracting at higher negotiated rates while competing in a particularly price-sensitive segment of commercial insurance is a tough place to be. In this context, Aetna’s 2026 Exchange exit is easier to understand mechanically: if you’re systematically paying more for the same services and locked into multi-year contracting relationships with providers, you start behind and it’s difficult to catch up.


## Was Aetna’s Decision the Right Business Decision?


In light of our findings from the TiC data, Aetna’s exit from the Exchange markets in 2026 makes more sense. **** Uwe Reinhardt’s famous line still holds:[it’s the prices, stupid](https://www.healthaffairs.org/doi/10.1377/hlthaff.22.3.89) **.** And the cost-efficient Exchange market is where that reality gets stress-tested most, because premiums are constrained and based on medical spending, competition is benchmark-driven, and small rate differences can compound quickly.


That’s the real punchline of this Aetna analysis. Using **standardized, enriched TiC data,** we can move beyond hand waving about Aetna’s exit and drill into its causes:


- For Exchange networks, **Aetna’s negotiated rates run materially higher than peers** (on the order of ~20% averaged across states), putting it at a structural disadvantage on unit cost of medical services, especially institutional (inpatient) services.


- Layer in its network changes and cost trends, and Aetna’s unit-cost exposure becomes even harder to manage in a market that rewards competitive premium (and thus in-network provider rate) setting.


This is exactly why enriched TiC data, relative to claims data or aggregate public reporting, is so powerful as a tool for market analysis specifically: it lets you connect **contracting outcomes** to **plan competitiveness** and **strategy choices** in a way that’s measurable, state-by-state, and payer-by-payer. Aetna missed an opportunity to apply transparency data itself to identify the problem early, quantify exactly where its rates were not competitive, and course-correct.


And we’re already seeing payers respond to this increasing market awareness. United Healthcare recently said it intends to[provide rebates to Exchange plan members in 2026](https://www.reuters.com/business/healthcare-pharmaceuticals/unitedhealth-offer-rebates-its-obamacare-customers-2026-01-21/) , a customer satisfaction and PR initiative made possible partly through the insurer’s comparably low in-network Exchange rates. That’s the direction the market is heading: pricing dynamics that shows up not only in pricing disclosures, but in business and policy decisionmaking.


**About Serif Health**


At Serif Health, our focus is to turn every transparency disclosure into reliable, comparable reimbursement intelligence so teams can make decisions with confidence. We’ll continue to publish insights on[our blog](https://www.serifhealth.com/blog) and highlight how payer rates data shows up in[live MRFs](https://www.serifhealth.com/inventory/) via our payer inventory.


If you’re interested in applying price transparency data, we’d love to connect. Please reach out tohello@serifhealth.com or schedule a[demo](http://calendly.com/serif-health/30min) . Also feel free to check out our[sample data](https://app.serifhealth.com/sample/data) on our web platform[Signal](https://www.serifhealth.com/products/signal) .


-


***Technical Notes***


**Data Sources**


- Transparency in Coverage (TiC) payer MRFs. Rate and plan network analytics are derived from payer-posted machine-readable files (MRFs) required under the federal Transparency in Coverage rules. We use Serif Health’s normalized, enriched rates table joined to Serif’s registry of currently active plan networks classified as Exchange plans.
- CMS Unified Rate Review (URR) Public Use File. Premium rate change data for Exchange networks come from CMS URR plan submissions (based on the CMS URR PUF, “Worksheet 2” fields. The URR portion of the study is summarized to (state × carrier) filtered to Exchange Silver renewing plans with effective dates aligned to January 2024 and January 2025.


State-by-state comparisons are performed on a predefined set of states and carriers where we have meaningful overlap across Aetna and comparator Exchange networks (e.g., Aetna vs UnitedHealthcare vs Centene/Ambetter vs Cigna vs local Blues, depending on state).


Across Aetna’s 16-state footprint (DE, FL, GA, IL, IN, KS, MD, MO, NC, NJ, NV, OH, TX, UT, VA), Ambetter operated in 12 of those states (not shown here), UnitedHealthcare in 10 (FL, GA, IL, KS, MD, MO, NC, NJ, TX, VA), and Cigna Corporation in 7 (FL, GA, IL, IN, NC, TX, VA) (45 total networks). Ambetter was also considered given overlap with many Aetna networks across the US. However, Ambetter postings had 90% fewer EINs and similarly incomplete codesets covered as Aetna/UHC/Cigna exchange plans in the same markets, leading to us excluding the payer from main comparisons. Data for[Molina Healthcare](https://www.serifhealth.com/inventory/payers/molina-healthcare/networks/confident-care-gold-1-nv) and[Oscar Healthcare](https://www.serifhealth.com/inventory/payers/oscar-insurance-corporation/networks/illinois-select-il) were excluded completely due to data quality issues.


**Study Sample and Unit of Analysis**


The primary unit of analysis in the TiC portion of our analysis is (payer × network × state × billing_class × month x code x ein), summarized from rate rows for:


- Institutional services (facility/inpatient reimbursement)
- Professional services (clinician/professional reimbursement)


Each negotiated rate row, we compute percent of Medicare relative to Serif benchmarks. To reduce noise from non-credible values and placeholders, we apply additional filters:


- Restrict to primary billed rates (is_billed_primary = 1)
- Exclude rows with modifiers
- Exclude penny/placeholder rates (rate > 0.01)
- Require valid Medicare baseline
- Restrict implied percent-of-Medicare to a plausible band between 50 - 1000%


Because a single provider may report multiple rate rows for the same code (e.g., varying service settings or contract artifacts), we deduplicate to a within-provider maximum in-network rate. For each (payer, network_name, EIN, code, month) we take MAX(percent of Medicare). We then summarize within each network-month the median(percent of Medicare) across EIN×code observations. For a small number of networks where 202401 was not observed for a payer/network, we allow Feb 2024 (202402) as the data month considered in 2024.


Percent changes at the network level were estimated from differences in median time point estimates across different months of data.


**Network Size and Growth**


Network breadth is approximated using distinct EINs and NPIs contracted with a network in a given month, derived from the TiC rate rows after applying the same quality filters. This provides a consistent, observable proxy for contracting breadth in TiC data, and estimates as well as growth rates are consistent using both EIN- and NPI- based approaches. However, results should not be interpreted as a formal network adequacy metric (which can depend on time/distance rules, specialty composition, member access standards, etc.).
