---
schema_version: "1.0.0"
document_id: "14bc17eaf8f4d2b93cdcd3ff9da72f40b030d051f908d04f4ca10a4d0b24c3a4"
company_key: "lam-research-corporation-common-stock"
company: "Lam Research Corporation"
source_id: "lam-research-corporation-common-stock-news-import-3e7486dafa86"
canonical_url: "https://newsroom.lamresearch.com/dont-scrap-it-save-it-feedforward-control-for-modern-semiconductor-manufacturing"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T22:39:01.504089+00:00"
fetched_at: "2026-07-31T22:39:02.256828+00:00"
content_hash: "sha256:f5ad080b5a5c2f34387e4cb6928aa56a389ee0ad4292426cd539156aad06d5d2"
---

# Don't Scrap It, Save It: Feedforward Control for Modern Semiconductor Manufacturing

# Blog


Don't Scrap It, Save It: Feedforward Control for Modern Semiconductor Manufacturing


[Swapnil Kailash More](https://newsroom.lamresearch.com/blog?author=147)


Jul 30, 2026


|


- [Industry](https://newsroom.lamresearch.com/blog?cat=2)
- [Technology](https://newsroom.lamresearch.com/blog?cat=3)


- Print Page


- Email URL


- [RSS Feed](https://newsroom.lamresearch.com/rss?rsspage=34141)


- [Facebook Share Button](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D645)


- [Twitter Share Button](https://twitter.com/share?url=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D645)


- [Linkedin Share Button](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D645)


- **Feedforward control** helps recover wafers before scrap: By measuring each wafer after lithography and adjusting the next etch step, fabs can pull more wafers back into spec.
- Simulation showed a major pass rate lift: Post-etch Cpk improved 2.5x, while pass rate increased from **60.82% to 96.77%** .
- The gain can outweigh throughput tradeoffs: Higher pass rates create headroom to absorb recipe-related slowdowns and still produce more good wafers per unit time.


Semiconductor yield is a constant battle against process variability. Every wafer that falls out of spec represents lost time, lost capacity, and lost value. In advanced semiconductor manufacturing, even small shifts in critical dimensions (CDs) can determine whether a wafer moves forward—or gets scrapped.


That is why process control strategy matters. Open-loop fabrication ignores variation entirely and can result in poor yield, while traditional feedback control reacts only after wafers have already missed spec.


**Feedforward control** takes a more proactive path: it measures each wafer after lithography and adjusts the next etch step to recover wafers before they are lost.


### What Is Feedforward Control?


Feedforward control is a process-control strategy that measures wafers after one manufacturing step and automatically adjusts downstream process parameters to compensate for variability before defects occur.


Feedforward control is emerging as a powerful method for reducing wafer scrap and improving semiconductor yield because manufacturers can recover wafers that would otherwise fall out of specification.


Using SEMulator3D® digital twin simulation, the Semiverse Solutions team evaluated how feedforward control can significantly improve process capability and wafer pass rates.


### Simulation Setup Using SEMulator3D®


To test the effectiveness of feedforward control, we simulated a simple lithography → etch process sequence using SEMulator3D®. Controlled variability is introduced at the lithography step, and the Cpk (process capability index) was measured after etch. We then implemented a feedforward correction at the etch step and checked whether post-etch Cpk improved using this correction.


*Figure 1. SEMulator3D® process flow used to evaluate feedforward control*


Each simulation run represented one wafer; post-lithography and post-etch critical dimensions (CDs) were treated as wafer mean values for pass/fail decision-making. Lower and upper specification limits were then respectively identified as USL + σwafer and LSL − σwafer, where σwafer is the within wafer CD standard deviation.


In real production, post-etch CD will vary based upon the hard mask (HM) and mandrel etch isotropy. For our simulation, we used isotropy values of 0.125 and 0.055, respectively, for the HM and mandrel etch step. The associated standard deviation values of 5 × 10-4 and 1 × 10-4 were used in the simulation, indicating a very tightly controlled etch process.


### Lithography Variability and Baseline Cpk


Process variability was introduced during simulation at the lithography step for 500 SEMulator3D® simulation runs. This type of wafer-to-wafer variability can occur in real life when lithography conditions during resist spin coating, exposure, and development are not well controlled between wafers.


In our simulation, the resulting lithography CD distribution had a mean of **60.12 nm** and a σ = **2.06 nm** . Even with a very tightly controlled etch process, this incoming lithography variability propagated through the etch process, producing a post-etch CD mean of **35.39 nm** with a σ = **2.07 nm** .


If we set spec limits at ±5% of CD (centered around the mean), the post-etch Cpk was **0.28** (very poor). Rather than reworking/scrapping these wafers after the combined lithography/etch process, feedforward correction offered a path to recover wafers downstream from the lithography process.


*Figure 2. Open-loop control: lithography CD is translated to etch CD using a fixed etch recipe, with no correction for incoming lithographic variability*


### Feedforward Strategy: Binning by Lithography CD


Rather than tailoring a unique recipe for every wafer, wafers were binned into three groups based on their post-lithography CD measurement:


- **CD < 59 nm (under target):** Etched with etch isotropy scaled down by 11.7%, reducing CD loss and lifting post-etch CD toward target.
- **CD between 59–61 nm (near target):** Etched with the standard recipe, no correction applied.
- **CD > 61 nm (over target):** Etched with etch isotropy scaled up by 9.9%, increasing CD loss and pulling post-etch CD back toward target.


These scaling factors were selected using SEMulator3D® simulation. The practical effect of modifying etch isotropy based upon the post-lithography CD measurement is that random lithography variations can be automatically corrected, lessening the CD spread after the etch process.


*Figure 3. Lithography CD to etch CD reduction under feedforward control using three binned etch recipes*


### Feedforward Control Results: Spread and Cpk Improvement


According to our simulation, feedforward correction dramatically tightened the post-etch distribution. Standard deviation dropped from 2.07 nm to **0.8268 nm** —roughly a **60%** reduction—and post-etch Cpk improved from 0.2854 to **0.7135** , which is a **2.5x** gain in process control.


These improvements occurred without having to rework any wafers after the lithography step.


*Figure 4. Post-etch CD histograms and control charts: open-loop vs. feedforward control*


### Impact on Yield and Throughput


The Cpk improvement translates into a wafer pass rate increase from **60.8% to 96.77%** . But pass rate alone is the wrong metric: feedforward control involves process recipe changes and recipe-specific etch time changes that can slow wafer throughput.


The correct metric in measuring success is the good wafers produced per unit of time, which can be calculated as the Pass Rate × Throughput. To match the baseline good-wafer output, the feedforward path can tolerate a production slowdown of up to (1 − 60.82/96.77) × 100% ≈ 37.14%. In other words, **the pass rate gain is large enough to absorb a meaningful throughput penalty and still come out ahead** .


### Feedforward Control Under Higher Variability Conditions


The simulation was repeated at increasing lithography variability:


- σ = 2.85 nm: Uncorrected etch Cpk falls to 0.21; feedforward recovers it to 0.47. Pass rate improves from 46.9% to 84.5%, providing headroom for up to a 44% slower throughput.
- σ = 3.75 nm: Uncorrected etch Cpk falls to 0.16; feedforward recovers it to 0.27. Pass rate improves from 36.3% to 58.9%, providing headroom for up to a 38% slower throughput.


*Figure 5. Feedforward control improves pass rate and Cpk across increasing lithography variability*


As lithography variability grows, feedforward control continues to deliver meaningful Cpk and pass rate improvements, even absorbing the throughput losses expected from etch step process changes.


### Key Takeaways


Feedforward process control transforms the economics of variability in semiconductor fabrication. It actively rescues out-of-spec wafers by adjusting downstream process recipe steps. By measuring each wafer's post-lithography CD and routing the wafer to one of three pre-tuned etch recipes based upon this CD, our simulations show that post-etch Cpk improves from 0.2854 to 0.7135 and the pass rate increases from 60.82% to 96.77%. These pass rate improvements are seen under nominal variability conditions, but substantial gains are also preserved as lithography variability increases.


Feedforward process control can provide a large, sustained pass rate gain in return for a controllable throughput penalty, making it a compelling lever to improve yield during modern semiconductor fabrication.


**Reference**


National Institute of Standards and Technology. 2012. *Engineering Statistics Handbook.* Section 6.1.6. "[What Is Process Capability?](https://www.itl.nist.gov/div898/handbook/pmc/section1/pmc16.htm) ”


### Related Articles


- [Accelerating GAA Logic Yield Optimization With Digital Twins](https://newsroom.lamresearch.com/accelerating-gaa-logic-yield-optimization-with-digital-twins?blog=true)
- [Improving Uniformity With Dummy Fill and SEMulator3D](https://newsroom.lamresearch.com/improving-uniformity-with-dummy-fill-semiverse-solutions?blog=true)
- [Calibrating for Accurate Predictions of FinFET Device Profiles](https://newsroom.lamresearch.com/calibrating-for-accurate-predictions-of-finfet-device-profiles?blog=true)
