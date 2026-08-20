---
schema_version: "1.0.0"
document_id: "1c37fdf3e52d213847765ea5168758ec87024a1a6e01addaea0cc87111b49546"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/lessons-learned-from-ronny-kohavi-and-luke-sonnet-running-trustworthy-experiments"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-23T22:17:24.982400+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:77dcc019e0fc36f52365b5e6d3f22ccc2bfd898f13ce3e28ece1c0a1ae2c6177"
---

# Lessons learned from Ronny Kohavi and Luke Sonnet: running trustworthy experiments

GrowthBook recently hosted a webinar with two people who have spent their careers on one hard problem: how do you know an A/B test result is real? Ronny Kohavi is the co-author of *Trustworthy Online Controlled Experiments* and previously led experimentation at Amazon, Microsoft (where his teams eventually ran about 300 new treatments every workday), and Airbnb. Luke Sonnet is head of experimentation at[GrowthBook](https://www.growthbook.io/products/experimentation) , where he leads the team building the statistics, metrics, and analysis behind the platform.


The theme was simple and a little uncomfortable: getting numbers is easy, getting numbers you can trust is hard. That line comes from an article Ronny co-wrote in 2010, and it still describes most published test results. Running an A/B test looks trivial. You ship the change, read the p-value, celebrate the lift. Underneath sit a dozen gotchas that quietly turn a "win" into noise.


This post distills what we took away: how easy it is to run an untrustworthy experiment, the steps that can improve trust during setup, the checks to run after it finishes, and the best questions from the live Q&A. You can watch the full webinar[here](https://www.growthbook.io/events/trustworthy-ab-testing) or download the slides[here](https://hs-8863617.f.hubspotemail.net/hubfs/8863617/Trustworthy_AB_Webinar.pdf) .


## 1. It's surprisingly easy to run an untrustworthy experiment


Ronny opened with a John F. Kennedy line: we choose to go to the moon "not because it is easy, but because it is hard." His twist on it describes what he sees constantly: too many teams run experiments not because it is easy, *but because they thought it would be easy.*


To make it concrete, he dissected a real, recently published experiment. The claims were impressive. A 44.8% lift, with a conversion rate jumping from about 55.7% to roughly 80%. "99% confidence," which the author defined as being 99% certain the result reflected a genuine difference and not a statistical accident. A 50/50 split with 54,000 users per variant. And an AI-powered treatment that, in the write-up, "smashed it out of the park." It was, as Ronny put it, buzzword compliant.


Then he took it apart. Four things were wrong, and every one of them is common:


- **The real sample was tiny.** The 54,000 "users per variant" counted everyone who entered the site. Only a small fraction actually reached the page that changed. That triggered population, the only one that matters for the test, was about 300 users total, split 176 and 124.
- **No power calculation.** With those numbers, the test had roughly 7% power instead of the standard 80%. In a low-power test, a significant result is almost guaranteed to exaggerate the true effect, here by an expected five times, and there was nearly a 10% chance the result pointed in the wrong direction entirely.
- **The p-value was misread.** "99% confidence" was treated as "99% chance the result is real." It is not. Accounting for a realistic win rate, the false positive risk on this result was about 87%.
- **Sample ratio mismatch.** A 50/50 design that produces 176 vs 124 has a p-value of about 0.003. The split being wrong was more statistically significant than the "win" itself.


The uncomfortable takeaway: this was not a uniquely bad experiment. It looks like a large share of the A/B test results published today. The rest of this post is about not being that example.


## 2. Steps to take during setup to make your test more trustworthy


Most trust problems are decided before a single user is bucketed. Here is what Ronny and Luke recommend building into setup.


### Run a power calculation, and respect what it tells you


Power analysis gives you the minimum sample size you need. It takes four inputs: your baseline variance (or conversion rate), alpha (industry standard 0.05), power (industry standard 80%), and the minimum detectable effect, or MDE. The first three are easy. The MDE is where teams go wrong.


The MDE is the smallest effect you want to be able to detect, and reality is humbling. Real average treatment effects are small. At Bing, across tens of thousands of experiments, the average effect was rarely above 0.3%. Airbnb Search’s successful experiments improved conversion by about 0.3%. One widely cited toolkit analysis found a median lift of 0.1% across more than a thousand experiments. Ronny’s rule of thumb: never set an MDE above 5%. Anything higher is unreasonably optimistic, because results above 5% almost never happen unless the product is broken.


The catch is that small MDEs demand large samples, and many teams do not have them. That is a real trade-off, not a moral failing, but you have to make it consciously. Ronny’s own community project, Trustworthy A/B Patterns, settled on a 2% MDE because that matched the sample sizes available.


To ground it, here is roughly how many users you need to detect a 5% relative change, by baseline conversion rate:


**Baseline conversion rate** **Users needed (total)**


50% (e.g. checkout completion) 12,000


20% (add-to-cart to checkout) 51,000


10% (initiate checkout) 100,000


5% (typical site conversion) 240,000


2% (expensive checkout) 600,000


1% (rare event) 1,300,000


Ronny’s blunt summary: if you only have around 10,000 users, you are nowhere unless your metric converts at around 50%.


### Pick your significance threshold on purpose


Luke’s framing is that trust in an experiment is built on transparency and reliability. Statistical significance is the signal most teams lean on, and you control the threshold, often without realizing it. The default alpha of 0.05 says that if there is no real effect, 5% of experiments will look significant anyway. But why 0.05 for everything?


The better question is how costly a wrong call is. A feature that will dictate your roadmap for three years, or that needs ongoing support, deserves a stricter threshold like 0.01 or 0.001. A small, low-consequence change can tolerate a more relaxed one. The number should reflect the risk you are actually willing to take, not habit.


### Plan against peeking and metric-shopping before the data can tempt you


Two of the most damaging habits are either baked in during setup or prevented there. Peeking, which means checking results and stopping the moment something looks significant, or simply running longer until it does, inflates your false positive rate. Running two weeks and then checking daily for another two weeks pushes a 5% Type 1 error rate up to about 17%, and it inflates the estimated effect size on top of that. Testing many metrics has the same effect: with one decision metric your error rate is 5%, with two it is 10%, with three about 14%.


If you know you will want to look early, plan for it. Use an alpha-spending approach that only looks at pre-set intervals, or sequential testing if you have robust enough data infrastructure to check continuously. If you genuinely need multiple decision metrics, apply a multiple-comparisons correction. The goal is not to make experimentation impossible; it is to decide the rules before the data can tempt you.


### Setup checklist


- Define your OEC (overall evaluation criterion) and guardrail metrics first; they set your MDE and required sample size.
- Run a power calculation. Do not skip it because the MDE is hard to pick.
- Set your MDE at or below 5%, and lower if your sample allows.
- Size for the triggered population that actually sees the change, not total site traffic.
- Choose alpha based on how costly a wrong decision would be.
- Commit to a fixed runtime or sample size up front, and write it down.
- Aim for a single decision metric; if you need several, plan a correction now.
- If you are underpowered, try variance reduction such as[CUPED](https://docs.growthbook.io/statistics/cuped) before concluding you cannot test.


## 3. What to check after you've run an experiment


A clean setup is not enough. Before you trust a result, run these checks.


### Check for sample ratio mismatch first


This was Ronny’s number one test. If you ran a 50/50 split, you should see roughly equal counts in each variant. When you do not, something is usually broken. He showed a Microsoft experiment split 50.2 / 49.8, which sounds close enough, but with large samples that deviation should occur only about 1 in 500,000 times. Since you did not run 500,000 experiments, you have a problem.


And SRM is not rare. Microsoft found 6% of its experiments had one, years into a mature program, measured against a strict 0.001 threshold. Convert.com found 6.5% after adding the check. One company Ronny consulted for had SRM in 20% of its experiments.


Why it matters so much: an SRM usually means a skewed population snuck in. He showed a Bing experiment with gorgeous results, five key metrics all up, some p-values as small as 2e-10. But the split was 0.497 instead of 0.5. The cause turned out to be a bot. Once they excluded it, none of the five metrics was significant anymore.


Back to the published example: 176 vs 124 on a 50/50 design gives a p-value of about 0.003, so the mismatch was more significant than the reported result. If your tool does not run a[sample ratio mismatch check](https://docs.growthbook.io/) automatically, it is a quick p-value calculation, and it is worth doing at the end of every experiment.


### Compute false positive risk, not just the p-value


"99% confidence" does not mean a 99% chance the result is real. A p-value is a conditional probability: it assumes the null hypothesis is true. What you actually want is the false positive risk, the chance that a statistically significant result is a false positive. That needs one extra input: the prior probability that an idea succeeds. Historical win rates give you that prior.


**Organization** **Success rate**


[Microsoft](https://www.microsoft.com/) (early, heavy QA) 33%


Avinash Kaushik (estimate) 20%


[Bing](https://www.bing.com/) 15%


[Booking](https://www.booking.com/) ,[Google Ads](https://ads.google.com/) ,[Netflix](https://www.netflix.com/) ,[Optimizely](https://www.optimizely.com/) ~10%


[Airbnb](https://www.airbnb.com/) Search 8%


At a median success rate and alpha 0.05, you do not have 95% confidence, you have about 22% false positive risk, so roughly 78% confidence. Ship at a looser 0.10, as some tools default to, and more than a third (36%) of your "significant" results are false positives. If you do not know your win rate, Ronny suggests assuming 10%. Applied to the published example, the false positive risk was about 87%.


### Apply Twyman’s Law to anything that looks great


"Any figure that looks interesting or different is usually wrong." When you see a beautiful lift, the instinct is to email the whole company. Do not. Double- and triple-check it first, because extreme results are usually bugs. Ronny has never seen a trustworthy experiment move a real OEC by 44%. It does not happen unless the product is broken, like a checkout that literally cannot complete.


### Run the background sanity checks


- **A/A tests.** Ronny’s top recommendation. Split users into two identical groups with no difference between them. If the system is healthy, a given metric should be significant only about 5% of the time. Better still, run many A/A tests and confirm the p-values are roughly uniform; deviations expose bugs in randomization, variance estimation, or the pipeline.
- **Bot traffic.** Most teams underestimate it. At Bing, 50% of US traffic was bot-generated, and 90% in Russia and China. Yours is probably lower, but bots create skew and SRMs, so filter them.
- **Novelty and primacy effects.** If the treatment effect drifts up or down over time, it may be users reacting to newness rather than a durable effect. It is less common than people fear, since early trends are often just noise, but it is worth watching.


### Post-run checklist


- Run the SRM check and treat a failing one as a red card, not a footnote.
- Compute false positive risk using your real win rate, not just the p-value.
- Do not peek or extend runtime just to reach significance.
- Do not cherry-pick whichever metric happened to turn out significant.
- Apply Twyman’s Law to any unusually large result.
- Confirm A/A tests, bot filtering, and novelty/primacy checks are in place.


## 4. Frequently asked questions from the webinar


**We are a B2B product with low user counts and are always underpowered. Should we just not A/B test?**


No. B2B is genuinely harder; even Microsoft struggled to power tests on products like Office when randomizing by company. But you have levers. Use CUPED variance reduction, which exploits historical data and pays off especially when customers are repeat visitors: it cut required sample size by roughly 50% at Bing and 5 to 10 times at Airbnb. Consider larger MDEs, since B2B changes are often bigger, and accept a longer runtime. Luke’s higher-level point is to fit experiments into your broader information ecosystem. Sometimes the honest goal is just to rule out that you are cratering a key flow, which you can do with a one-tailed non-inferiority test instead of pretending you have precision you do not.


**How do we keep the winner’s curse from burning us?**


Use holdouts. Combine a batch of shipped tests and compare their summed predicted effect against a long-running holdout. If your experiments claimed +10% conversion for the quarter but the holdout shows +5%, that gap tells you how much you are fooling yourself.


**Is Bayesian A/B testing affected by low power? Would it change the 300-user example?**


Not really. You still only have 300 users. Bayesian methods let you bring in prior information, but with uninformative priors the results align closely with frequentist ones. Informative priors only help if you genuinely understand the domain, and in online experiments, believing you have strong, correct priors is usually misleading. Notably, GrowthBook uses weak informative priors over the lift, which actually widens the uncertainty bounds and makes a 44% lift on 300 users harder to claim, exactly the Twyman’s-Law guardrail you want.


**How do we handle ratio metrics (like views per user) when randomization is at the user level?**


Use the Delta method to correct the variance for metrics whose denominator is not the randomization unit. Good tools handle this automatically in both the analysis and the power calculation; GrowthBook, for example, uses your historical data to size ratio metrics correctly.


**If one arm wins on every single day of the week, is that extra evidence it is a real winner?**


A little, but do not invent new rules like "three winning days means ship." Trust the aggregate and its p-value. The more useful version of this habit is the reverse: if an experiment is positive every day and then sharply negative one day, investigate that day for an outage or bug, but only exclude it if you find a real, documented cause.


**In low-power settings, can we use more sensitive proxy metrics, like add-to-cart instead of revenue?**


Yes, if you have a sound mental model of how the proxy relates to the real goal. Both Ronny and Luke are fans of validated surrogate metrics; Ronny’s teams ran experiments specifically to confirm a surrogate’s causal link before trusting it. Just keep an eye on the downstream metric so you are not optimizing add-to-cart while customers fall off later.


## 5. Conclusion


The theme running through the webinar: A/B tests are the gold standard, but trust does not come free. It comes from effort, from the sanity checks, the power calculations, and the discipline to honor the threshold you set. The published "44.8% lift" failed four independent checks at once, and it is representative, not exceptional.


Run the setup checklist before you launch and the post-run checklist before you ship, and you will catch the overwhelming majority of untrustworthy results before they reach a roadmap. If you want a platform that runs SRM checks, CUPED, sequential testing, and proper ratio-metric handling out of the box, you can[start experimenting in GrowthBook](https://www.growthbook.io/get-started) . And to hear the full discussion, including Ronny’s worked calculations,[watch the webinar recording](https://www.growthbook.io/events/trustworthy-ab-testing) or explore his[A/B testing courses](https://maven.com/kohavi/abtesting) on Maven.


‍
