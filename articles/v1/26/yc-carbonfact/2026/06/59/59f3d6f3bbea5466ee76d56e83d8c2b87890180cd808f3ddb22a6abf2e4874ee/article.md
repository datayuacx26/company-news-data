---
schema_version: "1.0.0"
document_id: "59f3d6f3bbea5466ee76d56e83d8c2b87890180cd808f3ddb22a6abf2e4874ee"
company_key: "yc-carbonfact"
company: "Carbonfact"
source_id: "yc-carbonfact-rss-e3c23fd0e117"
canonical_url: "https://www.carbonfact.com/blog/platform/uncertainty"
published_at: "2026-06-25T11:13:17+00:00"
first_seen_at: "2026-07-24T22:18:04.567508+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:adbf0fe01510fe9e8e313c7ffe0c20df429a3b21b1050e79d9d78bc71cf050c0"
---

# Carbonfact Explains: Uncertainty

## What is Carbonfact’s Uncertainty Metric?


*Part of Carbonfact’s*[Data Engine](https://www.carbonfact.com/data-management-software)


#### TL;DR


When a footprint relies on assumptions rather than primary data, the exact number can't be pinned down with certainty. That's why Carbonfact reports a range around every product's footprint.


The more primary data you add, the more assumptions you replace – narrowing the range and increasing a footprint’s accuracy. Uncertainty turns that range into a metric you can use to prioritize data collection, helping you prioritize primary data collection where the uncertainty range is the highest.


We built Uncertainty Metric because life cycle assessments (LCAs) are complex and rarely built on perfect data – to produce an initial LCA, Carbonfact uses heuristics to fill the gaps. For example, when a dtex is missing, when a raw material origin is unknown, or when a component weight didn’t make it into the Bill of Materials (BoM), Carbonfact fills the gap based on patterns from the


[2 million+ LCAs](https://www.carbonfact.com/lca-for-fashion) we run every day across our customer base.


The more assumptions behind a footprint, the higher its Uncertainty – and the bigger the case for collecting primary data. That’s exactly what the Uncertainty metric does: it shows you where assumptions are made, and which products to prioritize for data collection first.


## Using Uncertainty to Prioritize Data Collection


You work in sustainability at a sneaker brand. Shoes are among the most complex products in apparel and footwear, with 50 to 100 components on average. To calculate an initial footprint, Carbonfact will lean heavily on heuristics and, in places, secondary data. Bills of Materials (BoMs) for shoes are rarely 100% complete or correct.


The problem: your product footprint isn’t accurate enough to make a strong decision on where to reduce. Too many parts means too many assumptions – so you need more data. But where do you begin? The laces? The lining? The midsole? What about the processes to make each of these? You’re looking for a needle in a haystack.


This is when Uncertainty analysis in Explorer comes in. Instead of searching the haystack by hand, you’re searching it with a magnet. We’ll break down how you would manually explore a dataset using our filters, but please note you can bypass this by simply asking our AI Copilot “Which shoes in my product catalogue have the highest degrees of uncertainty, broken down by placement and process?”


Using data from our demonstration account, the most uncertain placements are the upper, the lining, and the insole. We now know which placement and which placement material need more primary data.


Next, switch the filter from “Materials” to “Processes.” It’s important to know not just which materials carry high Uncertainty, but the processes behind them. This immediately reveals that the raw material – Viscose in this case – contributes less to emissions than the processes behind it. This is exactly what Uncertainty is designed to do: reveal where assumptions are hiding, whether in material data or in process steps.


The same Uncertainty can be viewed by product instead of by placement. In our demonstration data, it lands on a single product, Best Sneakers. In your catalog, this view usually points to a small number of sneakers carrying most of the Uncertainty, telling you exactly which products need more primary data. Here, you can see how


[ARMEDANGELS uses Uncertainty](https://www.carbonfact.com/armedangels-webinar) to gather primary data in a prioritized manner. Most brands will want to aim for <30% Uncertainty in order to have faith in your overall scope 3.1 impact.


Now, when we open the LCA for the upper we see the specifics behind the process steps – note that the ± 3.3 signals the Uncertainty range per product SKU:


Although the material might be correct, supplier data is missing – no location is set on any of the process steps, and generic process steps have been applied throughout. This is typical for brands running their first footprints. The conclusion is simple: you’ll need to reach out to your supplier to better understand the upper, or


[check Carbonfact for Suppliers](https://suppliers.carbonfact.com/) to see if your supplier has already shared their LCA there. But now you can ask direct questions. For example:


How much and which type of energy is used in the uppers of sneakers made with EcoVero Viscose for the following processes:


- Finishing


- Textile formation


- Coloration


- Preparation


- Yarn formation


These steps drive a meaningful share of the footprint while also carrying high Uncertainty. Better data may not reduce the footprint immediately, but it will give you decisions you can justify. Relative to your production output, the finishing and textile formation stages generate the most emissions – with your supplier’s input, you can identify alternative process steps that still fit your brand, or phase out materials intrinsically linked to these production steps.


## How is Uncertainty calculated?


Every emission factor in an LCA database comes with variation – different databases can return different values for the same input. Producing 1 kg of raw Merino wool, for example, ranges from 28 to 142 kg CO₂e depending on which database you consult.


The Uncertainty metric captures that range. For each factor, we take the difference between its highest and lowest plausible value, then multiply by volume – the amount of that material in the product. A wide-ranging factor on a heavy component contributes more Uncertainty than the same factor on a light one. The same logic applies to anything else we can only express as a range, like a component weight that isn’t yet precisely known.


The engine sums these into the interval – the plus-or-minus range – you see around each product’s footprint. Everything is expressed in kg CO₂e, so Uncertainty is directly comparable across materials, suppliers, and products.


In other words:


*Uncertainty = (EF_max − EF_min) × amount*


**


## Uncertainty in a nutshell


If there’s only one thing to remember about Uncertainty, it’s that it shines a light on where assumptions hide. By showing you the range behind your footprint and pointing you toward the specific areas where more primary data will make the biggest difference, Uncertainty helps you prioritize your data collection and strengthen the reliability of your results. Every number comes with a clear view of the assumptions behind it – so when stakeholders, auditors, or regulators ask how you got there, you have a defensible answer.
