---
schema_version: "1.0.0"
document_id: "1bdbe534351c39c23bf44eceafcba142de825154e089608bae0cd4d43823be9c"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/benchmarking-opus-4-6-vuln-detection"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-24T06:50:52.708520+00:00"
fetched_at: "2026-07-28T22:16:03.530620+00:00"
content_hash: "sha256:5e997ad6538af4883ae673b7475c21f9807a290553fbc051a917c2925be5e473"
---

# Benchmarking Opus 4.6 For Vuln Detection: Flashes Of Brilliance But Lots of Noise

## Introduction


Opus 4.6 on its own seems to find software defects better than any previous Anthropic model, even without being embedded in a more complex workflow or agent.


We decided to find out exactly how good it is. Our testing revealed that with good prompting and tools, Opus can find as many as a quarter of single function C vulnerabilities. However, it still misses the majority of flaws, and the hits come at the expense of a high false positive rate and inconsistency across runs.


These results are impressive compared to previous generation models or human review, but they underline the need for embedding the model within larger systems for vulnerability discovery at enterprise scale with consistent results and manageable amounts of noise.


## The Test


### Overview


We presented Opus 4.6 with 435 known vulnerable C functions from real world CVEs. We tried four different prompts and tool configurations, each simulating the sort of thing you might package as a Claude Code skill to use on your own codebase.


Depending on approach, Opus correctly discovered between 25.1% and 28.5% of the vulnerabilities. However, false positive rates tended to be extremely high. As many as around 60% of all functions had at least one potentially spurious finding, although our structured reasoning approach reduced that to ~40%.


More concerningly, results varied widely across attempts using a single method. For each classification approach, there tended to be a large common core of functions correctly labeled across all runs, along with a sizable set whose labels changed from run to run.


It's worth noting that these vulnerabilities all made it past human review into production in widely-used open source projects. For a general purpose neural network to be consistently flagging ANY of these issues is incredible.


In discovering the strengths, weaknesses and foibles of these powerful new models, we're not discounting their usefulness, just doing the necessary work to understand how to correctly engineer them into rational, battle-tested systems like any other software component. Doing this well is the difference between drowning in noise and inconsistent results and moving at the speed of the AI-enhanced attackers that salespeople won't stop trying to scare us with.


### Dataset


In 2024, Yangruibo Ding and other researchers created the PrimeVul dataset as part of[this study](https://arxiv.org/abs/2403.18624) . One of the many notable things about it is its large collection of individual known-vulnerable C functions paired with the same function after patch. It's especially useful for evaluating LLM vulnerability detection because:


- The functions are from real CVEs in real code bases
- The quality of the dataset is much higher than many other academic vulnerability datasets, some of which have serious accuracy issues. Labeled vulnerable functions are much more likely to be actually vulnerable, and there is very little repetition in the data.
- The benign and vulnerable function pairs are perfect for seeing whether an LLM can alert on the real issue without false positiving on very similar benign code.


*Dataset of vulnerable functions before and after patching.*


For our work, we used a version of PrimeVul posted to hugging face:


[https://huggingface.co/datasets/colin/PrimeVul](https://huggingface.co/datasets/colin/PrimeVul)


We specifically used the paired subset, and the test slice to allow us to do a rough comparison of the P-C metric between the original study and our research so we could get a sense of how Opus 4.6 performs compared to the models available in 2024.


### Original Methodology


The original study covered too much ground to briefly summarize here. The part relevant to our Opus 4.6 benchmark is one of their techniques for evaluating model performance, which we borrowed and enhanced. Their original version:


- Give an LLM a vulnerable function. Ask it: "Is this function vulnerable? Yes/no"
- Give an LLM the patched version of that same vulnerable function. Ask for a binary classification again.
- Compare the classifications. Measure the number of times the LLM classified the vulnerable function as vulnerable AND the benign function as benign.


This approach is particularly notable because it places a premium on both precision and recall, and it captures precision in a very effective way.


The LLM cannot cheat its way to victory by flagging most things "vulnerable." Also, the only difference between the pre-patch and post-patch function is the flaw. It forces the model to distinguish between two otherwise very similar functions in a controlled way.


The original researchers labeled the measurement that captures the times model got both the vulnerable and benign halves of a pair "P-C." If you were randomly choosing a label for each half of a function pair, you'd expect to score about 25% on it. GPT-4, a state of the art model at the time of the study (2024), got only 12.94% right – worse than literally flipping a coin.


*Original methodology.*


### Updated Methodology


While the PrimeVul dataset was ideal for benchmarking Opus 4.6, we decided to update the original approach a bit.


There can be more than one vulnerability in a piece of code. As a result, we can't actually say that the benign function is free of flaws. A model could label both sides of a pair vulnerable because the fix only addressed one of many issues. It could also get lucky and label the vulnerable function and benign function correctly for the wrong reasons… you'd see that in the overall numbers, but you'd be unable to distinguish between a model that performs poorly because it's outright guessing from one that produces legitimate results with good reasoning, just at a low rate.


Our updated approach addresses some of these issues while still allowing a (very rough) comparison with the original study's results.


To start with, instead of asking the LLM to do a simple binary classification – "is it vulnerable or not" – we asked the LLM to list 0…n flaws in the function it was looking at. While this is a harder and different task, it lets us analyze the results in more depth.


We then used another LLM to link the findings from each side of the pair that were the same issue.


As a result, for each function pair we had:


- Flaws found only in vulnerable function
- Flaws found only in benign function
- Flaws found in both functions


We used this as weak signal for false positive analysis… if the model found a flaw only in the benign function, we treated this as somewhat more likely to be incorrect while acknowledging the possibility that the fix may have introduced a new problem.


The LLM-created links here were surprisingly high-quality. 100% were accurate in our hand checks (n=50).


Finally, because each vulnerable and benign function pair was associated with a CVE and a fix commit, we had Opus 4.6 research the CVE, review the fix diff, and evaluate how many of the findings related to the flaw described in the CVE. This is a substantially easier task than finding the issue without guidance, and in the sample of 52 judgments we hand evaluated, we found that Opus reached the same conclusions as an expert security researcher 98.1% of the time.


This approach let us reproduce the P-C measure from the original study: if finding count on vuln function > 0 and finding count on benign function = 0, we counted it as a hit for that measure.


It also let us produce some more nuanced measures, like CVE recall rate and P-C Rigorous which we'll discuss later.


*Our benchmark workflow.*


### Classification Approaches


We created 4 classifiers, all of which output a list of a flaws and a label for the input function using a single prompt:


- Ask for a list of flaws. Do not require the LLM to provide evidence for them.
- Ask for a list of flaws. Require the LLM to produce limited evidence for each.
- Ask for a list of flaws. Require the LLM to produce an extensive, structured justification of each flaw.
- Ask for a list of flaws. Require the LLM to produce an extensive, structured justification of each flaw. Also require that the LLM use a tool to invoke a judge agent to evaluate its output. Revise output in response to judge, or abandon finding.


Each classifier ran using Opus 4.6 with thinking effort set to medium, default sampling parameters, and max tokens set to 32 or 64k depending on the complexity of the approach.


We ran each classifier against the entire dataset 3 times, taking the median results across runs. Three runs is a limited sample for variance analysis, but was sufficient to reveal meaningful consistency patterns.


## Results


### Major Measures


- **P-C Compatible** : Included to allow comparison with PrimeVul study. % pairs for which classifier found:


- > 0 flaws in vulnerable function
- 0 flaws in patched function


- **P-C Rigorous** : % pairs for which:


- > 0 flaws in vulnerable function
- all flaws in vulnerable function relate to CVE
- 0 flaws in patched function


- **CVE Recall** : % of function pairs for which there was at least one finding only on the vulnerable side which matched the issue described in CVE and fixed in diff.
- **Vuln flagged** : Pairs for which one or more flaw was found in vulnerable function
- **Benign flagged** : Pairs for which one or more flaw was found in the patched function
- **Benign only** : Pairs with one or more finding found ONLY on the patched function


### P-C Compatible & Rigorous: Recall + Precision


The P-C scores capture function pairs where we have strong evidence the model didn't produce any false positives on the benign function while flagging flaws in its unpatched counterpart. P-C Compatible includes cases where the classifier found any flaws in the vulnerable function – replicating the measure from the original study, but potentially including findings that don't have to do with the known issue.


P-C Rigorous goes a step farther and only includes pairs where all flaws in the vulnerable function directly relate to the CVE AND the patched, benign function has no findings.


P-C Rigorous should be thought of as the minimum % that Opus nailed. The real number is likely higher: Some of the extraneous findings that disqualify a pair for P-C Rigorous may have been actual latent bugs, or new bugs introduced by the fix commit.


On these demanding and pessimistic measures, Opus 4.6 performance remained low, but it nearly doubled GPT-4 from the PrimeVul study when prompted well. Forcing Opus to justify its conclusions had the biggest positive effect on results, followed by forcing it to consult a second verification agent.


GPT 4 (original study) Opus - No Justification Opus - Limited Justification Opus - Extensive Justification Opus - Extensive Justification + Verification Agent


P-C Compat 12.94% 13.6% 19.3% 20.1% 23.2%


P-C Rigorous N/A 8.7% 14.5% 15.4% 16.1%


*Median P-C scores for runs of each classifier.*


### CVE Recall


The CVE recall score presents a much more optimistic picture than the P-C score. Opus 4.6 correctly picked out the specific vulnerability known to be in the code at a far greater than random chance would explain. Notably, human review caught none of these real issues (because they existed in actual production code), so this represents an improvement over at least some human-only approaches.


CVE Recall N/A 27.1% 25.1% 27.6% 28.5%


*Median CVE Recall scores for runs of each classifier.*


The high recall when no justification was required was coupled with lower precision than any other approach.


### False Positive Analysis


These positive results came at a cost… namely false positives. Each classifier found flaws in more than half of all vulnerable functions, but in many cases these flaws had nothing to do with the CVE. Additionally, the classifiers found flaws in 38-51% of benign functions.


While we can say for sure that none of these flaws were the one known to be in the code sample, it's hard to say exactly how many are false positives vs other real issues.


We do have some clues though:


- Low P-C Rigorous scores. The gaps between recall scores and P-C Rigorous scores range from 10-15%. It's possible that every function pair in this 10-15% had additional, legitimate flaws, but not likely.
- High Benign-Only score. Between 15-27% of the time, classifiers found issues in the benign function that were not in the original function. It's possible that as many as 1 in 4 bug fix commits introduce entirely new security flaws, but not plausible.


Vuln funcs with findings N/A 63.4% 52.2% 54.6% 57.2%


Benign funcs with findings N/A 52.4% 36.6% 37.7% 43.2%


Benign funcs with finding NOT found on vulnerable func 27.4% 15.6% 18.6% 24.7%


*Median finding count scores for runs of each classifier.*


### Variations Across Classification Approaches


#### Recall and Precision Differences


As expected, Opus asked to find vulnerabilities without justifying its findings achieved fairly high recall, at the expense of low precision (measured imprecisely by P-C, benign-only findings and absolute count of extraneous, non-CVE findings).


Requiring Opus to provide limited justifications for its findings dropped recall slightly, but increased precision significantly.


Requiring Opus to provide more extensive justifications for its findings increased recall slightly over limited justifications, but enough to surpass no justification test. More notably, it resulted in major precision improvements.


Finally, adding an independent verification agent produced better recall and P-C scores than any other approach.


#### Consistency: The Hidden Story


It would be tempting to conclude from the precision and recall findings alone that the best single prompt approach of the tested bunch is extensive justification + verification agent.


However, LLMs tend to struggle with producing consistent results across runs. We analyzed how the same pair of functions was classified across the 3 trials using each approach. Predictably, for every run there was a solid core of function pairs that consistently got correctly labeled as vulnerable without any false positives.


In addition to this consistent core though, a large number of function pairs were sometimes labeled correctly, and sometimes not. The verification agent approach, possibly because it involved another instance of the LLM, showed by far the highest instability…. So its better performance came at the expense of different invocations producing significantly different results.


*CVE Recall for extensive justification runs.*


In this table "P-C All 3" and "CVE Recall All 3" capture the function pairs labeled consistently across all 3 runs. "P-C Any" and "CVE Recall Any" capture the pairs correctly labeled in some runs, but not others:


Classifier P-C All 3 P-C Any Delta CVE Recall All 3 CVE Recall Any Delta


Opus - No Justification 44 (10.1%) 78 (17.9%) 34 93 (21.4%) 141 (32.4%) 48


Opus - Limited Justification 66 (15.2%) 105 (24.1%) 39 92 (21.1%) 127 (29.2%) 35


Opus - Extensive Justification 64 (14.7%) 117 (26.9%) 53 99 (22.8%) 142 (32.6%) 43


Opus - Extensive Justification + Verification Agent 49 (11.3%) 175 (40.2%) 126 83 (19.1%) 160 (36.8%) 77


Interestingly, while the scoring of particular pairs varied substantially across runs, the total scores themselves did not. That is, the quality of the things found seemed to remain close to the same, even while the specific pairs flagged correctly changed.


Metric No Justification Limited Justification Extensive Justification Extensive Justification + Verification Agent


P-C Compatible 13.1-15.2% (±1.0) 18.4-19.8% (±0.7) 19.4-22.6% (±1.6) 23.1-25.3% (±1.1)


P-C Rigorous 8.1-9.9% (±0.9) 14.3-14.7% (±0.2) 14.4-18.0% (±1.8) 15.3-17.0% (±0.9)


CVE Recall 26.7-28.0% (±0.7) 24.1-25.5% (±0.7) 27.1-29.7% (±1.3) 27.6-28.7% (±0.6)


## Takeaways


We studied Opus 4.6's performance finding known C vulnerabilities in single functions using a variety of single prompt approaches. Within this narrow slice, the model tended to find around 25% of known vulnerabilities, but with a lot of false positives, and with a lot of inconsistency between runs. Some of our classification approaches mitigated the noise and variability to an extent, but it remained an issue.


In actual practice, not all vulnerabilities are neatly confined to single functions, not all vulnerabilities exist in programs written in C, and not all vulnerabilities are coding issues – some, for example, are business logic issues.


The single vulnerable functions we tested are theoretically easier problems for the LLM, but it's hard to generalize from them to its performance on these other more complex vulnerabilities.


What we can say conclusively is that our results show that a powerful tool used in a naive way produces surprising, positive results that are by some measures better than human review… but it does that with many rough edges that limit its usefulness if not embedded in more sophisticated, carefully-engineered systems.


The limited amount of engineering we did with our four separate classification results showed the impact that subtle factors can have on output quality and consistency, and our experience at ZeroPath has been that larger scale work to harness, and enhance LLMs yields increasingly positive results that make the models more practical for use in production.


## Future Research


With this work, we really just scratched the surface. We're likely to continue in a number of directions, including possibly:


- Giving the LLM more context than just a single function
- Trying more prompting strategies and tool combinations
- Reasoning effort low vs medium vs high
- Trying a more heavyweight verification agent
- Looking for more external grounding mechanisms – e.g. ways to validate model's proposed trace of the vulnerable data flow against actual code programmatically


## Appendix


Source code and data from all experiments:


[https://github.com/ZeroPathAI/opus-benchmark](https://github.com/ZeroPathAI/opus-benchmark)
