---
schema_version: "1.0.0"
document_id: "5d0ab7116a2d13de31a25fb5aef7fa46b6f7234689f7419fff5262f12ba5beef"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/bed-sharing"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:7e057af4c97dbb541f7d5a433cac6f1f63aab82d63797b88377f6ab3353f9832"
---

# Do People Sleep Better Alone or Together? | Terra

## Within-Person (Same User: Shared-Bed vs Alone Nights)


The above cohort analysis compares *different people* . A stricter question is whether the user, on nights recorded as shared bed, looks different from nights recorded as slept alone. After restricting to people who have both contexts often enough in their history that person-level averages are not pure noise.


I restricted to people with over 3 nights for both labels, and the results were very similar to the population-level analysis. Awake time was the clearest within-person signal, increasing by 8 minutes on shared-bed nights, which is nearly mirrored by a 6-minute decrease in sleep time. In this analysis, I found an increase in resting HR (2.3 bpm, not significant) and a very slight decrease in RMSSD (not significant). These results, along with an investigation of the direction of these relationships for each user in the analysis, show mixed findings, indicating that the data is very noisy.


Dominant context: ≥75% shared vs ≥75% alone (user-level)


Here I am attempting to answer the question: Do people whose history is dominated by one context differ from people dominated by the other?


For each user, I computed the percentage of nights with shared beds and categorized ≥75% shared as “mostly shared,” and ≤25% shared (≥75% alone) as “mostly alone”. I then compared each person’s long-run mean across all their nights (using a Mann–Whitney test on user-level means).


The relationships were the same as in the night-level story: more awake time, slightly less asleep time; lower readiness for shared-dominant; and small HR differences.


One of the big learnings from the previous research and recent literature on sleep is that consistency in health metrics (as measured by the Coefficient of variation, CV) can be more informative than the raw signal. Sleep length CV tended lower (more stable night-to-night) for alone-dominant users when CV was computed with enough nights per person ( ≥50night-per-metric rule for inclusion).


***Figure 2:** Shows the % change distribution for each variable. The different shapes of the distributions are particularly interesting, as they demonstrate how the mean differences between cohorts are driven.*


## Three-Way Cohort


Here, I added a third category to the analysis: Mixed (25–75% shared). The signals from the previous analysis all held


Awake time was highest in mostly shared, middle in mixed, and lowest in mostly alone (70, 66, 57 minutes, respectively). Asleep time was lowest in the mostly shared cohort, middle mixed, and highest in the mostly alone (6.9, 7.0, 7.3 hours). Time in bed: slightly longer, mostly alone (~8.3 h vs ~8.2 h).


Resting HR still sleeps down toward mostly alone with mixed sat in the middle, and max sleep HR is highest when mostly shared.


***Figure 3:** shows cohort distribution using ridge lines of night-level asleep and awake distributions by group, with mean and median verticals. It demonstrates that the cohort-mean increase in time awake in bed is driven by a long tail, especially in the most-shared cohort.*


## **Checking for Differences**


The above analyses show differences between cohorts, but I wanted to know whether these patterns are robust enough to describe. I compared every shared-bed night to every alone night with Welch’s *t* -test—a two-sample test that asks whether the average on one side differs from the average on the other, without assuming both piles have the same spread. That is a population view: powerful, but it lumps many different people and life situations into two big buckets.


A stricter check looked within the same user: for people who had logged both contexts often enough, I took each person’s typical shared-bed night and typical alone night and computed the difference, then used a Wilcoxon signed-rank test to see whether those person-level gaps cluster around zero.


That test is nonparametric; it works on ranks rather than assuming perfect bell curves, and it is appropriate when you care whether “most people go the same way” rather than whether two perfect normal distributions differ. Here, the signed-rank result for awake time was the stand-out: roughly ten more minutes awake on shared-bed nights for the typical person in that slice, with most individuals pointing in the same direction.


For the two lifestyle cohort analysis, I used the Mann–Whitney *U* test (another rank-based, nonparametric test). For the three-way lifestyle cohort analysis, I used the Kruskal–Wallis test, the three-group version of the same idea. These tests measure whether at least one group tends to sit higher or lower than the others.


Strong separation for awake and asleep time produced very small *p* -values and medians that stepped down in order (about 70, 66, 57 minutes awake from mostly-shared to mixed to mostly-alone). RMSSD and heart rate were weak because the three-group tests did not show a reliable shift, HRV medians fell within a similar band, and heart rate differences were small or ambiguous.


What I am trying to test here is whether bed context is associated with systematic differences in sleep structure. Minutes awake (and, more quietly, minutes asleep) are the story, while HRV and HR do not separate cleanly in this dataset. The tests support treating time in bed as the evidence; they do not, by themselves, show that a partner *causes* the difference, only that, in this observational record, shared-bed context and more fragmented nights tend to travel together.


## Time to Head to the Spare Room?


These are associations, not trials. We always have to bear in mind that wearable-derived variables are tracking life in the real world, and all that entails: partners, travel, children, illness, and much more. The mixed cohort is the nearest thing to a crossover group; on timing, it sits between extremes, which I think is a good sign that we are picking up a directionally correct signal.


If you are designing a study rather than reading a blog, the honest next step is to model within-person changes around partner travel, illness, and alcohol, covariates that this dataset may capture, but we don’t know exist!


Thinking of going to sleep in the spare room tonight? I wouldn’t bother unless you’re a hardcore recovery nut, and you’re going to test your own sleep data over 10 plus nights of shared bed and sleeping alone, while controlling as many other factors as you can. If you do collect this data, let me know!


#### References


1. Grosicki, G.J., Carter, J.R., Laursen, P.B., Plews, D.J., Altini, M., Galpin, A.J., Fielding, F., von Hippel, W., Chapman, C., Jasinski, S.R., Beattie, U.K. and Holmes, K.E. (2026) ‘Heart rate variability coefficient of variation during sleep as a digital biomarker that reflects behavior and varies by age and sex’, *American Journal of Physiology-Heart and Circulatory Physiology* , 330(1), pp. H187–H199.[https://journals.physiology.org/doi/full/10.1152/ajpheart.00738.2025](https://journals.physiology.org/doi/full/10.1152/ajpheart.00738.2025)
2. Drews, H.J., Wallot, S., Brysch, P., et al. (2020) ‘Bed-Sharing in Couples Is Associated With Increased and Stabilized REM Sleep and Sleep-Stage Synchronization’, *Frontiers in Psychiatry* , 11, 583.[https://pmc.ncbi.nlm.nih.gov/articles/PMC7330166/](https://pmc.ncbi.nlm.nih.gov/articles/PMC7330166/)
3. Andre, C.J., et al. (2021) ‘The effects of bed sharing on sleep: From partners to pets’, *Sleep Health* , 7(1), pp. 49–57.[https://www.sciencedirect.com/science/article/pii/S2352721820303053](https://www.sciencedirect.com/science/article/pii/S2352721820303053)
4. Fuentes, B., Kennedy, K., Killgore, W.D.S., et al. (2022) ‘0010 Bed Sharing Versus Sleeping Alone Associated with Sleep Health and Mental Health’, *Sleep* , 45(Supplement_1), A4.[https://academic.oup.com/sleep/article/45/Supplement_1/A4/6592562](https://academic.oup.com/sleep/article/45/Supplement_1/A4/6592562)
5. Levy Andersen, M., et al. (2025) ‘Interactions between co-sleeping, sleep quality, and relationship health: The benefits and challenges of shared sleep environments’, *Journal of Health Psychology* .[https://journals.sagepub.com/doi/10.1177/13591053251356355](https://journals.sagepub.com/doi/10.1177/13591053251356355)
6. ‘Adults sleep better together than they do alone’ (2022) Sleep Research Society / University of Arizona (press release on Fuentes et al. study).[https://www.sciencedaily.com/releases/2022/06/220606181217.htm](https://www.sciencedaily.com/releases/2022/06/220606181217.htm)
