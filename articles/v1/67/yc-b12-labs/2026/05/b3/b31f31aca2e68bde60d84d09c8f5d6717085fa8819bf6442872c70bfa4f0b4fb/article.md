---
schema_version: "1.0.0"
document_id: "b31f31aca2e68bde60d84d09c8f5d6717085fa8819bf6442872c70bfa4f0b4fb"
company_key: "yc-b12-labs"
company: "b12 Labs"
source_id: "yc-b12-labs-rss-d3925c460a5b"
canonical_url: "https://b12-labs.com/blog/palladium-execution-layer/"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:09.639133+00:00"
fetched_at: "2026-07-28T21:13:26.363881+00:00"
content_hash: "sha256:f50b529f35d8aa78d05eb600ee242b8ff9f5c3e6860e49dd56913feaa8367829"
---

# Palladium: the planning layer for chemical synthesis

In drug discovery, the most interesting molecules are often not the easiest to make. The reactions needed to make them are substrate-dependent and tricky to get right, closer to open research problems in themselves than to clear recipes. The result is that these molecules don’t get made, however promising the lead, pushing medicinal chemistry teams to settle for well-known reactions like aminations and couplings. Huge regions of chemical space are ignored because **the cost of trying is too high to test at scale** .


At *b12 Labs* , we’re bringing that cost down. Today we’re announcing **Palladium** , our planning layer for chemical synthesis: a system that turns a proposed reaction into a high-throughput experimental plan that actually runs.


# Smart or scalable


Chemists train extensively to debug reactions, but they are limited by throughput. *Smart, but hardly scalable* . In a world where we can run 96 reactions in parallel, the natural alternative is Bayesian Optimization or Design of Experiments — methods built to explore a search space and optimize for yield or selectivity. But these methods start blind, and a human still has to define the search space: *scalable, but not smart* .


**Palladium** is built to address this gap: intelligence at the scale of the plate. Each well has a purpose — a specific hypothesis about what might work, a control, or a bet on a system interesting enough to try. All with intention.


Palladium’s plans are often good enough that the first plate is the last one. The experiment is designed right, instead of iterated over many rounds.


# What Palladium does


Palladium is a plate designer: a system that turns a proposed chemical transformation into a high-throughput experimental plan.


At a high level, Palladium does four things:


1. It interprets the reaction: what transformation is being attempted, what mechanisms are plausible, and what could go wrong.
2. It retrieves and reasons over precedent: papers, patents, examples, screens, and related transformations.
3. It adapts that precedent to the target reaction: accounting for substrate electronics, sterics, functional groups, solubility, catalyst compatibility, and operational constraints.
4. It proposes a structured plate: a compact set of experiments designed to cover a meaningful region of chemical space under HTE constraints.


The goal of this is starting optimization from an already strong place. If the first plate explores the wrong region, no amount of careful modeling will rescue the experiment efficiently. If the first plate is grounded in the right precedent and the right mechanistic hypotheses, every subsequent round becomes more informative.


# What this means in practice


Traditional reaction optimization is iterative. Bayesian Optimization or DoE will typically converge in 3 to 10 rounds of screening, each requiring plate execution and analytical processing. In high-throughput settings, the analytics, not the actual execution of the reaction, become the bottleneck: processing a 96-well plate is slow, and each round adds days or weeks to the timeline.


Palladium compresses this loop. By starting in the right region of chemical space, the first plate often contains workable conditions. In an internal run, a Palladium-designed first plate delivered **91% yield** — no optimization rounds, no iteration. Early partners have reported **yields above 95% on first-plate designs** for their own programs.


Going from 5 rounds to 1 is not a 5x speedup on the optimization step alone, but it eliminates the analytical bottleneck that makes each round expensive. One well-designed plate, run once, analyzed once, done!


# Our approach


Palladium is built on the intuition that useful reaction planning needs both strong chemical priors and grounding on past transformations.


A strong model with tools can recognize and analize a transformation, and even propose hypotheses worth testing. But precedent reactions, from literature and internal data, offer evidence and support for a given claim, and even inspiration for less standard ideas.


So Palladium searches for relevant examples, patents, papers, and related transformations, then adapts that evidence to the specific substrate and objective. The output is thus a structured set of experiments with concrete reagents and conditions, each defined intentionally: they are useful controls, safe bets, exciting hypotheses, etc.


Users can further review the process and explore the references considered by the system, so that each well can be explained from first principles. This is radically different from other black-box approaches, where the chemist is just supposed to just “trust the algorithm”.


This is the part we care about most: moving from a reaction idea to an experiment worth running.


# Our vision at b12


Our goal at b12 is making difficult chemical spaces practically accessible. Enabling a whole new region of the chemical space is what researchers are doing every day; we are making it accessible, at scale, for every drug designer.


Today, a large portion of chemical space is effectively unavailable. Synthesis is too hard, too slow, too uncertain, or too dependent on scarce human expertise. The path from molecular idea to a physical molecule is too long and expensive to walk repeatedly.


In a world where teams, models, and agents are imagining molecules, the bottleneck is not design but synthesis. Palladium is how we convert hypothetical ideas into real plans, enabling drug discovery teams to **build where the biology points, instead of only where synthesis is easy** .


This is how synthesis becomes the infrastructure for the next generation of molecular discovery.


—Andres M Bran, CTO


If you are working on synthesis bottlenecks in drug discovery, reaction optimization, or lab automation, we would love to talk. Reach us at[\[email protected\]](https://b12-labs.com/cdn-cgi/l/email-protection#b1d7dec4dfd5d4c3c2f1d380839cddd0d3c29fd2dedc) .
