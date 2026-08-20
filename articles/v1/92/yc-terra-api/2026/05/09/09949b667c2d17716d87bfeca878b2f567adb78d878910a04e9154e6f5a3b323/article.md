---
schema_version: "1.0.0"
document_id: "09949b667c2d17716d87bfeca878b2f567adb78d878910a04e9154e6f5a3b323"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/boring-is-fast"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:16a57ef7504f1083bad5b65c07c6529454e67bbbef2e8b0c3a552482c5d48984"
---

# Data Shows Most Runners Don't Actually Get Faster | Terra

## The Findings: What Running Training Data Reveals


1. Wearable data can detect aerobic fitness shifts — no lab required


By comparing matched heart rate bins within each runner across time, I can quantify whether someone is running faster at the same cardiac cost. Across 641 users with a median observation window of 3.5 years, the method produces stable, interpretable estimates.


This matters commercially: any product wanting to show users their fitness is changing can do it with speed and heart rate alone.


1. At population level, average improvement is close to zero — and that's the point


The median speed change at matched heart rates was +0.046 km/h. When I controlled for season, comparing the same calendar quarters year-on-year to remove the effect of summer fitness vs winter detraining, it dropped to +0.002 km/h. Roughly 50% of runners improved, 50% didn't.


This might look like a disappointing result. It's actually the opposite; it's the strongest evidence that the method is measuring something real.


If the speed-HR approach were picking up noise or systematic drift, device calibration shifting over time, seasonal artefacts, measurement error compounding, I'd expect a consistent bias in one direction. Everyone would appear to "improve" or "decline" together. Instead, I see an almost perfect 50/50 split with a median barely distinguishable from zero. That's exactly what a general population of recreational runners should look like. Most people maintain fitness. They don't dramatically gain or lose it.


The validation comes in the next step. When I stratify by training behavior, the groups separate. Dedicated runners improve, Casual runners don't, Intensity-Focused runners trend negative. If this were just noise, slicing by phenotype wouldn't produce different outcomes.


The fact that it does, and in the physiologically expected direction, is what turns a flat average into a credible method.


***Figure 2:** Distribution of speed change. **** The distribution of individual speed-HR improvements is wide and centered almost exactly on zero, with roughly half of runners improving and half declining. This near-perfect symmetry is evidence the method is capturing real variation, not systematic noise.*


1. The individual variation is enormous


The interquartile range spans nearly 0.9 km/h (−0.39 to +0.50). Some runners improved by over 2 km/h at the same heart rate; others declined by the same margin. The population average is a poor summary of a dataset this spread. The real question isn't whether improvement happens — it's for whom.


1. Three distinct training phenotypes emerge from the data


I used k-means clustering on four training dimensions — monthly volume, distance, consistency, and intensity — to identify natural groupings. Three phenotypes emerged:


- Casual runners (n=413, 64%): ~4 runs per month, 30 km/month, high month-to-month variability (CV=0.63). The majority of the dataset.
- Dedicated runners (n=197, 31%): ~12 runs per month, 120 km/month, low variability (CV=0.37). Steady, committed, moderate-intensity.
- Intensity-Focused runners (n=31, 5%): ~9 runs per month, 63 km/month, but 74% HR reserve and 45% of sessions at high intensity. They push hard.


1. Dedicated runners are the ones who improve


The Dedicated group showed a median speed-HR improvement of +0.100 km/h, with 56% improving. Significantly better than Casual runners (p=0.006, Mann-Whitney U). Their defining training feature isn't pace or intensity. It's reliability: 12 runs a month at moderate effort with minimal dropout.


To look at this another way, at a given heart rate, Dedicated runners were running 0.1 km/h faster by the end of the observation period than at the start — about 3–4 seconds per kilometer — without their heart working any harder.


***Figure 3:** Phenotype violin plots **.** Dedicated runners, consistent, moderate-intensity, high volume, show the clearest positive shift in their speed-HR curve, while Intensity-Focused runners trend negative despite running more than the Casual group.*


1. Intensity without volume trends negative


The Intensity-Focused group — who run at 74% HR reserve with nearly half their sessions at high intensity — showed a median decline of −0.090 km/h. There are two caveats. First, this is a small group (n=31), so I should hold the findings lightly. Second, I haven't formally controlled for volume in the phenotype comparison — the Intensity-Focused group differs from Dedicated runners on both intensity and total mileage simultaneously, so I can't cleanly isolate which factor drives the decline.


That said, there's a natural experiment embedded in the data that's worth noting. The Intensity-Focused group runs considerably more than the average Casual runner — 63 km per month versus 30 km — yet their speed-HR curve moves in the wrong direction. When I compared them to Casual runners running the same mileage, those low-intensity runners at equivalent volume improved by a median of +0.070 km/h; the Intensity-Focused group declined by −0.090 km/h. The direction is suggestive: it's not simply that they don't run enough. In the continuous variable analysis across all 641 users, high-intensity fraction showed a negative coefficient after controlling for volume, though the sample is too small for statistical confidence.


This aligns with what I found in my earlier marathon research, where athletes training with excessive time at threshold pace tended to run slower on race day, not faster. Two different methods, same direction. It's also coherent with established exercise physiology: high-intensity work improves anaerobic capacity and race-specific speed, but shifting the aerobic efficiency curve appears to require volume.


There is clearly a lot going on here, both with the physiology and other factors. I think it's a safe assumption that the high-intensity-focused runners are at the greatest injury risk, which would have a significant impact on the analysis.


1. There's a clear dose-response for volume


I split the 641 runners into five equal groups by monthly run frequency. The bottom quintile (2.3 runs/month) saw 47% improve. The top quintile (14.5 runs/month) saw 64% improve — a 17-percentage-point gap. The relationship is roughly linear across quintiles and statistically significant (r=0.097, p=0.014).


***Figure 4:** Volume quintile bars **.** Runners in the highest training volume quintile (14.5 runs per month) are 17 percentage points more likely to improve than those in the lowest (2.3 runs per month). The dose-response is roughly linear: each step up in volume shifts the odds measurably in your favor.*


1. Controlling for baseline fitness makes the training signal stronger, not weaker


Better runners have less room to improve — this is a well-known ceiling effect, and I needed to check whether it was confounding the phenotype comparison. Dedicated runners do start faster at baseline (10.7 km/h, around 5:36 per kilometer) compared to Casual runners (9.7 km/h, 6:09 per kilometer). The ceiling effect is real: baseline speed correlates negatively with improvement (r=−0.30, p<0.0001), meaning faster runners as a group improve less, as expected.


But Dedicated runners improve more despite this working against them. When I controlled for baseline speed, the volume and consistency effects didn't disappear — they roughly doubled:


Variable Raw r Controlling for baseline


Runs per month 0.090 **0.188** (p<0.0001)


km per month 0.080 **0.212** (p<0.0001)


Consistency (CV) −0.093 **−0.154** (p<0.0001)


Intensity fraction −0.001 **0.034** (p=0.39)


The within-quartile comparison confirms this is about training behavior, not pre-existing fitness. Among runners in the same baseline speed band (9.1–10.1 km/h), Dedicated runners improved by a median of +0.230 km/h while Casual runners improved by just +0.032. In the next quartile up (10.1–11.0 km/h), Dedicated runners stayed positive (+0.046) while Casual runners declined (−0.214). The training behavior signal is present at every fitness level.


1. Consistency predicts improvement at least as well as volume


Month-to-month variability in training — measured as the coefficient of variation in monthly run count — was one of the most robust predictors of speed-HR improvement, with a partial correlation of r=−0.154 (p<0.0001) after controlling for baseline fitness. Runners with low variability (CV below 0.4) clustered above zero; those with erratic patterns (CV above 0.7) trended negative.


In plain terms, it's not just how much you run. It's whether you keep showing up. It is the single most durable finding in this analysis.


**Figure 5:**


Consistency scatter. ****


Month-to-month variability in training frequency (lower = more consistent) predicts speed-HR improvement more robustly than any other variable tested.


1. Intensity alone doesn't predict improvement


Neither before nor after controlling for baseline fitness does high-intensity session fraction predict speed-HR improvement (raw r=−0.001, p=0.98; partial r=0.034, p=0.39). The signal in this dataset is in volume and consistency, not in how hard individual sessions are. This doesn't mean intensity is useless — it almost certainly matters for race-specific fitness and anaerobic capacity — but for shifting the aerobic efficiency curve, the data points elsewhere.


1. This holds up no matter how you slice the data


I tested five different temporal splits: halves, thirds, quarters, fifths, and tenths. Each compares a progressively narrower early window to a progressively narrower late window.


Split N users Median shift Consistency r (p) Q5 vs Q1 improved


Halves 641 +0.024 −0.123 (0.002) 63% vs 47%


**Thirds** **641** **+0.042** **−0.121 (0.002)** **64% vs 47%**


Quarters 641 +0.013 −0.108 (0.006) 60% vs 44%


Fifths 638 +0.029 −0.087 (0.027) 56% vs 45%


Tenths 584 +0.014 −0.089 (0.032) 57% vs 47%


Thirds produced the highest signal-to-noise ratio, but the story is identical across all five: consistency predicts improvement in every split. The Dedicated phenotype always improves most. The Q5-vs-Q1 dose-response always holds. As windows narrow, noise increases — fewer runs per window means less stable curve estimates — but the direction never changes.


This is the most methodologically reassuring finding in the analysis. The conclusions aren't an artefact of how I chose to divide the data.


## What This Means For Runners, Coaches, and The Wearable Industry


For runners: The data makes a case for the oldest advice in the sport. Don't do anything dramatic. Don't chase every session at threshold. Build a routine you can sustain month after month, and the aerobic system will respond — slowly, quietly, measurably. The Dedicated phenotype's median improvement of +0.100 km/h translates to roughly 3.5 seconds per kilometer. Over a marathon, that's about two and a half minutes. Not from a training camp or a magic session — from simply not stopping.


For the wearable industry: Speed-HR curves derived from consumer devices can detect real physiological adaptation at population scale. The method is crude compared to laboratory testing, but at n=641 with longitudinal data, the signal is there. Spathis et al. (2022) showed that free-living wearable data can predict VO₂max across 11,000 participants with r=0.82 — but required chest-worn accelerometers and deep learning infrastructure. The signal I've found here is less precise but requires only the pace and heart rate data already present on any GPS watch, which makes it far more scalable in practice. I am currently exploring how to incorporate this methodology into a live fitness tracking model, available directly via the Terra dashboard.


For coaches: The consistency finding deserves attention. In a dataset where I tested five independent temporal splits, month-to-month training variability was the only variable that predicted improvement in every single one. Volume mattered, but only robustly in the wider splits. Consistency — the boring, unsexy, "just keep showing up" variable — seems bulletproof.


### Limitations of Wearable Heart Rate Data


This is observational data. Fitter people may choose to run more, rather than running more making them fitter. I can't separate cause from effect.


Heart rate data from wrist-based optical sensors has known limitations, particularly at higher intensities where motion artefact increases. I cleaned aggressively, but some noise will remain.


I'm assuming runners perform broadly similar types of runs throughout the observation period. In reality, someone might shift from road to trail, or begin training on hillier routes — both of which would affect the HR-pace relationship independently of any fitness change. With session-level averages, I can't detect this.


"Early" versus "late" periods are defined by each user's own timeline, not a fixed calendar window. The median span is 3.5 years, but individual windows vary from 6 months to over 4 years.


The Intensity-Focused phenotype (n=31) is too small for confident conclusions. The negative trend is suggestive, not definitive.


I measured a proxy for aerobic efficiency only, one dimension of fitness. Speed work, hill strength, race tactics, and mental resilience don't show up in a speed-HR curve. A runner who "declined" by this metric may have improved in ways this method can't capture.


### Methodology: Speed-Heart Rate Curves and Exercise Physiology


Our dataset consists of 856,000 running activities. These activities were filtered for valid speed and heart-rate ranges, then narrowed to a longitudinal cohort with sufficient training history for matched speed–heart rate comparisons across early and late training periods.


For the Speed-HR methodology I used heart rate binned in 5 bpm windows. Median speed computed per bin in each period. Speed change = median of (late speed – early speed) across matched bins. Season-controlled replication compared the same calendar quarters across different years.


Phenotyping: K-means clustering (k=3) on standardized features: runs/month, km/month, monthly CV, and, where available, I used median HR reserve fraction and high-intensity session proportion. Clusters validated by silhouette analysis and manual inspection of training profiles.


Lastly, for the robustness: all core findings replicated across five temporal splits (halves through tenths). Thirds selected as primary analysis based on peak signal-to-noise ratio. Cross-split directional agreement ranged from 79% (thirds vs tenths) to 89% (thirds vs quarters).


#### References


1. Schimpchen J, Freitas Correia P, Meyer T. Minimally invasive ways to monitor changes in cardiocirculatory fitness in running-based sports: a systematic review. International Journal of Sports Medicine. 2023;44(2):95–107. doi:[10.1055/a-1925-7468](https://doi.org/10.1055/a-1925-7468)
2. Buchheit M. Monitoring training status with HR measures: do all roads lead to Rome? Frontiers in Physiology. 2014;5:73. doi:[10.3389/fphys.2014.00073](https://doi.org/10.3389/fphys.2014.00073)
3. Spathis D, Perez-Pozuelo I, Gonzales TI, Wu Y, Brage S, Wareham N, Mascolo C. Longitudinal cardio-respiratory fitness prediction through wearables in free-living environments. NPJ Digital Medicine. 2022;5:176. doi:[10.1038/s41746-022-00719-1](https://doi.org/10.1038/s41746-022-00719-1)
