---
schema_version: "1.0.0"
document_id: "ffe5e87f922d85c717c2812939261576fc6ffe1e931826ce5c0631d26a12fd65"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/effect-distribution-in-experimentation/"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:f850704e17a7a2ebe2163aec995d3d995be97eb48404cd669ab893f0e1117bc1"
---

# The effect distribution: The missing piece in experimentation programs

Tyler Buffington


A lesson we’ve learned in experimentation at Datadog is how easy it is to fall into interpretative pitfalls even when following rigorous conventions. For example, consider an experimentation program that appears to do everything right on the surface. The team pre-determines experiment sample sizes to provide a small minimal detectable effect (MDE) for a product change, waits until those sample sizes have been reached to make decisions, and ships only experiments that are statistically significant at the 95% confidence level.


The problem becomes apparent when the team looks at multiple experiments in aggregate. Recall what a 95% confidence level guarantees: Even when a change has no true effect, the test will still flag it as significant 5% of the time purely by chance. If only 5% of the experiments are statistically significant, the program has the same rate as one that tests exclusively changes that have zero true effects. In such a scenario, every winning experiment is a mirage despite the team using statistical methods that control the false positive rate (under the null hypothesis).


This example illustrates the importance of the *effect distribution* , or the distribution of *true* effects across a set of related experiments. In our previous example, the results are consistent with an effect distribution that is a point mass concentrated exactly at zero, which is equivalent to the distribution under the null hypothesis of no effect. In this case, it’s clear that the experimentation program would be chasing its own tail because there are no true wins, and common statistical methods fail us because any significant result is a false positive. An individual significant result might seem convincing in isolation, but it’s far less so with the context that 5% of the tests from the program are significant.


Virtually all experimentation programs can benefit from understanding their effect distributions and how they should inform testing strategies. In this post, we’ll discuss the challenges and key values of using the effect distribution in your experiments.


## Estimating the effect distribution


The main challenge of effect distribution is that it describes *true* effects, which we never observe directly in individual experiments. Instead, we estimate from *observed* effects, which are subject to sampling error. It’s tempting to use the distribution of observed effects as a proxy to the distribution of true effects, but the distribution of observed effects often leads to exaggerated expectations about the realistic magnitude of true effects.


To illustrate this, we simulated one million experiments. For each experiment, we first drew a random true effect from a distribution, used a random sample size between 10,000 and 100,000 users per variant, and then drew an observed effect based on the simulated true effect and sample size.


This result illustrates how large effects can seem far more common than they are. For example, observed effects (blue distribution) exceeding 5% are somewhat common, but the true effect is almost never so large. This can give teams a miscalibrated sense of realistic effect magnitudes, leading to poor experimental designs. If we see only the blue distribution, it seems reasonable to use an MDE of 5% in a sample size calculation since there would be many historical examples of winning experiments with such a large observed lift. However, the resulting experiments would be underpowered for the true effect, leading to exaggerated impact estimates (winner’s curse) and a higher rate of directionally misinformed decisions ([Type S errors](https://forrt.org/glossary/english/type_s_error/) ).


Even though we cannot observe the true effect directly in any single experiment, there are techniques that allow for estimating the distribution of true effects from multiple experiments. We believe that such methods unlock powerful program-level insights that improve the impact of experimentation programs in multiple ways.


## Quantifying the value of experimentation with the effect distribution


A compelling aspect of using the effect distribution in experiment programs is that it unlocks decision analysis concepts such as the[expected value of sample information (EVSI)](https://en.wikipedia.org/wiki/Expected_value_of_sample_information) . The idea is that the effect distribution can help us understand how much better off we are with the information from an experiment than without it.


At a high level, the framework for estimating EVSI is as follows:


1.


Treat each proposed product change as a draw from the effect distribution (a true effect).


2.


The measured lift in an experiment is a draw from the sampling distribution, conditional on the true effect from step 1.


3.


Apply a decision rule (e.g., ship if significant) on the measured lift from step 2.


4.


If you ship, the product realizes the true effect from step 1; if you don’t ship, the realized effect is zero.


By simulating steps 1–4 repeatedly, we can gain insight into the average realized effect evaluated across all simulated scenarios—both with and without the experiment. This requires defining *a default decision policy* , or in other words: What would we do if we couldn’t run experiments? In most cases, the default decision policy would be to ship changes that a team believes are good, but without running experiments, many changes would fail to improve (or may even harm) key metrics. The value of experimentation comes from filtering the good ideas from the bad.


To build intuition for EVSI, consider the limiting case in which experiments provide perfect information. This means that the results from experiments have no uncertainty (i.e., an infinite sample size), like a clairvoyant who can reveal the true impact of a product change with perfect accuracy. The resulting[value of clairvoyance](https://en.wikipedia.org/wiki/Value_of_information) establishes the upper bound of the value of a real experiment.


Let’s assume an example effect distribution that is centered at zero. This distribution captures the fact that some of our proposed changes improve our primary metric (such as revenue per user) and others hurt it. With a perfect experiment, we have the luxury of shipping an idea only if it has a positive effect on our metric.


Because our effect distribution is centered at zero, the expected effect we would ship to the product without experimentation would be zero. However, if we were to somehow know the true effect before making a release decision, we would only ship the positive-effect changes. The above example yields a distribution with an expected value of about 2% due to the fact that the negative-effect scenarios in the left plot are shifted to zero. This framework directly quantifies the value of information in terms of an expected lift shipped to a product: The distribution *with information* is linked to an average shipped effect size about two percentage points larger than the distribution *without information* .


In reality, experiments do not provide perfect information, so a realistic EVSI calculation is more complicated due to potential false positives and false negatives that arise due to measurement error. This topic is worthy of a[more in-depth discussion](https://dpananos.github.io/posts/2026-02-20-time/) , but the limiting case is helpful to illustrate how the value of experimentation comes from its role as a filter on the effect distribution.


## Applying the effect distribution for resource allocation


Instead of estimating a single effect distribution for all tests run by an experimentation program, we can also estimate separate distributions for meaningful categories of experiments. These analyses can provide insight into which types of product changes provide the biggest opportunity for experimentation.


For example, an experimentation program might estimate the effect distribution for two different product categories of experiments: customer service experiments and search ranking experiments. Let’s assume the search ranking algorithm in our example is already heavily optimized, so it is difficult to make changes that result in meaningful improvements compared to the customer service experience.


The effect distribution for the search ranking experiments is centered at a small negative value and has a relatively small variance. Conversely, there is a much larger variance associated with the customer service experiments, indicating both a larger upside and larger downside. Based on the concept of EVSI, the maximum value per experiment is higher for the customer support experiments. That’s because visually, the process of moving all density associated with negative effects to 0% would yield a higher expected effect size for the blue distribution than for the purple distribution. As a result, leadership might choose to allocate more resources into customer support experiments over search ranking experiments.


## Inform experimentation strategies with the effect distribution


The effect distribution turns experimentation from a series of isolated decisions into a program-level asset. It reveals when significant wins are indistinguishable from noise, guards against inflated expectations from treating observed effects as true ones, and quantifies what each experiment is worth so teams can invest where the upside is largest. Using the empirical effect distribution as a prior can offer benefits (including winner’s curse mitigation), but teams who prefer frequentist experiment analyses can also benefit from estimating effect distributions. Netflix’s paper[Optimizing Returns from Experimentation Programs](https://arxiv.org/html/2412.05508v1) uses the effect distribution to estimate optimal (frequentist) p-value thresholds. Another popular framework is the one presented in the[A/B Testing Intuition Busters](https://exp-platform.com/abtestingintuitionbusters/) paper, which contextualizes p-values with a false positive risk calculated from estimated success rates (essentially a binarized effect distribution). Regardless of one’s preferred statistical framework, there is significant strategic value in understanding an experimentation program’s effect distributions.


To learn more about running your own experiments with Datadog,[check out our documentation](https://docs.datadoghq.com/experiments/) . If you’re new to Datadog,get started with a 14-day free trial .


*Thank you to Lukas Goetz-Weiss and Demetri Pananos for the discussions that inspired this post, as well as helpful feedback on an earlier draft.*


-
-
-
