---
schema_version: "1.0.0"
document_id: "01a0b44e126e04faee545fa4f3bc6bd33037e6fddc8cd373d3993e9496b00e80"
company_key: "yc-empirical-health"
company: "Empirical Health"
source_id: "yc-empirical-health-news-import-485d82c6bcdb"
canonical_url: "https://www.empirical.health/blog/blood-tests-that-predict-heart-attack-risk/"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-21T18:04:02.816963+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:bb16cdc59b60fe9aec0a3add4a4a5b10195fa19becf69732c4038b21153dd865"
---

# Blood tests that predict heart attack risk

# Blood tests that predict heart attack risk


[Brandon Ballinger](https://www.empirical.health/blog/author/bballinger) ·


Jun 18, 2026


99% of heart attacks are preceded by a known risk factor: cholesterol (or ApoB), blood pressure, HbA1c, kidney function (eGFR), Lp(a), and inflammation (hs-CRP). The American Heart Association’s[PREVENT equations](https://www.ahajournals.org/doi/10.1161/CIRCULATIONAHA.123.067626) use the first four numbers as inputs. Lp(a) and hs-CRP are recommended in 2025 and 2026 guidelines, but are layered on top of the base model as a risk enhancer (in this blog post, we’ll call this layered approach PREVENT++).


This post covers six tests that predict heart attack risk: blood pressure, eGFR, and HbA1c, Lp(a), hs-CRP, and ApoB. For each one, we’ll define what it is, give the statistics on how it modifies cardiovascular risk, and describe how it fits into the heart attack equations (either in the base layer or as an enhancer).


*Six blood tests mapped to the PREVENT equation and its risk-enhancer layer.*


## How the PREVENT equations predict heart attack risk


PREVENT estimates your 10-year and 30-year risk of cardiovascular disease. It’s valid for ages 30-79, is race-free, and predicts three outcomes that older calculators split apart (heart attack, stroke, and heart failure). PREVENT’s main inputs are age, sex, non-HDL and HDL cholesterol, blood pressure (systolic), eGFR, diabetes, smoking, and whether you take blood-pressure or statin medication. It can optionally incorporate HbA1c. PREVENT’s accuracy (c-statistic) is 79% in women and 76% in men.


PREVENT doesn’t explicitly include Lp(a), a genetic risk factor a quarter of people carry. It also doesn’t include hs-CRP, an inflammation marker, which is an independent risk factor. Rather than ApoB, PREVENT uses non-HDL cholesterol. To incorporate those, the best way is to run PREVENT, then adjust for the risk enhancers.


Now, we’ll go through each test that predicts heart attack risk.


## How blood pressure predicts heart attack risk


Systolic blood pressure is one of the strongest heart attack risk predictors. Each 20 mmHg rise in systolic pressure roughly[doubles](https://pubmed.ncbi.nlm.nih.gov/12493255/) the risk of dying from heart disease (this is true across all ages). Blood pressure varies most from reading to reading,[swinging 20 mmHg](https://www.empirical.health/blog/blood-pressure-variability/) day to day. That’s why a it helps to have a blood pressure cuff at home, take multiple measurements, and average them.


## eGFR: kidney function & heart health


eGFR estimates the volume of blood that your kidneys filter. Adding eGFR was one of the largest changes for PREVENT. The reason is that kidney decline tracks cardiovascular decline closely enough to predict it. With an eGFR below 45, cardiovascular risk roughly[doubled](https://www.nejm.org/doi/full/10.1056/NEJMoa041031) . With an eGFR below 30, cardiovascular risk nearly tripled. This is independent of other risk factors like cholesterol and blood pressure.


## HbA1c: metabolic health & heart health


HbA1c reflects your average blood sugar over about three months. PREVENT offers a version that takes it directly, with separate coefficients depending on whether you already have diabetes. Prediabetes (an HbA1c of 5.7 to 6.4%) is linked to[roughly 15% higher cardiovascular risk](https://www.bmj.com/content/370/bmj.m2297) . It’s also linked to higher all-cause mortality than normal blood sugar.


## ApoB, LDL, and non-HDL cholesterol


ApoB is essentially a more accurate version of LDL cholesteorl. ApoB counts the number of artery-clogging particles in your blood, whereas LDL cholesterol measures the mass of those particles. The two often agree, but not always. Across[1,890 Empirical Health blood draws with LDL and ApoB measured the same day](https://www.empirical.health/blog/do-modern-ldl-formulas-replace-apob/) , 1 in 12 people had a normal LDL but a high ApoB. The[2025 National Lipid Association consensus](https://pmc.ncbi.nlm.nih.gov/articles/PMC11734832/) now treats ApoB as the more accurate marker than LDL or non-HDL cholesterol.


PREVENT uses non-HDL cholesterol rather than ApoB, mostly because it was developed on historical cohorts. The AHA’s risk-enhancer threshold for ApoB is 130, so that is a reasonable place ot flag.


## Lp(a) and heart attack risk


Lp(a) is the risk factor that only 0.24% of people test, but which is now recommended for everyone in the AHA’s March 2026 guidelines. Lp(a) an LDL-like particle wrapped in an extra protein, set almost entirely by your genes, and about[6x more atherogenic than ordinary LDL](https://pmc.ncbi.nlm.nih.gov/articles/PMC11437815/) . 1 in 4 people have elevated ApoB.


Cardiovascular risk rises about 11% for every 50 nmol/L of Lp(a). Because PREVENT doesn’t include Lp(a), we apply the modifier from the[2025 ESC/EAS guidelines](https://pubmed.ncbi.nlm.nih.gov/41706264/) , drawn from[UK Biobank](https://www.ahajournals.org/doi/10.1161/atvbaha.120.315291) . PREVENT stays reasonably[calibrated](https://pubmed.ncbi.nlm.nih.gov/40465279/) across Lp(a) levels, with adding Lp(a) producing a modest but real reclassification.


## hs-CRP and inflammation


High-sensitivity C-reactive protein measures inflammation, now understood as an independent risk factor for heart disease. The[Reynolds Risk Score](https://jamanetwork.com/journals/jama/fullarticle/205528) folded hs-CRP into cardiovascular prediction years ago, with risk rising about 12% per doubling of CRP. We carry that same coefficient forward as a PREVENT modifier.


## The six tests at a glance


Test What it measures Role in the model Key statistic Source


Blood pressure Arterial pressure PREVENT input +20 mmHg systolic ≈ 2× cardiac death[Lewington 2002](https://pubmed.ncbi.nlm.nih.gov/12493255/)


eGFR Kidney filtration PREVENT input Events ~2× below eGFR 45, ~3× below 30[Go 2004](https://www.nejm.org/doi/full/10.1056/NEJMoa041031)


HbA1c 3-month blood sugar PREVENT input (optional) Prediabetes ≈ +15% CVD risk[Cai 2020](https://www.bmj.com/content/370/bmj.m2297)


ApoB Atherogenic particle count Companion target −10 mg/dL ≈ −9% risk[NLA 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC11734832/)


Lp(a) Inherited LDL-like particle PREVENT+ modifier +11% risk per 50 nmol/L[Patel 2021](https://www.ahajournals.org/doi/10.1161/atvbaha.120.315291)


hs-CRP Inflammation PREVENT+ modifier +44% event reduction on statin if elevated[JUPITER 2008](https://www.nejm.org/doi/full/10.1056/NEJMoa0807646)


## How much do Lp(a) and hs-CRP change your number


The modifiers are multiplicative, so they compound. For example, here are numbers for a 50-year-old whose PREVENT 10-year risk comes back at 7.5%, with an Lp(a) of 150 nmol/L and an hs-CRP of 3 mg/L:


*Examples of how Lp(a) and hs-CRP modifiers change risk of the undelrying PREVENT equations.*


## How to test all six heart attack risk predictors


Empirical’s[heart health panel](https://www.empirical.health/product/heart-health-panel/) runs all six and feeds them through PREVENT and its risk-enhancer modifiers, so the number you see already accounts for the tests the standard equation leaves out. For the full picture of what to measure and why, see our guide to the[best biomarkers for heart disease](https://www.empirical.health/blog/best-biomarkers-heart-disease/) .


## Get your free 30-day heart health guide


Evidence-based steps to optimize your heart health.
