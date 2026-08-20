---
schema_version: "1.0.0"
document_id: "7a3cfa07917a061c72c469eb271e8bfb16ba905a36c770b87afc7443bdc6483a"
company_key: "yc-edexia"
company: "Edexia"
source_id: "yc-edexia-rss-6002ee771455"
canonical_url: "https://edexia.com/research/comparison-is-key"
published_at: "2025-11-02T00:00:00+00:00"
first_seen_at: "2026-07-27T02:02:54.491511+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:0cfaccd7e86d2695754225eea6e9e565b6f8a547752f1fba52b6d331f2bb264f"
---

# Comparison is Key

If you want to know how people feel about a set of things (foods, brands, crimes, etc.), don’t ask them to rate each on a fixed scale. Instead, have them choose between pairs, over and over.


This idea was first published and mathematically supported almost a century ago (Thurstone, 1927), and it remains a strong method for turning highly subjective fields into frameworks for more objective scoring.


Education is the field that tries hardest to make subjective judgement objective, primarily through rubrics. All modern-day classroom assessments come with rubrics which aim to define what each level on the scale looks like. Good rubric design and human training can lead to high inter-rater reliability, with QWK as high as 0.95, found among ETS examiners (Wendler, Glazer & Cline, 2019). But many rubrics are not designed with such rigor, and teachers do not receive the same level of training which examiners do, at which point it can become much easier to agree on which response is better, than to agree how a response meets the terms of the rubric.


Even with good rubrics, high-stakes examining bodies ensure reliability by having multiple people grade each submission – and this is where comparison pulls ahead. Ofqual (Holmes, Black & Morin, 2020) compared examiner accuracy on AS History against the judgement of principal examiners, across 3 organizations administering the UK’s A-levels. With traditional marking, a single examiner achieved a Spearman’s Rho of 0.47; averaging two examiners raised it to 0.54, three to 0.57, and eight to 0.62 – more opinions, higher accuracy. But when the same teams made pair-wise judgements on random pairs, the combined results exceeded a Spearman’s Rho of 0.65 with the equivalent of two examiners, 0.80 with eight, and 0.85 with twelve. Among groups of humans, synthesizing comparisons is consistently more accurate than direct grading.


Not only is a comparison judgement intuitively easier to perform, and hence itself more accurate, but comparison also unlocks exponential data scaling – a property especially valuable across teams of humans, or AI systems.


The Ofqual study involved 60 essays for each organization. If an examiner directly generates grades for each essay, the result is 60 datapoints. If eight people all grade the same 60 essays, the result is 480 datapoints, which you can average together for higher reliability. But if you were to compare each essay to every other essay in the set (comparing each essay to 59 others), the result would be 1,770 unique comparisons, which you could use to generate the most accurate rank ordering with math. The twelve examiners in the Ofqual study only performed 60 random comparisons each, totaling 720 – less than half of the possible combinations – and this still yielded significantly higher accuracy.


The strength of comparison is that the available datapoints grow exponentially relative to the source data, and each comparison between essays creates a relationship which helps position every other essay in an interconnected network. This network is also much more forgiving of occasional or even frequently inaccurate judgements. Because each essay in a set of 60 gets compared up to 59 times, the general sentiment should still be well captured if a few judgements are inaccurate.


While this is effective for human teams striving for accuracy, it is infeasible in live examination scenarios to deploy teams of examiners large enough to judge a significant portion of possible comparisons. Thousands of essays quickly become millions of possible combinations, and millions become billions.


But this exponential relationship is perfect for machine learning systems: 60 datapoints become 1,770. Turning fewer than 100 datapoints into well over 1,000 crosses the threshold where conventional machine learning can yield results.
