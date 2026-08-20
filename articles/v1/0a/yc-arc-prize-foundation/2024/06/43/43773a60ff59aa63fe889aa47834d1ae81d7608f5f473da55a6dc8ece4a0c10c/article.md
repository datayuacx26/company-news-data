---
schema_version: "1.0.0"
document_id: "43773a60ff59aa63fe889aa47834d1ae81d7608f5f473da55a6dc8ece4a0c10c"
company_key: "yc-arc-prize-foundation"
company: "ARC Prize Foundation"
source_id: "yc-arc-prize-foundation-atom-3536270edb25"
canonical_url: "https://arcprize.org/blog/day-1-update"
published_at: "2024-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:48.897807+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:61bc4e9a97a8782357026f29ee40a1c9e33e37326620ace1437d63814eb5d338"
---

# Day 1 Update

# ARC Prize Day 1 Update


We[launched](https://arcprize.org/blog/launch) just over 24 hours ago and the response exceeded our expectations!


Our goal with ARC Prize is to increase public awareness about the progress (or lack of) towards AGI and inspire more AI researchers to work on new ideas again.


From[trending on Twitter/X](https://x.com/i/trending/1800655074101985393) to the[#1 Kaggle competition](https://www.kaggle.com/competitions/arc-prize-2024) , here's an update on Day 1.


*Launch trending on Twitter/X*


---


### Launch Day Stats


- 675k aggregate views on social media
- 15k visits to[arcprize.org](https://arcprize.org/)
- 800 email newsletter subscribers
- 700[Kaggle](https://www.kaggle.com/competitions/arc-prize-2024) entrants
- 350 new members in[Discord](https://discord.gg/9b77dPAmcA)
- 70 Kaggle submissions, 30 teams on the[2024 leaderboard](https://www.kaggle.com/competitions/arc-prize-2024/leaderboard) , top score currently 18%
- 1 friends-and-family launch party ✅


We are incredibly grateful for everyone who helped share the news on launch day. In particular[Nat](https://x.com/fchollet/status/1800577019979411560/quotes) ,[Elad](https://x.com/eladgil/status/1800640524208017502) ,[Dwarkesh](https://x.com/dwarkesh_sp/status/1800587705228914769) ,[Jessie](https://x.com/jessfraz) ,[Wade](https://x.com/wadefoster/status/1800578619607330999) ,[Tsnarnick](https://x.com/tsarnick/status/1800644136942367131) ,[Gary](https://x.com/GaryMarcus/status/1800658944437928365) ,[Caleb](https://x.com/calebwatney/status/1800598714240598331) ,[Shaan](https://x.com/ShaanVP/status/1800677386691981484) , and many many others.


**Notable discussions:**


- [Twitter/X](https://x.com/i/trending/1800655074101985393) (trending)
- [Hacker News](https://news.ycombinator.com/item?id=40648960) (500 upvotes, 300 comments)
- [/r/LocalLlama](https://www.reddit.com/r/LocalLLaMA/comments/1ddma3t/arc_prize_arc_prize_is_a_1000000_public/) (100 upvotes, 35 comments)
- [/r/MachineLearning](https://www.reddit.com/r/MachineLearning/comments/1de2b16/d_fran%C3%A7ois_chollet_announces_new_arc_prize/) (50 upvotes, 50 comments)
- [/r/mlscaling](https://www.reddit.com/r/mlscaling/comments/1ddqjqx/francois_chollet_reboots_the_prevlm_arc_benchmark/) (40 upvotes, 20 comments)


While ARC-AGI remains unbeaten since 2019, launch day increased awareness of the benchmark.


#1 competition on Kaggle!


---


### What Worked


#### Arcprize.org


Daniel Gross told us to treat ARC Prize like a startup. The website needs to communicate and convert. Over the past month, we spent hundreds of hours researching, designing, and building[arcprize.org](https://arcprize.org/) . It seemed to pay off on launch day.


Our top goal for the[homepage](https://arcprize.org/) was to communicate that ARC-AGI is easy for humans, but hard for AI. We did this by putting a playable version of ARC-AGI tasks directly on the homepage along with an[eval saturation chart](https://arcprize.org/media/images/ai-benchmarks.png) showing ARC-AGI progress stalling.


Over 4% of visitors signed up (this comps well against high-growth startups like Zapier.)


And an overwhelming number of people said they played with ARC-AGI puzzles on the website. This appears to have been an incredibly effective communication strategy.


Tip: you can load and play a specific task ID like this:[https://arcprize.org/tasks/3aa6fb7a](https://arcprize.org/tasks/3aa6fb7a)


The[leaderboard](https://arcprize.org/leaderboard) was the second most visited page. We added this page late in order to accommodate the new secondary leaderboard ARC-AGI-Pub (in beta), which allows access to closed API-based models.


There were also several common critiques we'd like to address below.


---


### Common Critiques


#### Limited Compute, No Internet?


This is the most common critique. ARC Prize measures progress towards AGI using the[private evaluation set](https://arcprize.org/guide#private) . The competition imposes a couple of important limitations: Kaggle submissions get access to a P100 GPU for up to 12 hours, and submissions have no internet access (so no GPT-4, Claude, etc.) The previous version of ARC Prize gave access to a weaker GPU for only 5 hours. 2023's SOTA 34% score maxed out the runtime, which is why we doubled compute for 2024.


While we expect to continue increasing compute limits, the primary reason for limited compute is to target efficiency. AGI is defined as a system that can[efficiently acquire new skill](https://arcprize.org/arc#agi-definition) . If efficiency were not considered, intelligence could be built simply with a brute-force tree search, but this is not how humans reason. Francois expands further in his 2019 paper["On the Measure of Intelligence"](https://arxiv.org/abs/1911.01547) :


> A high-intelligence system is one that can generate high-skill solution programs for high generalization difficulty tasks (i.e. tasks that feature high uncertainty about the future) using little experience and priors, i.e. it is a system capable of making highly efficient use of all of the information it has at its disposition to cover as much ground as possible in unknown parts of the situation space. Intelligence is, in a way, a conversion rate between information about part of the situation space, and the ability to perform well over a maximal area of future situation space, which will involve novelty and uncertainty


While we do not know exactly how much scale or efficiency is required to beat ARC-AGI (and we will continue to increase limits over time), the compute limitations force researchers to reckon with this definition. And, practically, hosting the competition on Kaggle imposes resource and cost constraints, and we have to pick a starting line somewhere.


ARC Prize submissions don't allow internet access simply to reduce cheating, reduce contamination, and increase confidence in reported scores.


**NEW** : we launched a secondary leaderboard (in beta) called[ARC-AGI-Pub](https://arcprize.org/leaderboard) which measures the[public evaluation set](https://arcprize.org/guide#public) instead of the private evaluation set. This leaderboard allows internet access (so you can use closed models, make API calls, etc.) To increase confidence in reported scores, we'll be verifying submissions against a new semi-private evaluation set and ensuring good score agreement. While not officially part of ARC Prize 2024 today, it might be in the future. We'll also publish Kaggle notebooks alongside each high score on this leaderboard. We hope this enables more rapid experimentation using frontier models, educates more folks on the limits of LLMs, and provides a more accessible starting point for ARC Prize.


#### $1M Is Not Enough


Many pointed out the $1M prize pool is very small relative to the value created if AGI is discovered via ARC Prize.


First, ARC Prize Inc. is a nonprofit dedicated to the public advancement of open artificial general intelligence.


To counterbalance closed-source frontier research, we require fully reproducible methods and code to be put into public domain in order to claim *any* prize money. In the scenario where someone beats ARC-AGI privately, we will keep the prize going until someone openly reproduces the work.


Second, our goal with the prize is to increase public awareness of the progress towards AGI. We hope this broader awareness inspires many AI researchers to try new ideas again.


To that end, a "small" amount of money can have big leverage. (This leverage is a big reason why I personally put $1M into the prize pool.)


#### Beating ARC-AGI Doesn't Matter


The last most-common critique is that ARC-AGI is disconnected from real world value. Solving it does not matter. There is a long line of solved toy AI benchmarks, especially in reinforcement learning (and games like go, poker, chess...), that did not translate.


Let's start by acknowledging that beating ARC-AGI is necessary, but not sufficient, for real-world AGI. A solution to ARC looks more like the discovery of the Transformer architecture (though much more important, we expect.) We further expect researchers will need to build and scale the discovery.


Any AGI will necessarily beat ARC-AGI. ARC -AGI further serves as a *minimal reproduction of general intelligence* given the relatively small information complexity in the puzzles.


The biggest problem for application-layer AI today is low user trust. Hallucination, inaccuracy, and low consistency require "human in the loop" oversight of AI in any moderate- to high-risk deployment scenarios.


A solution to ARC-AGI, at a minimum, solves this. Solving ARC means writing a program that can generalize from arbitrary core knowledge priors to unseen, novel tasks with *exacting accuracy* .


---


### Fixed / Improved


[Max](https://x.com/MaxNadeau_/status/1800950145896546446) pointed out that[when we launched](https://x.com/arcprize/status/1800317717200982332) we incorrectly equivalated the Grand Prize target 85% with the human average performance on the[public evaluation set](https://arcprize.org/guide#public) ([public literature](https://cims.nyu.edu/~brenden/papers/JohnsonEtAl2021CogSci.pdf) has only tested against the[public training set](https://arcprize.org/guide#public) , which is easier). This is on me. And is now fixed both on the website and all public messaging going forward.


The fact is, humans can solve 100% of all ARC-AGI tasks, including the harder evaluation set. We know this because they all been hand-verified by humans. We debated the waterline for the Grand Prize. We considered scores as high as 95-100% for the goal. The target is somewhat arbitrary.


We ultimately settled on the 85% score goal because of the existing public literature and because it is high enough to consider ARC-AGI solved but low enough to acknowledge imperfections in the benchmark. We intend to continue improving the benchmark over time.


We want ARC-AGI to be a trusted public barometer of progress towards AGI and hope public disclosures like this increase trust.


---


## Future


We won't publish updates like this every day. Or even every week. But expect periodic updates from the team as we hit major milestones along the path towards beating ARC-AGI. All new high scores will be published here:[@arcprize](https://x.com/arcprize)


---


### ARC Prize


ARC Prize is a $1,000,000+ competition to beat and open-source a solution to the ARC-AGI eval.


Hosted by[Mike Knoop](https://mikeknoop.com/) and[François Chollet](https://fchollet.com/) .


#### Get Started


Ready to make the first significant leap towards AGI in years? No matter who you are, where you come from, what you do for a living, you are welcome to join this competition. New ideas might come from anywhere. Possibly you?


Sign up


Find competition format and prize details on[ARC Prize 2024 here](https://arcprize.org/competition) .


For more information on how to get started solving ARC-AGI[visit the guide.](https://arcprize.org/guide)


To learn more how ARC-AGI measures general intelligence[visit ARC-AGI.](https://arcprize.org/arc)


Stay updated on ARC Prize progress and SOTA solutions on[X/Twitter](https://x.com/arcprize) ,[YouTube](https://www.youtube.com/@arcprize) ,[Email](https://arcprize.kit.com/bc80575d89) , and[Discord](https://discord.gg/9b77dPAmcA) . You can also contact us at team@arcprize.org.
