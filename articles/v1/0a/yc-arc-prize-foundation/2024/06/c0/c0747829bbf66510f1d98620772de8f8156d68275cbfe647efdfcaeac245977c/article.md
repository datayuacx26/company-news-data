---
schema_version: "1.0.0"
document_id: "c0747829bbf66510f1d98620772de8f8156d68275cbfe647efdfcaeac245977c"
company_key: "yc-arc-prize-foundation"
company: "ARC Prize Foundation"
source_id: "yc-arc-prize-foundation-atom-3536270edb25"
canonical_url: "https://arcprize.org/blog/introducing-arc-agi-public-leaderboard"
published_at: "2024-06-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:48.897807+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:f3890f8d9d710dc5d457bfc2212d1ecf2b5100b188059694f5a2a31b15629b24"
---

# Introducing the ARC-AGI Public Leaderboard

# Introducing the ARC-AGI Public Leaderboard


We’re excited to officially introduce the ARC-AGI Public Leaderboard! And we’re committing $150,000 USD toward a verification fund for this new leaderboard.


## Beyond the Limit


The ARC Prize 2024 competition on Kaggle enforces constraints by design: no internet and limited compute.


With no compute limits, you could solve ARC-AGI through breadth-first brute force search over a domain-specific language (DSL). It wouldn't even be that expensive if you have a datacenter available. Compute limits force researchers to reckon with efficiency, an important part of AGI. ([More on the topic of compute.](https://arcprize.org/blog/day-1-update#limited-compute-no-internet) )


It’s also critical that the private evaluation dataset used to score contest submissions remains private. The key tenet of ARC-AGI is that the creators of a solution cannot know what the problem will be, or they risk encoding their intelligence into the solution.


These constraints mean that the official competition does not allow usage of the web or closed frontier models like GPT-4o, Claude 3.5 Sonnet, Gemini 1.5, or other powerful LLMs. (Note: open source models that do not require internet access, like LLama 3, are allowed.)


However, we know that many people, ourselves included, are curious about how these models perform against the ARC-AGI benchmark.


That’s why we’re introducing a new public leaderboard -[ARC-AGI-Pub](https://arcprize.org/leaderboard#arc-agi-pub) - which measures performance using the ARC-AGI public evaluation dataset, lifts compute restrictions, and allows internet access.


ARC-AGI-Pub high scores will be verified and published alongside reproducible open source code so you can quickly start experimenting with these exciting solutions.


---


### New High Scores


We’re kicking off the new leaderboard with a few verified scores.


#### 1. New Approaches


We've added a novel approach from Ryan Greenblatt, a Redwood Research engineer, who leverages GPT-4o to generate and refine multiple Python programs, selecting the most promising solutions for submission.


#### 2. LLM Baselines


We measured the default capabilities of several leading LLMs against ARC-AGI with minimal prompt engineering: OpenAI GPT-4o, Anthropic Claude 3.5 Sonnet, and Google Gemini 1.5.


#### 3. Past Competition Approaches


To calibrate and validate the new semi-public evaluation set, we measured the performance of the approaches that won previous ARC-AGI competitions: icecuber 2020 and armo 2022.


Name Score (public eval) Verification Score (semi-private eval)


[Ryan Greenblatt](https://arcprize.org/leaderboard#arc-agi-pub) 42% 43%


[icecuber 2020 \[Example\]](https://arcprize.org/leaderboard#arc-agi-pub) * 39% 17%


[Claude 3.5 \[Baseline\]](https://arcprize.org/leaderboard#arc-agi-pub) 21% 14%


[GPT-4o \[Baseline\]](https://arcprize.org/leaderboard#arc-agi-pub) 9% 5%


[Gemini 1.5 \[Baseline\]](https://arcprize.org/leaderboard#arc-agi-pub) 8% 4.5%


* This solution is an example. It does not meet the 12 hour runtime limit nor has good verification score agreement.


The[live public leaderboard](https://arcprize.org/leaderboard#arc-agi-pub) will link out to code you can use to duplicate listed solution.


---


### Leaderboards


Going forward there will be 2 leaderboards with distinct high scores.


1 - ARC-AGI Leaderboard


- Progress Prize $ and Grand Prize $, no reimbursements
- Measures the private evaluation set of tasks
- Scores verified and hosted on Kaggle
- No internet access and enforced compute limits


2 - ARC-AGI-Pub Leaderboard


- No associated prizes, verification reimbursements
- Measures the public evaluation set of tasks
- Scores verified and hosted on arcprize.org
- Internet access allowed, no enforced compute limits, enforced cost limit


#### Evaluation


Submissions to the ARC Prize 2024 competition hosted on Kaggle are, and will continue to be, measured against the private evaluation dataset.


ARC-AGI has endured measurement against this set of tasks. We assert that the Grand Prize solution on the private evaluation set is less likely to be overfit or contaminated. It is well known that many leading LLMs have been pre-trained on GitHub data, which contains the public eval set solutions as well as example solution programs from past contests and research, which tends to result in higher scores than when measured against the private eval set.


Despite these known issues in scoring against the public evaluation set, we still want a path for researchers to quickly get started, experiment, and show directionally what latest-generation models can do, where their limits are, and where new ideas are needed.


If someone achieves a conceptual breakthrough using the public evaluation set, we expect high scores on ARC-AGI-Pub to rapidly roll down to the ARC-AGI leaderboard. Existence proofs are powerful!


#### Submit a Solution


We've created a new official page for ARC-AGI on our website where you can find up-to-date rules and more.


[Learn more about submissions.](https://arcprize.org/arc-agi-pub)


---


### Funding Commitment


We're well aware that costs can be significant to run solutions against evaluation sets. To help support those contributing to this initiative, we are committing $150,000 USD to support the new public leaderboard.


For verified and reproduced high-score claims (submissions that exceed the current high score by at least 1%), we will reimburse participants up to $2,500 USD for API costs incurred while reproducing the solution during verification.


The delta between the $10,000 cost limit and $2,500 reimbursement encourages participants to optimize their code (or pay more out of pocket during verification.)


[Learn more about the verification fund.](https://arcprize.org/arc-agi-pub#fund)


---


### The Future


ARC-AGI data is comprised of several sets of tasks:


1. Public training set (easy)
2. Public evaluation set (hard)
3. Private evaluation set (hard)
4. NEW! Semi-private evaluation set (hard)


It is intended that all evaluation sets are equally hard. While establishing ARC-AGI-Pub we discovered evidence this is not the case. It appears the public evaluation set is easier than the private evaluation set.


We don’t know exactly why this is, but it could be due to:


- The eval sets are relatively small sample size
- Model contamination
- Solutions overfitting on public examples
- Uncalibrated difficult (the public eval set was created first)


While this does not invalidate the spirit of ARC Prize (100% of all tasks are human solvable), it does muddy up comparison between sets.


A future version of ARC-AGI will formally calibrate difficulty across evaluation sets.


For now, we feel the best course of action is to transparently share the evidence we have gained and report scores on both leaderboards.


---


### Onward


Thank you to our fast-growing community for surfacing issues and opportunities around ARC Prize. We’ll continue to listen and improve in order to achieve the goals of the competition:


- Increase the number of people working on frontier AGI research.
- Popularize an objective measure of AGI progress.
- Solve ARC-AGI and learn something new about the nature of intelligence.


#### Get Involved


- [Join the Discord](https://discord.gg/9b77dPAmcA)
- [Try out a solution template](https://www.kaggle.com/competitions/arc-prize-2024/code?competitionId=67357&searchQuery=template&excludeNonAccessedDatasources=true)
- Spread the word about @arcprize on social media!
