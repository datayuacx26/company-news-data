---
schema_version: "1.0.0"
document_id: "7f180757e31b4e84f5610eaa2add8f0f1610da7a8841d3db4310db8c2628f36f"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/variance-reduction-ab-testing"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T19:45:33.762968+00:00"
fetched_at: "2026-07-31T19:45:34.191941+00:00"
content_hash: "sha256:d47cc2764a50a01c6d72191f181046c938c713ecd10de1f34890992ed25cc465"
---

# Variance Reduction in A/B Testing: 5 Techniques to Increase Experiment Sensitivity

Big experimental wins are rare. On a mature product, most of what you test produces a small effect. Across 1,450 experiments on Microsoft's Bing,[most changes moved their target metric by a fraction of a percent](https://eduardomazevedo.github.io/papers/azevedo-et-al-ab.pdf) . An effect that small is hard for a standard[A/B test](https://www.growthbook.io/blog/what-is-a-b-testing) to detect. The treatment may be moving the metric, but the effect is small relative to its natural variation, and the sample isn't large enough to separate signal from noise, especially when the change reaches only a fraction of the user base. Without variance reduction, experiments like these often end up underpowered, with no significant result and a decision left to judgment rather than evidence.


To detect an effect that small, the experiment has to be more sensitive. Sensitivity is its ability to detect an effect when one exists, and short of waiting for a larger sample to accrue, the only input you can change is the variance of the metric itself. When that variance comes down, the same sample detects a smaller effect, or the same effect sooner.


This is a practitioner's guide to 5 variance reduction techniques in[experimentation](https://www.growthbook.io/products/experimentation) : metric choice, winsorization, CUPED, post-stratification, and triggered analysis. Each section covers how the technique works and the trade-off that comes with it. They're ordered from the simplest to the most complex, and, at the end, there's a decision framework for which to try first.


## Why variance is slowing your tests down


Variance slows your tests down because a noisier metric needs a larger sample to separate a real effect from chance, and a larger sample takes longer to collect. Required sample size scales linearly with the variance of the metric, so halving the variance roughly halves the time to significance and tightens the confidence interval you end up with.


Most of that variance has nothing to do with your experiment. The signal you're after is the effect of your change. The noise is everything else. A user who was always going to spend $400 this month sits in the same dataset as one who was always going to spend $4, and that variation exists before your treatment does anything. Required sample size scales with that variance, so a noisier metric costs you directly. It also scales with the inverse square of the smallest effect you want to detect, so detecting an effect half as large needs four times the sample, not twice. A noisy metric and a small target effect compound each other, and both push your runtime up.


Lower variance also helps in the other direction. At a fixed sample size, it shrinks the[minimum detectable effect](https://docs.growthbook.io/statistics/power) (MDE), the smallest effect your test can reliably detect, so a 1% change that was previously below that threshold now comes within range. That leaves 2 ways to detect a smaller effect: collect more data, or remove variance. You usually can't generate more units on demand, whatever your experiment's grain (users, sessions, requests), so the 5 techniques below all take the second path.


## 1. Fix your metrics before you fix your statistics


The largest change to your experiment's sensitivity is the metric you choose to measure, not the statistical method you apply to it. No amount of adjustment can rescue a primary metric that is intrinsically noisy, while a cleaner choice of metric can shorten the test's duration before you change anything in the analysis.


### Move the metric closer to the treatment


A proximate metric measures the action your feature directly affects. A distal metric sits far downstream, where your change is one of hundreds of influences. If you're testing a new checkout button, the proximate metric is checkout starts or add-to-cart rate. The distal metric is revenue, which is moved by pricing, inventory, seasonality, and everything else your team ships that month.


Proximate metrics carry less of that unrelated variation, so they respond faster and detect smaller effects. Moving your primary metric closer to the treatment is often the largest single sensitivity gain available, and the easiest to make.


The caveat is that proximity can cost you business relevance. The reason you care about *checkout starts* is that they are supposed to lead to revenue. But if *checkout starts* come back significant and positive while revenue stays flat, you've shipped a win that delivered none of the value you were after. A proximate metric is only worth using if it tracks your[Overall Evaluation Criterion (OEC)](https://www.growthbook.io/blog/designing-a-b-testing-experiments-for-long-term-growth) , the metric or weighted blend of metrics that defines a successful experiment. A good OEC is measurable within the experiment but still predicts the long-term value you ultimately care about. That lets you decide now instead of waiting weeks or months for lagging metrics like revenue or retention. Pick the closer metric, but confirm it actually moves with the[downstream outcome](https://www.growthbook.io/blog/7-steps-to-better-experiment-design) it stands in for.


Committing to a single primary metric also preserves statistical power. Each additional metric you test raises the probability of a spurious significant result, which forces a[multiple-comparison correction](https://docs.growthbook.io/statistics/multiple-corrections) that reduces sensitivity across every metric, including your primary. One well-chosen primary outperforms a battery of hedged ones.


### Use rates and binary metrics over totals


The shape of a metric drives its variance. A binary metric like conversion rate is naturally bounded. Each user contributes only a 0 or a 1, so no single user can dominate the variance. (Formally, for a conversion rate p the per-user variance is p(1 − p), which tops out at 0.25.) A total like revenue per user has a long right tail, where a handful of high-spending users sit far above the rest. Those users move the mean on their own, and the variance they add is what makes a small treatment effect hard to detect.


When possible, make a rate or a binary metric your primary. For example, instead of average order value or revenue per user, where a few big spenders dominate, measure the share of users who spend more than some amount, say $50. Each user counts once, above or below that line, so no single $5,000 order can dominate the result. That's a[proportion metric](https://docs.growthbook.io/app/metrics#proportion-metrics) , the rate form of a yes-or-no outcome, rather than a mean metric. The tradeoff is that you lose magnitude, since it tells you whether more users crossed the threshold, not how much they spent past it. Keep revenue as a secondary or[guardrail metric](https://docs.growthbook.io/app/metrics) so you still track it.


## 2. Handle outliers with winsorization


Winsorization, also known as[outlier capping](https://docs.growthbook.io/app/metrics#capped-value) , limits each user's aggregated value to a chosen threshold so a few extreme outliers can't dominate the result. If revenue per user normally runs around $40 and one customer places a $5,000 order, that single order can make whichever variation the customer was assigned to look like the winner. Set the cap at the 99th percentile, say around $200. The $5,000 order then counts as $200, so the customer still contributes to the result but can no longer make their variation look like the winner on their own.


Extreme values don't just move the mean. They inflate variance, which widens your confidence intervals and extends the experiment duration. Capping trades a small, deliberate bias for a reduction in variance.


Two practical notes matter here. First, the cap has to be pre-specified. Decide the rule before launch, whether that's an absolute cap or a percentile like the 99th, not after you've seen which users are outliers and which variation they helped. Choosing the threshold once results are in, to move the result in your favor, is[p-hacking](https://www.growthbook.io/insights/understanding-false-causality-examples) . GrowthBook's percentile capping computes the actual cutoff from experiment data, but you commit to the rule up front.


Second, and less often stated, capping changes the estimand, the quantity you're actually estimating. Once you cap, you're no longer estimating the treatment's effect on mean revenue. You're estimating its effect on capped revenue, and those aren't the same number. The deeper issue is that you can't see how the capped users would have responded, because you've flattened exactly the part of their behavior the feature might move. If the real value of your change lives in the tail, like a feature meant to get your highest spenders to spend more, capping can hide the effect you're testing for. Before trusting a capped result on a revenue metric, check the tail directly with a[quantile metric](https://docs.growthbook.io/statistics/quantile) to confirm the outliers aren't carrying the effect.


## 3. CUPED experiments: the most powerful general-purpose technique


[CUPED](https://docs.growthbook.io/statistics/cuped) (Controlled-experiment Using Pre-Experiment Data) is a form of covariate adjustment that reduces variance by adjusting each user's outcome for how they behaved before the experiment started. It removes the part of the metric you could have predicted in advance, leaving behind the part your treatment is responsible for.


### How CUPED works


CUPED uses each user's pre-experiment value of the metric to estimate what they would have done without the treatment, then subtracts it. Those baseline differences have nothing to do with your treatment, so subtracting them lowers the variance without biasing the estimate. For example, a frequent buyer who keeps buying at the same rate in treatment no longer looks like a treatment effect, because the adjustment already accounted for their baseline.


CUPED comes from a[2013 paper](https://exp-platform.com/Documents/2013-02-CUPED-ImprovingSensitivityOfControlledExperiments.pdf) by Deng, Xu, Kohavi, and Walker. It has one requirement: for each user, you need a pre-experiment measurement that predicts their behavior during the experiment. Usually that's the same metric measured beforehand—a user's revenue in the weeks before the test predicts their revenue during it. The stronger that relationship, the more variance CUPED removes, because that predictable, pre-existing variation is exactly what it subtracts out.


### When CUPED works (and when it doesn't)


CUPED's variance reduction is proportional to how strongly a user's pre-experiment behavior correlates with their in-experiment behavior. When that correlation is high, the gains are large.[Netflix reported](https://www.kdd.org/kdd2016/papers/files/adp0945-xieA.pdf) roughly a 40% variance reduction on key engagement metrics, and[Microsoft found](https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/articles/deep-dive-into-variance-reduction/) CUPED equivalent to adding about 20% more traffic to an experiment.


CUPED's limitation is that it does little when the correlation is weak or missing. Brand-new users have no pre-experiment history to adjust against. Rare conversions, like a first purchase, often have no usable pre-period signal either. GrowthBook applies CUPED across your metrics as an[organization-wide default](https://www.growthbook.io/blog/cuped-for-faster-experimentation-in-growthbook) , and skips it automatically where there's no pre-experiment data to use.


## 4. Post-stratification: stacking on top of CUPED


Post-stratification splits users into groups, measures the treatment effect within each group, and combines those into a weighted average. Where CUPED removes variance using each user's own history, post-stratification removes the variance between groups, like the gap between countries or devices, which your treatment didn't cause.


For example, comparing US users only to US users, and Indian users only to Indian users, removes the large between-country differences from the comparison. Because it targets variance that CUPED leaves behind, it stacks on top of CUPED rather than competing with it. GrowthBook combines the two into a single setting called[CUPEDps](https://docs.growthbook.io/statistics/cuped) , which in the right conditions can be equivalent to running your experiment with[20%+ more traffic](https://www.growthbook.io/blog/growthbook-version-4-3) . Beyond cutting variance, it also corrects for imbalance. If the treatment-control split in a country comes out 48/52 instead of 50/50, post-stratification reweights the two groups to the split you set.


Post-stratification only helps when the groups you stratify by actually differ on the metric. If they don't, there's little between-group variance for it to remove. A related technique, stratified randomization, balances the groups before assignment rather than after.


## 5. Use triggered analysis for low-coverage features


Triggering restricts your experiment to the users actually exposed to the treatment, not everyone assigned to it. It's most useful for low-coverage features, where the unexposed users dilute your effect the most.


### The dilution problem


When your analysis includes users who were assigned but never exposed, their treatment effect is exactly zero, and they pull your measured effect toward zero. The treatment effect concentrated among the exposed users gets averaged down across everyone who wasn't. If your feature adds $4 of revenue per exposed user but only 20% of assigned users are ever exposed, that $4 effect shows up as $0.80 across everyone assigned, because the other 80% contribute nothing.


Triggered analysis fixes this by filtering the unexposed users out of the analysis, leaving only those who were exposed. GrowthBook logs exposure from feature flag evaluation, so you can tell exactly who was exposed and filter the analysis to only include them.


Filtering relies on the treatment not changing who gets exposed. When it does, like a banner that drives more users to scroll or a variant that loads slower so fewer reach the feature, you get a[sample ratio mismatch](https://docs.growthbook.io/kb/experiments/aa-tests) (SRM) where the realized split no longer matches the one you set. The two groups are no longer truly random, and a measurable difference between them could be caused by that mismatch rather than the treatment. Run an SRM test on the triggered split before trusting a filtered result.


When the treatment might change who gets exposed, move randomization to the trigger point instead, assigning each user the moment they're first exposed rather than up front. If you're testing a redesigned cart page, assign users the first time they open the cart, not before. Only users who reach the cart enter the experiment, and because the redesign appears only once they're there, it can't have changed who reached the cart.


### The estimand caveat


A triggered result describes the triggered users only, not everyone. A 10% lift among users who experienced the change is not a 10% lift for your business, because most of your users never see the feature. Before you make a launch decision or a revenue projection,[translate the triggered effect](https://alexdeng.github.io/public/files/wsdm2015-dilution.pdf) back to the full population.


## Which technique should you use? A decision framework


Each technique can be stacked, so all 5 can run on a single experiment when relevant.


**Situation** **Technique** **Effort** **Watch out for**


Your primary metric is downstream or noisy Move to a proximate metric Low Confirm it's a true leading indicator of the outcome you care about


A few high-value users dominate a total metric Winsorization (capping) Low, one setting Pre-specify the cap, and check the tail with a quantile metric


Returning users with correlated pre-experiment data CUPED Medium, needs a pre-experiment pipeline Minimal gain for new users or rare events


The metric varies a lot across a known dimension Post-stratification, on top of CUPED Medium Needs groups that differ on the metric


Only a fraction of assigned users see the feature Triggered analysis High, needs exposure logging Translate the effect to the full population, and watch for SRM


## How GrowthBook implements variance reduction


GrowthBook implements most of these variance reduction techniques directly, so you don't have to build them yourself. Capping is a metric setting, either an absolute threshold or a percentile.[CUPED](https://docs.growthbook.io/statistics/cuped) runs as an organization-wide default on Pro and Enterprise plans, and post-stratification on Enterprise combines with it into CUPEDps.


Variance reduction is one half of running experiments faster. Safe peeking is the other, and GrowthBook makes this possible with[sequential testing](https://docs.growthbook.io/statistics/sequential) , which lets you check results repeatedly without inflating your false positive rate. The stats engine behind all of this is[open source](https://github.com/growthbook/growthbook/tree/main/packages/stats) , so you can read exactly how any adjustment is computed.


The[metric Debug tab](https://www.growthbook.io/blog/growthbook-version-4-3) added in GrowthBook 4.3 shows how CUPED, post-stratification, and capping each move a given metric's variance. Variance reduction is otherwise easy to treat as a black box, where the intervals come out tighter, and you trust that the adjustment was applied correctly. Seeing each layer's contribution makes that adjustment auditable.


If you want variance reduction by default, you can[try GrowthBook for free](https://www.growthbook.io/get-started) or[book a demo](https://www.growthbook.io/demo) with the team.
