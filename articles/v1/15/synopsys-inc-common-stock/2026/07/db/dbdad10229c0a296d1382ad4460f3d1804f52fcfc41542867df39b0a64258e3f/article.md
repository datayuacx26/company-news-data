---
schema_version: "1.0.0"
document_id: "dbdad10229c0a296d1382ad4460f3d1804f52fcfc41542867df39b0a64258e3f"
company_key: "synopsys-inc-common-stock"
company: "Synopsys Inc."
source_id: "synopsys-inc-common-stock-news-import-736729784437"
canonical_url: "https://www.synopsys.com/blogs/chip-design/multi-die-multiphysics-co-design.html"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T08:14:37.550587+00:00"
fetched_at: "2026-07-30T08:14:38.264825+00:00"
content_hash: "sha256:e6a766444620f856e6cf137ea35b2b68399a0f5d167a53cad8ed80a80f411c9e"
---

# Multi-Die, Multiphysics, and the New Rules of Chip Design

##


The computing demands of AI are pushing chip design beyond what a single piece of silicon can efficiently deliver.


“You have this beast that needs to be fed,” said Raja Swaminathan, corporate vice president at AMD, speaking alongside other executives from Broadcom, TSMC, and Synopsys at the 2026 Synopsys Executive Forum, part of[Synopsys Converge](https://www.synopsys.com/converge.html) . “The AI compute curve demands more than 4× per year scaling. You need a lot of innovation beyond what Moore’s Law is giving us.”


That innovation is increasingly happening at the system level, with multi-die designs that integrate chiplets on ever-larger substrates. Industry roadmaps already point toward system-on-wafer and, eventually, system-on-panel architectures.


The consequences are multifold. Multi-die systems are pushing multiphysics upstream, blurring domain boundaries, and accelerating the adoption of AI tools and digital twins.


##


---


## Multiphysics Fusion Technology for Multi-Die Designs Explained


Learn why multiphysics analysis must move earlier in the design flow.


[Download eBook](https://www.synopsys.com/resources/multiphysics-fusion-technology-for-multi-die.html)


##


---


## Multiphysics becomes architectural


One consequence of multi-die design is that multiphysics analysis now must happen earlier in the process. Stacking and integrating multiple dies introduces new, often unexpected thermal, mechanical, and power effects.


“We should not be surprised by multiphysics issues at the very end of critical programs,” said Swaminathan. “Stress analysis, thermal analysis, IR, and power distribution analysis all need to be ‘shifted left’ into prototyping and construction.”


This represents a fundamental change in design logic.


“When we designed chips before, packaging and multiphysics were afterthoughts,” said Asad Khamisy, senior vice president of engineering at Broadcom. “Now things are reversed. Multiphysics comes first, and it drives your product definition.”


He pointed to the power density constraints in the top and bottom dies of a 3D stack as an example.


“You have to develop a thermal model for how you will cool the chip even before you decide on your architecture,” Khamisy said.


But moving analysis into architectural exploration creates a new challenge: balancing accuracy and turnaround time.


“It’s a tradeoff,” said Shankar Krishnamoorthy, chief product development officer at Synopsys. “Do you really need the temperature gradient down to the last 0.5 degree Celsius, or can it be a one- or two-degree resolution? And do you want that answer in 10 minutes, or do you want the answer in 12 hours?”


A staged modeling strategy, he added, can provide a middle ground — using fast, approximate models to narrow the design space early, then increasing fidelity as the architecture converges.


*Left to right: Sally Ward-Foxton (moderator,* EE Times *), Asad Khamisy (Broadcom), Raja Swaminathan (AMD), Aveek Sarkar (TSMC), and Shankar Krishnamoorthy (Synopsys) at Synopsys Executive Forum*


## Waterfalls give way to co-design


Workflows are changing in other ways. Organizations are discovering that sequential handoffs between chip, package, and board teams no longer work well with the growing complexity of multi-die systems.


“That handoff-based waterfall approach started to break down,” said Aveek Sarkar, who leads TSMC’s ecosystem and alliance management division.


“Integration and validation are probably more than 50% of the design cycle,” added Khamisy. “And it gets multiplied because the same chiplet gets integrated into multiple different products.”


What’s needed is more collaboration and co-design across teams. Architects and packaging engineers cannot map the design space if they run separate tools and try to reconcile results on conference calls.


“The early pathfinding feasibility analysis that we’ve brute-forced has to become more platform-centric,” said Sarkar.


But getting everyone to work from consistent models creates problems that go beyond tools.


“It’s also a human problem,” Sarkar said. “It’s roles and responsibilities.”


Teams that once worked within clear boundaries must now make decisions jointly. At AMD, for example, packaging teams are working closely with their design and architecture counterparts.


“Packaging teams are having a significant say in the design,” said Swaminathan. “What should be done, what should not be done, what architectural tradeoffs should be made.”


The collaboration needs to extend across companies as well.


“In a multi-die AI super chip today, you have eight to 12 dies from a memory company sitting right alongside a compute die from another foundry,” Swaminathan explained, while admitting the process is still somewhat ad hoc. “Some sort of engineering framework needs to come together to communicate within teams, within a company, and across companies in the service of the entire package.”


## AI moves from feature to flow


For cross-domain co-design to be fully realized, underlying toolsets must be better integrated.


“We have architectural simulation tools and we have 3DIC package analysis tools, but linking them is still clunky,” Krishnamoorthy said. “Data often doesn’t flow cleanly between domains.”


AI-driven tools that[pair EDA with multiphysics](https://www.synopsys.com/solutions/multiphysics-fusion.html) will be critical. As these tools evolve from optimizers to assistants to fully autonomous agents, they will increasingly manage the repetitive, iterative workflows demanded by the scale and complexity of multi-die designs.


“With agents, you can iterate and perform design exploration over a large number of environments very well,” said Khamisy.


Digital twins will also play a key role, unifying data and models across domains. Sarkar described an environment where different teams work together on pathfinding and feasibility analysis.


“All of them are looking at the same thing, at the same time, making simultaneous decisions,” he said. “Co-design becomes a reality.”


It's also becoming a necessity. Multiphysics is moving into architecture, collaboration is tightening across domains, and AI is taking on more of the exploration.


As compute demand continues its steep climb, silicon-to-system co-design will be essential to keep the AI beast fed.


[On Demand: Synopsys Executive Forum 2026 Panel Videos](https://www.synopsys.com/converge/executive-forum-2026/watch.html)[eBook: Multiphysics Fusion for Multi-Die Designs](https://www.synopsys.com/resources/multiphysics-fusion-technology-for-multi-die.html)


- [About Synopsys](https://www.synopsys.com/blogs/chip-design/category.about-synopsys.html)
- [Executive Voices](https://www.synopsys.com/blogs/chip-design/category.executive-voices.html)
- [AI & Machine Learning](https://www.synopsys.com/blogs/chip-design/category.ai-and-machine-learning.html)
- [Multi-Die](https://www.synopsys.com/blogs/chip-design/category.multi-die-system.html)
- [Design](https://www.synopsys.com/blogs/chip-design/category.design.html)
- [Verification](https://www.synopsys.com/blogs/chip-design/category.verification.html)
