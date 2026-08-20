---
schema_version: "1.0.0"
document_id: "c86de2c6050319aa965926088ee1ad1f92071780642e5cab77a0ac98edcbdf0e"
company_key: "yc-cedana"
company: "Cedana"
source_id: "yc-cedana-news-import-d2b6a3d3a0df"
canonical_url: "https://cedana.com/post/where-does-it-all-go-from-here/"
published_at: "2025-08-14T00:00:00+00:00"
first_seen_at: "2026-07-24T23:42:29.819860+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:0479219d9211f47aa27e013fbed84376999363d70ccd37a8236b85007cd21467"
---

# The Fork in the Road Post GPT-5

cedana / blog


# The Fork in the Road Post GPT-5


On GPT-5 being underwhelming, scaling laws and where we can get reasonable progress from. Also, should AGI still be the goal?


[All posts](https://cedana.com/blog/)[Schedule a demo](https://cedana.com/connect/)[Read the docs](https://docs.cedana.ai/)


Published 2025-08-14 by Niranjan


GPT-5 released yesterday, with mixed responses across the board. This seemed inevitable to some, and the release + step-function increases in capability additionally just seem to show that we’ve hit the limits of scaling training.


We’ve hit the philosophical fork in the road - throwing increasingly more compute at the problem (in all modalities, from training to test-time) won’t get us to AGI. However we can’t reasonably defer solving hard problems until we achieve the mythical goal, so where do we go from here?


Through the lens of problem-solving, GPT-5 is a strong tool. It can stay on-task longer, reason more deeply, and tackle complex workflows. See the[METR](https://www.lesswrong.com/posts/SuvWoLaGiNjPDcA7d/metr-s-evaluation-of-gpt-5) graph below:


Worth keeping in mind though - the “2 hour task” itself is ill-defined and this is assuming a 50% chance of succeeding. Not great!


Therefore, ignoring model hype and focusing on outcomes is a good way to stay sane + deliver value to where it matters. With AI systems (and systems being the operative word here, see the[Interconnects](https://www.interconnects.ai/p/gpt-5-and-bending-the-arc-of-progress) write-up on GPT-5 for more info) becoming capable, we should continue to explore what it looks like to plug them into domains that can benefit.


At Cedana, we’re not interested in model-worship. We’re here to plug them into reality: and reality is messy, unpredictable, and full of constraints that pure scaling can’t solve.


Two domains in particular keep us up at night:


## Reinforcement Learning


We’ve written about RL before, but it’s worth talking about in this context. Quality is variable due to the synthetic datasets, to build systems that interact with the real world you’ve got to be able to replicate the infinitely mineable source of data that is the real world. How good enough can a video generation model be that can give you realistic mappings between inputs and outputs?


[Genie 3](https://deepmind.google/discover/blog/genie-3-a-new-frontier-for-world-models/) is a great example of the direction this could go. From the linked Deepmind post:


If (incredibly simplistically), the model is emulating video taken of the real world (probably mined from videos on Youtube); there is some information in its latent space that encodes information about the physical world. If then you can generate on-the-fly scenarios like the above, you can quickly spin up tens of thousands of scenarios that your agent (like a robotic arm for example) could train on.


## Science


We’ve also written about science before, and it’s also valuable to talk about in this context again.


We’ve already replaced expensive physical prototyping with simulation in countless fields. Airfoil cambers that once needed wind tunnels now get validated with CFD runs in the cloud. Biotech is moving toward atom-level simulations of proteins, drugs, and cell behavior.


From[NASA@SC18](https://www.nas.nasa.gov/SC18/demos/demo1.html) , showing CFD for rotorcraft design.


The next logical leap is swarming these simulations with autonomous agents that explore every parameter space, stress-test every assumption, and surface breakthroughs faster than human cycles allow.


This is what Dario Amodei (CEO of Anthropic)[calls](https://www.darioamodei.com/essay/machines-of-loving-grace#basic-assumptions-and-framework) “a country of geniuses in a datacenter”. This is incredibly compelling to us, because it’s a deep multi-systems level problem that can have high societal impact.


Cedana’s mission is to enable that frontier: resilient, large-scale compute systems that can run thousands of concurrent, high-fidelity experiments - continuously, reliably, and in concert. As a final thought, one of our inspirations is Greg Egan’s Permutation City, where the lead character signs onto a global compute cluster and kicks off what is more or less a higher-fidelity version of Conway’s Game of Life in a simulation called the Autoverse. Our goal is to bring this to life, enabling researchers, engineers, and autonomous agents all connected to a planetary-scale compute fabric, pushing the boundaries of our understanding.


Scaling models may be plateauing. Scaling what we can do with them is just getting started.
