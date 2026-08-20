---
schema_version: "1.0.0"
document_id: "15386ad1e41e66ef2facc851404fc7522beb49aee05c44d39415c47e45042b82"
company_key: "xanadu-quantum-technologies-limited-class-b-subordinate-voting-shares"
company: "Xanadu Quantum Technologies Limited"
source_id: "xanadu-quantum-technologies-limited-class-b-subordinate-voting-shares-news-import-9a24fb0a26c8"
canonical_url: "https://pennylane.ai/blog/2026/06/top-compilation-papers-spring-2026"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-26T05:56:13.943329+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:1db974af6bbc0b42e6c11b68e36f09fee5237993e392397d1765e93f96a19196"
---

# Top quantum compilation papers — Spring 2026 edition | PennyLane Blog

1. [Blog](https://pennylane.ai/blog) /


2. [Compilation](https://pennylane.ai/search?categories=compilation&sort=publication_date&contentType=BLOG) /


3.


Top quantum compilation papers — Spring 2026 edition


June 10, 2026


# Top quantum compilation papers — Spring 2026 edition


[Danial Motlagh](https://pennylane.ai/profile/Dan)


Welcome to the spring edition of our new top papers series on quantum compilation and fault-tolerant architectures! The following are our favorite papers of this season based on their potential to move the needle toward making practical quantum computing a reality.


A lot of good work happened this season, so without further ado, let’s get into it!


## Contents


- Top Papers


- 1. Heterogeneous architectures enable a 138x reduction in physical qubit requirements for fault-tolerant quantum computing under detailed accounting
- 2. A Denser Planar Surface Code
- 3. STAR-Magic Mutation: Even More Efficient Analog Rotation Gates for Early Fault-Tolerant Quantum Computer
- 4. Constant-Time Surgery on 2D Hypergraph Product Codes with Near-Constant Space Overhead
- 5. High-performance syndrome extraction circuits for quantum codes


- Honourable mentions


- 1. Clifford synthesis via generalized S and CZ gates
- 2. Quantum Hamlets: Distributed Compilation of Large Algorithmic Graph States
- 3. A Scheduler for the Active Volume Architecture


## Top Papers


### 1.[Heterogeneous architectures enable a 138x reduction in physical qubit requirements for fault-tolerant quantum computing under detailed accounting](https://arxiv.org/abs/2604.06319)


Introduces Q-NEXUS, a framework for developing heterogeneous architectures with specialized modules for computation, memory, and communication where functional specialization can be achieved via heterogeneous QEC encoding and/or hardware modalities. The framework is further complemented with a micro-architecture aware compiler and scheduler (Q-CHESS).


### 2.[A Denser Planar Surface Code](https://arxiv.org/abs/2605.30455)


Presents a QEC code implementable on a 2D grid that achieves a 4.5x improvement in encoding rate over the rotated surface code. Using a mixed-latency architecture and optimized lattice surgery compilation, they show chemically accurate phase estimation of FeMoco can be performed in under a month with only 89k noisy superconducting qubits.


### 3.[STAR-Magic Mutation: Even More Efficient Analog Rotation Gates for Early Fault-Tolerant Quantum Computer](https://arxiv.org/abs/2603.22891)


Introduces a more efficient protocol for implementing logical rotation gates on a quantum computer through direct preparation of magic states corresponding to small angle rotations using only a single surface code patch as ancillary space. Notably, for angles smaller than 1e-5, it achieves two-order-of-magnitude reduction in both the execution time and the error rate of analog rotation gates compared to T-gate synthesis using cultivated magic states.


### 4.[Constant-Time Surgery on 2D Hypergraph Product Codes with Near-Constant Space Overhead](https://arxiv.org/abs/2603.02157)


Constructs surgery gadgets for 2D hypergraph product codes that perform parallel logical measurements with an amortized constant time overhead and near-constant space overhead. This addresses a major bottleneck for qLDPC computation by keeping the space advantages of qLDPC codes without paying O(d) syndrome rounds for every logical operation.


### 5.[High-performance syndrome extraction circuits for quantum codes](https://arxiv.org/abs/2603.05481)


Introduces an automated framework for designing low-depth syndrome-extraction circuits for arbitrary CSS codes using generalized left-right circuits and residual-error metrics. Across diverse code families, the resulting circuits improve logical performance by up to an order of magnitude over existing single-ancilla syndrome-extraction designs.


## Honourable mentions


### 1.[Clifford synthesis via generalized S and CZ gates](https://arxiv.org/abs/2603.24731)


Shows any Clifford circuit can be implemented via at most 2 sets of mutually commuting Pauli product measurements. This provides a flexible framework for space-time trade offs in Clifford circuit synthesis, especially well-suited for qLDPC codes.


### 2.[Quantum Hamlets: Distributed Compilation of Large Algorithmic Graph States](https://arxiv.org/abs/2603.06387)


Introduces a distributed compilation method for large graph states that reduces the Bell-pair cost of distributed MBQC. The key idea is to optimize graph partitions using maximum matching sizes between partitions rather than just cut edges, leading to lower entanglement requirements across QPUs.


### 3.[A Scheduler for the Active Volume Architecture](https://arxiv.org/abs/2603.06376)


Introduces a scheduler for the Active Volume architecture, improving the accuracy of resource estimates. They demonstrate a 1.76x runtime speedup compared to the previous analytic model on a 4×4 Fermi-Hubbard benchmark.


---


We hope you enjoyed our collection of top quantum compilation papers. Stay tuned for the Summer 2026 edition! You can sign up to the[Xanadu newsletter](https://xanadu.us17.list-manage.com/subscribe?u=725f07a1d1a4337416c3129fd&id=294b062630) or follow PennyLane on[LinkedIn](https://www.linkedin.com/company/pennylaneai/) or[Twitter/X](https://twitter.com/PennyLaneAI) to get notified. If you haven't already, make sure to check out our series on[top algorithm papers](https://pennylane.ai/blog/2026/03/top-quantum-algorithms-papers-winter-2026/) .


## About the author


[Danial Motlagh](https://pennylane.ai/profile/Dan)


Searching for real world applications of quantum computers.


*Last modified:* *June 10, 2026*
