---
schema_version: "1.0.0"
document_id: "ed7dd9c3a2512173fb41d866083d2aeaa3e54306418200ba324d7719f6117574"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/3-things-every-biologist-should-know-about-p-values"
published_at: "2022-08-04T07:00:00+00:00"
first_seen_at: "2026-07-29T00:41:24.192337+00:00"
fetched_at: "2026-07-29T00:41:27.899557+00:00"
content_hash: "sha256:f77a962a2ec9b51d679424e5f4c28e31c5229e2b356225f17db3321d7470d4e8"
---

# 3 things every biologist should know about p-values

Let's admit it: many of us feel out of our comfort zone when we encounter the word: "p-value", but statistical analysis is unavoidable when we're trying to understand biological data. In this post, PipeBio's bioinformaticians line up some of their favorite facts about p-values.


## Fact 1: You can expect to see some significant p-values even when there is no effect


This is because the "p-value" is the probability of an observation being at least as extreme as what you’ve observed, *under the null hypothesis,* i.e. when there is no effect. So, say you have a p-value cutoff of ⍺ = 0.05. Suppose you now do 100 experiments, where in reality there is no true effect whatsoever in any of the experiments. You can expect that in 5 of those experiments you’ll still get a p-value < 0.05 just by chance. In those 5% of the cases you will incorrectly claim statistical significance, and detect an effect when there is none. This is also known as a false positive, or Type I error.


### How can I avoid Type I errors?


It is impossible to completely avoid Type I errors when doing statistical testing. If you are just doing a single statistical test, then you need to decide what degree of risk you’re willing to accept of making a Type I error, and set your p-value cutoff accordingly. A lower p-value cutoff will minimize your risk of falsely detecting an effect that is not there, at the cost of losing sensitivity to detect an effect that is there (Type II error).


If you are doing multiple independent statistical tests, such as in the case of a differential enrichment analysis of many sequence features (e.g. clusters or genes), then you can use a multiple testing correction method to adjust the p-values and reduce the number of Type I errors present in the whole dataset. Common to all these methods is that the p-values are increased, so fewer features will meet the cutoff for significance. Two commonly used p-value correction methods are:


1.


The Bonferroni correction method, which will reduce the probability of making even a single Type I error, and is thus one of the strictest methods. Choosing a Bonferroni p-value cutoff of 5% means that the probability of a single Type I error in the entire dataset is just 5%.


2.


The Benjamini-Hochberg FDR correction method (sometimes referred to as “BH” or just “FDR p-value”) is less strict than the Bonferroni method. If you choose a Benjamini-Hochberg FDR p-value cutoff of 5%, then you can expect that 5% of the positive calls will actually be false positives.


## Fact 2: P-values decrease with increasing sample size.


Suppose you wish to find out whether a candidate drug helps prevent death from a disease. You compare a group of people with the disease who took the drug, with a group of people with the disease who didn’t take the drug. In your first experiment, the numbers look like this:


Recovered


Died


Took drug


20,003


997


Didn’t take drug


20,000


1,000


You may recognise this as a contingency table for a[Chi-square test](https://en.wikipedia.org/wiki/Chi-squared_test) . Without going into the details of the Chi-square test (there are many software packages that can do this calculation automatically), the p-value is 0.9452, so the null hypothesis cannot be rejected at 5% significance: the observations are consistent with the hypothesis that the drug had no effect on survival.


Now let’s see what happens if we increase the sample size by quite a lot, but still observe the same proportions of recoveries and deaths. (For illustration purposes, we just multiply all numbers in the table by 1000):


Recovered


Died


Took drug


20,003,000


997,000


Didn’t take drug


20,000,000


1,000,000


The p-value under the Chi-square test is now 0.0296 < 0.05, so the null hypothesis is rejected, and now it looks like the drug did have a good effect on survival!


Wait, what just happened? As you increase the sample size, you can get more and more confident that the same proportional difference you see in the dataset is actually real. The p-values will decrease with increasing sample size. This, by the way, is a general property of p-values and will apply to any statistical test. The “truth”, if you can call it that in this theoretical experiment, is that the drug did actually have an effect, but this effect was minimal: it only prevented 3 out of 1000 deaths.


### How do I know if there is an effect?


P-values can never stand alone: you must also always look at the effect size. Even if the p-values suggest a “statistically significant” effect, it may not be very interesting biologically. A somewhat cynical rule-of-thumb is: if you can’t see an effect with the naked eye, it’s probably not interesting, even if it is there.


There are many possible ways to measure the effect size. The most useful measure will depend on the study. Two common measures are:


1.


The “[odds ratio](https://en.wikipedia.org/wiki/Odds_ratio) ” (abbreviated OR), typically reported in clinical studies, quantifies the strength of an association. Larger absolute odds ratios are typically more biologically interesting than odds ratios close to 1.0.


2.


Fold-changes are often used in genetics studies, to quantify the degree of enrichment for each tested feature (cluster, gene etc.). Larger absolute fold-changes indicate a larger effect size, and will typically be more biologically interesting than smaller fold-changes.


The most interesting features will have both significant p-values and large fold-changes. The volcano plot is a visual representation of the (logarithm) of the fold-changes on the x-axis and the (negative logarithm) of the p-values on the y-axis, enabling you to find the features that fulfil both criteria at once. The “most interesting” features will be located in the top of the plot, corresponding to the lowest p-values, and far out to the left (overexpressed features) or right (underexpressed features) side, corresponding to the most extreme absolute value of fold-changes.


If you are using[PipeBio](https://pipebio.com/) for differential enrichment analysis, the volcano plot is automatically produced as part of the analysis, so you can easily find the features in your dataset that are both statistically significant and biologically interesting.


**Figure 1.** Volcano plot showing p-values and fold-changes for a panning experiment in PipeBio


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Fact 3: Replicates are invaluable if you want accurate results, but more is not always better.


Variations in a biological dataset are always due to a combination of factors:


1.


“Real-world” effects, which may be:


2.


biologically interesting, such as differential enrichment of certain features due to changes in a biological condition


3.


less interesting, due to factors unrelated to the question we’re asking, including systematic errors in our experimental process


4.


Statistical errors due to limited sample sizes: we cannot exhaustively cover the full population with our data.


One way to think about statistical analysis is that its purpose is to disentangle those sources of variations in the data, such that the “biologically interesting” effects can be revealed.


Statistical errors can be reduced if you sample more, because you get closer and closer to exhaustively covering the full population. And, as we have seen in the previous example, this results in the statistical test becoming more and more sensitive to detecting the “real-world” effects (whether they are interesting or not). If you are doing differential enrichment analysis, “sampling more” corresponds to sequencing more and more reads. With more reads, you can be more and more confident in the fold-changes you observe. For example, you can be more confident about a 2-times enrichment if you have seen it with 200,000 reads vs. 100,000 reads, than if you have seen it with 2 reads vs. 1 read.


The “less interesting real-world effects” will, however, not disappear if you sample more. And it turns out that biological experiments are often very prone to such effects, which can give us a false picture of the biological system we are modelling. This is why technical and biological replicates are of critical importance.‍


### Replicates in differential enrichment analysis


Let’s take the example of differential enrichment analysis. Even if your experiment is not subject to the worst kinds of systematic errors, a “less interesting real-world effect” is that the expression of each biological feature has a natural, “normal” variation in biological samples, which is not known.


Suppose you just compare one sample from condition A with another sample from condition B. Suppose also that the expression of feature X is increased 2-fold in sample B. Now, you can do a statistical test to determine whether this difference is statistically significant. But you cannot know if it occurred because you changed conditions, or just because you had two different samples and there was a natural variation in this feature across samples even under the same condition.


This is where replicates can help. Even a single replicate in one of the conditions provides information about the natural variation of each feature. The most popular statistical methods for the analysis of biological count data (such as[edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html) and[DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html) ) employ a statistical model where the “uninteresting”, normal biological variation is estimated from the replicates. This way, the “interesting” variation due to differential enrichment can be revealed.


As a bonus, using replicates you can also discover if something went horribly wrong in one of your samples due to the experimental procedure. So, there is no doubt you really ought to have them.


### How many replicates should I have?


Now this is a very difficult question to answer. Clearly, high-quality replicates can improve both the false positive rate and the false negative rate of statistical testing. But replicates are also expensive, so is your money best spent on an extra replicate?


A rule of thumb is that the first replicate you add will result in the most dramatic improvement in the quality of your statistics, compared to having no replicates at all. It enables the statistical method to factor out some of the "uninteresting" variation in your data. So if it is at all possible, add a replicate to your experiment.


Furthermore, always choose a biological replicate rather than a technical replicate, because the biological variation is typically much greater than technical variation. As an example, in a simulated mouse single-cell gene expression RNA sequencing experiment,[Blainey et al.](https://www.nature.com/articles/nmeth.3091) calculated that going from 8 animals, 6 cells each with 3 technical replicates per cell to 12 animals, 12 cells per animal but only 1 replicate doubled the power to detect a 2-fold change in variance. In both cases, the total number of samples was 144, but the quality of the replication is better when technical replicates are replaced by biological replicates.


Number of animals


Number of cells per animal


Number of technical replicates per cell


Power to detect 2-fold change in variance


8


6


3


0.43


12


12


1


0.88


Simulation data from[Blainey et al.](https://www.nature.com/articles/nmeth.3091) shows that increasing the number of biological replicates dramatically increases the statistical power, even when the number of technical replicates is reduced.


Adding more and more replicates to each of your comparison groups will further increase statistical power, but there comes a point of diminishing returns. With a very large number of replicates, you may be able to detect a very tiny difference between groups, but that difference may not be biologically interesting to you.


When selecting sequences in a biopanning experiment for example, the challenge is usually that there are *too many* sequence candidates for further analysis, rather than too few. In this case, the specificity of the statistical method is most important, so that the few top-scoring sequences correlate with the most interesting biological effect. There is less need for the method to be sensitive to smaller changes.


The well-balanced experimental design will include biological replicates, at least for the pre-panning group, so the baseline variation for each sequence cluster can be modelled. However, 3-6 biological replicates per group are likely to be enough to detect the most interesting candidates.
