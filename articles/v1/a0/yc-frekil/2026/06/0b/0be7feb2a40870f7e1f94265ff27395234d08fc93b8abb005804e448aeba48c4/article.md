---
schema_version: "1.0.0"
document_id: "0be7feb2a40870f7e1f94265ff27395234d08fc93b8abb005804e448aeba48c4"
company_key: "yc-frekil"
company: "Frekil"
source_id: "yc-frekil-rss-a6f9e192a2a2"
canonical_url: "https://www.frekil.com/blog/empirical-calibration-negative-controls-rwe"
published_at: "2026-06-07T00:00:00+00:00"
first_seen_at: "2026-07-25T05:42:05.860173+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:8708fff790547eda6fbc8146af3f78d5cb9c0b64d74bade95f12cb07370f1a21"
---

# Can You Trust That p-value? The Math of Empirical Calibration

A statistically significant result is supposed to be reassuring. In a randomized trial, it usually is. In real-world data, a "significant" finding can be perfectly real, or it can be an artifact of bias that the p-value sitting next to it was never built to see. The good news is that the bias is measurable - and once you can measure it, you can correct the p-value itself.


Here is an uncomfortable way to feel the problem. Take a drug and a health outcome you are completely certain are unrelated. Run a standard observational analysis comparing people who took the drug against people who did not. By chance alone, you should land on a "statistically significant" association about 1 time in 20. In practice, with real patient data, you get one far more often than that.


If a method flags effects that cannot exist, how much should you trust it when it flags an effect that might?


That question sits underneath most of the skepticism aimed at real-world evidence. It is also, finally, answerable - not with a better opinion, but with a number. The technique is called empirical calibration, and it is worth understanding at the level of the actual math.


## The p-value only quantifies half the error


Every effect estimate carries two kinds of error. Random error is the noise that comes from a limited sample, and collecting more patients shrinks it. Systematic error is something else entirely - it is the bias baked into the comparison when treatment was not randomized and the two groups differ in ways you never measured, or never could. More data does not shrink it. A larger study just estimates a biased number more precisely.


A p-value, and the confidence interval beside it, is built to quantify the first kind of error and is blind to the second. As Martijn Schuemie and colleagues put it, the p-value "only reflects random error... It does not reflect systematic error, for example the error due to confounding."[Schuemie et al., Statistics in Medicine](https://pmc.ncbi.nlm.nih.gov/articles/PMC5108459/)


In a randomized trial that blindness is mostly harmless, because randomization neutralizes systematic error before the analysis ever begins. Real-world data gets no such protection. The bias is already in the records when you receive them, and the p-value has no way of knowing it is there. The[FDA's real-world evidence program](https://www.fda.gov/science-research/science-and-research-special-topics/real-world-evidence) draws the same line in different language: real-world data only becomes trustworthy evidence once it is shown to be fit for the specific decision being made.


The consequence is concrete. In one study of childhood infections and later multiple sclerosis, the researchers slipped in three negative controls - a broken arm, a concussion, a tonsillectomy - outcomes that no exposure like that could plausibly prevent. Two of the three came back statistically significant. The analysis was, in effect, reporting that catching a childhood infection protects you from breaking a bone. That is not a result. It is a confession that the study is biased - and the exact same trap is waiting inside any drug-versus-outcome question you run the same way.


## Negative controls: a stress test you can actually run


The fix starts with an old idea borrowed from laboratory science. A negative control is a question where you already know the true answer is "no effect." Marc Lipsitch and colleagues formalized this for epidemiology in 2010, defining negative controls as exposure-outcome pairs where a causal link is not plausible, and arguing they should be used routinely to detect confounding and bias.[Lipsitch et al., Epidemiology](https://pubmed.ncbi.nlm.nih.gov/20335814/)


The logic is almost embarrassingly simple. Pick a few dozen drug-outcome pairs where the drug cannot reasonably cause the outcome. Run each one through the exact same study design, on the same data, with the same model you are using for your real question. The true effect for every one of them is, by construction, zero. So whatever the method reports for them is not a real effect - it is the bias your design produces, made visible.


**Real question** Does this drug change the risk of the outcome we actually care about? True effect: unknown.


**Negative control** Does this drug change the risk of an outcome it cannot plausibly affect? True effect: known to be zero.


**What it reveals** Any nonzero "effect" on a control is a direct sample from the design's systematic error.


> A negative control is a question where you already know the answer is no. How often, and how strongly, your method answers yes is a measurement of its bias - not a matter of opinion.


## Empirical calibration: turning the controls into a corrected p-value


This is where the actual statistics begin, and it is the part most explanations skip.


Work on the log scale, where effect estimates are roughly normal. For each negative control *i* , the study produces an estimated log relative risk *y* i and a standard error *s* i. Here is the pivot the whole method turns on: every one of these controls has a true effect of zero, by construction - that is what made it a negative control in the first place. So an unbiased method would scatter the *y* i around zero, each estimate sitting within its own sampling spread *s* i, with nothing pulling the cloud off-center and nothing widening it beyond what *s* i allows. That is not what you see. The estimates sit shifted off zero, and they spread wider than their standard errors can account for - a bias in the center, plus a bias that itself varies from one question to the next.


So run fifty or a hundred negative controls and you no longer have anecdotes - you have an estimate of a distribution: the distribution of the method's errors on questions whose answer is zero. Schuemie's model writes each control estimate as a true-effect-of-zero, plus a systematic error drawn from a Gaussian, plus ordinary sampling error:


y


i = 0 + β


i + ε


i, with β


i ~ N


( μ


, σ


2), ε


i ~ N


(0, s


i2)


which marginally gives y


i ~ N


( μ


, σ


2 + s


i2)


That two-parameter object - the Gaussian N


(μ, σ2) - is the empirical null. The parameters have clean interpretations. **μ is the average systematic bias** : how far the method is shifted from the truth on questions where the answer should be zero. **σ is how much that bias varies from one question to the next** - the part of the error that is not even consistent, and that no single offset can remove. You fit both by maximum likelihood over the control set, maximizing


L


( μ


, σ


) = ∏i (2π( σ


2 + s


i2))−1/2 exp( −( y


i − μ


)2 / 2( σ


2 + s


i2) )


A useful way to see it is the calibration plot: estimate on one axis, standard error on the other. Under an unbiased method, about 95% of the negative controls should fall inside the funnel that brackets a true effect of zero. When they spill outside it, the funnel is in the wrong place and the wrong width - and μ and σ are exactly how much.


Now the payoff. To test your real outcome, you stop comparing its estimate to the textbook null N


(0, s


2) and start comparing it to the empirical null. The calibrated test statistic and two-sided p-value are


z


= ( y


− μ


) / √( σ


2 + s


2)


p


cal = 2 · Φ


(−| z


|)


The textbook p-value asks how surprising the result is if the drug has no effect and the study is flawless. The calibrated p-value asks a stricter question: how surprising is it against the spread of errors this method actually makes on questions whose answer is already known to be zero? The first assumes the design is unbiased; the second measures the bias and prices it in.


A real example from the[OHDSI EmpiricalCalibration](https://ohdsi.github.io/EmpiricalCalibration/articles/EmpiricalPCalibrationVignette.html) package makes the size of the correction visible. Across its negative controls, one analysis produced an empirical null of μ = 0.79 and σ = 0.28 - a large upward bias. The drug of interest came back with log(RR) = 0.73 and s = 0.074, a raw z of about 9.9 and a p-value indistinguishable from zero. Calibrate it:


z


= (0.73 − 0.79) / √(0.282 + 0.0742) ≈ −0.20 → p


cal = 2· Φ


(−0.20) ≈ 0.84


The raw analysis screamed a near-certain effect. Once the method's own bias is accounted for, the estimate sits almost dead-center in the cloud of errors the method makes on nothing - and the calibrated p-value is 0.84. The signal was the bias.


## Calibrating the interval, not just the p-value


A p-value is only half of what a study reports. To calibrate the confidence interval you need to know how the systematic error behaves not just at a true effect of zero, but across a range of true effects. That is what positive controls are for.


You cannot ethically manufacture a real drug harm, but you can synthesize positive controls: take a negative control and inject a known effect, so the true log relative risk is a known θ (for instance θ = log 1.5, log 2, log 4). Run those through the same pipeline and you can see how the error grows with the size of the true effect. Schuemie's confidence-interval model lets the mean of the systematic error move linearly with θ, and lets its spread change with θ too - modeled on the log scale so that a standard deviation can never come out negative:[Schuemie et al., PNAS](https://www.pnas.org/doi/10.1073/pnas.1708282114)


y


| θ


~ N


( a


+ b


· θ


, σ


( θ


)2 + s


2 )


σ


( θ


) = exp


( c


+ d


· θ


)


A method with no systematic error would have a = 0, b = 1, and σ(θ) collapsing toward zero - estimates landing on the truth with nothing but sampling noise around them. The four fitted numbers a, b, c, d are a compact report card on how far from that ideal a given design and database actually are. The calibrated 95% interval is then every value of θ that this model does not reject at the 0.05 level for the observed estimate - you invert the calibrated test instead of reading ± 1.96 standard errors off a single estimate.


The effect is not cosmetic. In the package's worked example, an uncalibrated 95% interval for a gastrointestinal-bleed risk ratio of \[0.61, 0.80\] - comfortably "protective," excluding 1.0 - became \[0.48, 1.11\] after calibration.[OHDSI EmpiricalCalibration](https://ohdsi.github.io/EmpiricalCalibration/articles/EmpiricalCiCalibrationVignette.html) The interval widened and shifted until it crossed 1.0, because an apparent 20-40% risk reduction was well within what that method's measured bias could produce on its own.


> Calibration does not make weak evidence strong. It makes the error bars honest - and an honest error bar sometimes erases the finding.


One assumption underneath all of this deserves to be said out loud, because it is where the method can still fail. Calibration only works if the bias measured on the controls is the same bias acting on your real question. If a confounder distorts your actual outcome but happens to touch none of the negative controls, it never shows up in the cloud - and calibration cannot subtract what it never saw. That is why the choice of controls is load-bearing rather than clerical: they have to be able to go wrong in the same ways your real question can. Empirical calibration corrects the bias you can see by proxy. It does not absolve you of thinking hard about the bias you cannot.


## What this looks like at scale


When Schuemie's team stress-tested standard observational designs this way, the verdict was uncomfortable: across studies, the uncalibrated 95% confidence interval - the one meant to miss the truth only 1 time in 20 - contained the true effect far less often than 95% of the time. After calibration against the controls, coverage returned close to its nominal 95%. The rigor was recoverable. It just had to be measured rather than assumed.[Book of OHDSI, Method Validity](https://ohdsi.github.io/TheBookOfOhdsi/MethodValidity.html)


The most ambitious demonstration is worth sitting with. In the LEGEND-HTN study, OHDSI investigators compared first-line blood pressure drug classes across 4.9 million patients in nine databases, with negative controls and empirical calibration built into the design to guard against residual bias.[LEGEND-HTN validity, JAMIA](https://academic.oup.com/jamia/article/27/8/1268/5895564) The clinical result, published in The Lancet, was not subtle. The most commonly prescribed first-line class, ACE inhibitors, started by 48% of patients, was not the best choice. Patients who began on a thiazide or thiazide-like diuretic instead had roughly 15% fewer major cardiovascular events - heart attacks, strokes, and heart-failure hospitalizations - and fewer side effects across nearly every category measured.[OHDSI, LEGEND-HTN](https://www.ohdsi.org/lancet-paper-shows-most-popular-hypertension-drug-isnt-most-effective-per-ohdsis-legend-study/)


The investigators estimated that more than 3,100 major cardiovascular events might have been avoided among the 2.4 million patients who started on an ACE inhibitor, had they started on a diuretic instead. Those 3,100 are not a rounding error in a results table. They are heart attacks, strokes, and heart-failure admissions that happened to real people who were handed the most-prescribed pill, while the data that pointed to a safer one was already sitting in databases. A claim that the standard-of-care drug is not the best one is exactly the kind of conclusion observational data is usually too fragile to support. Calibration against hundreds of controls is part of what earns it the right to be taken seriously.


## The takeaway


Empirical calibration changes what an observational result can honestly claim. Instead of asking a reviewer to take on faith that a design is unbiased, it estimates the bias directly - from a set of questions whose answers are already known - and folds that estimate into the p-value and the confidence interval. The honest version of a real-world result is not the bare z = y / s. It is the z after the method has been made to answer a few hundred questions whose answers were known in advance, and to show how often it got them wrong.


It is not a universal solvent, and it should not be treated as one. Calibration corrects the systematic error the controls can see. It cannot remove a confounder that distorts your real outcome but touches none of the controls, and it cannot recover a true signal that bias has already swamped. Its credibility rests entirely on negative and positive controls that can fail in the same ways the real question can, which makes the choice of controls a scientific judgment rather than a formality.


Used with that discipline, calibration turns a vague objection - "observational studies are biased" - into something a study can quantify about itself: an empirical null with an estimated mean and spread, a calibration plot, and intervals that have survived contact with their own error. That is the line between a result that is interesting and one that is trustworthy. For evidence that helps decide which drug a patient is actually given, that line is the whole point.
