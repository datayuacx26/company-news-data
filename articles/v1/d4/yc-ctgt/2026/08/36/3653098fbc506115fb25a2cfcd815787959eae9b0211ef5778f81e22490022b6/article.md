---
schema_version: "1.0.0"
document_id: "3653098fbc506115fb25a2cfcd815787959eae9b0211ef5778f81e22490022b6"
company_key: "yc-ctgt"
company: "CTGT"
source_id: "yc-ctgt-news-import-358f70d55d44"
canonical_url: "https://www.ctgt.ai/research/v4-flash-0731-drift"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T01:40:46.297130+00:00"
fetched_at: "2026-08-06T01:40:48.132496+00:00"
content_hash: "sha256:0d75d5fc7be407debc558b1bac39bdc074fcfdc54201360eeb49e0a2f49cc971"
---

# DeepSeek’s Official V4 Flash Censors More Than Its Preview, Selectively

On July 31, DeepSeek released` V4-Flash-0731` , the official release replacing the preview build we measured in our[original study](https://www.ctgt.ai/research/distillation-censorship-transfer) . We reran LineageEval against it with the same 152 matched sensitive/control pairs and the same four-judge panel.


#### ‍ **The official build is (selectively) more censored than its preview ‍**


‍ **‍** Mean matched gap rose from +32.0 to +44.0 while median gap rose from +33.3 to +56.1. The share of pairs where the sensitive side scored more censored rose from 79% to 88%. Censorship on China-sensitive prompts rose from 57.4 to 63.8, while censorship on the matched non-China controls fell from 25.4 to 19.8. The production build is therefore more forthcoming than its preview on everything except China-sensitive topics. A model that had merely become more censored overall would have seen both metrics rise.


This constitutes a measured behavioral change between two builds of one model family in a single run. We can hypothesize as to whether this reflects a deliberate tightening during productionization, a different post-training recipe, or something else entirely, but we cannot be certain of the mechanism. To our knowledge, this is the first controlled, matched-pair, judge-validated measurement of a Chinese frontier model across a production release; the relation to earlier community testing is below. The abstract question posed by many over the past weeks of “is this getting worse?” now has a paired-design data point: at the source, worse, and selectively so.


‍ ***Both builds will be live shortly in the***[playground](https://playground.ctgt.ai/) ***, and we encourage you to experiment yourself.***
‍


#### **Relation to prior measurement**


**‍** The most relevant measurement we’ve found is[SpeechMap.AI](https://speechmap.ai/) , built by the pseudonymous researcher xlr8harder. Last year they showed that DeepSeek's R1-0528 engaged less on contentious topics than its predecessors, which was the first public signal that this family drifts across releases. DeepSeek currently ranks above several American labs on SpeechMap because it measures the share of all sensitive prompts a model answers. LineageEval measures country-specific selectivity, namely that every China-sensitive prompt is twinned with a structurally matched non-China control, and the unit of measurement is the within-pair gap. On that axis the same model sits at +44.0 while GPT-OSS-120B sits at +3.9. These numbers are both valuable metrics but they decompose differently. A global compliance rate cannot separate refusing controversial things generally from censoring one country's topics specifically. LineageEval also exists to answer the question of what transfers from a teacher to its students.
‍


#### **Even with a more censored teacher, it still does not transfer**


**‍** We repeated the distillation experiment from the original study with both new frontier teachers: V4-Flash-0731 (the most censored model we have measured, 63.8 on the sensitive condition) and Thinking Machines' Inkling Small (the least, 11.6). We replicated the setup with a finance-reasoning objective, no China-sensitive content in any training stage by construction and hint-at-the-failure-step with reverse-KL over 100 on-policy tokens into GPT-OSS-120B. Both students landed at untouched-base level: the V4-0731-taught student at 21.7/20.1 (n=152), the Inkling-taught student at 22.2/19.9 (n=149), against 22.7/18.8 for the base itself. Both new arms retained effectively the full pair set (152/152 and 149/152). Teachers spanning a five-and-a-half-fold range of censorship, from the cleanest model we have measured to the most censored, produced students that are statistically indistinguishable from each other and from the base. Whatever moved between V4’s preview and release at the source, none of it rides along through capability distillation into a non-shared-lineage American base.


**Inkling Small is the least censored model we’ve measured.** Inkling Small scored 11.6 on sensitive prompts and 12.4 on controls whic constitutes a matched gap indistinguishable from zero and roughly half the baseline censorship of GPT-OSS-120B on both conditions.


‍


#### **What's next**


The configuration where transfer is most plausible is shared initialization, so we are examining a Chinese teacher into a Chinese-lineage base. That experiment, and a representational study of where this behavior lives inside these checkpoints, are running now.


‍
