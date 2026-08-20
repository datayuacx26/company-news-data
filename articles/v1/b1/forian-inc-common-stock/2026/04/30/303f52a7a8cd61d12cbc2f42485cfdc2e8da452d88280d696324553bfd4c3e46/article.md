---
schema_version: "1.0.0"
document_id: "303f52a7a8cd61d12cbc2f42485cfdc2e8da452d88280d696324553bfd4c3e46"
company_key: "forian-inc-common-stock"
company: "Forian Inc."
source_id: "forian-inc-common-stock-rss-1c01cf5ce14b"
canonical_url: "https://blog.forian.com/reconstructing-cidp-patient-journey-hybrid-claims-data"
published_at: "2026-04-20T20:42:08+00:00"
first_seen_at: "2026-07-20T23:19:46.527638+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:a2040486705f1c0bbd4901bc711503a4a48514396fa0680d1f00c8d9746e4341"
---

# Reconstructing the CIDP Patient Journey: What Hybrid Claims Reveal

Chronic Inflammatory Demyelinating Polyradiculoneuropathy (CIDP) is not an easy disease to study. It’s rare. It’s heterogeneous. And perhaps most importantly, it’s often diagnosed late, after a long and complex clinical journey that can involve multiple providers, misdiagnoses, and fragmented care. For researchers seeking to understand real-world patient experiences, these characteristics pose a fundamental challenge: the data often fail to tell the full story.


Traditionally, closed claims data has been the foundation for this kind of work. It offers structure, longitudinal enrollment, and a clean analytic framework. But it also comes with a tradeoff, one that becomes particularly apparent in rare diseases like CIDP. By requiring continuous enrollment in a single plan, closed claims inherently filter the population, favoring patients with stable insurance coverage within the healthcare system.


The question is: What sort of CIDP patients are we missing within closed claims that reveal themselves in hybrid data?


*This blog is part of a series on hybrid claims data and real-world evidence. Read the first post*[here](https://blog.forian.com/hybrid-claims-data-essential-strategy-rwe-heor-studies) *.*


### **A More Representative CIDP Population**


In this case study, we compared CIDP patients identified using closed claims alone with those identified through a hybrid approach that integrates open claims data with methods to approximate continuous activity over time.


The first and most obvious difference is scale. In[CHRONOS](https://forian.com/chronos/) , incorporating open claims i


**ncreased the identifiable CIDP population by 10x** (closed n=923 vs hybrid n=9,289) after applying strict lookback and follow-up activity business rules to confirm their diagnosis within claims.


But this isn’t just about adding patients; it’s about correcting bias.


Gender distribution shifts notably between the closed and hybrid groups. While closed claims skew more male (1.26:1), hybrid cohorts move closer to parity, approaching a ~50/50 male–female split across several patient deciles. At the same time, the hybrid population skews older,


**with a median age 2.5 years older than in closed** , reflecting increased capture of Medicare-eligible patients and those with less stable coverage histories.


This shift points to a critical missingness that occurs when looking at closed claims alone. The hybrid cohort captures individuals with Medicare coverage, intermittent insurance, or fragmented care pathways, bringing the dataset closer to the real-world population that clinicians actually see.


## **Heterogeneity as a Feature, not a Flaw**


One of the immediate reactions to hybrid data is that it looks “messier.” Distributions widen. Variability increases. The cohort becomes more heterogeneous.


In our CIDP population, this is clearly visible. Compared to closed claims, hybrid cohorts show broader dispersion across demographic deciles, particularly in older age bands and across sex distribution.


Rather than introducing noise, this reflects a more complete representation of the disease.


Closed claims compress variability by excluding patients with fragmented care or intermittent coverage. Hybrid data restores that variation, capturing patients at different stages of disease progression, with different access points into the healthcare system.


For rare diseases, that heterogeneity is not a limitation. It’s essential context.


##


## **Reconstructing the Patient Journey Through Visit Rates**


To better understand how these differences translate into care patterns, we examined healthcare utilization in the year before and after CIDP diagnosis. Visit rates were calculated on a per-patient-per-month basis and stratified by sex, region, and provider specialty.


Across every stratification, visit rates were consistently higher in the hybrid cohort.


For example, neurology visit rates post-diagnosis were approximately


**20–30%** higher in hybrid data compared to closed claims alone, indicating more complete capture of specialty care. This pattern held across regions, suggesting that the effect is not driven by a single geography or care model.


Similarly, when looking at total all-cause visits, hybrid cohorts showed


**double-digit percentage**


**increases** in PPPM rates, both before and after diagnosis. These differences were consistent across male and female populations, reinforcing that the effect is systemic rather than subgroup-specific.


The takeaway is not simply that utilization is higher, but that it is more fully observed.


##


## **The Overlooked Importance of the Pre-Diagnosis Period**


Some of the most meaningful differences appear in the year prior to diagnosis.


In the hybrid cohort, visit rates in the pre-index period were consistently higher, often by


**15–25% or more** , depending on stratification, compared to closed claims alone. This pattern was observed across regions and sexes.


This aligns with what we know clinically: CIDP diagnosis is often preceded by a prolonged and complex diagnostic journey. Patients may cycle through primary care, emergency settings, and multiple specialists before reaching a definitive diagnosis.


Closed claims compress this journey. Hybrid data expands it. For life sciences teams, this difference is critical. The pre-diagnosis period represents an opportunity for earlier identification, improved referral pathways, and more timely intervention.


**More Data, More Power, More Confidence**


From a methodological standpoint, the benefits of larger sample sizes are straightforward. Expanding the cohort increases statistical power, enabling more robust subgroup analyses and improving confidence in observed effects.


But what stands out in this analysis is that hybrid data delivers more than just power; it improves validity. The differences observed are consistent in direction and magnitude across stratifications. Effects on the order of 15–30% increases in observed utilization, combined with expanded cohort size, would be expected to meet conventional thresholds for statistical significance in large claims datasets.


More importantly, they align with clinical expectations. That alignment suggests that hybrid data is not introducing distortion, but correcting for it.


**Moving Beyond Convenience Samples**


Closed claims data has long been the standard because it is structured and convenient. But convenience comes at a cost, particularly in rare diseases, where every exclusion matters.


Hybrid claims approaches challenge that paradigm. By expanding the observable population and reconstructing longitudinal activity, they allow us to move beyond convenience samples toward datasets that better reflect real-world complexity.


For CIDP, that means not just identifying more patients, but understanding them more completely, their demographics, their care pathways, and their interactions with the healthcare system over time.


And in[rare disease research](https://blog.forian.com/hybrid-claims-game-changer-rare-disease-research) , that difference is not incremental; It can fundamentally change study outcomes.
