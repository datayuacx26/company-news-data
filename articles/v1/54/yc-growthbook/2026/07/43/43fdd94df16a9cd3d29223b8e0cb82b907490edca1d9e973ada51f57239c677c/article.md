---
schema_version: "1.0.0"
document_id: "43fdd94df16a9cd3d29223b8e0cb82b907490edca1d9e973ada51f57239c677c"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/avoid-false-positives-high-velocity-experimentation"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-23T22:17:24.982400+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:9e68405d7213ca7b36de234275667d0b56a20b05575c62e0010a9fdf73d1c8ef"
---

# How to Avoid False Positives in High-Velocity Experimentation

If you run[A/B tests](https://www.growthbook.io/blog/what-is-a-b-testing) long enough, you'll eventually ship a false positive. Your test calls a winner, you ship it, but the metric that should have moved stays flat. This happens far more often than the 5% significance threshold promises, even at big companies with mature experimentation programs like Microsoft, Bing, Netflix, and Airbnb. What's worse is that the faster you test, the more false positives you ship. This is the technical playbook for keeping that number down without slowing your program to a crawl.


## How bad is the false positive problem in A/B testing?


False positive rates in real-world A/B testing run far higher than the 5% most teams assume. Even mature[experimentation](https://www.growthbook.io/products/experimentation) programs carry 6% to 26% risk that a given statistically significant win is a false positive (a[Type I error](https://www.growthbook.io/insights/type-i-error) ), and at the industry-median success rate,[roughly 1 in 5 experiments called winners are a false positives](https://doi.org/10.1145/3534678.3539160) .


Below is a list of[false-positive risks by company](https://www.growthbook.io/blog/designing-a-b-testing-experiments-for-long-term-growth) . False positive risk is the probability that a statistically significant winner has no real effect, and it follows directly from how often a team's experiments succeed. The lower the success rate, the higher the risk.


**Company** **Experiment Success Rate** **False Positive Risk**


[Microsoft](https://www.microsoft.com/) 33% 5.9%


[Bing](https://www.bing.com/) 15% 15%


[Booking.com](https://www.booking.com/) 10% 22%


[Google Ads](https://ads.google.com/) 10% 22%


[Netflix](https://www.netflix.com/) 10% 22%


[Airbnb Search](https://www.airbnb.com/) 8% 26.4%


The more experiments you run, the more false positives you ship. A team running 100 experiments a month at the median 10% success rate sees about 10 winners, and roughly 2 of those are false positives with no real effect. Each one costs engineering time and gets counted as a win, so the team keeps building on a feature that did nothing. Over a year, that's around 24 of them, and the faster a program runs, the faster they accumulate.


## What causes false positives in experimentation?


Most false positives trace back to 4 causes: peeking at results early, testing too many things at once, assignments that are not truly random, and underpowered tests. Each one pushes your true false positive rate higher.


### Peeking


[Peeking happens](https://docs.growthbook.io/using/experimentation-problems) when you check results while a test is still running and stop as soon as you see a significant positive result. The bias comes from an asymmetry in how the test ends. A positive result that crosses significance stops it, while a flat or negative one keeps running to give it time to resolve. A test with no measurable effect will still cross into positive significance from time to time through sampling variation, and stopping at one of those crossings mistakes that fluctuation for a real win.


A p-value assumes a sample size fixed in advance and examined once, so repeated looks invalidate them. Each additional look[raises your false positive rate](https://doi.org/10.2307/2343787) . With no correction, it climbs from 5% at a single look to roughly 14% by 5 looks and 19% by 10.


### Multiple comparisons


With 20 metrics in a single test, your odds of at least one false positive are[about 64%](https://docs.growthbook.io/using/experimentation-problems) , even if nothing you changed had any effect. Each metric you add is another test with its own 5% chance of a false positive, so the family-wise error (FWER) rate keeps climbing as the list grows (FWER is the chance that any one of your metrics shows a false positive). Teams that track a wide metric dashboard on every experiment are especially exposed here. Learn more about[how to choose the right metrics and KPIs for A/B testing](https://www.growthbook.io/blog/kpi-playbook-ab-testing-metrics) .


### Sample ratio mismatch (SRM)


Sample Ratio Mismatch (SRM) happens when the traffic split you actually get doesn't match the one you configured. A 50/50 test that comes out at 53/47 because of a bot exclusion, or SDK bug, or targeting error no longer has comparable groups, so you can't separate a real effect from bias. SRM is common and easy to miss, and the only way to catch it is to test the observed split against the one you configured, usually with a chi-squared test, which[GrowthBook runs automatically](https://docs.growthbook.io/using/experimenting#sample-ratio-mismatch-srm) .


### Underpowered tests under pressure


An underpowered test without the sample size to reliably detect the effect you care about is usually framed as a false negative ([Type II error](https://www.growthbook.io/insights/type-i-vs-type-ii-error) ) risk, but it feeds false positives too. It produces wide confidence intervals and unstable point estimates. When an underpowered experiment does cross significance, that win is disproportionately likely to be spurious, and the winner's curse inflates its measured effect, since only the largest swings reach significance when power is low. Deadline pressure makes this worse, as teams peek, stop the first time the result crosses significance, and ship it. Underpowering and peeking reinforce each other, and a[high-velocity program](https://www.growthbook.io/blog/chasing-velocity-in-a-b-testing-why-more-experiments-can-mean-less-learning) tends to run into both at once.


## 7 ways to limit false positives in experimentation


Some false positives are unavoidable, but the inflated rates described above are not. Every cause has a practical control. Here are 7 actions you can take to lower your false positive rate.


### 1. Pre-commit to sample size and duration


Do your power analysis *before* launch, not after the early numbers start looking good. Define your minimum detectable effect (MDE), set power to at least 80%, and design your experiment around the required sample size. Then leave it alone. "It's trending positive" is not a stopping criterion. GrowthBook's[power analysis](https://docs.growthbook.io/statistics/power) computes the required sample size before you launch.


### 2. Calibrate with A/A tests


Run an[A/A test](https://docs.growthbook.io/kb/experiments/aa-tests) whenever you stand up a new experiment surface or change the data pipeline behind it. An A/A test is an A/B test where both arms serve the same variation to validate your instrumentation and assignments before a real experiment. With no true effect by construction, a sound setup should return roughly identical results for both arms.


A significant A/A test result for a single metric doesn't necessarily mean something is broken. Even when there is no real difference, some metrics end up significantly different by chance alone. At GrowthBook's default 95% chance-to-win threshold, that's[about 10% for a single metric](https://docs.growthbook.io/kb/experiments/aa-tests) , or 41% across 5 independent metrics. 1 metric out of 5 with a statistically significant difference is usually fine, especially if no SRM or other experiment issues are detected. A genuine problem produces a different pattern, with many metrics reaching significance simultaneously, often at extreme values such as 99%+ chance-to-win. To tell them apart, re-randomize and rerun the A/A test. Random noise will affect different metrics each time, while a real bug will consistently affect the same ones.


### 3. Use sequential testing to solve the peeking problem in A/B testing


[Sequential testing](https://docs.growthbook.io/statistics/sequential) lets you watch results as often as you want without inflating your false positive rate. It works by using confidence intervals that are wide enough to remain valid no matter how many times you look. GrowthBook implements asymptotic confidence sequences from[Waudby-Smith et al. (2023)](https://arxiv.org/abs/2103.06476) , with a tuning parameter (N*) that you can set to the sample size you expect to reach when you make a decision. Since sequential testing produces wider intervals than fixed-sample testing, you trade a little power for the freedom to peek. At any serious velocity,[catching a bad variation early and shutting it down](https://www.growthbook.io/blog/flavors-of-experimentation-in-growthbook) is worth the small loss in power.


Learn more about[common A/B testing methodologies](https://www.growthbook.io/insights/ab-testing-methodology) .


### 4. Run SRM checks before interpreting results


Each time you conclude an experiment, check for a sample ratio mismatch *before* you read the results. An SRM means the realized traffic split doesn't match what you configured, like a 50/50 test that comes out 53/47. When you detect an SRM, the comparison is invalid, and no metric can be trusted, even the ones that look unaffected. Discard the results, identify and fix the cause, and rerun the experiment. GrowthBook[automatically flags SRM in your experiments](https://docs.growthbook.io/app/experiment-results) , so you can catch it before acting on the results.


### 5. Apply multiple testing corrections


The more metrics or variants you test, the more likely it is that at least one will appear significant by chance. That's the multiple comparisons problem. A correction counters it by tightening your significance threshold as the number of comparisons grows. The two corrections, Holm-Bonferroni and Benjamini-Hochberg, are explained below. Decide on the correction and your primary metrics before launch, not after seeing which ones moved. Choosing after the fact[introduces bias from p-hacking](https://www.growthbook.io/insights/understanding-false-causality-examples) .


Which correction to use depends on how costly a single false positive is.


Holm-Bonferroni is the more conservative option. It raises the bar for calling any result significant, so it's unlikely that even one of your metrics is a false alarm. In statistical terms, it controls the family-wise error rate (FWER), the chance of even one false positive across all your comparisons. The tradeoff is power. A higher bar also means missing some real effects, so use it when a single wrong call is unacceptable, as in pricing, legal, or safety decisions.


Benjamini-Hochberg is the more lenient option. Rather than guard against every false alarm, it keeps the share of false winners among your significant results low. In statistical terms, it controls the false discovery rate (FDR), the expected proportion of your significant results that are wrong. You catch more real effects in exchange for tolerating a few false positives, so use it when you're monitoring many secondary metrics and can absorb some noise.


Microsoft's Windows experimentation team[evaluated both in production](https://medium.com/data-science-at-microsoft/how-to-better-control-for-false-positives-while-monitoring-your-experiment-62df02e8a451) across 200+ metrics and found Benjamini-Hochberg won on the combined scorecard of precision, recall, and A/A false positive rate, in exactly the many-metric setting where you'd reach for it. GrowthBook supports[both corrections](https://docs.growthbook.io/statistics/multiple-corrections) as an organization-wide default, applied across your goal metrics.


### 6. Validate winners with the causal chain


An increase in your primary metric is strong evidence for a trustworthy win, but not sufficient on its own. A real effect should also show up in the metrics downstream of your primary metric, not just in the metric itself. For example, if the session-to-free-trial rate rose, the session-to-paid-plan rate should rise too, since some of those incremental free trials could reasonably be expected to convert to paid.


Fanatics, which runs close to 100 experiments a month and credits experimentation with about 8% of its annual growth,[caught a false positive this way](https://www.growthbook.io/blog/how-fanatics-made-experimentation-a-strategic-growth-driver) . The team tested removing ads from its product grid pages, and the results showed a positive effect at the 95% confidence level, with higher revenue. But the supporting micro-metrics like products viewed and grid-to-cart all stayed flat, and nothing explained the revenue lift.


The team stopped the experiment, reran it, and the second run found no measurable effect. They have run the same change 6 to 8 times over the years, and it comes back flat every time. It turns out, shoppers simply overlook the ads, so removing them changes neither how people browse nor what they spend.


A win that can't be traced through the behavior that should have produced it is usually a false positive.


### 7. Reduce false positives through variance reduction with CUPED


[CUPED](https://www.growthbook.io/blog/cuped-for-faster-experimentation-in-growthbook) (Controlled-experiment Using Pre-Experiment Data) reduces variance in your results by using each user's pre-experiment behavior to account for noise unrelated to the treatment. Lower variance lets you reach a sound conclusion faster, removing the temptation to peek and stop a slow test early.


[Netflix reported](https://www.kdd.org/kdd2016/papers/files/adp0945-xieA.pdf) a roughly 40% reduction in variance for key engagement metrics, and Microsoft found CUPED to be equivalent to adding roughly 20% more traffic to an experiment. CUPED has one important limitation. It only helps when you have pre-experiment data correlated with your metric, so it does little for brand-new users or rarely observed events. Where it applies, faster experiments with lower variance mean fewer false positives. GrowthBook automatically applies CUPED to every metric as an organization-wide default, so variance reduction is built in.


## How GrowthBook handles false positives by design


The[best experimentation platforms](https://www.growthbook.io/blog/best-a-b-testing-platforms) offer multiple features to prevent false positives. They let you monitor results mid-test without inflating your false positive rate, they correct for multiple comparisons, they run SRM checks automatically on every experiment, and they reduce variance so experiments reach significance sooner and there's less reason to peek in the first place.


GrowthBook's[experimentation platform](https://www.growthbook.io/products/experimentation) supports all of these features.[Sequential testing](https://docs.growthbook.io/statistics/sequential) , included in[Pro and Enterprise plans](https://www.growthbook.io/pricing) , handles safe peeking, with the confidence sequences and configurable tuning parameter described earlier.[Multiple testing corrections](https://docs.growthbook.io/statistics/multiple-corrections) (Holm-Bonferroni and Benjamini-Hochberg) ship as organization-level defaults.[SRM detection](https://docs.growthbook.io/kb/experiments/troubleshooting-experiments) runs automatically on your experiments.[CUPED](https://docs.growthbook.io/statistics/cuped) , available on Pro and Enterprise plans, cuts variance and experiment duration.


GrowthBook's stats engine is also[open source](https://github.com/growthbook/growthbook/tree/main/packages/stats) , so you can read the exact code that produced any result instead of trusting a black box.


## The false positive prevention checklist


Use this checklist to reduce your false positive rate in every experiment, especially as your[experimentation program](https://docs.growthbook.io/using/programs) scales.


- Pre-commit to your sample size and stopping date before launch.


- Calibrate your setup with an A/A test on new surfaces and after tracking changes.


- Turn on sequential testing if you need to watch results while the test runs.


- Check for SRM before you interpret any metric.


- Apply multiple testing corrections: Benjamini-Hochberg for many metrics, and Holm-Bonferroni when a single false positive is unacceptable.


- Confirm every winner against its full causal chain.


- Reduce variance with CUPED to take the pressure off peeking in the first place.


False positives can slip in at any stage of an experiment, from[experimental design](https://www.growthbook.io/blog/7-steps-to-better-experiment-design) through implementation and analysis. The 7 controls above each close a different gap, and they compound, so teams that run all of them get far more trustworthy results than teams that run only 1 or 2.


Want these controls on by default? GrowthBook’s[experimentation platform](https://www.growthbook.io/products/experimentation) runs SRM checks automatically and lets you enable multiple corrections, sequential testing, and CUPED as organization defaults.


If you’re looking to run high-velocity experimentation,[try GrowthBook for free](https://app.growthbook.io/) or[book a demo](https://www.growthbook.io/demo) with our team.
