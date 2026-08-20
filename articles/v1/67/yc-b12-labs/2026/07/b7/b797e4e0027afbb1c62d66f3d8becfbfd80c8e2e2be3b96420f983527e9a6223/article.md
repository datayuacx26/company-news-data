---
schema_version: "1.0.0"
document_id: "b797e4e0027afbb1c62d66f3d8becfbfd80c8e2e2be3b96420f983527e9a6223"
company_key: "yc-b12-labs"
company: "b12 Labs"
source_id: "yc-b12-labs-rss-d3925c460a5b"
canonical_url: "https://b12-labs.com/blog/selenium-strategy-first-retrosynthesis/"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-22T07:41:23.742535+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:f16b80b157bbb1110d0c2069598eeabdbcb25ecb0574e8e5cb2b5679ac1e70d5"
---

# Agentic Synthesis: Making the molecules models want

We can now dream up beautiful molecules: new medicines, new materials, structures that no one has ever made.


But a molecule on a screen is not a molecule in a vial. Too often, the one that reaches the vial is the less handsome cousin: the watered-down version that was easier to make.


**The bottleneck is no longer our ability to imagine molecules, but our ability to make them.** Molecular design is beginning to outrun synthesis: models can propose more ambitious molecules than the chemistry we can run.


Before anyone can test the molecule they actually wanted, a chemist has to answer a more basic question:


**How do we actually make it?**


## How chemists make stuff


**Retrosynthesis** is how chemists create the plan for making a new molecule.1 The chemist starts with the target and reasons backward: what substances do I have to mix, to get to this product? What simpler structures could produce it? How could those in themselves be made? The process continues until it reaches simple starting materials that can actually be bought. That chain becomes the roadmap: the same reactions, run forward, that build the target from simple materials. None are guaranteed to work however, and some may never have been tried on these exact molecules.


**Every molecule is a different planning problem.** At each stage a chemist chooses both a reaction and its place in the sequence, and a step that looks plausible on its own can create a dead end later. A good strategy anticipates those interactions and organizes the whole synthesis around a few key decisions.


That plan determines **whether a new medicine lives or dies** : poor planning can make a promising molecule look years away and kill it before it reaches the lab, while smart planning can reveal a shorter, credible path. Until we can plan routes as ambitiously as models design them, synthesis remains the bottleneck.


## How we are solving synthesis planning


Chemists are trained precisely for this. Before writing down a single reaction, they study the molecule, identify where its complexity lies, anticipate the main challenges, and decide how those challenges might be overcome.


Traditional synthesis-planning systems often begin with reactions that look plausible and search through their possible combinations.2 This can produce many routes built from well-known chemistry, but it does not guarantee that the steps form a coherent plan.


Today, we are introducing **Selenium** , our synthesis agent. Selenium approaches the problem as a chemist would: understand the molecule, form a strategy, write the route, evaluate it, and iterate.


Selenium is a multi-agent system powered by frontier reasoning models. It runs a loop that starts by carefully analyzing the target to gain a deep understanding of its structure and form a high-level strategy. Route-developer agents turn that strategy into an actual sequence of reactions: an actionable object the system can evaluate. Other agents then criticize and evaluate each step in the route, and the loop begins again, now with better context on the strengths and shortcomings of the particular strategy. This is **agentic synthesis** : a way of coming up with novel synthetic ideas, making them concrete, and iterating on them to reach the full potential of each one.


At **b12 labs** , we treat synthesis as one connected planning problem. Selenium can incorporate strategic guidance from chemists, account for laboratory constraints, and revise its plans as new evidence appears.


When a model designs a molecule for a biological purpose, we should make that molecule, instead of quietly replacing it with an easier cousin or abandoning it because the chemistry filters deemed it “too ambitious.”


**Figure 1.** Synthesis does more than slow discovery: it changes what gets tested. An ambitious molecule may be replaced by an easier substitute or dropped entirely. Selenium is designed to preserve the original molecular intent and find a credible path to the lab. The PPARG and KRAS G12D binding poses shown here are computational illustrations, not experimental structures.


Preserving the original design only matters if the route is credible. We tested that directly.


## Does it think like a chemist?


We tested Selenium on 100 molecules with published syntheses from experts. Because Selenium proposes several routes for each target, we asked two separate questions:


**Does it recover the original idea?** That is, can the system come up with the same strategic idea as experts?


**Does it find other plans worth pursuing?** Even when not the same idea as experts, are solutions still valid? Are they chemically feasible as a whole?


Across all 100 targets, Selenium recovered the expert’s strategic idea on **80%** of cases, while traditional retrosynthesis systems did so on at most 20%. Its strongest routes also came very close to the expert references on the same feasibility measure, showing that different strategic ideas can still produce strong plans.


Selenium generates several distinct strategies, often all strong and strategically sensible. Surprisingly, the highest-quality route is frequently *not* the one that matches the expert’s: strategy and feasibility are measuring genuinely different things. The real proof, as ever, is a prospective target and the lab3 , more on that later.


On 100 diverse targets, Selenium recovered the expert strategy on 80% of them, versus at most 20% for traditional systems (vertical axis). The horizontal axis is easy to overlook and just as important: how close each system's strongest route came to expert-level feasibility. General coding agents that recover the idea ~56% of the time still leave a feasibility gap of 12 points; Selenium closes it to under one point, sitting essentially on top of the expert reference.


Two highlights from these results. First, even general-purpose coding agents show real chemical reasoning: they recover the expert’s strategic idea about 56% of the time, showing how much frontier models already carry. But *just* finding the idea is not the same as producing a valid route, or one that a chemist wants to run: the best routes from those agents still sit **more than 12 points below** expert-level feasibility. Selenium recovers the strategy 80% of the time *and* closes the feasibility gap to **under a single point** ; essentially indistinguishable from the expert reference. Finding the idea and executing it are different problems, and Selenium is the only system here that does both.


The final test, as always, is the lab. The benchmark, however, matches what we hear from our clients and early users. Early users at top pharmas, biotechs, CROs, and university research groups have tested Selenium on their own programs. Not only have they praised the chemical insight on the proposed solutions, but also Selenium’s interactivity: it can uptake a chemist’s ideas and quickly turn them into even stronger proposals.


## Making what the models want


Drug discovery has always balanced ambition against what can actually be made. Large libraries became standard because familiar building blocks and reliable reactions produce many compounds quickly4 . Good for numbers, but places a boundary around what can be explored. Generative design removes that boundary:5 a model is not limited to the catalog, and can propose entirely new molecules for a biological objective, even when no one yet knows how to make them.


The most valuable of those molecules are also the least repetitive. They live in the long tail: unusual scaffolds, delicate 3D shapes, reactive groups that interfere with each other, bonds with little precedent6 where every choice depends on the rest of the molecule. This is where **agentic synthesis** matters most. While traditional systems scale repetition and propagate what is already known, Selenium reasons around the details of each target, even when the best path has no close precedent. For every day work, scientists are using Selenium for scouting analog routes, improving process efficiency, and steering toward building blocks that they have on their shelves.


**As design models improve, more of the bottleneck moves downstream.** Models can easily produce thousands of structures, but none becomes a medicine or material until it becomes a physical sample that can be tested. At **b12 labs** , we build the tools to break through that bottleneck. Selenium is the planning layer between molecular intelligence and the physical world, but this is only the first layer. Together with[Palladium](https://b12-labs.com/blog/palladium-execution-layer/) and our partners, at b12 Labs we are also running these syntheses, and the goal is simple:


> **Make the molecules the models want.**


**Today, we are opening free access to academic and nonprofit researchers** . Bring the molecule you have been thinking about: the ambitious one, the one worth making. We would love to see what Selenium finds!


**Your chemistry stays yours.** Structures you submit are used only to plan your routes—they are not used to train models or shared with other users. Your data is encrypted in transit using TLS 1.3 and encrypted at rest, with access protected by strong authentication and security controls.


In the coming weeks we will walk through a single hard target end to end, show how Selenium reasons, and show what happens when its routes reach the lab.


[Try Selenium free](https://app.b12-labs.com/selenium) , no application or demo request. If you are working on a commercial synthesis program,[bring us your hardest molecule](https://b12-labs.com/cdn-cgi/l/email-protection#6d0b02180309081f1e2d0f5c5f40010c0f1e430e0200521e180f07080e195022181f485f5d050c1f09081e19485f5d000201080e180108) .


## References


1. Corey, “[The Logic of Chemical Synthesis: Multistep Synthesis of Complex Carbogenic Molecules](https://www.nobelprize.org/uploads/2018/06/corey-lecture.pdf) ,” Nobel Lecture, December 8, 1990.
2. Segler, Preuss & Waller, “Planning chemical syntheses with deep neural networks and symbolic AI,” *Nature* 2018.
3. Strategy matches were scored by an LLM-as-judge, an approach validated in: Bran et al., “[Chemical reasoning in LLMs unlocks strategy-aware synthesis planning and reaction mechanism elucidation](https://www.cell.com/matter/fulltext/S2590-2385(26)00175-X) ,” *Matter* 2026.
4. Roughley & Jordan, “[The Medicinal Chemist’s Toolbox: An Analysis of Reactions Used in the Pursuit of Drug Candidates](https://pubs.acs.org/doi/10.1021/jm200187y) ,” *J. Med. Chem.* 2011.
5. Gómez-Bombarelli et al., “[Automatic Chemical Design Using a Data-Driven Continuous Representation of Molecules](https://pubs.acs.org/doi/10.1021/acscentsci.7b00572) ,” *ACS Cent. Sci.* 2018; and[recent work on generative molecular design](https://arxiv.org/pdf/2607.12488) .
6. Lovering, Bikker & Humblet, “[Escape from Flatland: Increasing Saturation as an Approach to Improving Clinical Success](https://pubs.acs.org/doi/10.1021/jm901241e) ,” *J. Med. Chem.* 2009.
