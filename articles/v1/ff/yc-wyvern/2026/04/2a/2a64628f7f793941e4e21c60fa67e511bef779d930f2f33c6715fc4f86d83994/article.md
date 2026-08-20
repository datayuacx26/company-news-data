---
schema_version: "1.0.0"
document_id: "2a64628f7f793941e4e21c60fa67e511bef779d930f2f33c6715fc4f86d83994"
company_key: "yc-wyvern"
company: "Wyvern"
source_id: "yc-wyvern-news-import-543bd8ef8708"
canonical_url: "https://www.wyvern.space/blog/seeing-what-others-cant-how-hyperspectral-data-is-changing-maritime-domain-awareness"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-22T20:40:14.439396+00:00"
fetched_at: "2026-07-28T22:15:53.113233+00:00"
content_hash: "sha256:17d5c4ecea96c2dc12f17857fcba22ab5f3f4dda8b4d8796fcfb687a96485db1"
---

# Seeing What Others Can’t: How Hyperspectral Data Is Changing Maritime Domain Awareness

On March 27, 2026, the Government of Canada announced $6.1 million in funding through the Regional Defence Investment Initiative (RDII), a $379.2 million national program designed to integrate small and medium-sized enterprises into Canada’s defence supply chain. Wyvern received $2.9M in repayable funding through Prairies Economic Development Canada ([PrairiesCan](https://www.canada.ca/en/prairies-economic-development.html) ) to advance our hyperspectral ship identification and maritime monitoring capabilities.


The announcement, made by the Honourable Eleanor Olszewski, Minister responsible for PrairiesCan, took place at Wyvern’s Edmonton headquarters, where we design, build, and operate hyperspectral instruments.


Junior Mechanical EIT Brandon Hodge and Optical EIT Elliot Saive operate an early-TRL deployable mechanism breadboard during a demo for the Minister. Credit: David Bloom / Postmedia


This investment is a meaningful milestone for Wyvern and for Canada’s sovereign defence capacity. But the story behind this funding is also a story about how we monitor critical maritime zones and why the legacy tools we’ve depended on for decades can no longer meet today’s defence and intelligence demands.


## How maritime monitoring works today


Maritime domain awareness (MDA) is the ability to understand what’s happening on the world’s oceans: which vessels are where, what they’re doing, and whether they pose a risk. For countries like Canada—with the longest coastline on Earth, Arctic sovereignty obligations, and vast exclusive economic zones—MDA is a national security priority. It draws on a wide range of sources: cooperative transponder systems, coastal radar, patrol aircraft, signals intelligence, and satellite monitoring and imagery. Each layer contributes something the others cannot.


On the satellite imagery side, two sensor types dominate: high-resolution optical imaging and synthetic aperture radar (SAR). Understanding what each can and cannot do helps explain why[hyperspectral adds](https://www.wyvern.space/product) a meaningful new layer.


### High-resolution optical satellites


Optical satellites capture images in the visible spectrum, much like a very powerful camera in orbit. Providers such as Maxar and Airbus deliver imagery at sub-metre resolution, which is detailed enough to see individual vessels, deck structures, and even equipment on board. Analysts use this imagery to visually identify ship types, estimate size, and sometimes read hull markings.


High-resolution optical imagery excels at visual identification, but it captures light in only a few broad spectral bands. That tells an analyst what a vessel looks like: its shape, size, and colour. What it doesn’t reveal is what the vessel is made of, what coatings are on the hull, or what materials are on deck. Two ships painted the same colour will look identical in a conventional photograph, even if their surface compositions are entirely different.


### Synthetic aperture radar (SAR)


SAR satellites transmit microwave pulses and measure the signal that bounces back. This makes SAR an all-weather, day-and-night sensor. Providers like ICEYE, Capella Space, and the Canadian[RADARSAT](https://www.asc-csa.gc.ca/eng/satellites/radarsat/) constellation deliver SAR imagery that can detect vessels in any conditions, including through cloud cover and at night.


SAR is excellent at detecting that a vessel is present. The radar return from a metal hull against open water is distinctive and reliable. But SAR imagery is fundamentally a measurement of surface texture and reflectivity at microwave wavelengths. It can tell you a ship is there and give you its approximate size and heading. What it struggles with is identification. SAR can classify a vessel by type (e.g. a fishing trawler versus a bulk carrier, for instance) based on size and shape. But two ships of the same class will look nearly identical. It cannot discriminate between specific vessels or characterise their surface materials.


### Automatic Identification System (AIS)


AIS is the transponder-based system that requires vessels to broadcast their identity, position, speed, and course. It’s the backbone of cooperative maritime tracking and is received by both ground stations and satellites. But AIS is a cooperative system, as in, it depends on vessels choosing to transmit. Vessels engaged in illegal fishing, sanctions evasion, smuggling, or military manoeuvres routinely disable their AIS transponders, becoming “dark” vessels. These are precisely the ships that matter most for defence and security, and they’re invisible to AIS.


In practice, maritime analysts combine these sources. AIS provides a baseline of cooperative vessel traffic. SAR detects all vessels, including dark ones. Optical imagery can confirm and classify detections when the weather cooperates. Analysts then cross-reference the data to find anomalies, for example, SAR detections with no matching AIS signal, or vessels behaving unexpectedly. This process works, but it’s labour-intensive, limited by sensor availability, and constrained by what these instruments can actually measure.


An example hyperspectral data cube of an LNG tanker ship (left), and an example spectral curve of a hull coating (right).


## What hyperspectral adds


Where a standard satellite camera captures light in a limited number of broad colour bands, Wyvern’s Dragonette satellites capture hyperspectral satellite imagery across 31 narrow spectral bands in the visible and near-infrared (VNIR) spectrum. Each band isolates a specific slice of the electromagnetic spectrum, and the result is more than just an image: it’s a detailed spectral signature for every pixel in the scene.


A spectral signature is like a fingerprint for a material. Different paints, metals, coatings, and surface treatments all reflect light differently across these wavelengths. A standard photograph might show two vessels that look the same colour to the human eye. Hyperspectral data can reveal that their spectral signatures are distinct, indicating different hull coatings or surface treatments that are invisible in a conventional photograph.


For maritime domain awareness, this means hyperspectral can do something optical and SAR fundamentally cannot: characterise a vessel’s unique spectral signature, not just its shape, size, or presence. This capability supports ship classification and identification workflows that go beyond shape and size, providing discriminative information that helps analysts resolve ambiguity and characterize targets with greater confidence.


## Hyperspectral intelligence for defence


Wyvern is building a hyperspectral-enabled ship identification and tracking product built for defence and maritime security users. It supports detection, classification, and identification workflows by applying spectral analysis to vessels observed by our Dragonette constellation. This effort expands on existing[Wyvern technology for the defence industry](https://www.wyvern.space/industries/peace-and-security) , enhancing current features in material identification, camouflage detection, and remote surveillance.


With the RDII funding, we’re advancing this capability in several specific ways: improving our identification models to better classify vessels using spectral signatures, accelerating data delivery timelines so intelligence reaches analysts faster, advancing our next-generation sensor technology, and investing in regulatory compliance to meet the standards required for Canada’s defence supply chain.


## Dual-use infrastructure


One of the most compelling aspects of this investment is its dual-use nature. The same constellation that supports maritime defence also serves Canadians in agriculture, forestry, energy, and environmental monitoring. The satellites collecting data for ship identification are the same ones helping foresters assess wildfire risk, farmers monitor crop health, and energy companies track environmental compliance.


As Wyvern CEO Chris Robson noted during the announcement: “The same infrastructure that supports defence also serves Canadians in other ways. The data we collect for maritime security comes from the same constellation that helps foresters manage wildfire risk and farmers increase profitability. This investment strengthens defence readiness and builds dual-use infrastructure that benefits all of Canada.”


Wyvern currently operates five satellites on orbit, serves customers and partners across more than 30 countries, and employs a team of 35 people, the majority based in Edmonton. Our Dragonette constellation captures data in 31 spectral bands at approximately 5 m resolution, generating over 60 TB of data per month.


An image over the Khor Fakkan port in the United Arab Emirates taken by Dragonette-003 on February 18th, 2026.


## Strengthening Canada's defence ecosystem


The RDII investment in Wyvern is part of a broader $6.1 million funding package announced for the Edmonton region on March 27th. Alongside Wyvern, the Government of Canada invested in the University of Alberta’s DEFENDS initiative and the Council for the Advancement of Native Development Officers (Cando), which is developing an Indigenous Defence Supply Chain Program pilot. The announcement followed a separate$6.5 million RDII investment announced in February supporting two other Edmonton-area companies: Logican Technologies, which is expanding manufacturing of submarine sonar and naval defence electronics, and[Zero Point Cryogenics](https://www.zpcryo.com/) , which is commercializing sub-Kelvin cryogenics for quantum sensing and defence applications.


These investments reflect a strategic approach to building sovereign defence capacity across the Prairies, connecting SMEs, academic institutions, and Indigenous communities into Canada’s defence industrial base. The RDII is part of Canada’s Defence Industrial Strategy, designed to keep Canadian technology onshore and ensure sovereign access to the intelligence and capabilities the country needs.


For Wyvern, this funding validates what we’ve been building since 2017: a Canadian-owned, Canadian-operated Earth observation capability that serves both commercial and defence markets. Earth observation data is essential infrastructure, and Canada needs sovereign access to it.


## Looking ahead


Maritime domain awareness is evolving. The combination of optical, SAR, AIS, and now hyperspectral creates a richer, more layered picture of what’s happening on the world’s oceans. Each sensor type fills a gap the others leave. Hyperspectral doesn’t replace optical or SAR, it adds a capability they cannot provide: the ability to see not just where a vessel is or what it looks like, but its unique spectral signature—a fingerprint that supports more confident identification.


As Wyvern continues to grow our constellation and develop our next-generation sensor technology, we’re building the earth intelligence layer that Canada and its allies need to monitor and protect the maritime domain with greater confidence and precision.


We’re grateful to PrairiesCan and Minister Olszewski for recognising the value of what we’re building here in Edmonton. This is the kind of investment that keeps Canadian technology onshore and ensures Canada has sovereign access to the intelligence it needs.


Curious about how Wyvern's hyperspectral imaging and how it works in military operations?[Book a slot in Chris’s calendar.](https://app.usemotion.com/meet/chris-robson/web-defence)
