---
schema_version: "1.0.0"
document_id: "cebd4e3cd92a840ebb16e452d1b7210bccc28ee93b7329196cdff5c868939b74"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/artificial-intelligence/optimization-generative-ai-quantum-making-complex-supply-chain-decisions-faster-more-accessible"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:1ef0a80bb18a8a4f7b903ee75b74e096c27e8199a421d8e74d93238aff89a103"
---

# Optimization, GenAI and quantum: Making complex supply chain decisions faster and more accessible

### Key topics


1. Complexity is computationally solvable
2. Two engines and an accelerator
3. From slow model-building to rapid decision cycles
4. Speed matters, but adoption matters more.
5. To use GenAI effectively in optimization workflows, treat it like a powerful but imperfect assistant.
6. Optimization—a capability, not a project


Supply chain leaders don’t lack optimization opportunities. In fact, they face constant disruption: inventory positioning, production sequencing, transportation routing, network design, talent planning, port delays, tariff updates, carrier constraints, labor rules, and shifts in customer priority. Today, global economic volatility makes optimization a continuous requirement, not a periodic exercise. Each change requires updating constraints, retesting assumptions and realigning stakeholder expectations.


The most challenging part of optimization isn’t running the model. It’s building a model that reflects business realities fast enough to support operational decisions.


### Complexity is computationally solvable


Even with the enormous combinatorial complexity of modern supply chains, computational power is not the only limiting factor. In most organizations, the challenges to optimization are organizational, rather than technical.


Optimization models are often treated as black boxes—understood, trusted and modified by a small group of operations research (OR) specialists. This black-box perception creates a bottleneck. When trade rules shift, capacity changes or leadership wants to test a new policy, the model does not evolve at the pace the business requires. This is not because the solver (or the software engine) is slow, but because the workflow surrounding it is.


Optimization does not fall short because mathematics cannot handle complexity.[Quantum computing](https://www.cgi.com/en/quantum-computing) and hybrid quantum-classical approaches are emerging as potential accelerators, for highly complex routing, scheduling and network design decisions where solution spaces are extremely large and densely constrained.


However, in practice, it’s what happens before and after the optimization model runs that slows it down. This includes:


> - Translating ambiguous business rules into precise constraints and objectives
> - Reconciling data definitions and ownership across functions
> - Designing scenario sets that represent uncertainty credibly
> - Explaining results in business terms that decision-makers trust and can act on
> - Repeating the cycle every time assumptions change and re-testing scenarios


This is where[generative AI (GenAI)](https://www.cgi.com/en/artificial-intelligence/generative-ai) can materially improve how people interact with optimization models—not by replacing OR, but by enabling business users to engage with models in their own language and analyze scenarios quickly.


### Two engines and an accelerator


It helps to be precise about what each tool does well.


**Optimization ensures decision integrity:** Optimization (deterministic – known parameters or stochastic – varied parameters) selects the best decision variables such as routes, quantities, assignments and schedules within defined constraints and objectives. It is strongest when consistent, defensible decisions must be made under constraints like capacity, labor rules, service levels, safety stock policies and regulatory restrictions.


**GenAI translates and communicates:** GenAI operates in the “human layer” around decision-making. It converts unstructured information into structured inputs, translates intent into draft code or constraints, generates candidate scenarios, and communicates trade-offs clearly.


**Quantum accelerates select workloads:** Quantum computing, especially hybrid quantum-classical architectures, can speed up certain combinatorial optimization workloads by exploring large solution spaces more efficiently.


Together, they offer a powerful solution to supply chain challenges that are not only computationally complex but organizationally complex as well.


### From slow model-building to rapid decision cycles


Stakeholders rarely describe constraints in mathematical terms. They say:


> “We can’t ship hazmat through that lane.”
> “These SKUs must stay refrigerated end-to-end.”
> “This customer must get priority if inventory is short.”


GenAI can translate these statements into structured constraint templates—not final constraints templates—significantly speeding up formulation of models.


Whether OR teams use Pyomo, OR-Tools, Gurobi APIs, or commercial planning engines, GenAI can accelerate the generation of initial model scaffolding, data pipelines and code. The objective isn’t to blindly accept generated code without review; it is to shorten the time to create a first working prototype.


### Speed matters, but adoption matters more.


Where GenAI can have an even greater impact is in the adoption of OR models through improved explainability.


Optimization succeeds when decision-makers trust it. GenAI can produce decision narratives that explain trade-offs in business terms: “We increased cost by 1.2% to protect a 4.5% service-level risk in Region A under capacity constraints at DC2.”


Clear explanations often determine whether a model stays in a lab or drives operational decisions.


### To use GenAI effectively in optimization workflows, treat it like a powerful but imperfect assistant.


Common failure modes include:


> - Plausible but incorrect constraints
> - Mathematical formulation errors, such as sign mistakes, missing indices and broken logic
> - Overconfident explanations that understate uncertainty
> - Data leakage or security risks when prompts include sensitive commercial information


If these failure modes are left unmanaged, they erode trust in the model. The response is not to limit GenAI but to govern it deliberately.


From our experience, practical guardrails include:


> - Using GenAI to propose, never to finalize, constraints and models
> - Adding deterministic validators: feasibility checks, constraint unit tests and sanity bounds to catch errors automatically.
> - Maintaining transparent assumption logs for every scenario
> - Keeping humans in the loop for approval before encoding any policy changes into a model
> - Establishing clear model governance: version control, audit trails and monitoring


### Optimization—a capability, not a project


Treat optimization as a living capability, not a one-time project. Supply chain ecosystems are highly complex and intricate networks—interwoven across industries and geographies—and they continuously evolve. Tariffs shift, suppliers and networks change, and service expectations rise, so organizations need to update and re-run decision models at the pace of the business.


A practical path forward is a hybrid approach.


Optimization ensures decision integrity under constraints. GenAI makes the models more accessible, allowing leaders, managers and analysts to explore scenarios in their own language, test assumptions and see how changes affect outcomes in real time. Quantum-enabled methods can further accelerate the most complex routing, scheduling and assignment challenges—scenarios where speed often matters the most.


Put together, these capabilities do not simply make optimization faster or more efficient. They make it more transparent, more usable and more aligned with how decisions are actually made.


Connect with our experts to learn how this approach can help support your supply chain strategy.


Back to top
