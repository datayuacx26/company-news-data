---
schema_version: "1.0.0"
document_id: "7030f1e4bfdc5f83a4aea2e448e3e16afe5b8ae85e998faf74e4a56909a4ba4f"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/blueprint-for-fault-tolerant-trapped-ion-quantum-computing-the-walking-cat-architecture"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:67372669074103ca935e108d8bed566ae48f8a58000302a9683d2d3fbdcc183c"
---

# Blueprint for Fault-Tolerant Trapped-Ion Quantum Computing: The Walking Cat Architecture

Cumulative government investment in quantum technologies now exceeds $56 billion worldwide. Private investors added nearly $5 billion more in 2025 alone, more than double the prior year. IonQ has led this field at every stage, consistently delivering on its roadmap while the rest of the industry works to keep pace.


All of that capital has been building toward one moment: a complete, buildable engineering specification for a fault-tolerant quantum computer that a team can actually execute against. That moment has arrived.


The hardware capabilities this machine requires have already been experimentally demonstrated: high-fidelity two-qubit gates and reliable ion transport. IonQ is actively scaling physical qubit counts toward the thousands needed to run it. The Walking Cat Architecture is that specification. In 1945, John von Neumann's draft report on the EDVAC articulated the stored-program architecture that every classical computer since has been built on. It was not a theoretical proposal. It was a complete engineering specification. This paper is the quantum equivalent: the first end-to-end blueprint for a fault-tolerant quantum computer grounded in realistic engineering constraints, covering compiler, logical architecture, and micro-architecture for a machine capable of running millions of gates on hundreds of logical qubits. Just as von Neumann's architecture did not describe a single machine but the foundation of an era, this blueprint is designed to scale: it is the framework IonQ will build on through our 2030 objective of 2 million physical qubits and 80,000 logical qubits.


The architecture is named for what it does at the physical level: cat states, walking.


- "Cat" refers to cat states, a specific class of quantum resource state used to perform fault-tolerant logical measurements. A quantum computer cannot read a logical qubit directly without destroying the quantum state it is trying to protect. Cat states solve this problem: they act as probes, sent to interact with the logical qubits to check for errors, reporting what went wrong without collapsing the underlying computation. The concept traces back to Erwin Schrödinger's 1935 thought experiment, which birthed (and maybe killed) the famous cat. When Peter Shor authored one of the first fault-tolerant quantum error correction schemes in 1996, he chose the cat as a natural moniker for this specific resource, although it has rarely been used in practical architectures since.


- "Walking" describes how those states move: ions are physically shuttled through a quantum charge-coupled device (QCCD) chip across a grid of specialized zones, traveling to dedicated gate zones where two-qubit operations are performed and optical zones where measurement and reset occur. Because any ion can be routed to any zone on the chip, the architecture achieves any-to-any qubit connectivity through movement rather than fixed wiring. Multiple zones operate simultaneously across the chip. The architecture scales by adding zones, not wires.


[The paper](https://arxiv.org/abs/2604.19481) is direct about what it set out to do:


> An end-to-end blueprint for an FTQC architecture based on modern quantum error-correcting codes and designed with realistic engineering constraints in mind is still missing in the literature. In this work, we bridge that gap.


This is the blueprint that IonQ will use to build the fault-tolerant era.


This is the first in a seven-part series working through the architecture systematically. What follows goes deep on each layer.


The architectural choices that follow share a common purpose. At the fault-tolerant layer, the central cost driver is overhead: how many physical qubits are consumed to maintain a single reliable logical qubit, and how much time it takes to perform an operation — not a single physical gate, but a full logical operation. The code families chosen, the unified framework, and the component structure are each a direct answer to that problem. The paper includes example configurations spanning 102 to 220 logical qubits, achievable with physical qubit counts from a few thousand to tens of thousands. These configurations run at a conservative logical error rate target and are proof points of a system that can actually be built, not performance claims. The architecture also supports configurations with higher error rate targets that require fewer qubits per logical qubit, different fidelity-for-speed tradeoffs, and other choices that we can make to suit various specific needs and use cases, all from the same basic physical hardware and modular architecture. That configurability is why you see different numbers in different places in this blog post and the supporting paper — the *architecture* supports many positions on those sliders with minimal rework, which allows us to continue to iterate towards the best configuration (or configurations!) that drive the most application-oriented value over time.


## What Makes the Walking Cat Different


Prior quantum architecture work typically falls into one of two categories: theoretical proposals that address a single layer of the stack, or designs that optimize for performance by mixing different code families across components, a choice that adds engineering complexity that rarely survives the transition from paper to physical machine. The walking cat architecture takes a different approach, organized around four principles called HMRS: Hierarchy, Modularity, Regularity, lifted directly from classical computer architecture. To these, we add Simplicity to emphasize that the goal is to build this machine in practice, not principle. The acronym is pronounced "hammers," a name that reflects how non-negotiable these tenets are to the design. Every component choice in the architecture traces back to one of them.


- **Hierarchy** structures the machine in three distinct layers: compiler, logical architecture, and micro-architecture, each building on the one below. A change to the error correction scheme in the logical layer requires only an update to resource cost estimates passed to the compiler, not a redesign of the compiler itself. Hardware improvements at the micro-architecture layer do not invalidate the logical architecture above it. Each layer can evolve independently.
- **Modularity** means components operate independently and interface exclusively through resource states produced in one component and consumed in another. Cat states, magic states, Bell states: each is manufactured in a dedicated factory and transported to where it is needed. The magic factory produces the resource states required for non-Clifford gates. The cat factory produces the cat states required for logical measurements. Neither needs to know how the other works. This is what enables the machine to run different operations simultaneously across the chip.
- **Regularity** means the same components tile repeatedly across the hardware. Memory blocks, magic factories, and cat factories share the same code families and transport mechanisms. The three-ring framework used in the memory block is reused across the magic factory and cat factory. The same design language applies everywhere, making the architecture predictable to build and straightforward to verify at scale.
- **Simplicity** is where the walking cat architecture makes its most consequential choices. Prior architectures typically combine topological codes, surface codes, and LDPC codes across different components, each requiring its own interfaces, error correction logic, and decoder. The walking cat uses one framework throughout. Memory blocks, magic factories, and cat factories all draw from the same three code families: generalized bicycle codes, bivariate bicycle codes, and cyclic hypergraph product codes. The same code design governs both memory and magic factory components. A single decoder handles all of them. The result is fewer interfaces, fewer failure points, and a machine that can be reasoned about as a unified whole rather than a collection of interleaved subsystems.


## The Architecture in Three Layers


Managing a machine of this complexity requires clear boundaries between its layers. The walking cat architecture draws three: compiler, logical architecture, and micro-architecture, each of which can be designed, tested, and improved without disrupting the others. This is the same separation of concerns that has kept classical computer architectures maintainable across decades of hardware change.


At the top, a compiler takes a high-level quantum program and decomposes it into a sequence of logical instructions. At the bottom, the micro-architecture maps those instructions onto the device instruction set of the underlying QCCD chip. In the middle sits the logical architecture, which defines what the instructions do and organizes the five types of components that carry them out: memory blocks that store logical qubits under continuous error correction, magic factories that produce the resource states needed for non-Clifford gates, cat factories that generate the cat states required for logical measurements, Bell factories that connect components across the chip, and a qubit factory that handles ion replacement when physical qubits are lost, a specific consideration for atomic qubit modalities.


Full block connectivity and full block parallelism are features of the logical architecture: any two-block operation can be assigned to any pair of blocks regardless of physical distance, and any combination of logical instructions acting on different blocks can execute simultaneously. This is significantly more flexible than fixed-connectivity architectures, which are typically limited to nearest-neighbor block operations.


## Built on Hardware That Already Exists


IonQ's walking cat architecture is designed specifically for two capabilities that have been experimentally demonstrated on QCCD trapped-ion hardware: two-qubit gate fidelity above 99.99%, and reliable ion transport. Both are capabilities commercial systems have already achieved, not aspirational targets for future hardware generations.


The high gate fidelity matters specifically because it enables cat state preparation in a few attempts, a process called post-selection. If preparation required many retries, the overhead would make the system impractically slow. Ion transport matters because LDPC codes require qubit connectivity that extends beyond nearest neighbors. Superconducting systems achieve this through physically long-range couplers, which impose manufacturing constraints. Trapped-ion systems achieve it naturally through ion shuttling, which the walking cat architecture treats as a first-class design primitive.


The paper states this directly: it aims to provide "a blueprint to enable this breakthrough, including a complete description of the architecture components and subcomponents and thorough simulations of their performance."


These requirements are not incidental to the design. The architecture was built around what trapped-ion systems do naturally, and IonQ has already demonstrated both capabilities at the scale this blueprint requires. IonQ has approached every stage of its roadmap the same way: demonstrate the underlying capability, then build on it. The walking cat architecture follows that same logic.


## A Glimpse of What Becomes Possible


To ground the architecture in practical application, the paper compiles a concrete simulation: the Heisenberg Hamiltonian simulation of a random 7-degree regular graph, the class of material science calculations relevant to understanding disordered magnetic systems such as spin glasses. The paper characterizes this class of problem as classically intractable at the relevant scales and out of reach for NISQ machines, requiring millions of quantum gates.


Compiled onto a walking cat architecture instance with approximately 10,000 physical qubits, the estimated execution time to reach chemical accuracy (the precision threshold at which quantum simulation results are reliable enough to inform material science research) is on the order of one month. These estimates are conservative, grounded in demonstrated hardware performance of today and not adjusted for performance improvements expected as IonQ's systems scale. The significance is structural: the same class of architecture that can be built from tens of thousands of physical qubits is projected to place classically intractable problems within reach.


The paper also demonstrates compilation of Shor’s period-finding algorithm for a 20-bit integer onto a 102-qubit walking cat instance, confirming that the architecture’s logical ISA is universal and that the compiler stack can translate real-world quantum algorithms end to end.


## From Theory to Blueprint


This paper is IonQ opening the books. Not on what a fault-tolerant quantum computer might look like, or what one could theoretically achieve. On what we are building, how we are building it, and why the choices we have made produce a machine that can be fabricated on hardware that exists today. The compiler is specified. The logical architecture is specified. The micro-architecture is specified. The decoder is implemented and simulated. The component designs are complete. This is the architecture IonQ will use to build the fault-tolerant era, and this series will work through every layer of it in detail.


## About This Series


"The Walking Cat Architecture" covers the full stack from compiler to chip across six substantive technical sections, each representing a depth of engineering detail that warrants its own treatment. Here is what the series covers:


**Post 2: The Architecture Model.** The moving qubit model that forms the physical ISA, the noise model, and the design principles that shape every component.


**Post 3: The Logical Architecture** . Memory blocks, magic factories, cat factories, Bell factories, the qubit factory, and the logical instruction set architecture in depth.


**Post 4: Implementation of the Components** . The three-ring memory framework, syndrome extraction circuits, error and loss correction gadgets, and cat factory design.


**Post 5: The Micro-Architecture** . How logical instructions map to QCCD chip operations, the memory micro-architecture, and the cat factory micro-architecture.


**Post 6: Compilation and Applications** . The logical compiler, the Heisenberg simulation in full, and Shor’s period finding: the benchmarks that put fault-tolerant capability into concrete terms.


**Post 7: Enrichments and the Road Ahead** . The enriched walking cat architecture with transversal and cyclic gates, the code database, and what the architecture makes possible next.


**Note to Investors Regarding Forward-Looking Statements**


This press release contains forward-looking statements. All statements contained in this press release other than statements of historical fact are forward-looking statements, including statements regarding IonQ improving its quantum computing technology, IonQ’s roadmap towards fault tolerant and utility-scale quantum computing, and IonQ’s effect on the quantum computing industry. In some cases, you can identify these statements by forward-looking words such as “build,” “strengthen,” “scale,” “unlock,” “perform,’ “blueprint,” “breakthrough,” “trajectory,” “shape,” “pending,” “look forward,” “accelerate,” “anticipate,” “expect,” “suggest,” “plan,” “believe,” “intend,” “estimate,” “target,” “project,” “should,” “could,” “would,” “may,” “will,” “forecast,” “confident” and other similar expressions. These statements are only predictions based on our expectations and projections about future events as of the date of this press release and are subject to a number of risks, uncertainties and assumptions that may prove incorrect, any of which could cause actual results to differ materially from those expressed or implied by such statements, including, among others, those described under the heading “Risk Factors” in our most recent filings with the Securities and Exchange Commission. New risks emerge from time to time, and it is not possible for our management to predict all risks, nor can management assess the impact of all factors on our business or the extent to which any factor, or combination of factors, may cause actual results to differ materially from those contained in any forward-looking statement we make. Investors are cautioned not to place undue reliance on any such forward-looking statements, which speak only as of the date they are made. Except as otherwise required by law, we undertake no obligation to update any forward-looking statement, whether as a result of new information, future events or otherwise.
