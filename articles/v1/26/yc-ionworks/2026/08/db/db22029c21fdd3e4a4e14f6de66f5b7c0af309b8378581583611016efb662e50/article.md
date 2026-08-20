---
schema_version: "1.0.0"
document_id: "db22029c21fdd3e4a4e14f6de66f5b7c0af309b8378581583611016efb662e50"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/case-study-faam"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T20:24:57.978245+00:00"
fetched_at: "2026-08-06T20:24:59.176981+00:00"
content_hash: "sha256:f168df5c925da769384f0ac5f8a6a648d3cc12b6a046763b198eaaba898f2260"
---

# How FAAM got 80% of an engineer's time back

## Who they are


FAAM is an Italian manufacturer of eco-friendly lithium-ion energy storage systems for industrial, automotive, and renewable-energy applications. Their cell-simulation team, three engineers inside R&D, turns cycling data into the parameterised cell models that underpin FAAM's degradation projection, capacity-fade work, and the warranty claims they make to stationary-storage customers.


## The problem


Building those models accurately is a core R&D competency. Before Ionworks, the team ran a self-built[PyBaMM](https://ionworks.com/blog/our-relationship-with-pybamm) pipeline: cost function, minimiser, optimiser population, and all the orchestration around it. It worked, but it was substantial software infrastructure for a small group to own alongside the science. Roughly 80% of the lead engineer's time went into keeping it alive rather than into the modelling work that delivers on FAAM's product needs.


## What changed


With Ionworks running the pipeline, the team's time moved from devops back to insight.


The clearest example is FAAM's LFP positive electrode. The coating is a mix of small primary particles and larger agglomerates, and capturing that distribution accurately is critical to a model that reflects reality. Earlier setups failed to resolve both the large- and small-particle dynamics at once, collapsing the distribution onto a single effective particle size and hiding the physics that drives pulse-rate behaviour. Ionworks helped[parameterise](https://ionworks.com/blog/battery-parameter-estimation) and simulate the full distribution, so the cell model FAAM uses for downstream product decisions matches what is actually inside the cell.


The figure above shows the difference. Two positive-electrode particle-size distributions are matched on total surface area, the kind of constraint a team uses when replacing a measured distribution with a single-lognormal approximation. Under a distribution-aware solver the two keep their timescales separate, and the approximation becomes visible as a pulse-response difference in voltage and surface stoichiometry, rather than getting silently absorbed into a lumped diffusion coefficient.


Two chemistries now run in parallel without a second set of scaffolding, and protocol choices move through simulation before the cycler.


## The business case


The return breaks down three ways:


- **$150K/yr in direct operations savings.** The lead engineer's split used to run roughly 80% infrastructure to 20% modelling. With Ionworks running the pipeline it inverts, and senior modelling capacity that used to absorb infrastructure work is back on the team's broader pipeline.
- **A 30×+ build-vs-buy gap.** On an annual spend of $50K with Ionworks. The in-house alternative is roughly $1.5M+ over three to four years with a team of ten engineers, fully loaded — a capital-intensive software project that would not otherwise reach parity.
- **$10K–$100K+/yr in mis-started tests avoided.** When a protocol is configured wrong but runs anyway, the cost is cycler energy spent on a dud, chambers tied up, and calendar weeks lost to re-runs. Every protocol vetted in simulation first is one fewer mis-started run on the floor.


## Why it matters


**Warranty and lifetime confidence.** The validated cell model is the technical foundation that degradation projection and capacity-fade work are built on — direct evidence behind the multi-year warranties FAAM stands behind to its stationary-storage customers.


**Models that capture material reality.** The full-distribution LFP model reflects what is actually inside the cell rather than a single effective particle size. The models build trust, and that trust is what supports the downstream warranty work.


**Formulation design conversations.** When the team changes a coating and sees different cell behaviour, simulation now reproduces it. Formulation reviews move from empirical debate to a quantitative, modelling-backed argument with management.


## What the team said
