---
schema_version: "1.0.0"
document_id: "22b646fb40713acb993b1aedc4bd7083a9d8e39fc23925f0a490f34d24c58ef9"
company_key: "yc-arc-prize-foundation"
company: "ARC Prize Foundation"
source_id: "yc-arc-prize-foundation-atom-3536270edb25"
canonical_url: "https://arcprize.org/blog/2024-progress-arc-agi-pub"
published_at: "2024-12-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:48.897807+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:9dc2f761a85d762c301f32267ca457013c8ff5608d35906cd2c86e3371338141"
---

# 2024 Progress on ARC-AGI-Pub

# 2024 Progress on ARC-AGI-Pub


At the end of the 2024 ARC Prize competition, two new verified ARC-AGI-Pub submissions achieved state-of-the-art scores on the ARC-AGI Public Leaderboard (ARC-AGI-Pub). These were achieved by Jeremy Berman (53.6%) and a combined MIT & Cornell team (47.5%).


ARC-AGI-Pub is our secondary leaderboard that allows internet access and relaxed compute constraints to test the performance of closed-source, commercial, frontier models against the ARC-AGI benchmark. We use 100 tasks from the Semi-Private eval for this leaderboard to prevent any data leakage with the fully private eval used in the official Kaggle competition.


Let's dive into these new ARC-AGI-Pub solutions, both of which are open source and reproducible.


---


## Method #1: Evolutionary Test-time Compute


> I just got first place on the public ARC-AGI benchmark using Claude Sonnet 3.5 and Evolutionary Test-time Compute[https://t.co/OoRF76cqXs](https://t.co/OoRF76cqXs)[pic.twitter.com/oqOFLQ3TUF](https://t.co/oqOFLQ3TUF)
>
>
> — Jeremy Berman (@jerber888)[December 6, 2024](https://twitter.com/jerber888/status/1865112099015291379?ref_src=twsrc%5Etfw)


Jeremy Berman, Co-founder & Co-CEO of[Params](https://params.com/) , submitted a novel approach, "Evolutionary Test-time Compute", inspired by genetic algorithms and scoring 53.6% on the Semi-Private eval.


Jeremy's approach used Anthropic Claude Sonnet 3.5 - which out of the box scores 14% on the Semi-Private eval - to generate many Python transform functions. These functions were then tested against ARC-AGI task examples, of which, the "fittest" candidates were used as the basis for creating new, slightly varied solutions.


This iterative cycle of selection, variation, and testing mirrors the process of genetic evolution, continuously refining the pool of functions. The process looped multiple times, generating up to 500 functions and 31 dynamic prompts per task.


To avoid a local maxima (overfitting to a subset of tasks), Jeremy built in mechanisms to preserve diversity among candidate solutions to encourage exploration of alternative solution paths. This increases the likelihood of discovering globally optimal solutions.


Dive deeper into this exploration with Jeremy's[full write-up](https://jeremyberman.substack.com/p/how-i-got-a-record-536-on-arc-agi) or[code](https://www.kaggle.com/code/jerber/jeremy-arc) to reproduce his results.


Params & ARC Prize Co-founders in NYC.


Jeremy's new startup, Params, is an “Intro.co” for GitHub. Teams can speak directly with great engineers for advice on specific projects. You can explore code and book a call with[the ARChitects](https://params.com/@the-architects/arc-prize-2024) (#1 team on the ARC Prize private leaderboard),[Jeremy](https://params.com/@jeremy-berman/arc-agi) (#1 on the ARC-AGI public leaderboard), or ARC-AGI creator[François Chollet](https://params.com/@fchollet) .


---


## Method #2: Test-Time Training


Researchers from MIT and Cornell teamed up to achieve a 47.5% accuracy on the Semi-Private eval. If you’ve read our[2024 ARC Prize Technical Report](https://arcprize.org/2024/report) , you’ll know that test-time training (TTT) was a standout approach in this year’s ARC Prize, both for the Kaggle and Public leaderboards.


[Wen-Ding Li](https://www.cs.cornell.edu/~wdli/) ,[Kevin Ellis](https://www.cs.cornell.edu/~ellisk/) , and others at Cornell along with[Zenna Tavares](https://www.zenna.org/) from non-profit AGI research lab[Basis](https://basis.ai/) authored the paper["Combining Induction and Transduction for Abstract Reasoning"](https://arxiv.org/abs/2411.02272) (1st place ARC Prize 2024 Paper Award winner), which makes the case that induction and transduction are highly complementary. They found that even when trained on the same data and using the same architecture, induction and transduction excelled at different types of ARC-AGI tasks.


[Ekin Akyürek](https://ekinakyurek.github.io/) and team at MIT published the paper["The Surprising Effectiveness of Test-Time Training for Abstract Reasoning"](https://arxiv.org/abs/2411.07279) (2nd place ARC Prize 2024 Paper Award winner), which documents using a TTT approach to give a 6x improvement in accuracy when solving ARC tasks as compared to base fine-tuned models. By applying their TTT methodology to an 8B-parameter language model, they achieved 53% accuracy on the public eval set - a significant improvement over previous neural approaches.


With existing relationships and complementary approaches, the teams partnered up to build a single ARC-AGI-Pub submission, scoring 47.5% on the Semi-Private eval. Their performance improved after training on 400k synthetically-generated ARC tasks from ARC-Heavy and ARC-Potpourri, created by the team at Cornell.


Kevin Ellis & Zenna Tavares at NeurIPS 2024.


### Scaling Test-Time Compute with Modal


Verifying an LLM-based approach that relies on test-time compute can be challenging to scale. Through collaboration with ARC Prize, the MIT + Cornell team partnered with[Modal](https://modal.com/) , a company that provides high-performance AI infrastructure, to provide both credits and infrastructure to make this possible.


Huge thanks to[Charles Frye](https://twitter.com/charles_irl) - Dev Advocate at Modal - for working with us to spin up an environment that efficiently ran the MIT/Cornell model for verification. A special thanks as well to Wen-Ding and Ekin for the extra effort in porting over their submission to ensure it was easily reproducible -- a service to the community.


---


## ARC-AGI-Pub Progress


Look for more testing of frontier models on ARC-AGI-Pub in collaboration with research teams, major labs, and independent contributors as we continue to discover substantial progress towards ARC-AGI-1 and help guide the creation of ARC-AGI-2.


Subscribe to our newsletter to stay in the loop.
