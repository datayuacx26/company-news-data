---
schema_version: "1.0.0"
document_id: "2e6763edfad179f9ea2a3d270d733cd9f93ee851a23ee673a8a67ee48f5fa258"
company_key: "yc-empirical-health"
company: "Empirical Health"
source_id: "yc-empirical-health-news-import-485d82c6bcdb"
canonical_url: "https://www.empirical.health/blog/biological-aging-early-cancer-risk/"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-21T18:04:02.816963+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:fd220c752e4bc652edcd615f19fd6b65c5bb50e241ee924da1cb90cc5e6c9805"
---

# Younger generations are aging faster biologically, raising early cancer risks

# Younger generations are aging faster biologically, raising early cancer risks


[Brandon Ballinger](https://www.empirical.health/blog/author/bballinger) ·


Jun 22, 2026


Early-onset cancer rates have increased 24% in the last 30 years \[1\]. Nobody knows why for sure, but a[June 2026 Nature Medicine study](https://doi.org/10.1038/s41591-026-04448-w) tested the hypothesis that cancer is rising because younger generations are aging faster biologically.


The researchers calculated biological age (e.g., PhenoAge) for different generations, and found younger generations have a larger gap between biological and chronological age. They then used the resulting age gap to predict risk of early onset cancer. Each standard deviation in age gap was associated with an 8% higher risk of cancer overall. Most of the increase in risk was concentrated in a few types of cancers, namely lung, GI, colorectal, and uterine.


This post walks through how the researchers set up the study and what data sources they used, what biomarkers go into a biological age score like PhenoAge, why the gap is widening with each generation, and how a bigger gap tracks with early cancer risk independent of your genes.


## The study used two large cohorts, Biobank and All Of Us


The researchers analyzed 154,169 UK Biobank participants who were under 55 when they enrolled, then followed them forward through national cancer and death registries to count who developed an early-onset solid cancer. They repeated the whole analysis in 10,262 people from the US All of Us Research Program to check that the pattern held in a second, more diverse population. Both cohorts include rich blood chemistry, multi-omics profiles, and long-term outcomes, which is what makes a question like this answerable in the first place. (Biobank and All of Us are large, government-sponsored studies where the data set is available to many researchers.)


*The study compared four biological aging clocks across two biobanks, then linked each to early-onset cancer risk. Source:[Nature Medicine 2026](https://doi.org/10.1038/s41591-026-04448-w) .*


The main measure of biological age was PhenoAge, but the researchers confirmed the findings with other biological aging clocks (Klemera-Doubal, a metabolomic clock, and a proteomic approach that scores aging organ by organ).


## What is PhenoAge?


PhenoAge estimates how old your body looks based on routine blood work. PhenoAge was trained to predict mortality, so a high PhenoAge means your blood chemistry resembles that of people who tend to die sooner (sorry, this whole field is sometimes a bit morbid). PhenoAge is calculated from nine standard lab values:


- [Albumin](https://www.empirical.health/tests/albumin) , a marker of liver function and nutrition
- [Alkaline phosphatase](https://www.empirical.health/tests/alkaline-phosphatase) , a liver and bone enzyme
- [Creatinine](https://www.empirical.health/tests/creatinine) , which reflects kidney filtration
- [C-reactive protein](https://www.empirical.health/tests/hs-crp) , a measure of inflammation
- [Glucose](https://www.empirical.health/tests/glucose) , reflecting blood sugar control
- [Mean cell volume](https://www.empirical.health/tests/mcv) and[red cell distribution width](https://www.empirical.health/tests/rdw) , two red blood cell measures
- White blood cell count and[lymphocyte](https://www.empirical.health/tests/lymphocytes) percentage, which track immune status


Your “age gap” is just this biological estimate minus your real age. For example, a 40-year-old with the blood profile of a typical 45-year-old has a 5-year gap.


## Younger generations have a bigger age gap


When the researchers plotted age gap against birth year, the line climbs steadily:


*Biological age gap rises with each successive birth cohort, fastest in men. Same source above.*


This result *does* line up with other things we already knew were shifting earlier in life: puberty, obesity, type 2 diabetes, and even stroke are all arriving younger in recent cohorts.


## Biological age gap predicts cancer risk


So what does the age gap say about early onset cancer? Each standard-deviation increase in PhenoAge gap was associated with 8% higher risk of cancer overall (in other words, a hazard ratio of 1.08). Cancer risk was concentrated in a handful of parts of the body, mostly lung, GI, colorectal, and uterine:


Cancer site Risk per 1 s.d. older biological age 95% CI


All solid cancers 1.08 1.03-1.13


Lung 1.57 1.24-1.97


Gastrointestinal (all) 1.17 1.06-1.30


Colorectal 1.14 1.01-1.29


Uterine 1.31 1.04-1.66


## The effect isn’t just genetics


An obvious objection is that some people are simply born with genes that age them faster and raise cancer risk, and the age gap is just a readout of that inheritance. The researchers tested this hypothesis directly. They adjusted for polygenic risk scores for both longevity and for lung, colorectal, and endometrial cancer, and separately for leukocyte telomere length, a classic genetic marker of aging. The associations barely moved. So whatever the age gap is capturing, it’s mostly the accumulated imprint of how you’ve lived and what you’ve been exposed to.


## Organ-specific aging and cancer


The team also scored aging one organ at a time using blood proteomics, which surfaced links which make some biological sense. An aging immune system was associated with an 89% higher chance of early-onset lung cancer. Aging fat tissue was associated with a 60% higher chance of early-onset colorectal cancer. Both fit known biology: chronic airway inflammation and immune remodeling in the lung, and the metabolic and inflammatory crosstalk between fat tissue and the gut.


*Immune aging stands out for lung cancer; fat-tissue aging stands out for colorectal cancer. Stars mark statistically significant associations. Source:[Nature Medicine](https://doi.org/10.1038/s41591-026-04448-w)*


These organ-specific results are the study’s most exploratory part. The proteomic samples were smaller (this technique is new) and partly overlapped the main analysis, so the authors flag them as hypotheses to validate rather than settled findings.


## Caveats: studies like these are observational


As with many studies, this is observational work. So it shows association, not proof that a faster clock causes cancer. Residual confounding is always possible, the case counts for individual sites are small, and both cohorts skew heavily white and British or American, which limits how far the numbers generalize. So an honest takeaway is that biological age gap is a promising marker that ties a messy set of generational exposures into one number.


## Footnotes


\[1\] “Early onset” = before age 50


## Get your free 30-day heart health guide


Evidence-based steps to optimize your heart health.
