---
schema_version: "1.0.0"
document_id: "8d100560268e4aad4d5b199f26d58f4e59eabe442e23c7b3b9ba607a9b193bff"
company_key: "recursion-pharmaceuticals-inc-class-a-common-stock"
company: "Recursion Pharmaceuticals Inc."
source_id: "recursion-pharmaceuticals-inc-class-a-common-stock-news-import-f0ced5d1ab91"
canonical_url: "https://www.recursion.com/news/modeling-animal-data-for-more-efficient-ai-drug-discovery"
published_at: "2025-01-10T00:00:00+00:00"
first_seen_at: "2026-07-22T11:05:14.614366+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:670680f0dcfd1feced8f9d602d402381e055785ae84a526cfe2785e48cb765b9"
---

# Modeling Animal Data for More Efficient AI Drug Discovery

At Recursion, one of our data layers is called “InVivomics” – a word we coined that refers to omics-level data gathered from in vivo, or animal, studies. Our labs have approximately 1,000 digital animal enclosures, or vivariums, that allow us to monitor animals around the clock. These “smart” cages track a range of measures in the animals’ physical state – including activity, body temperature, and breathing rates – and provide real-time alerts related to disease and toxicity.


Digital cages allow us to get a fuller picture of a disease we are investigating, mirroring the effects that would be measured on a person in the clinic, and providing a reliable baseline for testing the safety and efficacy of compounds.


To extract maximum insights from this data, we built a deep learning model called InVivoPrint V1 (IVP-1) that increases our ability to decode signals coming from these smart cages – detecting liabilities such as inflammation or toxicity earlier than our previous digital biomarker approach. IVP-1 allows us to detect organ toxicities linked to a compound or dose candidate as early as possible during in vivo tolerability studies – and to prioritize new drug candidates for the efficacy phase.


‍


‍


**How IVP-1 Works**


IVP-1 is a discriminative model – a machine learning model that can distinguish between different categories in a dataset – that’s been trained with a multi-task objective (to solve multiple problems simultaneously) in order to encode all the modalities that represent an in vivo experiment.


‍


‍


The model incorporates time-series, video-based animal data with new data modalities from cage sensors, for a total of 19 inputs. It also incorporates clinical chemistry, hematology and over a hundred other modalities during training in order to find subtle liability signals. Liabilities could include: hepatotoxicity (liver toxicity), systemic inflammation, fibrosis, nephrotoxicity (kidney toxicity), cardiotoxicity (heart toxicity).


As the name implies, this tool produces a “print,” or more specifically a “Multi-Experiment-Based InVivoPrint” which gives us an experimental fingerprint of what one of these liabilities looks like in an animal model. The model then compares Compound-Dose InVivoPrints to Liability InVivoPrints to determine the relationship between compound, dose, and liability.


In other words, IVP-1 seeks to understand: “What about these compounds, which are all known to be associated with Liability A, is similar, such that it is likely to represent a fingerprint of the liability itself?”.


And then: “Based on our nuanced understanding of what Liability A looks like, does this new chemical entity look like it is associated with Liability A?”.


Our current InVivomics dataset includes 1 million hours of video; 1 million hours of digital biomarkers such as locomotion, body temperature, wheel speed, and cage humidity levels; 149,000 environment data points, including cage slottings, rack used, rack room, sex, and birth time; and 13,000 assays, as well as a number of other categories.


As we continue to evolve our machine learning models, we’re improving our ability to detect potential drug toxicities and to quickly identify the best drug options - saving both time and resources. If we can confidently identify a narrow range of doses, we can accelerate clinical timelines, cutting off months for each dose we can eliminate.


‍


*Author:*[Brita Belli](https://www.linkedin.com/in/brita-belli/) , *Senior Communications Manager at Recursion.*


‍
