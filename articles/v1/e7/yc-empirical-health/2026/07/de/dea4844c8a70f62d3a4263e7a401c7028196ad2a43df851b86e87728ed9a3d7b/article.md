---
schema_version: "1.0.0"
document_id: "dea4844c8a70f62d3a4263e7a401c7028196ad2a43df851b86e87728ed9a3d7b"
company_key: "yc-empirical-health"
company: "Empirical Health"
source_id: "yc-empirical-health-news-import-485d82c6bcdb"
canonical_url: "https://www.empirical.health/blog/half-of-us-adults-statin-new-cholesterol-guidelines/"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T18:30:30.427304+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:e2032795fba9bf74cd00c0da82509f73b0ba4f67d26872fdd654ca2e844917ad"
---

# 57% of US adults recommended a statin under new guidelines

# 57% of US adults recommended a statin under new guidelines


[Brandon Ballinger](https://www.empirical.health/blog/author/bballinger) ·


Jul 24, 2026


A cardiologist I worked with once joked that statins are so effective that we should just put them in the water supply. He might be getting his wish. The[2026 cholesterol guidelines](https://www.empirical.health/blog/cholesterol-guidelines-heart-health/) recommend earlier treatment, and a[new study out today](https://jamanetwork.com/journals/jama/article-abstract/2851792?utm_source=BulletinHealthCare&utm_medium=email&utm_term=072126&utm_content=MEMBER&utm_campaign=article_alert-morning_rounds_daily&utm_uid=6156383&utm_effort=DAMR01) estimates that 57% of US adults would be recommended a statin under the new guidelines.


I’m a bit split. On the one hand, it can feel like obvious over-medicalization to put half of society on a prescription drug. On the other hand, biomedical science has advanced to the point where it’s conceivable to eradicate heart disease: we can predict risk 30 years in advance, have biomedicines that[cut cholesterol by 80% or more](https://www.empirical.health/blog/statins-vs-pcsk9-inhibitors) , including an[oral PCSK9 inhibitor](https://www.empirical.health/blog/oral-pcsk9-inhibitor-lipfendra) approved just last week, and novel biomarkers let us refine risk prediction beyond traditional LDL cholesterol. At some level, it becomes silly to let a preventable health condition make an abattoir of society when we have the biomedical tools to solve it.


For the rest of this article, I’ll review some of the science underlying why medical societies are recommending heart disease treatment start earlier than before. We’ll start with mechanisms, then go into statistical models and biomarkers, new drugs (including statin alternatives), and wrap up with some society-wide implications.


## Cholesterol exposure works like cigarette pack-years


If you smoke a pack of cigarettes, you don’t get lung cancer that same afternoon. Risk comes from dose times years, which is why oncologists count cigarette exposure in terms of pack-years. We’ve increasingly learned that cholesterol behaves the same way. ApoB-containing particles (LDL, VLDL remnants, and Lp(a)) cross the artery wall, deposit cholesterol, and the deposits grow over the course of decades. As a result, what predicts a heart attack is the area under your LDL curve.


Cardiologists have started formally calling this measure[“cholesterol-years”](https://www.jacc.org/doi/10.1016/j.jacc.2020.08.004) . For example, a personw ith an average LDL of 125 mg/dL for 40 years has accumulated 5,000 mg/dL-years. The estimate is that roughly 5,000 mg/dL-years is[the threshold](https://www.nature.com/articles/s41569-024-01039-5) at which enough plaque has accumulated for events to start happening.


At an LDL of 100 mg/dL, you’d cross that line around age 50. At 160 mg/dL, you’d cross it around age 31. We know this theory is true in part due to autopsies of people aged 15-34 who died of accidents, homicides, and suicides. Those autopsies show raised fatty streaks in the right coronary arteries for about 10% of 15-to-19-year-olds and about 30% of 30-to-34-year-olds.


*Cross-sections are schematic. Lesion prevalence from[PDAY](https://pubmed.ncbi.nlm.nih.gov/10052443/) .*


Serial CT angiography finds the same pattern in living people.[NATURE-CT](https://www.empirical.health/blog/nature-ct-coronary-plaque-progression) scanned 205 adults with low calcium scores. Over 5 years, plaque volume roughly doubled.


*Soft plaque doubled over five years in a non-symptomatic population. Source:[Aldana-Bitar et al., NATURE-CT, 2026](https://www.empirical.health/blog/nature-ct-coronary-plaque-progression) .*


Beyond LDL cholesterol, independent risk factors like[Lp(a)](https://www.empirical.health/blog/lipoprotein-a-blood-test) , blood pressure, and[inflammation](https://www.empirical.health/blog/inflammation-and-heart-health) act as multipliers on the base risk.


## Statistical models now predict risk 30 years out


The old cardiovascular Pooled Cohort Equations answered one question: what’s your chance of a heart attack in the next 10 years? For a healthy 35-year-old, the answer is always “low,” which is why young people were almost never recommended a statin.


The new[PREVENT equations](https://www.empirical.health/blog/heart-attack-risk-calculator) predict both 10-year and 30-year risk. They cover ages 30 to 79 instead of 40 to 75, incorporate kidney function, drop race as an input, and optionally factor in social factors via zip code. The 30-year estimate drives nearly all of the new statin recommendations.


You can see why on the risk curve itself. If you take one person with modestly high cholesterol (non-HDL-C 160, HDL 45), normal blood pressure, no diabetes, and no smoking, and run PREVENT at each age, their 10-year risk stays under 5% until their sixties but their 30-year risk crosses 10% (the level at which medication starts to become recommended) at about age 45.


*The same person is far below the treatment line on 10-year risk and above it on 30-year risk for most of midlife. Computed from the[AHA PREVENT equations](https://www.ahajournals.org/doi/10.1161/CIRCULATIONAHA.123.067626) .*


The[JAMA study](https://jamanetwork.com/journals/jama/article-abstract/2851792) referenced above found 57% of US adults would be recommended a statin for primary prevention, up from 42.7% under the 2018 guidelines, which is 21.5 million more people.


A companion NHANES analysis (a[preprint](https://arxiv.org/abs/2605.10979) from a different group) shows the bulk of the expansion comes in middle aged people: *The 30-year criterion applies only to ages 30–59, and that’s where the added recommendations concentrate. Source:[Diao et al., preprint, 2026](https://arxiv.org/abs/2605.10979) , class 2 threshold.*


Absolute eligibility still climbs with age (about 11% of people in their 30s are recommended a statin, versus 85% in their 60s and 93% in their 70s), but almost everyone in their 70s already qualified under the old rules too. The genuinely new group is middle-aged. The newly eligible are younger and lower-risk than the people who already qualified: mean 10-year risk of 3.1% versus 6.1%. This is the design working as intended, and it’s also the fair objection. A 3.1% ten-year risk means about 97 out of 100 people won’t have an event in the next decade whether or not they take anything, and the case for treating them rests on a 30-year projection that hasn’t directly been tested in clinical trials. (Medication trials run five years. The genetic evidence is the substitute, and it’s good evidence, but it’s inference rather than a randomized answer.)


## New biomarkers refine risk estimates


A 30-year projection built on a standard lipid panel is a 30-year projection built on one of the three biological pathways that cause atherosclerosis. Better inputs are cheap and available now.


- **[ApoB](https://www.empirical.health/blog/apob-vs-ldl-cholesterol)** counts atherogenic particles instead of estimating the cholesterol inside them. In about 20% of people LDL-C reads normal while ApoB is elevated. The 2026 guideline notes that when you assess lipid markers together, “only apoB remains significant” as a predictor of heart attacks.
- **[Lp(a)](https://www.empirical.health/blog/lipoprotein-a-blood-test)** is inherited, stable for life, and untouched by diet or statins. In the[HELIUS cohort](https://www.empirical.health/blog/adding-lpa-hscrp-to-prevent-equations) , the top fifth of Lp(a) carried 1.86× the risk of the bottom fifth, a steeper gradient than LDL-C. About 20% of people have elevated levels, and one test settles it forever.
- **[hs-CRP](https://www.empirical.health/blog/inflammation-and-heart-health)** tracks the inflammatory arm. The top fifth carried 1.51× the risk in the same cohort, independent of cholesterol.
- **eGFR and HbA1c** pull in kidney and metabolic health. PREVENT already uses eGFR, which is part of why it outperforms the equations it replaced.


Since these risk factors are independent, their effects stack rather than overlap. Someone in the top quintile for all three of LDL-C, Lp(a), and hs-CRP had 2.4x the risk of someone elevated on none. One interesting input is blood pressure. It’s still in PREVENT as a single number, but when we analyzed 650,000 connected cuff readings, we found[day-to-day systolic swings of 10 mmHg](https://www.empirical.health/blog/blood-pressure-variability) on average, with 20 mmHg swings about 5% of the time. Conceivably, the continuous blood pressure measurements we’re seeing on wearables in 2026 might refine this further.


Vitamin D is an open question. Vitamin D is consistently associated with cardiovascular risk in observational data. A[2025 trial](https://newsroom.heart.org/news/heart-attack-risk-halved-in-adults-with-heart-disease-taking-tailored-vitamin-d-doses) in people who’d already had a heart attack found that titrating vitamin Ddoses to a target blood level halved repeat cardiovascular events, which is promising, though very preliminary. If that result holds up, vitamin D may earn a place in future risk models.


## New drugs are unreasonably effective


The arsenal of cardiovascular drugs has never been stronger. Beyond statins, we have ezetimibe and PCSK9 inhibitors (including an oral PCSK9 inhibitor approved last week):


*[Bempedoic acid](https://www.empirical.health/blog/apob-lowering-medications) is the option for the roughly 10% of patients with statin-attributed muscle symptoms.[Lipfendra](https://www.empirical.health/blog/oral-pcsk9-inhibitor-lipfendra) , approved July 16, 2026, is the first[PCSK9 inhibitor](https://www.empirical.health/blog/pcsk9-inhibitors-lower-ldl) you can take as a pill.*


Triple therapy (all three) cuts LDL by 80–85%, into the 20s and 30s mg/dL. At that point, you’re not slowing the accumulation of cholesterol-years, you’re essentially stopping the clock entirely.


## Care delivery is still a bottleneck


All of this biomedical innovation counts for little if the “last mile,” care delivery, fails. And unfortunately, 46% of US counties have zero cardiologists, <1% of patients get the optimal medical therapy described above, and the doctor shortage is getting *worse* . Someone has to order the ApoB and Lp(a), interpret a 30-year risk estimate, have a real conversation about a decades-long medication, and then actually follow up on the labs six months later. There aren’t enough cardiologists to do that, and there won’t be in our lifetimes.


This is an interesting role for AI. Cardiovascular prevention is algready “algorithmic”, driven by protocols. Could cardiovascular prevention scale more like software than clinical visits? Obviously we think so here, and Empirical Health is a bet in this direction.


**Know your 30-year risk, not just your cholesterol.** Empirical Health's[heart health panel](https://www.empirical.health/product/heart-health-panel?utm_source=statin_eligibility_blog) measures ApoB, Lp(a), hs-CRP, and standard lipids, and runs your numbers through the PREVENT equations.


## Get your free 30-day heart health guide


Evidence-based steps to optimize your heart health.
