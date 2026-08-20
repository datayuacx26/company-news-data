---
schema_version: "1.0.0"
document_id: "51214739a3a93bb5f982c41dc0f79dc06509bdca4a8193e603faed61299e39cb"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/our-ai-benchmark-couldnt-detect-a-7-point-regression/"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T17:11:34.718293+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:ae2d8fb5fe639cbdc338678ae5bb79a302b0c88d03c1b29f4e3275a231106758"
---

# Our AI benchmark couldn't detect a 7-point regression

We run an internal benchmark called Chat Bench against our chat agent, the one that answers data questions by writing SQL against your warehouse. Every scenario is a question with a hidden ground truth, an LLM grader scores the answer, and for about a year the output was a leaderboard: experiment name, mean score, standard deviation, 3 trials each.


Then we tried to use it for a real decision. A new model came out that was 70% cheaper than our default, and we wanted to know if we could switch. The leaderboard said 55.9% vs 63.1%, which looks like a clear no, until you notice the error bars on both numbers were wide enough to overlap comfortably.


We worked out the minimum detectable effect of our own harness and it came to roughly 10 points. The cheaper model was about 7 points worse. Our benchmark, the thing we built specifically to answer this question, could not distinguish that regression from noise. And most of the changes we actually ship (prompt tweaks, tool description edits, context changes) are worth 1 to 3 points, so we’d been eyeballing leaderboard rows and telling ourselves stories about differences the harness couldn’t resolve at all.


## Where the noise comes from


The naive comparison treats each experiment as a bag of trial scores and compares the means. That throws away the structure of the data, and the structure is where all the variance hides.


The biggest source is scenario difficulty. Some scenarios are easy and every model scores 100 on them, and some are brutal and everything scores 30. When you compare two experiment means, the spread across scenarios (easily 40+ points) dominates the thing you care about, which is whether model A does better than model B *on the same question* .


The second source is correlated failure. Our scenarios share hidden-truth fixtures: several questions probe the same seeded revenue table from different angles. If a model misreads that table, it fails 4 scenarios at once. Treating those as 4 independent samples makes your confidence intervals look much tighter than they really are.


The third source was the grader itself, but we’ll get to that.


## Pairing, then bootstrapping


The fix for scenario difficulty is old, boring statistics: pair the observations. Instead of comparing experiment means, we compute a per-scenario delta (candidate’s mean on that scenario minus baseline’s mean on that scenario) and analyze the deltas. A scenario where both models score 30 contributes 0 to the delta, exactly as it should. Pairing cancels the difficulty variance that dominated the naive comparison.


For the correlated failures, we group scenarios into clusters with a union-find pass: any 2 scenarios that share a hidden-truth fixture, or that belong to the same variant group, end up in the same cluster. A cluster is 1 unit of evidence, however many scenarios it contains.


Then a hierarchical bootstrap gives us the uncertainty. Each of 10,000 iterations resamples clusters (weighted by how many scenarios they contain, since the headline score equal-weights scenarios), then resamples trials within each scenario, and records the mean paired delta. The 2.5th and 97.5th percentiles of those deltas are the 95% confidence interval. We seed the PRNG so the same pair of experiments always produces the same interval, which matters more than you’d think when an agent is re-running the compare command in CI.


The verdict rule is deliberately dumb: CI entirely above 0 is an improvement, entirely below 0 is a regression, anything else is “no reliable change”. The tool also prints the probability of superiority and the current minimum detectable effect, so when it says “can’t tell”, you know whether the answer is “run more trials” or “this effect is genuinely tiny”.


## A score with no comparator is not a result


The statistics were the smaller half of the change. The bigger half was making comparison the only thing the harness does.


There’s a pinned baseline per model in a` baselines.json` file, and every run auto-compares against its pin. The leaderboard still exists but it’s demoted behind a flag, because 2 leaderboard rows from different weeks tell you almost nothing: the suite changed, the seeded warehouse changed, the grader changed. Every experiment manifest now records the git SHA (and a dirty flag, for the honest among us) so a comparison can refuse to run when the versions don’t line up.


That refusal is load-bearing. When the benchmark version, warehouse seed, or grader model differs between candidate and baseline, the compare command prints why and exits with code 2 instead of printing a verdict. Improvement or “no change” exits 0, a significant regression exits 1. Agents and CI can gate on that without parsing any prose.


## The grader was lying to us


Once the statistics could resolve small effects, they immediately pointed at scenarios where the “effect” was the grading. With 12 trials per scenario across 4 experiments, patterns show up that 3 trials never reveal.


One scenario asked whether a discount policy was violated, and the correct answer was no. The grading criteria included an exclusion regex that flagged any answer containing the policy ID, so a correct answer saying “DISC-ENT-04 is not violated” failed on all 12 trials of every model. Another scenario asked “how many accounts” while the rubric graded the account *names* , so a model could answer the question that was actually asked and still score 0.


My favorite was a truth mismatch rather than a grading bug. A scenario asked about signups “last week”, but the seeded warehouse data ends on April 30th. Models would either refuse (reasonably) or pick some week, and the hidden truth expected one specific week the prompt never identified. The scenario is now anchored to a concrete date, and the trap it was designed to test (Monday vs Sunday week boundaries) survived the fix.


We also replaced most regex-based criteria with extraction grading: the grader pulls the specific claimed values out of the answer first, then checks them against the truth. Regexes on free-form model prose were adding noise that had nothing to do with answer quality, and every point of grader noise inflates the MDE for every future comparison.


The saturated scenarios (5 of them scored 100% on every trial of every model) got reflagged as guardrail canaries. They contribute nothing to a mean, but a model suddenly failing one is alarming, so the compare output treats any guardrail score below 100 as an alarm that overrides the verdict’s exit code.


## Wall clock is a trap too


Speed comparisons had their own noise problem: provider throughput drifts. The same model on the same prompt can generate tokens 30% slower on Tuesday than it did on Friday, and a raw wall-clock comparison between runs from different days mostly measures the provider’s load balancer.


So the compare reports speed in tiers. Tier 1 is drift-free drivers: LLM round trips, output tokens, and tool time, all derived from traces and valid against any baseline. Tier 2 estimates what the baseline workload would cost at the candidate run’s observed provider speed, using per-call timings to separate overhead from generation. When the observed tokens-per-second drifts more than 15% between the 2 runs, the raw wall-clock numbers are suppressed entirely and only the normalized estimate is shown. Tier 3 is an opt-in mode that runs both arms in the same time window from a cached worktree of main, for when you need a real paired measurement.


## What it settled


The cheap model question got a real answer: a reliable regression of 7.3 points, 95% CI \[-14.6, -0.2\]. Significant, barely, which is exactly what the old harness could never have told us. We didn’t switch.


A different model at 22% cheaper came back as “no reliable change” with better process scores, and that one we did switch to as the default. Same harness, same decision procedure, opposite outcomes, both defensible.


The honest case is the prompt rewrite we shipped alongside all this. Overall delta +3.6 points, CI \[-2.4, +12.0\], probability of superiority 89%. The tool correctly refuses to call that a reliable overall improvement. But the per-tag breakdown showed the failure modes we’d targeted were clearly fixed (ambiguity handling up 45 points, user-correction handling up 22), so we shipped it on that evidence instead of a headline number the statistics couldn’t support. Being forced to say “the overall effect is uncertain, the targeted effect is real” is a feature.


## Takeaways


1. **Compute your benchmark’s minimum detectable effect before trusting any comparison from it.** Ours was ~10 points while we were making decisions about 2-point changes. That’s the whole failure in one number.
2. **Pair everything.** Per-scenario deltas cancel difficulty variance, which is probably the largest variance component in any eval suite with mixed difficulty. It’s the cheapest statistical upgrade available.
3. **Model your correlation structure.** Scenarios that share fixtures fail together. Resampling them independently gives you confidence intervals that are flat-out wrong.
4. **Grader noise compounds.** Every brittle regex criterion inflates the MDE of every future comparison. Extraction-based grading and a hand-labeled calibration set are worth the setup cost.
5. **Make refusal a first-class output.** A compare tool that prints a verdict across incompatible versions is worse than no tool. Exit code 2 (“can’t compare, here’s why”) has saved us from more bad conclusions than the CIs have.


We’ve written before about[teaching the agent what’s in your database](https://www.basedash.com/blog/teaching-our-ai-agent-whats-actually-in-your-database) and[what we learned running background jobs on Postgres](https://www.basedash.com/blog/what-we-learned-running-background-jobs-on-postgres) . This one’s the same shape: the infrastructure around the AI turns out to matter as much as the AI.


And if you want to see the agent all this benchmarking is for,[Basedash](https://www.basedash.com/) is an AI-native BI platform. The chat agent that answers your data questions is the one Chat Bench grades.
