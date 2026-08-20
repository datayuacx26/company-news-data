---
schema_version: "1.0.0"
document_id: "389b340d816bb418e700ba51675460793419753a148e97df36bb7d50f023525c"
company_key: "yc-carbonfact"
company: "Carbonfact"
source_id: "yc-carbonfact-rss-e3c23fd0e117"
canonical_url: "https://www.carbonfact.com/blog/platform/bespoke-ef"
published_at: "2026-06-24T08:47:04+00:00"
first_seen_at: "2026-07-24T22:18:04.567508+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:21127ab4d07eada82a77339fe357a8130739e67b28155cd378266f3d44acea27"
---

# Carbonfact Explains: Bespoke Emission Factors

## **What are Carbonfact's Bespoke Emission Factors?**


*Part of Carbonfact’s*[Data Engine](https://www.carbonfact.com/data-management-software)


Bespoke emission factors (EFs) are custom EFs Carbonfact's Science team builds when standard databases don't capture what a brand actually uses or makes. Apparel and footwear brands often work with novel materials, specialized processes, or specific suppliers that fall outside what's documented. Bespoke EFs close that gap using primary data from the brand or supplier, modeled in Brightway with the EF 3.1 method. The result? A more accurate footprint exactly where a generic EF would have blurred the picture.


#### TL;DR


Bespoke emission factors are the custom EFs Carbonfact's Science team builds from primary data – for materials, processes, or suppliers that standard databases don't cover accurately. They're purposely built for your brand alone.


## **How Bespoke EFs work**


Every bespoke EF starts with primary data. The Science team collects what goes in and what comes out of a process, for example: electricity, materials, water, waste, and transport.


The resulting EF lands in your platform and is used only for your products. Two brands working with the same factory won't share each other's bespoke EFs unless they choose to – the EF belongs to your account.


Carbonfact has built 750+ bespoke EFs to date across novel materials, specific supplier processes, and supplier-specific energy mixes. The full methodology is[PwC-audited](https://www.carbonfact.com/blog/platform/carbonfact-pwc-reviewed) and ISO 14040-aligned.


## **How Bespoke EFs Show up in Your Data**


Most of your footprint runs on standard EFs from Ecoinvent and EF 3.1, and that's fine where those databases cover the ground. Bespoke EFs fill in where they don't. We’ll break down three of the most common cases:


### 1. A specific variation of a material or process


Your brand uses an autoclave process to make foamed EVA midsoles – a method that produces lighter midsoles than standard injection molding, but uses substantially more electricity to do it. Standard databases only have an EF for injection-molded EVA. If you used that EF, your


[autoclave midsoles](https://www.carbonfact.com/blog/yawa5-autoclave-midsole) would look lower-impact than they really are, because the lighter weight would mask the higher energy use.


Carbonfact's Science team collects primary data from your manufacturer. Think of electricity per kg, nitrogen input, water, EVA composition, and waste rates. The Science team then builds a bespoke EF for the autoclave process specifically. The result: a unique midsole footprint that reflects what's actually happening at the factory.


### 2. A supplier-specific process


Your elastic supplier runs 18 different dyeing setups across their lines, varying by technology, shade, and order quantity. A generic dyeing EF treats all of these the same. Working with the supplier, the Science team builds 18 distinct EFs that capture the real variation – so when you produce a small-batch dark shade on one machine and a large-batch pastel on another, the two footprints reflect the actual difference.


The same logic applies wherever a supplier's process is meaningfully different from the industry default: a low-temperature dyeing technology, a closed-loop water system, a specific elastic-formation method. Each one becomes its own EF instead of being averaged into a generic step.


### 3. A patented or proprietary material


Your brand sources a recycled polyester from a specialist supplier whose chemical recycling process produces a fundamentally different chemistry than the mechanical recycling captured in standard databases. If you used the generic recycled polyester EF, your products would show the footprint of the average recycled-polyester category – not the footprint of the specific material you've chosen.


The supplier shares their LCA data with Carbonfact's Science team, who validate the inputs and build a bespoke EF for that exact material. Now, if you were to model swapping all other polyester variants in favor of this patented or proprietary version of polyester, you’ll get a realistic view of what would happen with your impact.


## **What Goes into a Bespoke EF**


Building a bespoke EF takes weeks of Science work. It starts with finding a representative set of suppliers that run the specific process, are willing to share data, and have the capability to measure it properly. From there, the team collects primary data – think of energy, water, chemicals, waste, and transport.


That gets cross-checked against Carbonfact's internal benchmarks and the published literature, then refined through several rounds of follow-up with the supplier to fill missing flows and resolve outliers. This phase is where most of the time goes: supplier data rarely arrives clean on the first pass, and the back-and-forth is what separates a defensible EF from a guess.


Only once the inventory is solid does the modeling start. The Science team computes the impact scores using the EF 3.1 method across[all 16 PEFCR indicators](https://www.carbonfact.com/blog/policy/pefcr-apparel-and-footwear) (climate change, eco-toxicity, water use, and the rest), benchmarks the results against comparable factors, corrects where needed, interprets what the numbers mean for the brand's footprint, and assigns a Data Quality Rating (DQR). Every assumption along the way is documented, so the EF can be audited end-to-end.


## **Bespoke EFs in a nutshell**


Bespoke EFs are how Carbonfact moves beyond generic industry averages where it matters most. When your brand uses a novel material, a specific supplier process, or a factory with a measured energy mix, the Science team builds a custom EF from primary data, which modeled, audited, and scoped to your account alone. So the next time a stakeholder, auditor, or regulator asks why your dyeing footprint, your midsole footprint, or your Tier 2 supplier footprint looks different from the industry default, you have a clear answer: because that's what your supply chain looks like, modeled with your actual data.


**


**


**
