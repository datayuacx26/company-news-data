---
schema_version: "1.0.0"
document_id: "75c340ac900e1e9bf30c88d12620c1e3a664a318e94b3baead23d145d0d485e9"
company_key: "international-business-machines-corporation-common-stock"
company: "International Business Machines Corporation"
source_id: "international-business-machines-corporation-common-stock-news-import-da8ebfcd6a45"
canonical_url: "https://research.ibm.com/blog/gridfm-neural-solver-power-grid"
published_at: "2026-08-11T13:00:40.825+00:00"
first_seen_at: "2026-08-12T01:49:53.952128+00:00"
fetched_at: "2026-08-12T01:49:56.034559+00:00"
content_hash: "sha256:4975c508f39d8768f7ea9c78613ce7df1b2a639102e094789760cc50365fad34"
---

# From vision to reality: a unified neural solver for the power grid

The electric grid has never mattered more — or faced more pressure. As transportation, buildings, and industry electrify, ever more of modern life runs through the same network of wires. At the same time, the grid is being reshaped from both ends.


On the supply side, decentralization is accelerating. Roughly 2.6 terawatts of mostly clean generation and storage — nearly double the entire capacity installed across the United States today — sits in interconnection queues, waiting several years to connect. On the demand side, AI is adding load at a pace the grid has rarely seen: Global data-center electricity use is set to more than double by 2030, and these facilities want to plug in within months, not years.


More variable generation, shifting load patterns, and large new interconnections all add uncertainty — and every one of them multiplies the number of grid conditions that operators and planners must analyze to keep the lights on.


In late 2024 IBM Research and its collaborators laid out a vision for[foundation models for the electric grid](https://research.ibm.com/blog/how-ai-can-prepare-the-electrical-grid-for-the-low-carbon-era) : fast, physically consistent, task-agnostic neural solvers that could give grid operators and planners a new tool to keep pace with an ever-changing system.


Now, that vision is on its way to becoming a reality. Today, we’re releasing our first contribution for steady-state grid analysis through the[OpenGridFM project](https://lfenergy.org/projects/opengridfm/) at Linux Foundation Energy (LF Energy).


Today’s release has two parts: GENCO (Geometric Neural Corrective Solver), an open-source neural solver that unifies three steady-state grid-analysis tasks into a single, shared representation; and the GridFM Development Framework, which makes building and benchmarking neural grid solvers fair, fast, and reproducible. The[paper](https://doi.org/10.48550/arXiv.2608.09921) describing the solver and framework is available on arXiv.


## The mathematical bottleneck


The U.S. Department of Energy’s (DOE)[National Transmission Planning Study](https://www.energy.gov/oe/national-transmission-planning-study-0) notes that planning has historically relied on a relatively small number of system snapshots. With more variable renewables, changing demand, and power transfers between regions and states, planners will increasingly need to analyze thousands, or millions, of chronological power-flow cases — and operations are no less demanding. A large system operator such as the Midcontinent Independent System Operator (MISO), which looks after 15 U.S. states’ (and Manitoba, Canada’s) power consumption, evaluates on the order of 10,000 contingencies every few minutes, adding up to well over a billion grid calculations across the year.


At that scale, the detailed numerical alternating current (AC) models that engineers trust become too slow to run for every case, said co-author Etienne Vos, a manager at IBM Research working on this project. So the industry falls back on faster direct current (DC) approximations. These trade away accuracy and leave key grid variables, such as voltage magnitudes and reactive power, out of the picture entirely. Operators often face a trade-off between computational speed and model accuracy.


## GENCO: one solver for three tasks


That trade-off is exactly what GENCO is built to soften. Compared with classical AC solvers, it delivers speedups orders of magnitude greater. Compared with DC approximations, GENCO improves quality and recovers the full set of grid variables. It occupies a valuable middle ground in the modeling toolkit that utilities rely on today.


There are three main core grid computations: power flow (PF), optimal power flow (OPF), and state estimation (SE). Each typically requires its own specialized solver and pipeline, whereas GENCO can handle all three in a single unified model with a shared representation. That makes complex assessments, which often chain several of these tasks together, far simpler to run.


Under the hood, GENCO is built on a graph of the power grid, with a[heterogeneous graph transformer](https://arxiv.org/abs/2003.01332) as its backbone, corrective layers that enforce physical consistency, and physics decoders that turn its predictions into feasible solutions for each task — on grids of up to 10,000 buses (nodes in an electrical network).


The unified design pays off in development effort as well as speed. Because the same backbone and hyperparameters serve all three tasks, the expensive tuning that usually dominates a data scientist’s time is done once, rather than repeated for every task and pipeline.


The performance gains are substantial. For relevant grid sizes, GENCO is up to 30 times faster than a Newton–Raphson AC solver at computing how much power flows through each line (PF), at residuals comparable to DC solvers. It’s also up to 85 times faster than interior-point solvers at finding the least-cost generator settings (OPF), at an optimality gap of 0.3% or less. And for state estimation, GENCO outperforms classical solvers in accuracy under sparse measurements and noisy signals, and it always returns a result, even when classical methods fail to converge.


“For years, teams built a separate model for every grid task. GENCO shows you don’t have to,” said paper co-author Thomas Brunschwiler, principal research scientist and manager at IBM Research. “One architecture learns the shared physics of the grid and handles power flow, optimal power flow, and state estimation — at a fraction of the development cost and orders of magnitude faster than the classical tools.”


GENCO was also tested extensively for robustness, including operating outside nominal conditions and high-order contingencies — up to 20 components like power lines or transformers failing — where it outperformed DC solvers. In those calculations, median residuals were about 3 times lower, meaning that the predicted grid state better aligns with the physical laws of power conservation.


The team also took first steps toward generalizing the model to new power grids. They pre-trained the model on 100 decomposed grids, which doubled the data efficiency when adapting to a new grid. True zero-shot performance on entirely unseen topologies remains an open challenge, and one they are actively working on.


“Today, grid operators and planners are forced to make critical decisions using only a small fraction of the possible futures the system may face,” said paper co-author Hendrik Hamann, chief AI scientist for Innovation, Science, and Security at Brookhaven National Laboratory and professor at Stony Brook University. “GridFM with GENCO matters because it brings together the grid community to build foundation AI models that can evaluate those futures at unprecedented speed and scale — giving us the foresight to operate the grid more securely today and plan it more reliably for decades to come.”


At Brookhaven Lab, Hamann is leveraging GridFM to build GridSearch, a system for accelerating electric grid interconnection of AI data centers.


GENCO (Geometric Neural Corrective Optimizer) functions within the development framework that includes gridfm-graphkit for training and evaluating the neural solver, and gridfm-datakit for generating synthetic data.


## Proven on a real grid


GENCO was successfully validated on a real transmission-grid topology, using a full year of supervisory control and data acquisition (or SCADA) data from energy company Hydro-Québec’s 1,200-bus transmission network. The real-world operational data was used under strict data governance and security safeguards. This type of validation is important because access to operational grid data is necessarily limited in the energy sector. Demonstrating a model on genuine SCADA data — rather than synthetic benchmarks alone — is a major milestone for the industry.


“Validating steady-state capabilities on a real network as the one from Hydro-Québec is a necessary first step,” said study co-author François Mirallès, researcher at the Hydro-Québec Research Center.


## An open framework for the community


The project is also a statement about how AI for critical infrastructure should be developed: together and in the open.


Scientists at IBM Research developed GENCO and the GridFM Development Framework under the guidance of Hydro-Québec, which brought domain expertise and validated the model on real-world cases. LF Energy hosts the OpenGridFM project, and Stony Brook University, Brookhaven National Laboratory, and ETH Zürich contributed as research partners, advancing and optimizing the libraries.


These organizations are all part of the wider GridFM community, founded by IBM Research and currently led by Hamann. It spans industry, academia, and government, with more than 200 organizations and over 500 members. GridFM is supported in part by the DOE Office of Critical Minerals and Energy Innovation.


“Each partner has contributed their unique expertise, and everyone has pushed for truly validated results that can be adopted by industry,” said Juan Bernabé-Moreno, director of IBM Research Europe for Ireland and the UK. The team pushed for reproducible results that industry can inspect, compare, and adopt, over a one-off model hidden behind closed assumptions, he added.


“This work is distinguished by the breadth of the community behind it: Many dozens of organizations representing the full spectrum of grid operations and power systems, collaborating closely to develop foundational AI capabilities for the power sector,” said Hamann. “This paper and model mark an important step toward realizing that shared vision and transforming the industry.”


## We built the tools we wish we had


To compare GENCO fairly against other models, first author Alban Puech, who worked on the paper as a Ph.D. student at IBM Research, spent countless hours contacting the authors of individual papers just to recover the missing details of their experiments. “That shouldn’t be the price of doing good science,” he said. “Our framework bakes those details in, so the next comparison takes minutes, not months.”


He found that building neural solvers for power systems is hard for four stubborn reasons: very limited access to real-world grid data, limited open-sourcing in the community, missing details needed to reproduce and compare results, and a requirement for deep expertise in both computer science and power systems.


So Puech and team built the fully open-source GridFM Development Framework — the core of the LF Energy OpenGridFM project, released under an Apache 2.0 license — to address all four:


- **gridfm-datakit** lets the community easily generate diverse but realistic datasets, including load and topology perturbations. Its realism was validated against the Hydro-Québec SCADA data.
- **Open datasets:** The researchers harmonized several community-accepted datasets and extended them with datakit data for large grids, forming an open collection of 4 million instances across 6 grid topologies, hosted on Hugging Face.
- **gridfm-graphkit** supports training and benchmarking of GENCO — and of your own neural solvers — in a low-code environment.
- **Fair runtime analysis:** They provide scripts for rigorous speed comparisons between neural and classical solvers, which is tricky because the two run on different hardware — GPUs and CPUs, respectively.


Together, these tools let the research community innovate faster and more transparently, while power-system engineers can apply GENCO to their own grid without needing deep computer-science expertise.


"OpenGridFM is what you get when organizations align their interests and commit to collaboration that produces actual working technology. Working in the open cuts the time and cost of building shared tools, and raises the level of trust at the same time," said Alex Thornton, executive director of LF Energy. "Operators need to inspect and test these solutions before they can rely on them, and a model validated on a real utility network, with open data and open benchmarks anyone can check, is something an engineer can interrogate and a regulator can scrutinize. LF Energy's job is to make that kind of collaboration safe and durable, handling the governance, licensing, and legal frameworks so the work can move out of the research lab and into everyday planning and operations."


“This ability to combine each organization’s goals with investment in a shared asset is key to present and future successes,” said Mirallès.


## Built for real operations: a hybrid workflow


While GENCO represents a promising research advancement, it remains part of an ongoing validation effort and is not currently positioned as a replacement for the established tools used in real-time grid operations. The researchers recommend deploying it alongside a classical AC solver, so operators gain throughput without giving up numerical assurance. The safeguarded workflow has three steps: GENCO rapidly evaluates the bulk of scenarios; power-balance residuals and violation checks — enabled by GENCO’s full state prediction — flag suspect and critical cases; and classical solvers provide authoritative solutions for those few cases.


With well-chosen criteria for detecting critical cases, the cost of running GENCO is largely offset by avoiding full AC evaluations for the large fraction of scenarios that don’t need them. The result is a secure, scalable assessment across large ensembles that prevents unphysical or erroneous predictions from propagating downstream.


“Validating steady-state capabilities on a real network such as the one from Hydro-Québec is a necessary first step,” said Mirallès. “We should consolidate the framework with more advanced testing and keep the option to pair these new tools with legacy tools to get closer to industrial applications. This work will help jumpstart an extension of the GridFM concept in the time domain with even higher value applications in sight.”


## The next frontier: quantum power grid optimization


“GENCO is an important milestone — but it’s only the beginning,” said Bernabè-Moreno. Unit commitment and transmission switching, for example, require optimizing both continuous variables like power flows and discrete decisions like which generators or transmission lines are switched on or off. “We see a promising future in pairing GENCO with quantum computing: quantum processors tackling the combinatorial decisions, while GENCO rapidly solves the continuous optimization,” he added. “Early experiments are already underway, and the first results are encouraging. Stay tuned.”


Alongside this latest release, the community momentum is growing. This October, the[6th Conference on Foundation Models of the Electric Grid](https://gridfm.org/porto/) will convene in Porto. In September, the[Linux Foundation Energy Summit Europe](https://events.linuxfoundation.org/lfenergysummit-europe/) will meet in Berlin. There will also be a[grid foundation models workshop](https://ai4powergrids.github.io/) at the Conference on Neural Information Processing Systems (NeurIPS) in December, with a call for papers currently out.


For now, the call to the community is straightforward: Use the tools, test the benchmarks, generate new data, and extend the framework. The future grid will be built by many organizations working together. GENCO provides an open starting point.
