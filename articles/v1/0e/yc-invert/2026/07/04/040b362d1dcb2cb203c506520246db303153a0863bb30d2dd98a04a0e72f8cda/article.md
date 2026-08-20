---
schema_version: "1.0.0"
document_id: "040b362d1dcb2cb203c506520246db303153a0863bb30d2dd98a04a0e72f8cda"
company_key: "yc-invert"
company: "Invert"
source_id: "yc-invert-news-import-968576ef11e8"
canonical_url: "https://invertbio.com/blog/ipsc-trigger-timing-soft-sensor"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:27.297188+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:8af014ac8671b29b4a2a9891e1506a5b995561e4c4c3a5e48f4a10f3f31adbad"
---

# Using in-line signals to predict viable-cell yield across iPSC-derived production lots

Across 16 simulated iPSC-derived production lots, we used **Invert Assist** to infer each lot's viable-cell count live from in-line signals, and traced the lot swing to when the differentiation switch is triggered.


Here's Invert Assist working through it: reading all 16 lots as one dataset, inferring viable cells per lot from the[reactor signals](https://invertbio.com/blog/analyzing-real-time-time-series-data-in-bioprocess-with-invert) , and ranking what actually moved yield. It's interactive; give it a second to think.


*Assist reads all 16 iPSC-derived production lots as one dataset, infers viable cells per lot from in-line signals to within about 5–10% on lots it never trained on, and ranks the differentiation-trigger timing above every early process signal. Illustrative example, simulated data.*


In cell therapy PD you already know that some lots have low yields, and the explanation for these low yields is donor-to-donor variability. The problem is that the signal confirming this variability often doesn't appear until the post-harvest release assay, days or weeks after any intervention is possible. When donor material, medium, and feed are held consistent across lots, donor variability becomes a much less plausible explanation.


In this illustrative case study, we used simulated data from 16 iPSC-derived production lots on the same platform to build and test a soft sensor for viable-cell count and determine which controllable process variable best explained the yield differences between lots. Donor material, medium, and feed were held consistent, while differentiation-trigger timing was varied deliberately; viable-cell yield ranged from about 1.35 to 4.68 ×10⁹ cells per lot, with the target switch from expansion to differentiation occurring near 120 hours.


In our simulation, one lot was switched 48 hours late. Given the extra time, the cell count kept growing in expansion medium, overshot, then crashed once the switch finally came. That lot harvested at about 1.35 ×10⁹ cells against the expected >4 ×10⁹ cells for the on-time lots. The in-line soft sensor reported 1.51 ×10⁹ cells at harvest, already below the 2.50 ×10⁹ cells release floor, and about 60 hours before the assay confirmation.


## What Assist did to catch the differentiation switch trigger point


We used Invert to[consolidate the datasets from the 16 lots](https://invertbio.com/blog/why-bioprocess-data-fragmentation-is-slowing-down-the-industry) : the reactor signals (capacitance, glucose, lactate), the daily cytometry, and the LIMS release count. From those in-line signals alone, Assist built a model to predict final harvest viable-cell count within about 5–10% of the assay result.


## What this means for you


Using Invert Assist, you can easily model your process to estimate the harvest viable-cell count while the run is still going, not days after harvest, so a lot with a poor viability trajectory is something you can still catch. Because the analysis lives as code, the estimation is reproducible and consistent for every run and your colleagues can rerun it against their own lot. In addition to the ability to respond to and adjust poor growing cells, Invert provides a[shared data and analysis layer](https://invertbio.com/blog/invert-assist-ai-bioprocessing-quality-control-data-integration) that makes this analysis repeatable across scientists, studies, and future lot analyses.


*Figures here are from a simulated 16-lot dataset, shown as one illustrative example, not real data or a benchmark.[See what Assist finds in your own lots →](https://invertbio.com/demo)*
