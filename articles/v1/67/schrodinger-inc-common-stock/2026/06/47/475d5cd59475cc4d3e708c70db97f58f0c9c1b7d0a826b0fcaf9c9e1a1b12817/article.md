---
schema_version: "1.0.0"
document_id: "475d5cd59475cc4d3e708c70db97f58f0c9c1b7d0a826b0fcaf9c9e1a1b12817"
company_key: "schrodinger-inc-common-stock"
company: "Schrodinger Inc."
source_id: "schrodinger-inc-common-stock-rss-d7bf526e9e3e"
canonical_url: "https://extrapolations.com/physics-meets-agents-the-next-chapter-of-our-work-with-nvidia/"
published_at: "2026-06-23T13:46:28+00:00"
first_seen_at: "2026-07-20T23:18:31.059214+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:5f87a9b6eb1a9407869431baac47200b2c8ba2aa7e2407ff5f2661a411766ae7"
---

# Physics Meets Agents: The Next Chapter of Our Work with NVIDIA

[Life Science](https://extrapolations.com/category/life-science/)


# Physics Meets Agents: The Next Chapter of Our Work with NVIDIA


BY Schrödinger Editorial Team


Jun 23, 2026


For years, computationally driven drug discovery has faced two bottlenecks at once: accuracy and scale. It’s not enough to run one good calculation. Scientists need results they can stake real decisions on, and they need them across a chemical space far too vast for any wet lab to explore by hand. Physics-based approaches deliver the accuracy; the challenge has been running calculations at the scale and speed the discovery loop demands without trading one for the other.


Our approach has long been to replace trial-and-error with prediction – using physics-based simulation and AI to decide which molecules are worth making. But even when accuracy and scale are solved, a third barrier stands: expertise. Setting up, executing, and interpreting rigorous computational chemistry has historically required specialized training, putting these methods out of reach for many of the teams making day-to-day design decisions. The next transformational shift tackles all three at once. We are moving beyond tools that accelerate isolated steps and entering the era of AI agents: intelligent systems that reason through an R&D problem, plan the right calculations, execute them, and act on the results autonomously.


This evolution relies on a tight integration between domain-specific software and accelerated computing. It’s a frontier we’ve been exploring alongside NVIDIA for several years.


**A collaboration built on acceleration**


Schrödinger has been building on NVIDIA’s accelerated computing for years. Our platform went CUDA-native early and we’ve drawn on the[BioNeMo](https://www.nvidia.com/en-us/industries/healthcare-life-sciences/) CUDA-X library ecosystem to make sophisticated molecular dynamics simulations routine.


That work shows up in concrete performance. In recent benchmarking of[FEP+](https://www.schrodinger.com/platform/products/fep/) , our free energy perturbation method for predicting how tightly drug candidates bind to their target, NVIDIA[RTX PRO™ 6000 Blackwell Server Edition GPU](https://www.nvidia.com/en-us/data-center/rtx-pro-6000-blackwell-server-edition/) delivered roughly a[10x increase](https://learn.schrodinger.com/private/edu/release/current/Documentation/html/fep/fep_user_manual/fep_performance.htm) in calculations per day over prior-generation hardware. This turns what was once a scheduling constraint into headroom for scientists to explore more chemistry.


Our joint research has steadily lowered the cost of a high-quality prediction. The natural next step is to let an intelligent system orchestrate those predictions end to end. Enter Bunsen.


**Meet Bunsen: Our agentic co-scientist**


Earlier this year we announced[Bunsen](https://www.schrodinger.com/platform/products/bunsen/) , our AI co-scientist, with an early-access version becoming available this summer. Bunsen can set up and monitor complex computational simulations and reason through R&D workflows, autonomously handling multi-step work that used to require an expert at the keyboard at every stage.


Consider one real request a scientist made to Bunsen on a drug discovery project: “Suggest regions on this molecule I can modify to improve potency, but only where we already have supporting lab data, reliable simulation predictions, and space in the 3D binding site. Then run de novo design at that position and return the ideas predicted to be most potent and synthetically tractable.” Behind that request, Bunsen performs a chain of expert reasoning that would normally occupy a team of specialists:


- **Connects disparate systems.** Bunsen reads the molecule’s 3D structure from the scientist’s computer and pulls the experimental history of hundreds of related compounds in[LiveDesign](https://www.schrodinger.com/platform/products/livedesign/) , our enterprise informatics platform.
- **Weighs competing scientific criteria.** Bunsen recognizes the shared chemical skeleton and identifies where chemists had historically made changes. For each candidate location, it balances how much potency varied with past modifications, how much prior data exists, and how trustworthy the simulations are.
- **Maps chemistry to biology in 3D.** Bunsen maps each site into the target protein’s 3D structure to explain why a chemical change should work.
- **Expertly operates our software and recovers from setbacks.** With the plan approved, it operates Schrödinger’s software in order – building the docking target map, validating inputs, launching our[De Novo Design Workflow](https://www.schrodinger.com/platform/products/de-novo-design-workflow/) to generate and rank new ideas, running FEP+ to predict potency, and assessing synthetic tractability with[RetroSynth](https://www.schrodinger.com/platform/products/retrosynth/) . Crucially, when an early setup step fails, it diagnoses the cause, fixes the configuration, and relaunches the simulation autonomously.


Bunsen is designed to be language-model-flexible. For instance, for customers interested in cutting edge open source LLM performance, we are excited about the potential of integrating open source[NVIDIA Nemotron](https://www.nvidia.com/en-us/ai-data-science/foundation-models/nemotron/) models within the[BioNeMo Agent Toolkit](https://github.com/NVIDIA-BioNeMo/bionemo-agent-toolkit) , which are built for high-throughput, long-context reasoning. Because Bunsen is built to support a variety of frontier models, enterprise teams retain control to deploy the reasoning engine that best matches their environment, security protocols, and existing infrastructure.


**The shift: From reactive testing to proactive design**


The payoff for scientific teams is a shift from reactive lab-testing to proactive, predictive design. The friction of deploying advanced computational methods evaporates because the AI agent handles the heavy lifting of orchestration. The path from spark of an idea, to computation, to physical experiment gets drastically shorter.


The true power of this technological evolution is that it gives scientists back their most valuable resource, the time to think. As NVIDIA brings agentic tooling to the broader life sciences community, we’re proud to be building alongside them – drawing on years of collaboration to move toward a future where AI agents and physics simulations collaborate to explore chemistry before the first molecule is ever synthesized.


Schrödinger Editorial Team


### Sign Up Today


Sign up to receive quarterly updates with the latest from Extrapolations.


### Up Next


[Life Science](https://extrapolations.com/category/life-science/)[How Innovations in Virtual Screening are Revolutionizing Drug Discovery In a new perspective published in the Journal of Medicinal Chemistry, industry experts share how these technologies are reshaping drug discovery and opening doors to treat diseases once thought undruggable. We sat down with two co-authors of the paper — Steven Jerome, Executive Director at Schrödinger and Paraskevi Gkeka, Principal Scientist at Sanofi — to explore how the age of trial-and-error is giving way to a smarter, more computationally-driven approach to drug discovery. BY Steven Jerome, Ph.D. & Paraskevi Gkeka, Ph.D.](https://extrapolations.com/how-innovations-in-virtual-screening-are-revolutionizing-drug-discovery/)
